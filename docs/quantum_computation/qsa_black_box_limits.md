# Black box algorithm limits 

In this section, we will discuss for a given $F$, a Boolean function, how fast (measured in number of queries) can a classical and quantum computer compte these functions given an oracle for $f$? 

You may think its difficult to answer this question without knowing something about the function $f$, but in face a great deal can be determined even in thie "black box" model, where the mean by which the oracle accomplished its task is taken for granted, and complexity is measured only in terms of the number of required oracle queries.

## Method of polynomials

Let's start with some useful definitions:

*   **$D(F)$**: Deterministic query complexity: Minimum number of oracle calls a classical algorithm needs to compute $F$ with certainty.

*   **$Q_E(F)$**: Exact quantum query complexity: Minimum number of oracle calls a quantum algorithm needs to compute $F$ with 100% accuracy.

*   **$Q_2(F)$**: Bounded-error quantum query complexity: Minimum number of queries for a quantum algorithm to get the correct result with probability $\geq \frac{2}{3}$, where $\frac{2}{3}$ is an arbitrary number, the probability need only be bounded finitely away from $\frac{1}{2}$ to be boosted close to $1$ by repetitions.

*   **$Q_0(F)$**: Zero-error quantum query complexity: The output is either correct or "I don't know" (inconclusive), but never wrong.

These complexities satisfy the relation:

$$
Q_2(F) \leq Q_0(F) \leq Q_E(F) \leq D(F) \leq N
$$

We can represent any Boolean function $F(X)$ using a real-valued multilinear polynomial $p(X)$, where each input variable $X_k \in \{0,1\}$.

The polynomial is defined as:

$$
p(X) = \sum_{Y \in \{0,1\}^N} F(Y) \prod_{k=0}^{N-1} \left[1 - (Y_k - X_k)^2 \right]
$$

-   Each $X_k$ is binary, so $X_k^2 = X_k$. This ensures the polynomial is multilinear.
-   For a given $Y$, the product $\prod_{k}(1 - (Y_k - X_k)^2)$ evaluates to 1 **only if** $X = Y$, and 0 otherwise.
-   This construction guarantees that $p(X) = F(X)$ for all $X \in \{0,1\}^N$.

The minimum degree of such a representation for $F$, denoted as $\text{deg}(F)$, measures a complexity of $F$. Giving the fact that the degree of most function is of order $N$. 

$$
D(F) \leq 2\text{ deg}(F)^{4}.
$$

This results places an upper bound on the performance of deterministic classical computation in calculating most Boolean functions.

If a polynomial satisfies $|p(X) = F(X)| \leq 1/3$ for all $X \in \{0,1\}^{N}$, we say *p approximate* $F$, and $\text{deg}(F)$ denotes the minimum degree of such an approximating polynomial. It is known that 

1.  $\text{deg(OR)} \in \Theta(\sqrt{N})$
2.  $\text{deg(AND)} \in \Theta(\sqrt{N})$
3.  $D(F) \leq 216 \text{ deg}(F)^{6}$


## References 

[1]. M. A. Nielsen and I. L. Chuang, *Quantum Computation and Quantum Information*, 10th Anniversary Ed., Cambridge: Cambridge University Press, 2010.