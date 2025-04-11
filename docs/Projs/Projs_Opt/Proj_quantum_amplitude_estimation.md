# Quantum Amplitude Estimiation

Suppose an unitary opeartor $\mathcal{A}$ acting on a register of $(n+1)$ qubits such that 
$$
\mathcal{A}|0 \rangle_{n+1} = \sqrt{1-a} |\psi_{0} \rangle_{n} |0\rangle + \sqrt{a}|\psi_{1}\rangle_{n}|1\rangle
$$ 
for some normalized states $|\psi_{0}\rangle_{n}$ and $|\psi_{1}\rangle_{n}$, where $a\in [0,1]$ is unknown. 

Amplitude estimation allows the efficient estimation of $a$, i.e., the probability of measuring $|1\rangle$ in the last qubit.[[1].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) This is done by using an opeartor $Q$, and Quantum Phase Estimation [[2].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) to approximate certain eigenvalues of $Q$. 

### Step 1.
This Quantum Phase Estimation requies $m$ addiational qubits and $M = 2^{m}$ applications of $Q$. The $m$ qubits are first put into equal superposition by applying Hadamard gates. Then these superposition $m$ qubits are used to control different powers of $Q$.

### Step 2.
After applying an inverse Quantum Fourier Transform on these $m$ qubits, their state is measured. The results in an integer $y \in \{0,\cdots,M-1 \}$, which is classically mapped to the estimator $\widetilde{a}$. The estimator $\widetilde{a}$ satisfies

$$
\begin{array}{cc}
|a - \widetilde{a}|& \leq \frac{2\sqrt{a(1-a)\pi}}{M} + \frac{\pi^{2}}{M^{2}} \\
& \frac{\pi}{M} | \frac{\pi^{2}}{M^{2}} = O(M^{-1}),
\end{array}
$$

with probability of at least $\frac{8}{\pi^2} \approx 0.811$.

### Estiamte the expected value of a random variable

Here, we will go through how to use amplitude estimation of approximate the expected value of a random variable. Suppose we have a quantum state 

$$
|\psi\rangle_{n} = \sum_{i = 0}^{N-1} \sqrt{p_{i}}|i\rangle_{n},
$$

where the probability of measuring the state $|i\rangle_{n}$ is $p_{i} \in [0,1]$, with $\sum_{i=0}^{N-1} p_{i} =1$ and $N = 2^{m}$. The state $|i\rangle_{n}$ is one of the $N$ possible realizations of a bounded discrete random variable $X$. For instance, $|i\rangle$ can represent a discretized interest rate or the value of a portfolio.

### Step 3

Considering a function $f: \{0,\cdots N-1\} \rightarrow [0,1]$ and a corresponding operator 
$$
F: |i\rangle_{n}|0\rangle \mapsto |i\rangle_{n} \bigg(\sqrt{1-f(i)}|0\rangle + \sqrt{f(i)}|1\rangle\bigg),
$$
for all $i \in {0,\cdots,N-1}$, acting on an ancilla qubit. Applying $F$ to $|\psi\rangle |0\rangle$ leads to the state
$$
\sum_{i=1}^{N-1}|i\rangle_{n}\bigg(\sqrt{1-f(i)}\sqrt{p_i}|0\rangle + \sqrt{f(i)}\sqrt{p_i}|1\rangle\bigg)
$$
Now we can use amplitude estimation to approximate the probability of measuring $|1\rangle$ in the last qubit, which equals $\sum_{i=0}^{N-1}p_{i}f(i) = \mathbb{E}[f(X)]$. Choosing $f(i) = i/(N-1)$ allows us to estimate $\mathbb{E}[\frac{X}{N-1}]$ and hence $\mathbb{E}[X]$. If we choose $f(i) = i^{2}/(N-1)^{2}$ we can efficiently estimate $\mathbb{E}[X^{2}]$ which yields the variance $\text{Var}(X) = \mathbb{E}[X^{2}] - \mathbb{E}[X]^{2}$.

### How to evaluate risk measures such as VaR and CVaR
For a given confidence level $\alpha \in [0,1]$. $\text{VaR}_{\alpha}(X)$ can be defined as the smallest value $x \in \{0,\cdots,N-1\}$ such that $\mathbb{P}[X \leq x]\leq (1-\alpha)$. 


## Reference
1. Quantum Amplitude Amplification and Estimation, G. Brassard, P. Hoyer, M. Mosca, and A. Tapp,
arXiv:0005055 (2000). [https://arxiv.org/abs/quant-ph/0005055](https://arxiv.org/abs/quant-ph/0005055)

2. Quantum measurements and the Abelian Stabilizer Problem, A. Y. Kitaev (1995), [https://arxiv.org/abs/quant-ph/9511026](https://arxiv.org/abs/quant-ph/9511026)

