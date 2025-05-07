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

We use thee quantum computer to approximate $\mathbb{P}[|1\rangle] = \text{sin}^{2}((2k+1)\theta_{a})$ for the last qubit in $\mathcal{Q}^{k} \mathcal{A}|0\rangle_{n}|0\rangle$ for different power $k$.

Suppose a confidence interval $[\theta_{i},\theta_{u}] \subseteq [0, \pi/2]$ for $\theta_{a}$ and a power $k$ of $\mathcal{Q}$ as well as an esitmate for $\text{sin}^{2}((2k+1)\theta_{a})$. From trigonometric, we transform our estimate from $\text{sin}^{2}((2k+1)\theta_{a})$ into estimate for $\text{cos}^{2}((4k+2)\theta_{a})$ from the fact $\text{sin}^{2} = (1-\text{cos}(2x))/2$.

!!! Note "One-to-one"
    Here, we are trying to estimate $\theta_{a}$, where:
    $$
    a = \text{sin}^{2}(\theta_{a}) \Rightarrow \text{cos}((4k+2)\theta_{2a})
    $$
    But $\text{cos}$ is not one-to-ont over $[0,2\pi]$. To uniquely determine $\theta_{a}$ from $\text{cos}((4k+2)\theta_{a})$, we must ensure the possible value of $((4k+2)\theta_{a}) {\text{mod} \ 2\pi}$ fall entirely within either $[0,\pi]$ or $[\pi, 2\pi]$

The one-to-one facts allows us to invert the cosine. If $((4k+2)\theta_{a})_{\text{mod} \ 2\pi}$ falls into $[0,\pi]$, cosine is decreasing, we said that inverse is unique, vice versa. An Iterative Amplitude Estimation picks a $k$ such that 

$$
((4k+2)[\theta_{l},\theta_{u})] \ {\text{mod} \ 2\pi} \subset [0,\pi] \ \text{or} \ [\pi, 2\pi]
$$

IAE: 
-   Applying controlled Grover operations $Q^{k}$.
-   Measuring $\text{cos}(K_{i}\theta)$
-   Inverting to recover $\theta$
-   Narrowing your estimate iteratively

