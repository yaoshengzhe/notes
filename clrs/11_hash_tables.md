## Exercises
### 11.1-1
do linear search on table T. Total complexity = O(m) 

### 11.1-2
first set all bits to 0. to add an element and then set the x.key % m bit to 1.

### 11.1-3

INSERT: if T[x.key] is occupied, check the next element until it's empty

SEARCH: check T[x.key], if not what we are looking for then move to the next. stop search search until we reach an element whose key != x.key or an empty slot.

DELETE: start from T[x.key], for each element y, check if y.key == x.key and y.val == x.val. if it's true then set that index T[indexOf(y)] = Nil. stop when y.key != x.key or y is Nil.

### 11.2-1
\begin{align*}
E(C)& = 1/n * \sum_{i=1}^n (i-1) / m \\
	& = 1/n^2* \sum_{i=0}^{n-1} i \\
	& = 1/2 * (n-1)*n/m^2 \\
	& = 1/2 * \alpha * n-1/m \\
	& = 1/2 * \alpha (\alpha - 1/m) \\
	& = 1/2 * \alpha^2 - 1/2 * \alpha/m)
\end{align*}

### 11.2-3
* Successful searches: improve worst case time to $\Theta(\log \alpha)$
* Unsuccessful searches: improve worst case time to $\Theta(\log \alpha)$
* Insertions: average and worst case run becomes $\Theta(\alpha)$
* Deletions: average and worst case run becomes $\Theta(\log \alpha)$

### 11.2-5
|U| > nm, thus the statement is true.

### 11.3-1
First hash the given key, during list searing, only compare two keys if their hashes are equal.

### 11.3-4
700, 318, 936, 554, 172