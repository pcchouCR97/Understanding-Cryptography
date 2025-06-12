# Period-finding 


Suppose $f$ is a periodic function producing a single bit as output and such that $f(x+r) = f(x)$, for some unknown $0<r<2^{L}$, where $x,r \in \{0,1,2,...\}$. Given a quantum black box $U$ which performs the unitary transform $U|x\rangle|y\rangle \mapsto |x\rangle|y\oplus f(x)\rangle$ (where $\oplus$ denotes addition modulo $2$) how many black box queries and other operations are required to determin $r$? 

In practice U operates on a finite domain, whose size is deteremined by the desired accuracy $r$. 

### Algorithm overview (*one query*)
==**Inputs:**== 

1.  A black box which performs the operation $U|x\rangle|y\rangle = |x\rangle|y\oplus f(x)\rangle$.
2.  A state to store the function evaluation, initialized to $|0\rangle$
3.  $t=O(L+\text{log}(1/\epsilon))$ qubits initialized to $|0\rangle$.

==**Outputs:**== The least integer $r>0$ such that $f(x+r) = f(x)$

==**Runtime:**== one use of $u$, and $O(L^{3})$ operations. Succeeds with probablility $O(1)$

==**Procedure:**== 

$$
\begin{array}{lll}
1. & |0\rangle|0\rangle & \text{initial state}\\
2. & \rightarrow \frac{1}{\sqrt{2}}\sum_{x=0}^{2^{t}-1}|x\rangle|0\rangle & \text{create superposition}\\
3. & \rightarrow \frac{1}{\sqrt{2^{t}}}\sum_{x=0}^{2^{t}-1}|x\rangle|f(x)\rangle \ & \text{apply} U\\
 & \approx \frac{1}{\sqrt{r2^{t}}}\sum_{l=0}^{r-1}\sum_{x=0}^{2^{t}-1}e^{2\pi ilj/r}|x\rangle|\widehat{f}(l)\rangle & \\
4. & \rightarrow \frac{1}{\sqrt{r}}\sum_{l=0}^{r-1}|l/r\rangle|\widehat{f}(l)\rangle & \text{apply inverse Fourier transform to first register}\\
5. & \rightarrow s/r & \text{meausure the frist register}\\
6. & \rightarrow r & \text{apply continued fractions algorithm}
\end{array}
$$

The key to understanding this algorithm, which is based on phase estimation, and its basically a order-finding problem. In step 3, we introduce the state 

$$
|\widehat{f}(l)\rangle\equiv \frac{1}{\sqrt{r}}\sum_{x=0}^{r-1}e^{-2\pi ilx/r}|f(x)\rangle
$$

the Fourier transform of $|f(x)\rangle$. The identity used in step 3 is based on 

$$
|f(x)\rangle = \frac{1}{\sqrt{r}}\sum_{k=0}^{r-1}e^{2\pi ilx/r}|\widehat{f}(l)\rangle
$$

which is a easy to verfify noting that $\sum_{k=0}^{r-1}e^{2\pi ilx/r} = r$ for integer $x$ an integer multiple of $r$, and zero otherwise. The approximate equality in step $3$ is required since $2^t$ may not be an integer multiple of $r$ in general. After applying the inverse Fourier transform at the step 4 we can get $l/r$, where $l$ is chosed randomly. $r$ can be calcualted by using a continued fraction expansion.

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