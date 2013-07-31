## Exercises

##### 9.1-1

group adjacent two elements and compare them and only the smaller one will be considered in the next round. In the next round, we will apply above procedure but only for elements survived from last round, thus only $\lceil n/2  \rceil$ elements will be considered. Repeat above procedure until only one element survived and this is the smallest element.

The complexity of above procedure equals to n - 1 comparisons. Now for the second smallest one, it must be the smallest one among those elements used to be compared with the smallest one and there are $\lceil \lg n \rceil$ of them, and thus it takes $\lceil \lg n \rceil - 1$ comparisons to find it.

Put it together, the second smallest of n elements can be found with $n + \lceil \lg n \rceil - 2$ comparisons in the worst case.

##### 9.2-1
For RANDOMIZED-SELECT(A, p, q-1, i), this will be called only if i < k which indicates 

	q - p + 1 > i >= 1 => 
	q - p + 1 > 1 => 
	q > p =>
	q - 1 >= p

For RANDOMIZED-SELECT(A, q+1, r, i-k), this will be called only if i > k which indicates 

	q - p + 1 < i =>
	q + 1 < i + p =>
	q + 1 < r - p + 1 + p =>
	q < r =>
	q + 1 <= r
	
Put it together, RANDOMIZED-SELECT never makes a recursive call to a 0-length array.
 
##### 9.2-3
	RANDOMIZED-SELECT(A, p, r, i)
		while p <= r
			if p == r
				return A[p]
			q = RANDOMIZED-pARTITION(A, p, r)
			k = q - p + 1
			if i == k
				return A[q]
			else if i < k
				r = q - 1
			else
				p = q + 1
				i = i - k
				
##### 9.2-4
9 8 7 6 5 4 3 2 1

## Problems
##### 9-1 Largest i numbers in sorted order
a. $O(n\lg n)$

b. $O(n + i \lg n)$

c. $O(n + i\lg i)$

##### 9-2 Weighted median
a. Let $x_k$ be the median (lower median if number is even), then we know

	  $\sum_{x_i < x_k} w_i$ 
	$= (\lfloor n/2 \rfloor - 1) * 1/n < 1/2$
	                     
	  $\sum_{x_i > x_k} w_i$
	$= \lfloor n/2 \rfloor * 1/n <= 1/2$
	
b. 
	1. sort elements by their weights.
	2. create an array where $A[i] = \sum_{j <= i} w_j$, this can be done in O(n)
	3. do a binary search on A and find the median
	
Overall complexity: $O(n\lg n)$

