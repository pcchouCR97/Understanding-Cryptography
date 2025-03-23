# Portfolio Optimization

The portfolio Optimization is a problem looks into identify the optimal number of shares of each stock to purchase in order to minimize risk(variance) and maximize returns, while staying under some specified spending budget.

## Problem Definition

Consider a set of $n$ types of stockds to choose from, with an average monthly return per dollar spent of $r_i$ for wach stock $i$. Let $\sigma_{i,j}$ be the covariance of the returns of stocks $i$ and $j$. For a spending budget of $B$ dollars, let $x_i$ denote the number of shares of stock $i$ purchased at price $p_i$ per share. 

The problem and be formaulated as 

$$
\begin{array}{ll}
\text{minimize or maximize} & \text{objective function}\\
\text{subject to} & \text{constraint(s)}
\end{array}
$$

For example, if we choose $n=3$, our decision vriables must satisfied 

$$
\sum_{i=1}^{n=3} x_{i} \leq \text{budget},\ x_{i}\geq 0.
$$

Let's follow saome assumptions below:
1.  Fraction share is NOT acceptable.
2.  No short-selling is allowed.
3.  No transcation costs.

We denote an average monthly return per dollar spent of $r_i$ for wach stock $i$, then the return (profit) on $x_i$ dollars invested in stock $i$ is $r_{i}x_{i}$ and the total (random) return on our investment is $\sum_{i=1}^{3}r_{i}x_{i}$. 

The expected return on our investment is then $\mathbb{E}[\sum_{i=1}^{3} r_{i}x_{i}] = \sum_{i=1}^{3}\overline{r_{i}}x_{i}$, where $\overline{r_{i}}$ is the expected value of the $r_{i}$. Since we want to have an expected return of at least $ $50.00$, $x_{i}'s$ must be such that:

$$
\sum_{i=1}^{3}\overline{r_{i}}x_{i} \geq 50.00.
$$

Now, lets try to minimize the risk. The risk can be closely approximated by minimizing the variance of the return of the investment portfolio. The variance is given by:

$$
\begin{array}{ll}
\text{Var}[\sum_{i=1}^{3}r_{i}x_{i}] & = \mathbb{E}\bigg[\bigg(\sum_{i=1}^{3}r_{i}x_{i} - \sum_{i=1}^{3}\overline{r_{i}}x_{i}  \bigg)\bigg]\\
& = \mathbb{E}\bigg[\bigg(\sum_{i=1}^{3}(r_{i} - \overline{r_{i}})x_{i}\bigg)\bigg(\sum_{j=1}^{3}(r_{j} - \overline{r_{j}})x_{j}\bigg)\bigg]\\
& = \sum_{i=1}^{3}\sum_{j=1}^{3}x_{i}x_{j}\mathbb{E}[(r_{i} - \overline{r_{i}})(r_{j} - \overline{r_{j}})] \\ 
& = \sum_{i=1}^{3}\sum_{j=1}^{3}x_{i}x_{j}\sigma_{ij},
\end{array}
$$

where $\sigma_{ij}$ is the covariance of the return of stock $i$ with stock $j$. Therefore, our problem can be defined, in a general form, as:

$$
\begin{array}{ll}
\text{min} & \sum_{i}^{I}\sum_{j}^{J} x_{i}x_{j}\sigma_{ij}, \\
\text{subject to} & \sum_{i}^{I} x_{i} \leq B, \\
& \sum_{i}^{I} x_{i} \geq R, \\
& x_{i} \geq 0, \forall i. 
\end{array}
$$

where $B$ is the total budget, $R$ is the expected return after a time period.

We can also write our formula into a matrices and vectors,

$$

$$

where $x$ is the decision vector of size $n$ ($n$ is the number of stocks), $e$ is an $n$-vector of ones, $\overline{r}$ is the $n$-vector of expected returneds of the stocks, and $Q$ is the $n\times n$ covariance matrix (whose $i-j$th element $Q_{ij} = \sigma{ij}$)


## Constructing Hamiltonian 
From the problem definination, we can construct our Hamiltonian by introducing penalty terms from our constraints listed above.

$$
H = \sum_{i}\sum_{j}x_{i}x_{j}\sigma_{ij} + \lambda_{1}\bigg(\sum_{i}x_{i}-B\bigg)^{2} + \lambda_{2}\bigg(R-\sum_{i}\overline{r_i}x_{i} \bigg)^{2}
$$

where $\lambda_{1}$ and $\lambda_{2}$ are penalty coefficients.