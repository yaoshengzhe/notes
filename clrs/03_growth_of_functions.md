## Exercises

##### 3.1-1 
Let f(n) and g(n) be asymptotically nonnegative functions. Using the basic definition of \Theta-notation, prove that max(f(n), g(n)) = \Theta(f(n) + g(n)).

By \Theta definition, we need to proof following inequations

	(1) c1(f(n) + g(n)) >= 0
	(2) c1(f(n) + g(n)) <= max(f(n), g(n))
	(3) max(f(n), g(n)) <= c2(f(n)+g(n))
	
(1) holds since both f(n) and g(n) are asymptotically nonnegative functions.

(2) holds because max(f(n), g(n)) >= f(n) and max(f(n), g(n)) >= g(n). Thus we get max(f(n), g(n)) >= (f(n) + g(n))/2, obviously, we can choose c1 = 1/2.

(3) holds because max(f(n), g(n)) <= f(n) + g(n) and c2 could be any positive number larger or equals to 1.

##### 3.1-2
Show that for any real constants a and b, where b > 0,
	(n + a)^b = \Theta(n^b).
	
Proof

	(1) c1(n^b) >= 0
	(2) c1(n^b) <= (n + a)^b
	(3) (n + a)^b <= c2(n^b)
	
(1) holds because both n and b are larger than 0

(2) holds because if we choose any c1 larger than 1 and choose n_0 = -a / (sqrt_b(c1) - 1) which implies c1(n^b) <= (n + a)^b

(3) holds because if we choose any c2 larger than 1 and choose n_0 = a / (sqrt_b(c2) - 1)

Thus final n0 = max(-a / (sqrt_b(c1) - 1), a / (sqrt_b(c2) - 1)), c1 and c2 are any positive number larger than 1.

##### 3.1-3
Explain why the statement, “The running time of algorithm A is at least O(n^2) is meaningless.

Usually, we use "O" notation to indicate upper bound of function running time. Above statement indicates the running time of best case is at least O(n^2), which is meaningless. Because best running time could be \Theta(1), could be \Theta(n) or any thing slower than O(n^2). It doesn't give us any running time upper bound for average case and worst case, even worse, we cannot tell the exact running time bound for the best case.

##### 3.1-4 Is 2^{n+1} = O(2^n) ? Is 2^{2n} = O(2^n) ?

	2^{n+1} = 2 * 2^n
			= O(2^n) 
			
	2^{2n} = 2^n * 2^n
		   = O(2^n) * O(2^n)
		   = O(4^n) != O(2^n)
		   
##### 3.1-5 Prove Theorem 3.1.
_For any two functions f(n) and g(n), we have f(n) = \Theta(g(n)) if and only if f(n) = O(g(n)) and f(n) = \Omega(g(n))._

Proof

	f(n) = \Theta(g(n)) => 0 <= c1 g(n) <= f(n) <= c2 g(n) for all n >= n0
						=> 0 <= f(n) <= c2 g(n) and 0 <= c1 g(n) <= f(n) for all n >= n0
						=> f(n) = O(g(n)) and f(n) = \Omega(g(n))
						
	f(n) = O(g(n)) and f(n) = \Omega(g(n)) 
		=> 0 <= f(n) <= c2 g(n) for all n >= n1 and 0 <= c1 g(n) <= f(n) for all n >= n2
		let n0 = max(n1, n2), thus
		=> 0 <= f(n) <= c2 g(n) for all n >= n0 and 0 <= c1 g(n) <= f(n) for all n >= n0
		=> 0 <= c1 g(n) <= f(n) <= c2 g(n) for all n >= n0
		=> f(n) = \Theta(g(n))
		
##### 3.1-6 Prove that the running time of an algorithm is \Theta(g(n)) if and only if its worst-case running time is O(g(n)) and its best-case running time is \Omega(g(n)).

same as 3.1-5

##### 3.1-7
Prove that o(g(n)) \intersect w(g(n)) is the empty set.

	f(n) = o(g(n)) => 0 <= f(n) < c1 g(n) for all n >= n1
	f(n) = w(g(n)) => 0 <= c2 g(n) < f(n) for all n >= n2
	
	if such f(n) exists, let us choose n0 = max(n1, n2), then
	c2 * g(n) < f(n) < c1 * g(n) for all n >= n0
	=> c2 < f(n) / g(n) < c1 
 	=> f(n) = \Theta(g(n)) => contradict with definition of o and w notations
	=> such f(n) doesn't exist
	=> o(g(n)) \intersect w(g(n)) is the empty set
	
##### 3.1-8
We can extend our notation to the case of two parameters n and m that can go to infinity independently at different rates. For a given function g(n, m), we denote by O(g(n, m)) the set of functions
	
	O(g(n, m)) = {f(n, m): there exist positive constants c, n0, and m0
				  		   such that 0 <= f(n, m) <= c*g(n, m)
					   	   for all n >= n0 or m >= m0}.
						  
