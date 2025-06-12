# Phase estimation applications: order-finding and factoring

The phase estimation procedure can be used to solve a variety of interesting problems.We now describe two of the most interesting of these problems: the order-finding problem, and the factoring problem.

To understand the quantum algorithms for factoring and order-finding requires a little background in number theory.

## Order finding 
For positive integers $x$ and $N$, $x<N$, with no common factors, the *order* of $x$ modulo $N$ is defined to be the least positive integer $r$, such that $x^{r} = 1 (\text{mod} \ N)$. The order-finding problem is to determine the order for some specified $x$ and $N$. 

 
!!! example 
    A quick review of order, showing that the order of $5^{r} = 1 (\text{mod} \ 21)$, $r = 6$
    $$
    5^{6} = 15625, 15625/21 = 744...1.
    $$


The quantum algorithm for order-finding is just the phase estimation algorithm applied to the unitary operator 

$$
U|y\rangle = |xy(\text{mod}\ N )\rangle,
$$

with $y \in \{0,1\}^{L}$ (this means y is a bitstring with length L). Note that when $N \leq y\leq 2^{L}-1$, we use the convention that $xy(\text{mod}\ N)$ is just $y$ again. That is, $U$ only acts non-trivially when $0\leq y \leq N-1$. This means that we only use the range $0\leq y \leq N-1$ for our phase estimation algorithm, we zero-pad or ignore when $y \geq N$ since it's redundent. 

From linear algebra, a eignestate of a cyclic shift of length $r$ are 

$$
|u_{s}\rangle \equiv \frac{1}{\sqrt{r}}\sum_{k=0}^{r-1} e^{-2\pi i sk/r} |x^{k} \text{mod} \ N\rangle,
$$

> $|u_s\rangle$ is an eigenstate of $U$, with $U|u_{s}\rangle = e^{2\pi i s/r}|u_{s}\rangle$

Applying a unitary opreator $U$,

$$
U|u_{s}\rangle = \frac{1}{\sqrt{r}}\sum_{k=0}^{r-1} e^{-2\pi i sk/r} |x^{k+1} \text{mod} \ N\rangle,
$$

set $j = k+1$, when $k=0, \ j=0$, and $k = r-1, \ j = r$. Thus,

$$
U|u_{s}\rangle = \frac{1}{\sqrt{r}}\sum_{j=1}^{r} e^{-2\pi i s(j-1)/r} |x^{j} \text{mod} \ N\rangle,
$$

factor out the phse term, 

$$
U|u_{s}\rangle = e^{2 \pi i s/r}\frac{1}{\sqrt{r}}\sum_{j=1}^{r} e^{-2\pi i sj/r} |x^{j} \text{mod} \ N\rangle,
$$

here is the key, since $x^{r} = x^{0} \text{mod}\ N$, we relabel $j$ as $k$ again. 

$$
\begin{array}{rl}
U|u_{s}\rangle &= e^{2 \pi i s/r}\frac{1}{\sqrt{r}}\sum_{k=0}^{r-1} e^{-2\pi i sk/r} |x^{k} \text{mod} \ N\rangle,\\
 & = e^{2 \pi i s/r}|u_{s}\rangle.
\end{array}
$$

Using the phase estimation procedure allows us to obtain, with high accuracy, the corresponding eigenvalues $e^{2\pi i s/r}$.

??? note "Relabel \( j \) to \( k \)? how?" 
    You may be confused about relabeling, however, let's set 

    $$
    k = j \text{mod}\ r
    $$

    when $j = 1,2,...,r$

    $$
    \begin{array}{c}
    k = 1 \text{mod} \ 1, \ k = 1\\
    k = 2 \text{mod} \ 2, \ k = 2\\
    \vdots \\
    k = 3 \text{mod} \ r, \ k = 0
    \end{array}
    $$

    thus the range of k is $0,1,...,r-1$

### Requriements 
There are two key requriements for us to be able to use phase estimation procedure:

1.  We must have efficient procedures to implement a controlled-$U^{2^{j}}$ operation for any integer $j$, and we must be able to efficiently prepare an eigenstate $|u_{s}\rangle$ with a non-trivial eigenvalue, or at least a superposition of such eigenstates. To achieve the first requriement, we can use a method called *modular exponentiation*, which we can implement the entire sequence of controll-$U^{2^{j}}$ operations used by the phase estimation.


2.  You may already notice that: there is a $r$ in the $|u_{s}\rangle$ representation already? So this is out of the question. Fortunnately, there is an observation allows us to workaround this obstacle

$$
\frac{1}{\sqrt{r}}\sum_{s=0}^{r-1}|u_{s}\rangle = |1\rangle.
$$

