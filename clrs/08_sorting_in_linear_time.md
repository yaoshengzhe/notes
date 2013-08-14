## Exercises

### 8.1-1
smallest possible depth = n if array is sorted in increasing order.

### 8.2-2

        for j = A.length downto 1
                B[C[A[j]]] = A[j]
                C[A[j]] = CA[j]]-1

From above code snippet, counting sort insert them into B as the same order in A. Therefore, it is a stable sort algorithm.

### 8.2-3
counting sort still works but it is no longer stable.

##### 8.2-4
Do first two loops in counting sort and get a array C, C[i] indicates how many elements smaller than i. This could be done in $\Theta(n+k)$.

Given [a..b], the result is C[b] - C[a-1] if (a > 1) or C[b] if a == 1

### 8.3-2
stable sort: insertion sort, merge sort

For given array A, create array A' = [(1, A[1]), ... (i, A[i])]. Then apply any sort algorithm on A', the compare function on pairs (i, A[i]) and (j, A[j]) is as follows, return A[i] - A[j] if A[i] != A[j] otherwise return i - j. Then copy A' back to A. The overall complexity is $\Theta(n) + T(n)$ where T(n) is the complexity of sort algorithm.

### 8.4-3
\begin{align*}
&<(0, 0), (0, 1), (1, 0), (1, 1)> \\
E(X) &= 0 * 1/4 + 1 * 1/2 + 2 * 1/4 \\
         &= 1 \\
E^2(X) &= 1 \\
E(X^2) &= E((X_i - E(X))^2) + E^2(X) \\
           &= 1 * 1/4 + 1 * 1/4 + 1 \\
           &= 3/2
\end{align*}
## Problems
### 8-1 Probabilistic lower bounds on comparison sorting
a. Since for a n-element array, there are only n! permutations. Therefore, only n! leaves are labeled 1/n! and that the rest are labeled 0.

b. H(T, i) denotes the depth of a particular leaf i. If the leaf is in the left subtree of T, it is easy to sea H(T, i) = H(LT, i) + 1; same if the leaf is in the right subtree H(T) = H(RT, i) + 1

\begin{align*}
D(T) &= \sum_i^n H(T, i) \\
         &= \sum_i^t H(LT, i) + \sum_{i=t}^n H(RT, i) + k \\
         &= D(LT) + D(RT) + k
\end{align*}

c. Consider a decision tree T with k leaves that achieves the minimum. Let $i_0$ be the number of leaves in LT and k - $i_0$ the number of leaves in RT. Thus,

\begin{align*}
d(k) &= D(T) \\
         &= D(LT) + D(RT) + k \\
     &= d(i_0) + d(k - i_0) + k
\end{align*}

Since T minimize T, we conclude such T also minimize d($i_0$) + d(k - $i_0$). In order words, it must minimize $min_{1<=k<=k-1}{d(i)+d(k-i)}$. Therefore
\begin{align*}
d(k) &= D(T) \\
         &= d(i_0) + d(k - i_0) + k \\
         &= \min_{1<=i<=k-1} {d(i) + d(k-i) + k}
\end{align*}

d.
take divertive of expression $i\lg i + (k-i)\lg(k-i)$ for i
\begin{align*}
\lg i + 1/ln2 - \lg(k-i) - 1/ln2 = \lg i/(k-i)
\end{align*}
To let above expression equal to zero, i should equal to k/2

Assume $d(k) >= Ak\lg k + Bk$ where 1 >= A > 0, B > 0 and both A and B are constant,
\begin{align*}
d(2k) &= \min_{1<=i<=2k-1} {d(i) + d(2k-i) + 2k} \\
          &>= 2d(k) + 2k \\
          &>= A2k\lg k + 2Bk + 2k \\
          &= A2k\lg 2k + 2Bk + 2k (1 - A) \\
      &>= A2k\lg 2k + 2Bk
\end{align*}
Therefore, $d(k) = \Omega(k\lg k)$