Give corresponding definitions for \Omega(g(n, m)) and \Theta(g(n, m)).

	\Omega(g(n, m)) = {f(n, m): there exist positive constants c, n0, and m0
			  		   such that 0 <= c*g(n, m) <= f(n, m)
				   	   for all n >= n0 or m >= m0}.

	\Theta(g(n, m)) = {f(n, m): there exist positive constants c1, c2, n0, and m0
				   	   such that 0 <= c1*g(n, m) <= f(n, m) <= c2*g(n, m)
				   	   for all n >= n0 or m >= m0}.

##### 3.2-1
Show that if f(n) and g(n) are monotonically increasing functions, then so are the functions f(n) + g(n) and f(g(n)), and if f(n) and g(n) are in addition nonnegative, then f(n) * g(n) is monotonically increasing.

	f(n) and g(b) are monotonically increasing functions =>
	for any x1 < x2, f(x1) < f(x2) and g(x1) < g(x2) =>
	f(x1) + g(x1) < f(x2) + g(x2) =>
	let z(n) = f(n) + g(n), then z(x1) < z(x2) =>
	z(n) is monotonically increasing function
	
	f(n) and g(b) are monotonically increasing functions =>
	for any x1 < x2, f(x1) < f(x2) and g(x1) < g(x2) =>
	since both f(n) and g(n) are nonnegative, f(x1) * g(x1) < f(x2) * g(x2) =>
	let z(n) = f(n) * g(n), then z(x1) < z(x2) =>
	z(n) is monotonically increasing function
	
##### 3.2-2
Prove equation (3.16).

_For all real a > 0, b > 0, c > 0, and n,_
	a^(log_b c) = c^(log_b a)
	
	take log_b on both side =>
	log_b(c) * log_b(a) = log_b(a) * log_b(c)
	Q.E.D
	
##### 3.2-3
Prove equation (3.19). Also prove that n! = w(2^n) and n! = o(n^n)

_lg(n!) = \Theta(nlgn)_

	n! = \sqrt(2 *pi *n)(n/e)^n(1 + \Theta(1/n)) =>
	lg(n!) = lg(\sqrt(2 *pi *n)) + nlg(n/e) + lg(1 + \Theta(1/n))
		   = \Theta(nlg(n))
		   
	n! / 2^n = \sqrt(2 *pi *n)(n/e)^n(1 + \Theta(1/n)) / 2^n
			 = \sqrt(2 *pi *n) (n/(2*e))^n (1 + C/n)
			 >= C * n^(n-1)
	thus n! >= 2^n and there is no constant c such that n! >= c 2^n
	n! = w(2^n)
	
	n! = 1*2*...*n < n*n...*n = n^n => n! = o(n^n)
	
##### 3.2-4
Is the function (\upper lgn \upper)! polynomially bounded ? Is the function  (\upper lglgn \upper)! polynomially bounded ?

TODO

##### 3.2-5
Which is asymptotically larger: lg(lg^* n) or lg^*(lgn)?

TODO

##### 3.2-6
Show that the golden ratio \phi and its conjugate \hat{\phi} both satisfy the equation x^2 = x+1.

	\phi^2 = (6 + 2\sqrt(5)) / 4
		   = (3 + \sqrt(5)) / 2
		   = (1 + \sqrt(5)) / 2 + 1
		   = \phi + 1

	\hat{\phi}^2 = (6 - 2\sqrt(5)) / 4
	   		     = (3 - \sqrt(5)) / 2
	   		   	 = (1 - \sqrt(5)) / 2 + 1
	   		     = \hat{\phi} + 1
				 
##### 3.2-7
Prove by induction that the ith Fibonacci number satisfies the equality
	
	F_i = (phi^i - \hat{\phi}^i) / \sqrt(5),

where \phi is the golden ratio and \hat{\phi} is its conjugate

	F_1 = 1 = (phi - \hat{\phi}) / \sqrt(5)
	F_2 = 1 = (phi^2 - \hat{\phi}^2) / \sqrt(5)
	Assume it's true for F_k, then for F_{k+1}
	F_{k+1} = F_k + F_{k-1}
		    = (phi^k - \hat{\phi}^k) / \sqrt(5) + (phi^(k-1) - \hat{\phi}^(k-1)) / \sqrt(5)
			= (phi^k + phi^(k-1) - \hat{\phi}^k - \hat{\phi}^(k-1)) / \sqrt(5)
			= (phi^(k-1) * (phi^k + 1) - \hat{\phi}^(k-1) * (1 + \hat{\phi})) / \sqrt(5)
			= (phi^(k-1) * phi^2 - \hat{\phi}^(k-1) * \hat{\phi}^2) / \sqrt(5)
			= (phi^(k+1) - \hat{\phi}^(k+1)) / \sqrt(5)
	
##### 3.2-8
Show that klnk = \Theta(n) implies k = \Theta(n/ln(n))

TODO		