If we use $t = 2L + 1 + \lceil \text{log}(2 + \frac{1}{2\epsilon}) \rceil$ qubits in the first register (see [Quantum Fourier Transform](fourier_transformation.md/#qft-circuit)), and prepare the second register in the state $|1\rangle$, it follows that for each $s$ in the range $0$ to $r-1$, we will obtain an estimate of the phase $\phi \approx s/r$ accurate to $2L+1$ bits, with probability at least $(1-\epsilon)/r$.

==TODO exercise 5.13/14==

### Modular Exponentiation

How can we compute the sequence of controlled-$U^{2^{j}}$ operations used by the phase estimation procedure? We wish to compute the transformation

$$
\begin{array}{rl}
|z\rangle|y\rangle& \mapsto |z\rangle U^{z_{t}2^{t-1}}...U^{z_{1}2^{0}}|y\rangle \\
\ & = |z\rangle |x^{z_{t}2^{t-1}} \times \cdots \times x^{z_{1}2^{0}} y \text{mod} \ N\rangle \\
\ & = |z\rangle |x^{z}y(\text{mod}\ N)\rangle \\
\end{array}
$$

Thus the sequence of controlled-$U^{2^{j}}$ operations used in phase estimation is equivalent to multiplying the contents of the second register by the *modular exponentiation* $x^{z}(\text{mod}N)$, ==where $z$ is the contents of the first register.==

This algorithm for computing the modular exponential has two stages.

1.  Compate $x^{2}(\text{mod}N)$, $x^{4}(\text{mod}N)$, ..., $x^{2^{j}}(\text{mod}N)$, for all $j$ up to $t-1$. We use $t = 2L +1 + \lceil\text{log}(2+1/(2\epsilon))\rceil = O(L)$, so a total of $t-1 = O(L)$ squaring operations is performed at a cost of $O(L^{2})$ each, for a total cost of $O(L^{3})$ for the first stage.

2.  The second stage is based on the observation we have already noted,

    $$
    x^{z}(\text{mod} \ N) = \bigg (x^{z_{t}2^{t-1}}(\text{mod} \ N)\bigg )\bigg (x^{z_{t-1}2^{t-2}}(\text{mod} \ N)\bigg )...\bigg (x^{z_{1}2^{0}}(\text{mod} \ N)\bigg ).
    $$

    Performing $t-1$ modular multiplications with a cost $O(L^{2})$ each, we see that this product can be computed using $O(L^{2})$ gates.

### The continued fraction expansion

The very last step of obtaining the desire $r$ from the result of the phase estimation (PE), $\phi \approx s/r$. We only know $\phi$ to $2L+1$ buts, but we also know a *prior* that it is a rational number and if we compute the neatest such fration to $\phi$ we might obtain $r$. Suppose $s/r$ is a rational number such that 
    
$$
|\frac{s}{r} - \phi|\leq \frac{1}{2r^{2}}.
$$

Then $s/r$ is a convergent of the continued fration for $\phi$, and thus can be computed in $O(L^{3})$ operations using the continued fractions algorithm. Since $\phi$ is an approximation of $s/r$ accurate to $2L+1$ bits, it follows that $|s/r - \phi|\leq 2^{-2L-1}\leq 1/2r^{2}$, since $r \leq N \leq 2^{L}$. We obtain approximated $\phi$, which can be written into fraction $s'/r'$ without having commom factor for $s'$ and $r'$, by checking if number $r'$ in $x^{r'} (\text{mod}N)$ produce result 1. If so, then we are done!

### The continued fractions algorithm (number theory)
The idea of the continued fractions algorithm is to describe real numbers in terms of integers alone, using expressions of the form

$$
[a_{0},...,a_{M}] = a_{0} + \frac{1}{a_{1} + \frac{1}{a_{2} + \frac{1}{\cdots + \frac{1}{a_M}}}}
$$

where $a_{0},...,a_{M}$ are postiive integers. The continued fractions algorithm is a method for determining the continued fraction expansion of an arbitrary real number.

Let's say you obtained a result $\phi = 31/128$ from the phase estiamtion and you want to retrive the order $r$. We can use the continued fraction alforithm and find it's convergent. We start with inverting $31/128$ to $128/31$ and express as $a_0 +$ some fraction 

$$
\frac{128}{31} = 4+\frac{4}{31},
$$

then we can express $\frac{4}{31}$ by using continued fraction algorithm as,

$$
\frac{128}{31} = 4+\frac{1}{\frac{31}{4}} = 4+\frac{1}{7+\frac{3}{4}} = 4+\frac{1}{7+\frac{1}{\frac{4}{3}}},
$$

if we continue with this algorithm, we eventally obtain

$$
\frac{128}{31} = 4+\frac{1}{7+\frac{1}{1+\frac{1}{\frac{1}{3}}}}
$$

The algorithm terminates, since $\frac{1}{3}$ doesn't need to invert. We have 

$$
[a_{0},a_{1},a_{2},a_{3}] = [4,7,1,3]
$$

Next, we can work on the convergent based on [number theory](https://en.wikipedia.org/wiki/Number_theory). Let's start with $h_{n}/k_{n}$ where it's defined as

$$
\frac{h_n}{k_n} = 
\begin{cases}
h_{n} = a_{n}h_{n-1}+h_{n-2}\\
k_{n} = a_{n}k_{n-1}+k_{n-2}\\
\end{cases}
, n = 0,...,M
$$

where

$$
\frac{h_n}{k_n} = 
\begin{cases}
h_{-2} = 0,\quad h_{-1} = 1 \\
k_{-2} = 1,\quad k_{-1} = 0
\end{cases}
$$

after a little bit of calcualtion, we have $\frac{1}{4},\frac{7}{29},\frac{8}{33},\frac{1}{128}$ for $[a_{0},a_{1},a_{2},a_{3}] = [4,7,1,3]$. We can do a quick check that 

$$
\begin{array}{rccccl}
a_{0} & \rightarrow & \frac{s_{0}}{r_{0}} & = & \frac{1}{4} & = & 0.25\\
a_{1} & \rightarrow & \frac{s_{1}}{r_{1}} & = & \frac{7}{29} & = & 0.241237...\\
a_{2} & \rightarrow & \frac{s_{2}}{r_{2}} & = & \frac{8}{33} & = & 0.242424...\\
a_{3} & \rightarrow & \frac{s_{3}}{r_{3}} & = & \frac{31}{128} & = & 0.2412875\\
\end{array}
$$

they look fairy close to your result $\phi = 31/128$. Then we can verify each $r$ by 

$$
x^{r} = 1\ (\text{mod} \ N)
$$

then we are done! *Congratulations you just find the $r$! Easy peasy!*😃

!!! note "Cost of the continued fraction algorithm"
    If $\phi = s/r$ is a rational number, and $s$ and $r$ are $L$ bit integers, then the continued fraction expansion for $\phi$ can be computed using $O(L^3)$ opeartions - $O(L)$ 'split and invert' steps, each using $O(L^{2})$ gates or elementary arithmetic.

### Algorithm overview
==**Inputs:**==

1.  A black bos $U_{x,N}$ which performs the transformation $|j\rangle|k\rangle\mapsto |j\rangle|x^{j}k\ \text{mod} N\rangle$, for $x$ co-prime to the $L$-number $N$.
2.  Set `t = 2L + 1 + math.ceil(np.log2(2+1/epsilon))`, `t` qubits are used in the first register.
3.  $L$ qubits initialized to the state $|1\rangle$ for the second register.

==**Outputs:**== The least integer $r>0$ such that $x^{r} = 1\ (\text{mod}N)$

==**Runtime:**== $O(L^{3})$ operations.

==**Procedure:**== 

$$
\begin{array}{lll}
1. & |0\rangle|1\rangle & \text{initial state}\\
2. & \rightarrow \frac{1}{\sqrt{2}}\sum_{j=0}^{2^{t}-1}|j\rangle|1\rangle & \text{create superposition}\\
3. & \rightarrow \frac{1}{\sqrt{2^{t}}}\sum_{j=0}^{2^{t}-1}|j\rangle|x^{j} \ \text{mod} N\rangle & \text{apply} U_{x,N}\\
 & \approx \frac{1}{\sqrt{r2^{t}}}\sum_{s=0}^{r-1}\sum_{j=0}^{2^{t}-1}e^{2\pi isj/r}|j\rangle|u_{s}\rangle & \\
4. & \rightarrow \frac{1}{\sqrt{r}}\sum_{s=0}^{r-1}|s/r\rangle|u_{s}\rangle & \text{apply inverse Fourier transform to first register}\\
5. & \rightarrow s/r & \text{meausure the frist register}\\
6. & \rightarrow r & \text{apply continued fractions algorithm}
\end{array}
$$

??? note "Is \(U \) a diagonal matrix in Shor's algorithm?"
    The short answer: NO.
    A unitary operator $U$ is defined as 

    $$U \equiv
    \begin{bmatrix}
    1 & 0 \\
    0 & e^{2\pi i \theta}
    \end{bmatrix}
    $$

    is only valid in basic phase estimation examples where $U$ acts on 1 qubit and has eigenstates like $|1\rangle \rightarrow e^{2\pi i \theta}|1\rangle$.

## References 

[1]. M. A. Nielsen and I. L. Chuang, *Quantum Computation and Quantum Information*, 10th Anniversary Ed., Cambridge: Cambridge University Press, 2010.

[2]. Quantum Fourier transform (wiki) [https://en.wikipedia.org/wiki/Quantum_Fourier_transform](https://en.wikipedia.org/wiki/Quantum_Fourier_transform)

[3]. Continued fraction (wiki) [https://en.wikipedia.org/wiki/Continued_fraction](https://en.wikipedia.org/wiki/Continued_fraction)

[4]. Modular exponentiation (wiki) [https://en.wikipedia.org/wiki/Modular_exponentiation](https://en.wikipedia.org/wiki/Modular_exponentiation)

[5]. Coprime integers (wiki) [https://en.wikipedia.org/wiki/Coprime_integers](https://en.wikipedia.org/wiki/Coprime_integers)

[6]. Greatest common divisor (wiki) [https://en.wikipedia.org/wiki/Greatest_common_divisor](https://en.wikipedia.org/wiki/Greatest_common_divisor)

[7]. qiskit-community-tutorials/algorithms
/shor_algorithm.ipynb [https://github.com/qiskit-community/qiskit-community-tutorials/blob/master/algorithms/shor_algorithm.ipynb](https://github.com/qiskit-community/qiskit-community-tutorials/blob/master/algorithms/shor_algorithm.ipynb)