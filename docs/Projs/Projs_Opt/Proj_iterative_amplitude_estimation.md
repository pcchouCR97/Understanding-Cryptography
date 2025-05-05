# Iterative Amlitude Estimation


## Quantum Amplitude Estimation
An Operator $\mathcal{Q}$ is defined as 

$$
\mathcal{Q} = \mathcal{A}\mathcal{S}_{0}\mathcal{A}^{\dagger}\mathcal{S}_{\psi_{0}}
$$

where $\mathcal{S}_{\psi_{0}} = \mathbb{I} - 2 |\psi_{0}\rangle \langle\psi_{0}|\otimes|0\rangle\langle0|$ and $\mathcal{S}_{0} = \mathbb{I} - 2|0\rangle_{n+1}\langle0|_{n+1}$.

The cononical QAE follows the form of QPE: it usese $m$ ancilla qubits - initialized in equal superposition - to represent the final result, it defines the number of quantum samples as $M = 2^{m}$ and applies geometrically increasing powers of $\mathcal{Q}$ controlled by the ancillas. Followed by performing the QFT before they are measured. Then the measured integer $y\in \{0,\cdots,M-1 \}$ is mapped to an angle $\tilde{\theta_{a}} = y\pi/M$. Thereafter, the resulting estimate of $a$ is defined as $\tilde{a} = \text{sin}^{2}(\tilde{\theta_{a}})$. The with a probability of at least $8/\pi^{2} \approx 81\%$, the estimate $\tilde{a}$ satisfies

$$
|a - \tilde{a}| \leq \frac{2\pi\sqrt{a(1-a)}}{M}+\frac{\pi^{2}}{M^{2}},
$$

which implies the quadratic speedup over a classical MC simulation with an estimation error $\epsilon = \mathcal{O}(1/M)$.

All variants of QAE without QPE are based on the fact that 

$$
\mathcal{Q}^{k} \mathcal{A}|0\rangle_{n}|0\rangle = \text{cos}((2k+1)\theta_{a})|\psi_{0}\rangle_{n}|0\rangle + \text{sin}((2k+1)\theta_{a})|\psi_{1}\rangle_{n}|1\rangle,
$$

where $\theta_{a}$ is defiend as $a = \text{sin}^{2}(\theta_{a})$. The probability of measuring $|1\rangle$ in the last qubit is given by

$$
\mathbb{P}[|1\rangle] = \text{sin}^{2}((2k+1)\theta_{a}).
$$

## Iterative Quantum Amplitude Estimation