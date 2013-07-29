## Exercises

##### 7.1-2

q = r 

	PARTITION(A, p, r)
		x = A[r]
		i = p - 1
		for j = p to r-1
			if A[j] <= x
				i = i + 1
				exchange A[i] with A[j]
		exchange A[i+1] with A[r]
		if A[p] == A[r]
			return (p+r)/2
		else
			return i+1
			
##### 7.1-3
array will be scanned once and thus T(PARTITION) = \Theta(n)

##### 7.1-4
just change PARTITION function

	PARTITION(A, p, r)
		x = A[r]
		i = p - 1
		for j = p to r-1
			if A[j] > x
				i = i + 1
				exchange A[i] with A[j]
		exchange A[i+1] with A[r]
		if A[p] == A[r]
			return (p+r)/2
		else
			return i+1
			
##### 7.2-1

	T(n) = T(n-1) + \Theta(n)
		 = T(n-2) + \Theta(n-1) + \Theta(n)
		 = ...
		 = \sum_{i=0}^n \Theta(i)
		 = \Theta(n^2)
		 
##### 7.2-2
T(n) = \Theta(n^2)

##### 7.2-3
Since partition algorithm always select the last element of the subarray as pivot, therefore 

	T(n) = T(n-1) + n =>
	T(n) = \Theta(n^2)
	
##### 7.2-4
The running time of insertion sort depends on how many inversions in the array, T(n) = \Theta(NumOfInversions * n). In almost sorted array, we can assume num of inversions is roughly a constant number, say C. Since quick sort still needs nlogn on average case even if array is almost sorted. Therefore, we know

	Running time of insertion sort T(n) = \Theta(Cn) = \Theta(n)
	Running time of quick sort T(n) = \Theta(nlogn)
	
##### 7.2-5

	min height satisfies
	(1-a)^h = 1/n =>
	h = -lgn/lg(1-a)
	
	max height satisfies
	a^h = 1/n =>
	h = -lgn/lg(a)
	
##### 7.3-2
	Worst case: \Theta(n)
	Best case: \Theta(lgn)

##### 7.4-5
Once the subproblem size is less or equals to k, we just stop and sort k elements using insertion sort.

	The height of such tree satisfies (1/2)^h*n = k =>
	h = lg(n/k)

Plus sorting n/k lists with each has k elements, the total sorting complexity is n/k * O(k^2) + O(nlg(n/k)) = O(nk + nlg(n/k))

## Problems
##### 7-1 Hoare partition correctness
a. 
	
	6, 19, 9, 5, 12, 8, 7, 4, 11, 2, 13, 21 (i=1, j=11)
	6, 13, 9, 5, 12, 8, 7, 4, 11, 2, 19, 21 (i=2, j=11)
	6, 2, 9, 5, 12, 8, 7, 4, 11, 13, 19, 21 (i=2, j=10)
	
b. To prove this, we only need to show x must be in A[i..j]. We first keep decreasing j until A[j] <= x, which guarantees x is on the left side of A[j]; similarly, we then keep increasing i until A[i] >= x, still it guarantees x is on the right side of A[i]. Finally, we conclude x must be in A[i..j] after we exist loop and thus i and j will never access an element of A outside the subarray A[p..r]

d. As partition function shows, A[j+1..r] always great than x.

e. 

	QUICKSORT(A, p, r)
		if p < r
			mid = HOARE-PARTITION(A, p, r)
			QUICKSORT(A, p, mid)
			QUICKSORT(A, mid+2, r)
		
##### 7-2 Quicksort with equal element values

a. T(n) = n^2			

b. 

	PARTITION'(A, p, r)
		q = p
		t = r
		i = p+1
		x = A[p]
		while i < t + 1
			if A[i] < x
				exchange A[q] and A[i]
				q = q + 1
			else if A[i] > x
				exchange A[t] and A[i]
				t = t - 1
			else
				i = i + 1
		return (q, t)
		
c.

	RANDOMIZED-PARTITION'(A, p, r)
		i = RANDOM(p, r)
		exchange A[r] with A[i]
		return PARTITION'(A, p, r)
		
	QUICKSORT'(A, p, r)
		if p < r
			q, t = RANDOMIZED-PARTITION'(A, p, r)
			QUICKSORT'(A, p, q-1)
			QUICKSORT'(A, t+1, r)
			
d. Adjustment: If there are duplicates in the array, once we choose one of the duplicate elements as pivot, we will never choose other duplicate in the subarray. In other words, once a pivot x is chosen with z_i <= x <= z_j, we know what z_i and z_j cannot be compared at any subsequent time.

##### 7-3 Alternative quicksort analysis
TODO

##### 7-4 Stack depth for quicksort
a. last line of while loop is equivalent to call TAIL-RECURSIVE-QUICKSORT(A, q+1, r)   

b. Array is in decreasing order.

c. 

	TAIL-RECURSIVE-QUICKSORT(A, p, r)
		while p < r
			q = RANDOMIZED-PARTITION(A, p, r)
			TAIL-RECURSIVE-QUICKSORT(A, p, q-1)
			p = q + 1
			
##### 7-5 Median-of-3 partition
a. Pr(x less than i) * Pr(x larger than i)

##### 7-6 Fuzzy sorting of intervals
TODO