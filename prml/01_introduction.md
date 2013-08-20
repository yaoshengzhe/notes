## Exercises
### 1.1
\begin{align*}
&\sum_{j=0}^M A_{ij}w_j &=& T_i \\
&\sum_{j=0}^M \sum_{n=1}^N (x_n)^{i+j} w_j &=& \sum_{n=1}^N(x_n)^i t_n \\
&\sum_{n=1}^N \sum_{j=0}^M (x_n)^{i+j} w_j - \sum_{n=1}^N(x_n)^i t_n &=& 0 \\
&\sum_{n=1}^N (\sum_{j=0}^M (x_n)^{i+j} w_j - (x_n)^i t_n) &=& 0 \\
&\sum_{n=1}^N ( (x_n)^i \sum_{j=0}^M (x_n)^j w_j - (x_n)^i t_n) &=& 0 \\
&\sum_{n=1}^N (x_n)^i (\sum_{j=0}^M (x_n)^j w_j - t_n) &=& 0 \\
&\sum_{n=1}^N (x_n)^i (y(x_n, \mathbf{w}) - t_n) &=& 0 \\
\end{align*}

Take derivative of
$$E(\mathbf{w}) = \frac{1}{2}\sum_{n=1}^N {y(x_n, \mathbf{w}) - t_n}^2$$
in terms of $\mathbf{x}$, then we get
\begin{align*}
\frac{\partial E}{\partial \mathbf{x}} &=& \sum_{n=1}^N (y(x_n, \mathbf{w}) - t_n) \\
&=&
\end{align*}
