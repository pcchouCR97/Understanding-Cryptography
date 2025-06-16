# Factoring Shor's Algorithm

Given a positive compositve integer $N$, what prime numbers when multiplied together equal it? This *factoring problem* turns out to be equivalent to the [order-finding problem](order_finding.md). The reduction of factoring to order-finding proceeds in two basic steps. The first step is to show that we can compute a facotr of $N$ if we can find a non-trival solution $x\neq \pm 1(\text{mod}\ N)$ to the equation $x^{2} = 1 (\text{mod}\ N)$. The second step is to show that a randmly chosen $y$ co-prime to $N$ is quite likely to have an order $r$ which is evenm and such that $y^{r/2}\neq \pm 1 (\text{mod} \ N)$, and thus $x\equiv y^{r/2} (\text{mod}\ N)$ is a non-trival solution to $x^{2} = 1(\text{mod} \ N)$. 

> Theorem 1: Suppose $N$ is an $L$ bit composite number, and $x$ is a non-trival solution to the equation $x^{2} = 1(\text{mod}\ N)$ in the range $1\leq x \leq N$, that is , neither $x = 1(\text{mod} N)$ nor $x = N-1 = -1(\text{mod}\ N)$. Then at least one of gcd$(x-1,N)$ and gcd$(x+1,N)$ is a non-trival factor of $N$ that can be computed using $O(L^{3})$ operations.

---

> Theorem 2: Suppose $N = p_{1}^{\alpha_1}\cdots p_{m}^{\alpha_m}$ is the prime factorization of an odd composite positive integer. let $x$ be an integer chosen uniformaly at random, subject to the requirements that $1\leq x \leq N-1$ and $x$ is co-prime to $N$. let $r$ be the order of $x \text{mod}\ N$. Then

$$
p(r\ \text{is even and }x^{r/2} \neq -1 (\text{mod}\ N)) \geq 1 - \frac{1}{2^{m}}.
$$

### Algorithm overview
==**Inputs:**== A composite number $N$.

==**Outputs:**== A non-trival factor of $N$. 

==**Runtime:**== $O(L^{3})$ operations.

==**Procedure:**== 

1.  If $N$ es even, return the factor 2.
2.  Deteremine whether $N = a^{b}$ for integers $a\geq 1$ and $b\geq 2$, if so return the factor $a$.
3.  Randomly choose $x$ in the range $1$ to $N-1$. if $\text{gcd}(x,N)\geq 1$ then return the factor $gcd(x,N)$.
4.  Use the order-finding subroutine to find the order $r$ of $x \text{mod} \ N$.
5.  If $r$ is even and $x^{r/2} \neq -1(\text{mod}\ N)$  then compute $gcd(x^{r/2}-1,N)$ and $gcd(x^{r/2}+1,N)$, and test to see if one of these is a non-trival factor, returning that factor if so. Otherwise, the algorithm fails.




## Period Finding
Let's first describe the quantum period finding algorithm. This algorithm takes two coprime integers, $x$ and $N$, and ouputs $r$, the period of $\mathcal{F}(a) = x^{a}\ \text{mod} \ N$.

## References 

[1]. M. A. Nielsen and I. L. Chuang, *Quantum Computation and Quantum Information*, 10th Anniversary Ed., Cambridge: Cambridge University Press, 2010.

[2]. Quantum Fourier transform (wiki) [https://en.wikipedia.org/wiki/Quantum_Fourier_transform](https://en.wikipedia.org/wiki/Quantum_Fourier_transform)

[3]. Continued fraction (wiki) [https://en.wikipedia.org/wiki/Continued_fraction](https://en.wikipedia.org/wiki/Continued_fraction)

[4]. Modular exponentiation (wiki) [https://en.wikipedia.org/wiki/Modular_exponentiation](https://en.wikipedia.org/wiki/Modular_exponentiation)

[5]. Coprime integers (wiki) [https://en.wikipedia.org/wiki/Coprime_integers](https://en.wikipedia.org/wiki/Coprime_integers)

[6]. Greatest common divisor (wiki) [https://en.wikipedia.org/wiki/Greatest_common_divisor](https://en.wikipedia.org/wiki/Greatest_common_divisor)

[7]. qiskit-community-tutorials/algorithms
/shor_algorithm.ipynb [https://github.com/qiskit-community/qiskit-community-tutorials/blob/master/algorithms/shor_algorithm.ipynb](https://github.com/qiskit-community/qiskit-community-tutorials/blob/master/algorithms/shor_algorithm.ipynb)

[8]. Realization of a scalable Shor algorithm by Thomas Monz, Daniel Nigg, Esteban A. Martinez, Matthias F. Brandl, Philipp Schindler, Richard Rines, Shannon X. Wang, Isaac L. Chuang, Rainer Blatt [https://arxiv.org/abs/1507.08852](https://arxiv.org/abs/1507.08852)