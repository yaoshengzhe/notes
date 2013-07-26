## Exercises

##### 6.1-1 What are the minimum and maximum numbers of elements in a heap of height h?

	Minimum: 2^h
	Maximum: 2^{h+1} - 1
	
##### 6.1-2 Show that an n-element heap has height \lower lgn \lower.

Assume heap height is h, then 
	2^h <= n <= 2^{h+1} - 1 =>
	h <= lgn and lg(n+1) - 1 <= h =>
	lg(n+1) - 1 <= h <= lgn
	since lgn - lg(n+1) + 1 < 1 and h must be an integer
	therefore, h = \lower lgn \lower
	
##### 6.1-3 Show that in any subtree of a max-heap, the root of the subtree contains the largest value occurring anywhere in that subtree.

In max-heap, we know A[PARENT(i)] >= A[i], which implies  the root of the subtree contains the largest value occurring anywhere in that subtree.

##### 6.1-4 Where in a max-heap might the smallest element reside, assuming that all elements are distinct?

The smallest element is in one of the heap leaf nodes.

##### 6.1-5 Is an array that is in sorted order a min-heap?

yes

##### 6.1-6 Is the array with values <23, 17, 14, 6, 13, 10, 1, 5, 7, 12> a max-heap?

No, because A[3] = 6 < A[8] = 7, but A[3] is the parent of A[8]

##### 6.1-7
Since 2 * \lower n/2 \lower <= n which implies this node must have at least one child. Instead 2 * (\lower n/2 \lower + 1) >= n+1, thus there is no child for node \lower n/2 \lower + 1, that is true for all nodes after \lower n/2 \lower + 1. Therefore the statement is true.

##### 6.3-2
Otherwise, the top node may not be the node with maximum value.

##### 6.4-3
increasing order: \Theta(nlgn)

decreasing order: \Theta(nlgn)

##### 6.4-4

The worst-case running time of MAX-HEAPIFY is \Omega(lgn), thus the worst-cast running time of heapsort is \Omega(nlgn)

##### 6.5-3

	HEAP-MINIMUM(A)
		return A[1]
		
	HEAP-EXTRACT-MIN(A)
		if A.heap-size < 1
			error "heap underflow"
		min = A[1]
		A[1] = A[A.heap-size]
		A.heap-size = A.heap-size -1 
		MIN-HEAPIFY(A, 1)
		return min
		
	HEAP-DECREASE-KEY(A, i, key)
		if key > A[i]
			error "new key is larger than current key"
		A[i] = key
		while i > 1 and A[PARENT(i)] > A[i]
			exchange A[i] with A[PARENT(i)]
			i = PARENT(i)
			
	MIN-HEAP-INSERT(A, key)
		A.heap-size = A.heap-size + 1
		A[A.heap-size] = \infinity
		HEAP-DECREASE-KEY(A, A.heap-size, key)
		
##### 6.5-4
We have to give the new node a initial value and actually that could be any value as long as it is smaller than key.

##### 6.5-5

loop start: A[1..A.heap-size] satisfies max-heap property except A[i] and A[i] > A[PARENT(i)]

in loop: exchange A[i] and A[PARENT(i)] if A[i] > A[PARENT(i)]. After exchange, we still get A[1..A.heap-size] satisfies max-heap property except A[PARENT(i)] and A[PARENT(i)] > A[PARENT(PARENT(i))]. Finally, we let i = PARENT(i), thus loop invariant still holds.

exit: A[1..A.heap-size] satisfies max-heap property except A[i] and A[i] > A[PARENT(i)] and such i must be the first element. Therefore, A[1..A.heap-size] satisfies max-heap property

##### 6.5-6
	HEAP-INCREASE-KEY(A, i, key)
		if key < A[i]
			error "new key is less than current key"
		while i > 1 and A[PARENT(i)] < key
			A[i] = A[PARENT(i)]
			i = PARENT(i)
		A[i] = key
		
##### 6.5-7

queue: maintain a global decreasing count, decrease that count and use it as heap node priority for each element insertion.

stack: queue: maintain a global increasing count, increase that count and use it as heap node priority for each element insertion.

##### 6.5-8
	HEAP-DELETE(A, i)
		exchange A[i] and A[heap-size]
		A.heap-size = A.heap-size + 1
		MAX-HEAPIFY(A, i)
	
##### 6.5-9

	class Node {
		public Integer val;
		public int whichArray;
		public Node(int val, int whichArray) {
			this.val = val;
			this.whichArray = whichArray;
		}
	}
	
	List<Integer> kmerge(List<List<Integer>> arrs) {
		PriorityQueue<Node> minHeap = new PriorityQueue<Node>(new Comparator<Node>() {
			public int compare(Node a, Node b) {
				return a.val - b.val;
			}
		});
		List<Integer> result = new ArrayList<Integer>();
		int k = arrs.size();
		int[] arrPos = new int[k];
		for (int i=0; i < k; ++i) {
			arrPos[i] = 1;
			minHeap.add(new Node(arrs[i][0], i));
		}
		while (!minHeap.empty()) {
			Node node = minHeap.pop();
			result.add(node.val);
			int curPos = arrPos[node.whichArray];
			if (arrs[node.whichArray].length > curPos) {
				arrPos[node.whichArray]++;
				minHeap.add(new Node(arrs[node.whichArray][curPos+1], node.whichArray));
			}
		}
		return result;
	}
	
