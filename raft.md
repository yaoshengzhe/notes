## Raft

Node三种状态: Leader, Candidate and Follower
变化顺序: Follower -> Candidate -> Leader

Follower:
    1. 启动
	2. 超时: 转为Candidate, 启动Leader Election, 重置timer
	
Candidate:
    1. 超时: 启动Leader Election, 重置timer
	2. 收到VoteResponse或者AppendEntriesResponse，并且Response中的term高于自身的term:
	   转为Follower, 重置timer
	3. 收到Majority of VoteResponse:
	   转为Leader, 重置timer
	   
Leader:
    1. 收到VoteRequest或者AppendEntriesResponse，并且Response中的term高于自身的term:
       转为Follower，重置timer
	   
                time out, starts election                          
               +------------------------+    time out, new election
               |                        |   +--------------------+ 
 Starts Up     |         discovers      v   |                    | 
   +           |         current leader     |                    | 
   |      +----+------+  or new   +---------+-+                  | 
   +----> |  Follower | <---------+ Candidate | <----------------+ 
          +-----------+  term     +------+----+                    
                                         |                         
               ^                         |                         
discover server|                         | recevies votes from     
with higher    |      +-----------+      | majority of servers     
term           +------+   Leader  | <----+                         
                      +-----------+                                

## Leader Election
何时启动新一轮Leader Election: 使用timeout, 每个server有自己的timer, 一旦timer超时，该server就发起一轮leader election。
因此，Leader必须不断发送AppendEntries(没有任何log entry)给follower来避免启动不必要的Election

如果某一server决定要election
    1. 本地的term加一: current_term++
	2. 转为Candidate
	3. 发送VoteRequest给其他servers
	4. 重置timer
	
开始election之后，server继续运行，直到以下任一情况发生
    1. server赢了election
	2. 另外一个server赢了election
	3. 超时
	
赢得election的充分条件：得到大于一半的选票(VoteResponse)

成为Leader之后:
    1. 不停发送heartbeat(AppendEntries without log entry)给其他server 
	2. 发送其他命令
	
## Log Matching Property
1. 对于在不同log里面的两个log entry, 如果它们有同样的index和term, 那么它们一定具有一样的命令(command)
2. 对于在不同log里面的两个log entry, 如果它们有同样的index和term, 那么它们之前的所有log entry都一一对应, 完全相同。

换言之，对于两个log: L1 and L2, 以及两个log entry: E1, E2, 这里E1属于L1, E2属于L2
第一条是指, if (E1.index == E2.index && E1.term == E2.term) then E1.command == E2.command
第二条是指, if (E1.index == E2.index && E1.term == E2.term) then {
	for (int i=E1.index; i > -1; --i) {
		assertTrue(L1[i] == L2[i]); // L1[i] 表示log里第i条log entry
	}
}

如何保证第一条: Leader最多只创建一条log entry 对于给定的log index和term, 并且在log中log entry不会被改变位置
如何保证第二条: 运用归纳法，
           1. 一开始log都是空的，所以正确; 
		   2. 假设两个log的第k个log entry相同，前k-1完全相同。那么对于第k+1个,
           3. 每个server只当接受到AppendEntriesRequest时才可能增加log entry, 并且
		      server会拒绝增加log entry如果request中的prev_log_term和prev_log_index不
			  存在于当前server log中。因此，增加log的唯一前提是该server的第k个log entry
			  和leader 发来的AppendEntriesRequest中的prev_log_term和prev_log_index相同。
			  此时，该server会决定增加log，并且第k+1个log entry的term会等于当前的leader term,
			  index 会等于prev_log_index + 1。并且，由于k+1的log entry是由leader创建的，根据
			  性质一，此log entry是唯一的。所以这两个log第k+1个会相同，如果他们接受了同样的request。
			  如果他们接受的是不同的request, 那么会出现两种情况 (1) 某个server不接受，k+1不相同。(2)
			  两个server都接受(这种情况会出现如果发生Leader Election)，但是term或者index不同(
			  还是由于性质一)。

在Raft中，当leader和follower之间log不一致(inconsistent)时, 解决的办法是强制follower拷贝leader的log。先从后往前找到follower和leader共有的log entry，根据LMP性质二，之前的log entry一定相同。之后follower放弃自身剩下的log entry，拷贝leader的log entry.

当Leader收到失败的AppendEntriesResponse时，简单的将自身前一个entry发给这个follower

Leader永远不会删除或者改变已有log entry的位置，只能追加(append)

## Safety
Commit概念: committed log entry 是指这个log entry已经被majority所接受，并且一定会被所有机器执行，eventually.
Leader限制(restriction): 只有有所有committed log entry的server才有资格成为leader.
  因此，任一时间的leader都有所有的committed, 所以leader没有必要接受其他follower的log entry，所以log entry增加是单向的！！！leader接受client request之后在自己的log entry上加一，然后发送给所有的follower，直到收到majority的成功response，才将这条log entry变成committed.

Vote限制(restriction): 为了达成以上的leader限制(restriction)，在leader election时必须要保证选出的server有所有的committed log。实现的方法是在VoteRequest里加上server自身的term和index; 然后对于任意server, 当接收到VoteRequest时，当且仅当request里的log更加update时才做出肯定回复，否则就拒绝这次的request.

Up-To-Date概念：对于两个log: L1和L2, 如果L1.term < L2.term，那么L2更加up-to-date; 如果L1.term == L2.term && L1.index < L2.index, 那么L2更加up-to-date

Commit的一种edge case: leader收到majority的肯定回复，但是在准备commit该log entry之前，leader挂了。新的leader将被选出，但是新的leader不能根据该log entry已经被majority所接受就推断出该log entry已经被commit。

解决以上edge case的办法: raft永远不commit上一个term的log entry。根据raft协议，每次选举必然会将term增加1，所以如果term不变，那么表示leader没有crash，可以commit。否则考虑到以上的edge case，新leader不能commit上一任leader的最后一条log entry，否则可能会导致committed log entry不一致。

## Membership Changes		   