## Exercises

##### 2.2-1 Express the function n^3/1000 - 100n^2 - 100n + 3 in terms of \Theta notation.
	\Theta(n^3)
	
##### 2.2-2 Consider sorting n numbers stored in array A by first finding the smallest element of A and exchanging it with the element in A[1]. Then find the second smallest element of A, and exchange it with A[2]. Continue in this manner for the first n - 1 elements of A. Write pseudocode for this algorithm, which is known as selection sort. What loop invariant does this algorithm maintain? Why does it need to run for only the first n - 1 elements, rather than for all n elements? Give the best-case and worst-case running times of selection sort in ‚-notation.

	for (int i=1; i < A.length; ++i)
		int min = i
		for (int j=i+1; j < A.length; ++j)
			if A[j] < A[min]
				min = j
		swap(A[i-1], A[min])
		
Loop invariant: A[0] ... A[i-1] are sorted.

we don't need to run the algorithm if A.length == 1.
if A.length > 1, we only need to put (A.length-1) elements in the right position and thus we need loop n-1 elements

Best case: n^2
Worst case: n^2

##### 2.2-3 Consider linear search again (see Exercise 2.1-3). How many elements of the in- put sequence need to be checked on the average, assuming that the element being searched for is equally likely to be any element in the array? How about in the worst case? What are the average-case and worst-case running times of linear search in ‚-notation? Justify your answers.

Checked elements on average: n/2 = \Theta(n)
Worst case: n = \Theta(n)

##### 2.2-4 How can we modify almost any algorithm to have a good best-case running time?

Find the input of base case -> Pre-calculate the solution -> check if input is the bast case at the beginning of the algorithm -> return pre-calculated answer if it is; continue to run regular algorithm if not.

##### 2.3-1 Using Figure 2.4 as a model, illustrate the operation of merge sort on the array A <3, 41, 52, 26, 38, 57, 9, 49>.

3, 9, 26, 38, 41, 49, 52, 57
3, 26, 41, 52   |   9, 38, 49, 57
3, 41 | 26, 52 | 38, 57 | 9, 49
3 | 41 | 52 | 26 | 38 | 57 | 9 | 49

##### 2.3-2 Rewrite the MERGE procedure so that it does not use sentinels, instead stopping once either array L or R has had all its elements copied back to A and then copying the remainder of the other array back into A.

	Merge (A, p, q, r)
		n_1 = q - p + 1
		n_2 = r - q
		// let L[1..n_1 + 1] and R[1..n_2+1] be new arrays
		for (int i=0; i < n_1; ++i)
			L[i] = A[p+i-1]
		for (int i=0; i < n_2; ++i)
			R[i] = A[q+i]
		int i, j;
		i = j = 0 
		while (i < n_1 && j < n_2)
			if (L[i] < R[j]) 
				A[i+j] = L[i]
				i++
			else
				A[i+j] = R[j]
				j++
		while (i < n_1)
			A[i+j] = L[i]
			i++
		while (j < n_2)
			A[i+j] = R[j]
			j++
		
##### 2.3-3 Use mathematical induction to show that when n is an exact power of 2, the solution of the recurrence
	T(n) = 2             if n =2
	       2T(n/2) + n   if n=2^k, for k > 1
is
	T[n] = nlgn

_Proof_

if n = 2 => T(n) = 2 = 2 lg 2 = nlgn
assume statement holds for T(k) where k > 2
then T(2k) = 2T(k) + 2k = 2klgk + 2k = 2k(lgk + 1) = 2k(lgk + lg2) = 2klg2k

Q.E.D.

##### 2.3-4 We can express insertion sort as a recursive procedure as follows. In order to sort A[1..n], we recursively sort A[1..n-1] and then insert A[n] into the sorted array A[1..n-1]. Write a recurrence for the running time of this recursive version of insertion sort.

	insertion_sort(A, n)
		if (n > 1)
			insertion_sort(A, n-1)
			key = A[n]
			int i=n-1
			for (; i > -1; --i)
				if (A[i] > key)
					A[i+1] = A[i]
			A[i+1] = key
			
##### 2.3-5 Referring back to the searching problem (see Exercise 2.1-3), observe that if the sequence A is sorted, we can check the midpoint of the sequence against v and eliminate half of the sequence from further consideration. The binary search al- gorithm repeats this procedure, halving the size of the remaining portion of the sequence each time. Write pseudocode, either iterative or recursive, for binary search. Argue that the worst-case running time of binary search is \Theta(lgn).

Iterative

	binary_search(A, start, end, target)
		while (start < end)
			mid = start + (end - start)/2
			if (A[mid] == target)
				return mid
			else if (A[mid] < target)
				start = mid + 1
			else
				end = mid
		return -1
		
Recursive

	binary_search(A, start, end, target)
		if (start < end)
			mid = start + (end - start) / 2
			if (A[mid] == target)
				return mid
			else if (A[mid] < target)
				return binary_search(A, mid+1, end, target)
			else
				return binary_search(A, start, mid, target)
		return -1

Base on above code, for each iteration, algorithm will throw away about half elements based on mid point compare result. Therefore T(n) = T(n/2) => T(n) = lgn

#####2.3-6 Observe that the while loop of lines 5–7 of the INSERTION-SORT procedure in Section 2.1 uses a linear search to scan (backward) through the sorted subarray A[1..j-1]. Can we use a binary search (see Exercise 2.3-5) instead to improve the overall worst-case running time of insertion sort to \Theta(nlg n)?

No, because array insert operation is O(n) so overall complexity is still O(n) * O(n) = O(n^2)

##### 2.3-7 Describe a \Theta(nlg n)-time algorithm that, given a set S of n integers and another integer x, determines whether or not there exist two elements in S whose sum is exactly x. 

sort S -> for each element of sorted S, test wheter (x - element) is in it by binary search.

Overall complexity = O(sorting) + O(n) * O(binary_search)
if we use merge sort, then = O(nlogn) + O(n)*O(lgn) = O(nlgn)

## Problems

##### 2-1 Insertion sort on small arrays in merge sort
Although merge sort runs in \Theta(nlgn) worst-case time and insertion sort runs in \Theta(n^2) worst-case time, the constant factors in insertion sort can make it faster in practice for small problem sizes on many machines. Thus, it makes sense to *coarsen* the leaves of the recursion by using insertion sort within merge sort when subproblems become sufficiently small. Consider a modification to merge sort in which n=k sublists of length k are sorted using insertion sort and then merged using the standard merging mechanism, where k is a value to be determined.

_a. Show that insertion sort can sort the n=k sublists, each of length k, in ‚.nk/ worst-case time._

_b. Show how to merge the sublists in \Theta(nlg(n/k)) worst-case time._

_c. Given that the modified algorithm runs in \Theta(nk + nlg(n/k)) worst-case time, what is the largest value of k as a function of n for which the modified algorithm has the same running time as standard merge sort, in terms of \Theta-notation?_

_d. How should we choose k in practice?_

a. sort cost for each sublist in worst case = \Theta(k^2)
Overall cost = n/k * \Theta(sorting) = n/k * \Theta(k^2) = \Theta(nk)

b. There are n/k lists and we should do lg(n/k) merges. Each merge operation will take 