## Problems
##### 6-1 Building a heap using insertion

a. Not always, here is an counterexample.

For array A = <1, 2, 3, 4, 5>, original build heap algorithm will return A' = <5, 4, 3, 1, 2> vs the new algorithm will return <5, 4, 2, 1, 3>

b. The cost of each insertion operation = \Theta(lgh) where h the heap height at the time of insertion.

For inserting n elements, the total cost T(n) 
	
	T(n) = \sum_{i=2}^{n} \Theta(lgh)
		 = \sum_{i=2}^{n} \Theta(lgi)
		 = \Theta(lg(n!))
		 = \Theta(nlgn)
		 
##### 6-2 Analysis of d-ary heaps
a. For node A[i], it has children: A[d*i], A[d*i+1], ..., A[d*i+d-1]

b. 

	n <= d^{h+1}-1 =>
	log_d(n+1) - 1 <= h <= log_d(n+1)
	h = \upper log_d(n) \upper
	
c. Overall complexity = O(dlog_d(n))  

	EXTRACT-MAX(A)
		if A.heap-size < 1
			error "heap underflow"
		max = A[1]
		A[1] = A[A.heap-size]
		A.heap-size = A.heap-size -1 
		MAX-HEAPIFY(A, 1)
		return max
		
	MAX-HEAPIFY(A, i)
		k = i
		for j=d*i to d*i+d-1 
			if A[j] > A[k]
				k = j
		if k != i
			exchange A[i] and A[k]
			MAX-HEAPIFY(A, k)
			
d. Overall complexity = O(log_d(n))  

	HEAP-INSERT(A, key)
		A.heap-size = A.heap-size + 1
		A[A.heap-size] = -\infinity
		HEAP-INCREASE-KEY(A, A.heap-size, key)

e.  Overall complexity = O(log_d(n)) 
	
	HEAP-INCREASE-KEY(A, i, key)
		if key < A[i]
			error "new key is larger than current key"
		A[i] = key
		while i > 1 and A[PARENT(i)] > A[i]
			exchange A[i] with A[PARENT(i)]
			i = PARENT(i)
			
##### 6-3 Young tableaus

a. 

	  2    4     12    14
	  3    9    \inf  \inf
	  5    16   \inf  \inf
	  8   \inf  \inf  \inf
	  
b. Since Y[1,1] < Y[1,2] and Y[1,1] < Y[2, 1], thus Y[1,1] = \inf indicates both Y[1,2] and Y[2,1] are equals to \inf, thus Y is empty.

Since Y[m-1,n] < Y[m,n] and Y[m,n-1] < Y[m, n], thus Y[m,n] < \inf indicates both Y[m-1,n] and Y[m,n-1] are less than \inf, thus Y is full.

c. 
			
	EXTRACT-MIN(Y)
		min = Y[1,1]
		Y[1, 1] = \inf
		EXTRACT-MIN-HELPER(Y, 1, 1)
		return min
		
	EXTRACT-MIN-HELPER(Y, i, j)
		a, b = i, j
		if i+1 <= m and Y[a, b] > Y[i+1, j]
			a = i+1
		if j+1 <= n and Y[a, b] > Y[i, j+1]
			b = j+1
		if i != a and j != b
			exchange Y[i, j] and Y[a, b]
			EXTRACT-MIN-HELPER(Y, a, b)
			
d.
	
	INSERT(Y, key)
		INSERT-HELPER(Y, key, 1, 1)
	
	INSERT-HELPER(Y, key, i, j)
		if Y[i, j] < \inf 
			if Y[i, j] > key
				exchange Y[i, j] and key	
			if i < m
				INSERT-HELPER(Y, key, i+1, j)
			else
				INSERT-HELPER(Y, key, i, j+1)
		else
			Y[i, j] = key
			
e. First construct a Y table and then call EXTRACT-MIN n^2 times
	
	SORT(A)
		init Y[n, n]
		for i=1 to n^2
			INSERT(Y, A[i])
		for i=1 to n^2
			A[i] = EXTRACT-MIN(Y)

BOTH INSERT and EXTRACT-MIN are called n^2 times; since each of them has time complexity O(n), thus the overall sort complexity is O(n^3)

f.

	EXISTS(Y, key)
		row, col = m, n
		while row > 0 and col > 0
			if Y[row, col] == key
				return true
			else if Y[row, col] > key
				col = col - 1
			else
				row = row - 1
		return false