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