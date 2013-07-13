## Exercises

##### 4.1-1 What does FIND-MAXIMUM-SUBARRAY return when all elements of A are negative?

It will return a index to the largest negative number in A.

##### 4.1-2 Write pseudocode for the brute-force method of solving the maximum-subarray problem. Your procedure should run in \Theta(n^2) time.

	int[] acc_sum = new int[n];
	acc_sum[0] = A[0];
	for (int i=1; i < n; ++i) {
		acc_sum[i] = acc_sum[i-1] + A[i];
	}
	max_sum = A[0];
	for (int i=0; i < n; ++i) {
		for (int j=i; j < n; ++j) {
			max_sum = max(max_sum, acc_sum[j] - acc_sum[i] + A[i]);
		}
	}
	return max_sum
	
##### 4.1-3
Implement both the brute-force and recursive algorithms for the maximum- subarray problem on your own computer. What problem size n0 gives the crossover point at which the recursive algorithm beats the brute-force algorithm? Then, change the base case of the recursive algorithm to use the brute-force algorithm whenever the problem size is less than n0. Does that change the crossover point?

Yes, that changes the crossover point.

##### 4.1-4
Suppose we change the definition of the maximum-subarray problem to allow the result to be an empty subarray, where the sum of the values of an empty subarray is 0. How would you change any of the algorithms that do not allow empty subarrays to permit an empty subarray to be the result?

We can add a wrap method and do array empty in that wrap method. e.g.

	Find-Max-Subarray-Wrapper(A)
		if (A.isEmpty()) {
			// return an error code or throw an exception
		}
		// Apply our Find-Max-Subarray algorithm here.
	
##### 4.1-5

	max_end_here = 0
	max_so_far = 0
	for (int i=0; i < n; ++i) {
		max_end_here = max(0, max_end_here + A[i])
		max_so_far = max(max_so_far, max_end_here)
	}
	return max_so_far
		