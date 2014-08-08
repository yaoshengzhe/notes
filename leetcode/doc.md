## Reverse Words in a String

* 算法：先反转整个string，然后反转每个word。
* 难点：根据要求，注意预处理string(去除前后space, word之间多余一个的space等等)

## Evaluate Reverse Polish Notation

* 算法：基本的stack应用，遇到操作符就将stack元素拿出，进行计算后再把结果压栈。
* 难点：基本没有，注意代码简洁即可。


## Max Points on a Line

* 算法：枚举每条线段(n * (n-1)条)，计算斜率(slope)，然后用一个map，key是斜率，value是这条线上点的个数。最后找到value最大的就是结果
* 难点：

    - 计算斜率的时候要考虑两点y相同斜率无限大的情况(可以用Double.MAX_VALUE)。
    - 输入中可能有相同的点，比如(0, 0), (1, 1), (0, 0)这种情况，输出应该为3。可以用一个小技巧，在枚举时，使用双循环

            for (int i=0; i < points.length; ++i) {
                map = new mapint numOfSamePoints = 1; // 这里用一个变量表示多少点和points[i]相同，因为这个点在外层循环只会被处理一遍
                int max = 0;
                for (int j=i+1; j < points.length; ++j) {
                if (points[i] == points[j]) {
                    numOfSamePoints++;
                } else {
                    map.put(slope, map.get(slope, 0) + 1);
                }
            }
            
            for (val : map.values()) {
                if (max < val + numOfSamePoints) {
                    max = val + numOfSamePoints;
                }
            }
            return max;

## Sort List

* 算法：先找list中点(用fast, slow point解法)，这样把list分成两部分，然后就可以做mergesort
* 难点：

    - 找list中点的时候要考虑清楚哪里是中点。
    - 做归并的时候代码要写对，因为是list，所以注意正确设置next(递归比较好写)。

## Insertion Sort List

* 算法：基本的插入排序
* 难点：注意因为是list，所以要从前往后搜索，并且因为是list，所以插入代码要考虑更新next指针。

## LRU Cache

* 算法：使用HashMap + LinkedList(Double)。HashMap的value是(Node, value), 其中Node是对应LinkedList里面的Node。
* 难点：

      - 理解问题，get和set都要更新key在LinkedList里面的位置。
      - HashMap的value是(Node, value)，这是为了要更新最近访问的key在LinkedList中的位置。
      - LinkedList最好是双向链表，这样可以以O(1)的操作将最近访问的key更新
      - 在set时，如果key已在HashMap里，只用更新key在LinkedList的顺序就行；否则则必须删除呆的最久的key
      - DoubleLinkedList和Pair结构要自己实现。。。

## Binary Tree Postorder Traversal

* 算法：基本的后序遍历实现，先左再右然后当前节点。
* 难点：无

## Binary Tree Preorder Traversal

* 算法：基本的先序遍历实现，先当前节点再左然后右。
* 难点：无

## Reorder List

## Linked List Cycle II

* 算法：先通过快慢指针法确定链表是否有环。若有环，令一指针指向head，然后慢指针和该指针每次同时移动一个，直到他们指向同一个节点，该节点即为环入口。
* 难点：

    - 快慢指针的退出条件(fast != null && fast != slow) 以及一开始要确保fast和slow已经移动(不指向head)
    - 如何找出环入口。假设慢指针在遇到快指针时走了k个节点(所以快指针走了2k个节点)，环入口是第r个节点，环总共有i个节点。
      那么必然有：(k - r) % i == (2k - r) % i
      根据同余性质: a mod d = b mod d => (a - b) 整除 d
      所以这里 ((2k - r) - (k - r)) = k, k 整除 i
      所以设k = n * i
      那么慢指针离环入口有k - r = n*i - r个node
      所以如果令一指针从head开始走r个节点，并且慢指针也走r个节点，它们会在环入口相遇。
      换言之：如果它们相遇，从链表头开始走的指针指向环入口。

## Linked List Cycle

* 算法：快慢指针。
* 难点：快慢指针的退出条件(fast != null && fast != slow) 以及一开始要确保fast和slow已经移动(不指向head)

## Word Break II

## Word Break

## Copy List with Random Pointer

* 算法：三次循环。
      第一次：对于原链表每一个节点，复制一份添加到后后面。
      第二次：将复制节点的random赋予正确的值。
      第三次：分裂成两个链表，返回复制的链表。
      比如：如果原来链表是 A -> B -> C -> D
        第一遍之后的链表变为: A -> A' -> B -> B' -> C -> C' -> D -> D' (A'是A的拷贝，但是random还未赋值)
        第二遍便利链表，将A'.random设为A.random.next (A.random是原链表的某节点，根据算法，下一个必为其拷贝)
        第三遍分裂链表, 得到 A -> B -> C -> D 和 A' -> B' -> C' -> D'
* 难点：

    - 注意random可能为null, 所以第二遍时，要在random不为null时才用X'.random = X.random.next
    - 第三遍分裂链表，在设置拷贝节点的next时要注意，只有当该节点不是最后一个节点时，才用X'.next = X.next.next

            if (X.next != null) {
                X'.next = X.next.next;
            }

## 

* 算法：
* 难点：

## 

* 算法：
* 难点：

## 

* 算法：
* 难点：


## Merge Intervals

* 算法：先按Interval.start排序。然后用一个current表示待插interval，扫一遍数组，如果当前的interval.start大于current.end就将current插入到结果中并设current = interval；否则如果interval.end >= current.end，则设current.end = interval.end。结束循环后注意判断current是否为null，如果不为null就将current加入。
* 难点：

    - 先排序可以使整个算法变为O(nlgn)并简化算法
    - 循环时，如果当前的interval不和current重合的条件是interval.start > current.end，因为已按start排序
    - 如果有重合，只需将current.end设为当前interval.end，同样，原因是已按start排序

## 

* 算法：
* 难点：

## Rotate Image

* 算法：先将矩阵沿对角线翻转，然后再延中间行翻转

    1 2 -> 4 2 -> 3 1
    3 4    3 1    4 2

* 难点：

    - 对于矩阵(i, j)，对应的对角线对称点是(n - j - 1, n - i - 1)
    - 对于矩阵(i, j)，对应的中间行对称点是(n - i - 1, j)

## First Missing Positive

* 算法：如果A[i] != i+1, 将A[i]与A[A[i]-1]交换，直到无法进行，然后i++。扫描一遍数组并完成以上操作后，在扫描数组第二遍，如果碰到A[i] != i+1，就直接返回i+1。如果第二次循环能够完成，则返回A.length + 1。

* 难点：

    - 考虑数组只有一个元素情况: {0}，我们应该返回1。所以这里A[i]要等于i+1, 来回避这个情况。
    - 第一次扫描时，要确保A[A[i]-1]存在并且其值不等于A[i]，所以条件是A[i] >= 1 && A[i] != (i+1) && A[i] - 1 < A.length && A[i] != A[A[i]-1]
    - 第一次扫描时，如果发生A[i]与A[A[i]-1]的交换，则不能i++。只有当我们无法交换A[i]与A[A[i]-1]时，才考虑下一个元素。
    - 算法O(n)的原因是，第一次扫描最多用n次交换既能保证A[i] == i+1，所以最多循环2n次(n次交换 + n次i++)。

## Combination Sum II

* 算法：将输入排序。然后使用一个辅助函数，该函数意义是将所有num[start:]中符合target的结果保存到result中。基本情况: 如果target==0，将结果保存；如果target < 0, 直接返回；如果start == num数组长度，直接返回。之后的递归，分两种情况:

    - 结果中包含num[start]：将num[start]添加到buf中，然后递归(start + 1, target - num[start]) 
    - 结果中不含num[start]: 直接递归(start + 1, target)

* 难点：

    - 为了去重，解法中的两个递归要按顺序，先递归包含num[start]，然后再递归不含num[start]。中间跳过所有和num[start]相同的元素。道理也很简单，递归函数的意义是结果中包含一个至多个num[start]；既然如此，第二个递归不应该考虑和num[start]相同的元素。这也是为什么一开始要对num数组排序的意义。

## Generate Parentheses

* 算法：Dfs思想，如果左括号数量没达到n, 递归调用genParen(numLeft+1, numRight)；之后如果左括号数量大于右括号数量，调用genParen(numLeft, numRight+1)
* 难点：

    - 基本条件，当左右括号总数为2n时，添加到最后结果中。
    - 使用辅助函数，用一个char[2n]的缓冲来表示当前括号组合可能
    - 先对numLeft < n递归，然后对numLeft > numRight递归。第一种情况设buf[numLeft+numRight]为'('，后一种为')'

## Valid Parentheses

* 算法：用一个栈，如果当前ch是左括号则入栈；如果不是，当栈为空或者栈顶括号不匹配时返回false，否则则匹配并弹出栈顶元素。出了循环后，返回栈是否为空。
* 难点：注意可以用一个辅助函数判断括号是否匹配。

## String to Integer (atoi)

* 算法：规范化一下输入的string, 注意判断是否第一位是符号位，然后简单的10 * result + s.charAt(i) - '0'
* 难点：

    - 用long来简化溢出判断。
    - 注意脑残leetcode的测试，用一个normalize(String str)先规范化一下输入的str比较好

## Reverse Integer

* 算法：通过x%10得到个位，然后result = 10*result + x%10, 之后x /= 10，直到x == 0退出
* 难点：记得处理x为负数情况。

## Longest Palindromic Substring

算法：1

## Add Two Numbers

* 算法：有递归解法和迭代解法。递归要注意基本条件里处理进位的情况。这题其实迭代比较简单。迭代法：创建一个dummy node，循环终止条件是(l1 != null || l2 != null || carry != 0)，循环体里面计算l1.val + l2.val + carry(如果l1或者l2为null就不加)，然后carry = val / 10，新的node为val % 10
* 难点：递归记得处理基本条件(不要忘记carry)。迭代要想清楚循环退出条件。

## Median of Two Sorted Arrays

* 算法：用find Kth elements in two sorted arrays做。如果两个数组长度为奇数，就返回findK(A, B, (A.len + B.len) / 2)，否则就调用两次。注意，这里k的范围是\[0, A.len + B.len - 1\] (当然可以从1开始，算法小改动)
* 难点：

    - 基本情况：当一个数组为空时，返回另外一个数组B[startB + k - 1]
    - 考虑两个数组[startA, endA)和[startB, endB)中寻找第k大的元素
    - 首先计算midA = startA + (endA - startA) / 2 和midB = startB + (endB - startB) / 2
    - 如果k小于midA + midB - startA - startB + 1，比较A[midA]和B[midB]，丢掉大的那个数组的后半部分(包括对应的mid)
    - k大于等于的情况类似，只是要丢掉A[midA]和B[midB]小的那个数组的前半部分(包括对应的mid)

## Two Sum

* 算法：使用一个HashMap，扫一遍数组。HashMap的key是数字，value是该数字在数组中所有index最大值。然后扫第二遍，每次都查HashMap看看target - numbers[i]在不在HashMap中，如果在并且HashMap返回的index不等于当前i，则能找出这样的组合。
* 难点：某个特定数字可能只出现一次，比如要找target = 4，数组是[1, 2]，那么我们就算在HashMap找到了这个值也不能直接判断。这里我们要确认有两个2。方法就是如算法所说，用HashMap并且判断返回的index不能等于当前数字的index。


