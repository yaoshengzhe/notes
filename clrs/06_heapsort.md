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
			error "new key is larger than current key"
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