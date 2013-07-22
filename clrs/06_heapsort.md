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