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