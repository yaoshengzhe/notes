## Exercises

##### 8.1-1
smallest possible depth = n if array is sorted in increasing order.

##### 8.2-2

	for j = A.length downto 1
		B[C[A[j]]] = A[j]
		C[A[j]] = CA[j]]-1
		
From above code snippet, counting sort insert them into B as the same order in A. Therefore, it is a stable sort algorithm.

##### 8.2-3
counting sort still works but it is no longer stable.

##### 8.2-4
Do first two loops in counting sort and get a array C, C[i] indicates how many elements smaller than i. This could be done in \Theta(n+k).

Given [a..b], the result is C[b] - C[a-1] if (a > 1) or C[b] if a == 1 
		
##### 8.3-2
stable sort: insertion sort, merge sort

For given array A, create array A' = [(1, A[1]), ... (i, A[i])]. Then apply any sort algorithm on A', the compare function on pairs (i, A[i]) and (j, A[j]) is as follows, return A[i] - A[j] if A[i] != A[j] otherwise return i - j. Then copy A' back to A. The overall complexity is \Theta(n) + T(n) where T(n) is the complexity of sort algorithm. 