## Reverse Words in a String

算法：先反转整个string，然后反转每个word。
难点：根据要求，注意预处理string(去除前后space, word之间多余一个的space等等)

## Evaluate Reverse Polish Notation

算法：基本的stack应用，遇到操作符就将stack元素拿出，进行计算后再把结果压栈。
难点：基本没有，注意代码简洁即可。


## Max Points on a Line

算法：枚举每条线段(n * (n-1)条)，计算斜率(slope)，然后用一个map，key是斜率，value是这条线上点的个数。最后找到value最大的就是结果
难点：1. 计算斜率的时候要考虑两点y相同斜率无限大的情况(可以用Double.MAX_VALUE)。
      2. 输入中可能有相同的点，比如(0, 0), (1, 1), (0, 0)这种情况，输出应该为3。可以用一个小技巧，在枚举时，使用双循环
         for (int i=0; i < points.length; ++i) {
           map = new map
           int numOfSamePoints = 1; // 这里用一个变量表示多少点和points[i]相同，因为这个点在外层循环只会被处理一遍
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
         }
         return max;

## Sort List

算法：先找list中点(用fast, slow point解法)，这样把list分成两部分，然后就可以做mergesort
难点：1. 找list中点的时候要考虑清楚哪里是中点。
      2. 做归并的时候代码要写对，因为是list，所以注意正确设置next(递归比较好写)。

## Insertion Sort List

算法：基本的插入排序
难点：注意因为是list，所以要从前往后搜索，并且因为是list，所以插入代码要考虑更新next指针。

## LRU Cache

算法：使用HashMap + LinkedList(Double)。HashMap的value是(Node, value), 其中Node是对应LinkedList里面的Node。
难点：1. 理解问题，get和set都要更新key在LinkedList里面的位置。
      2. HashMap的value是(Node, value)，这是为了要更新最近访问的key在LinkedList中的位置。
      3. LinkedList最好是双向链表，这样可以以O(1)的操作将最近访问的key更新
      4. 在set时，如果key已在HashMap里，只用更新key在LinkedList的顺序就行；否则则必须删除呆的最久的key
      5. DoubleLinkedList和Pair结构要自己实现。。。
