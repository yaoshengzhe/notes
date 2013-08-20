## Exercises
### 1.1
\newcommand{\w}{\mathbf{w}}
\newcommand{\sqrterr}{\sum_{n=1}^N(y(x_n, \w) - t_n)}
\newcommand{\half}{\frac{1}{2}}
\newcommand{\dydw}{\frac{\partial y(x_n, \w)}{\partial \w}}
\newcommand{\dydwi}{\frac{\partial y(x_n, \w)}{\partial w_i}}
\newcommand{\Aij}{\sum_{n=1}^N (x_n)^{i+j}}
\newcommand{\Ti}{\sum_{n=1}^N(x_n)^i t_n}

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
$$E(\mathbf{w}) = \frac{1}{2}\sum_{n=1}^N (y(x_n, \mathbf{w}) - t_n)^2$$
in terms of $w_i$, then we get
\begin{align*}
\frac{\partial E}{\partial w_i} &= \sum_{n=1}^N (y(x_n, \mathbf{w}) - t_n)\frac{\partial y(x_n, \w)}{\partial w_i} \\
&= \sum_{n=1}^N (y(x_n, \mathbf{w}) - t_n) (x_n)^i
\end{align*}

In order to minimize $E(\w)$, we should let
$$\frac{\partial E}{\partial w_i} = 0$$

Therefore, the statement is true.

### 1.2
Take derivative of
$$\widetilde{E}(\w) = \half\sqrterr + \frac{\lambda}{2} \lVert \w \rVert^2$$
in terms of $w_i$, then we get
\begin{align*}
&\frac{\partial \widetilde{E}(\w)}{w_i} &=& \sqrterr^2\dydwi + \lambda w_i \\
&\frac{\partial \widetilde{E}(\w)}{w_i} &=& \sqrterr(x_n)^i + \lambda w_i \\
\end{align*}

In order to minimize $E(\w)$, we should let
$$\frac{\partial \widetilde{E}(\w)}{w_i} = 0$$
thus
\begin{align*}
& \sqrterr(x_n)^i + \lambda w_i  &=& 0 \\
&\sum_{j=0}^M A_{ij}w_j + \lambda w_i &=& T_i \\
\end{align*}
where $$A_{ij} = \Aij$$ and $$T_i = \Ti$$

### 1.3