e. The min value of $D(T_A)$ is d(n!).
Since we know $d(n!) = \Omega(n!\lg n!)$, therefore,  $D(T_A) = \Omega(n!\lg n!)$

Since there are n! possible input and total sort cost is $D(T_A)$, therefore, the average-case time to sort n element is $\Omega(n!\lg n!) / n! = \Omega(\lg n!) = \Omega(n\lg n)$

f. We introduce a sort algorithm A: since we know the where is the final result (the leaf), A always do comparisons for nodes on the root-leaf path.

By tree path definition, there is no other comparison chain shorter than it. Therefore, A always do the minimum comparisons.

### 8-2 Sorting in place in linear time
a.

        SORT(A)
                numOfZeros = 0
                for i=1 to A.length
                        if A[i].key == 0
                                numOfZeros = numOfZeros + 1
                let B be a new array
                numOfOnes = A.length - numOfZeros
                for i=A.length downto 1
                        if A[i].key == 0
                                B[numOfZeros] = A[i]
                                numOfZeros = numOfZeros - 1
                        else
                                B[numOfOnes] = A[i]
                                numOfOnes = numOfOnes - 1
                copy B to A

b.

        SORT(A)
                i = 1, j = A.length
                while i < j
                        if A[i].key == 0
                                i = i + 1
                        else if A[j].key == 1
                                j = j - 1
                        else
                                exchange A[i] and A[j]

c.

        SORT(A)
                SORT-HELPER(A, 1, A.length)

        SORT-HELPER(A, p, r)
                if p < r
                        q = STABLE-PARTITION(A, p, r)
                        SORT-HELPER(A, p, q-1)
                        SORT-HELPER(A, q+1, r)

d. Only sort algorithm in (a) works. (b) is not stable and (c) takes $O(n\lg n)$ time to sort

e.

        SORT(A)
                allocate Buf with size k
                for i=1 to k
                        Buf[i] = 0
                for i=1 to A.length
                        Buf[A[i]] += 1
                for i=2 to k
                        Buf[i] += B[i-1]
                i = 1
                while i < A.length
                        key = A[i]
                        if i != Buf[key]
                                swap(A[Buf[key]], A[i])
                                Buf[key] -= 1
                        else
                                i += 1

### 8-4 Water jugs
a. For each red jug, do the comparisons with all blue ones until find the right one. The algorithm take $\Theta(n^2)$

c. Randomly select one red jug and use it to partition n blue jugs based on comparison result. Above step will find a match for this red jug and we use that matched blue jug to partition all red jugs. After above two steps, both red jugs and blue jugs are divided into two groups and all matches only exist in corresponding group. Therefore, using above procedure, T(n) = 2T(n/2) + O(n), which implies $T(n) = O(n\lg n)$. In the worst case, we will do $O(n^2)$ comparisons.

### 8-5 Average sorting
a. it means the array is sorted.

b. 2 1 3 4

c.
\begin{align*}
\frac{\sum_j^{j+k-1} A[j]}{k} & <= \frac{\sum_{j=i+1}^{i+k} A[j]}{k} => \\
                  \sum_j^{j+k-1} A[j] & <= \sum_{j=i+1}^{i+k} A[j] => \\
                  A[i] & <= A[i+k]
\end{align*}

d. Sort sublist A[i], A[i+k], A[i+2k]... using quick sort, it takes $O(n/k \log(n/k))$. There are k such sublist, apply the same procedure for each of them. Therefore, the total complexity is $O(n\lg (n/k))$

e. build a min heap using the first element, pop the minimum one and add the next element in that element's sublist. Each heap insert and pop operation takes O(k) and we will do n times, the total complexity is $O(n\lg k)$

### 8-6 Lower bound on merging sorted lists
a. $$\binom{2n}{n}$$

c. so obvious

### 8-7 The 0-1 sorting lemma and columnsort
a. First A[q] != A[p], otherwise we break the given condition. If A[q] < A[p] then A[p] will not be the smallest value in A that algorithm X puts into the wrong location (A[q] instead, since it being put into p's location). Therefore A[q] > A[p].
