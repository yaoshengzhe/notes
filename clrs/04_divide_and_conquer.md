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
	
##### 4.3-1 Show that the solution of T(n) = T(n-1) + n is O(n^2)

Assume T(n) = an^2 + bn + c where a >= 1, b >= 0 and c >= 0

	T(n) = T(n-1) + n
		 = a(n-1)^2 + b(n-1) + c + n
		 = an^2 - 2an + a + bn - b + c + n
		 = an^2 + bn + c + (1 - 2a)n + a
		 <= an^2 + bn + c when n >= 1
		 
##### 4.3-2 Show that the solution of T(n) = T(\upper n/2 \upper) + 1 is O(lgn).

Assume T(n) = algn + b, where a >= 10

	T(n) = T(\upper n/2 \upper) + 1
		 <= alg(n/1.9) + b + 1
		  = a (lg(n) - lg1.9) + b + 1
		  = alg(n) + b + 1 - alg1.9
		  <= alg(n) + b when n >= 40
		  
##### 4.3-3 
We saw that the solution of T(n) = 2T(\lower n/2 \lower) + n is O(nlgn). Show that the solution of this recurrence is also \Omega(nlgn). Conclude that the solution is \Theta(nlgn).

Assume T(n) = anlgn + bn + c, where 0 <= a <= 1, b >= 0 and c >= 0

	T(n) = 2T(\lower n/2 \lower) + n
	     = anlg(n/2) + bn + 2c + n
		 = anlgn + (b+1-a)n + 2c
		 >= anlgn + bn + c
		 
##### 4.3-4
T(n) <= anlgn + bn where a >= 1

	T(n) <= anlgn/2 + bn + n
	  	  = anlgn + bn + n - an
	  	  = anlgn + (b+1-a)n
	  	 <= anlgn + bn
		 
##### 4.3-5
Assume T(n) <= anlgn + bn, where a >= 2, b >= 0

	T(n) = 2T(\upper n/2 \upper) + n
		<= anlg(n/2+1) + bn + n
		<= anlg(n/1.9) + bn + n when n >= 40
		 = anlgn + (b+1-alg1.9)n
		<= anlgn + bn 
	thus T(n) = O(nlgn)
	
Assume T(n) >= anlgn + bn, where 0 <= a < 1, b >= 0 

		T(n) = 2T(\upper n/2 \upper) + n
			>= anlgn/2 + bn + n
			 = anlgn + (b+1-a)n
			>= anlgn + bn 
		thus T(n) = \Omega(nlgn)
	
Therefore, T(n) = \Theta(nlgn)

##### 4.3-6 Show that the solution to T(n) = 2T(\lower n/2 \lower + 17) + n is O(nlgn).

Assume T(n) <= anlgn + bn, where a >= 2, b >= 0

	T(n) = 2T(\lower n/2 \lower + 17) + n
		<= a*(n+34)*lg(n/2+17) + b(n+34) + n
		 = anlg(n/2+17) + (b+1)n + 34b + 34alg(n/2+17)
		<= anlg(n/1.9) + (b+1)n when n >= 1000
		 = anlgn + (b+1-alg1.9)n
		 <= anlgn + bn 
	thus T(n) = O(nlgn)
	
##### 4.3-7

Assume T(n) <= cn^{log_3 4}

	T(n) = 4T(n/3) + n
	    <= 4c*(n/3)^{log_3 4} + n
		 = cn^{log_3 4} * 4 / (3^log_3 4) + n
		 = cn^{log_3 4} + n > cn^{log_3 4}
		 
Thus a substitution proof with the assumption T(n) <= cn^{log_3 4} fails.

Assume T(n) <= an^{log_3 4} + bn where a >= 0 and 0 <= b < 2

	T(n) = 4T(n/3) + n
    	<= 4a*(n/3)^{log_3 4} + bn/3 + n
	  	 = an^{log_3 4} * 4 / (3^log_3 4) + (b+3)n/3
	 	 = an^{log_3 4} + (b+1)/3 * n 
		<= an^{log_3 4} + bn
		
##### 4.3-8

Typo, T(n) = \Theta(n^2lgn)

##### 4.3-9

	T(n) = 3T(\sqrt(n)) + lgn
	let T(n) = T(2^m) = S(m), 
	theregore m = lgn and T(n/2) = T(2^m/2) = S(m/2)
	S(m) = T(n) = 3T(\sqrt(n)) + lgn = 3S(m/2) + m
	based on master throrem,
	S(m) = m^{lg_2 3}
	T(n) = (lgn)^{lg_2 3}

##### 4.5-1

a. T(n) = \sqrt(n)

b. T(n) = \sqrt(n)lgn

c. T(n) = n

d. T(n) = n^2

##### 4.5-2

since Strassen's algorithm = o(n^lg7),
 
	n^{log_4 a} <= n^lg7 =>
		log_4 a <= lg 7 =>
		lga / 2 <= lg7 =>
		lga <= lg49 =>
		a <= 49

thus the largest a = 49

##### 4.5-3
log_b a = 0, based on master theorem (2), T(n) = lgn

##### 4.5-4
No, since n^2lgn = \Omega(n^2) but af(n/b) = n^2(lgn - 1) > cf(n) for n larger enough.

##### 4.5-5
T(n) = 4T(n/2) + n^2lgn

## Problems
##### 4-1 Recurrence examples

a. T(n) = \Theta(n^4)

b. T(n) = \Theta(n)

c. T(n) = \Theta(n^2lgn)

d. T(n) = \Theta(n^2)

e. T(n) = \Theta(n^lg7)

f. T(n) = \Theta(\sqrt(n)lgn)

##### 4-2 Parameter-passing costs

a.

	1) T(n) = T(n/2) + 1 => T(N) = lgN
	2) T(n) = T(n/2) + N => T(N) = NlgN
	3) T(n) = T(n/2) + n => T(N) = N
	
b.
	1) T(n) = 2T(n/2) + n => T(N) = NlgN
	   T(n) = 2T(n/2) + 2N => T(N) = N^2lgN
	   T(n) = 2T(n/2) + 2n => T(N) = NlgN
	   
##### 4-3 More recurrence examples

a. T(N) = \Theta(n^{log_3 4})

b. T(N) = \Theta(n/lgn)

c. T(N) = \Theta(n^2\sqrt(n))

d. T(N) = \Theta(n)

e. T(N) = \Theta(n/lgn)

f. T(N) = \Theta(n)

g. T(N) = w(n)

h. T(N) = \Theta(nlgn)

j. T(n) = \Theta(nlglgn)

	T(n) = T(2^m) = S(m), m = lgn, \sqrt(n) = 2^{m/2}
	S(m) = 2^{m/2} S(m/2) + 2^m =>
	S(m) = \Theta(2^mlgm)
	thus, T(n) = \Theta(nlglgn)
	
##### 4-4 Fibonacci numbers
TODO

##### 4-5 Chip testing
	

