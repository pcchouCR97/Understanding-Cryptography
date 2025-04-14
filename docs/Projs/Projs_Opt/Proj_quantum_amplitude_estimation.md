# Quantum Amplitude Estimiation

Suppose an unitary opeartor $\mathcal{A}$ acting on a register of $(n+1)$ qubits such that 
$$
\mathcal{A}|0 \rangle_{n+1} = \sqrt{1-a} |\psi_{0} \rangle_{n} |0\rangle + \sqrt{a}|\psi_{1}\rangle_{n}|1\rangle
$$ 
for some normalized states $|\psi_{0}\rangle_{n}$ and $|\psi_{1}\rangle_{n}$, where $a\in [0,1]$ is unknown. 

Amplitude estimation allows the efficient estimation of $a$, i.e., the probability of measuring $|1\rangle$ in the last qubit.[[2].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) This is done by using an opeartor $Q$, and Quantum Phase Estimation [[3]](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) to approximate certain eigenvalues of $Q$. 

<div style="text-align: center;">
    <img src="../../Projs_Opt/images/Qcurcuit_ae.png" alt="Quantum circuit for amplitude estimation" style="width: 550px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Quantum circuit for amplitude estimation. \(H\) and is the Hadamard gate and \(F^{\dagger}_{m}\) denotes the inverse Quantum Fourier Transform on m qubits.
    </p>
</div>

??? note "How to read this quantum circuit??"
    There are 3 sections in this quantum circuit. The first one is the top $m$ qubits (in figure, these are 0 to j to $m-1$). These are the counting qubits [Quantum counting algorithm(wiki)](https://en.wikipedia.org/wiki/Quantum_counting_algorithm) and [Grover's algorithm(wiki)](https://en.wikipedia.org/wiki/Grover%27s_algorithm). Each starts in $|0\rangle$ and gets a Hadamard gate. After the Hadamard gate, these $m$ qubits are in equal supersposition $\frac{1}{\sqrt{2^{m}}}\sum_{k=0}^{2^{m}-1}|k\rangle$. Second is the middle $n$ qubits (its the $|0\rangle_{n}$ in the above image). This is the state register that will store $|i\rangle$, drawn from a distribution $p_{i}$. And the last bottom 1 qubit is the ancilla qubit. This is where $f(i) \in [0,1]$ is encoded as a rotation amplitude.

??? note "What is operator $A$?"
    Operator $A$ acts on the bottom $n+1$ wires, $A|0\rangle^{\otimes (n+1)} \mapsto \sum_{i}\sqrt{p_i}|i\rangle_{n}(\sqrt{1-f(i)}|0\rangle + \sqrt{f(i)}|1\rangle)$, **this prepares your quantum probability distirbution and encodes your function $f{i}$** into the ancilla amplitude. Here, "acts on the bottom $n+1$ wires" mean that operator $A$ acts simultaneously on $n$ qubits (which store $|i\rangle$) and the 1 ancilla qubit (used to encode $f(i)$).

??? note "What is operator $Q$?"
    An Operator $Q$ is a Grover opeartor. Each counting qubit (top wire) controls powers of the Q, acting on the bottom $n+1$ register. The notation $Q^{2^{0}}$ means controlled by qubit 0, $Q^{2^{j}}$ means by controll qubit $j$, and $Q^{2^{m-1}}$ means its controlled by qubit $m-1$.

??? note "Inverse QFT $F^{\dagger}_{m}$?"
    The inverse quantum fourier transform applies to the counting resiger, which decodes the eigenphase that encodes the amplitude $a =\mathbb{E}[f(X)]$.

??? note "What is a "register" qubits?"
    In the circuit, $n$ is the number of **state qubits**, which encoded input distribution $|\psi\rangle$, 1 is the **ancilla qubit** which will be used to encode $f(i)$, rotate to encode $\sqrt{f(i)}|1\rangle$. The operator $A$ acts on a **combined register** of
    $$
    |0\rangle_{n}\otimes |0\rangle
    $$
    which is a **total of $n+1$ qubits** such that $n$ is for preparing $\sum\sqrt{p_{i}}|i\rangle$ and 1 is for loading amplitude via fcuntion $f(i)$.


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

where the probability of measuring the state $|i\rangle_{n}$ is $p_{i} \in [0,1]$, with $\sum_{i=0}^{N-1} p_{i} =1$ and $N = 2^{m}$. The state $|i\rangle_{n}$ is one of the $N$ possible realizations of a bounded discrete random variable $X$. For instance, $|i\rangle$ can represent a discretized interest rate or the value of a portfolio. For example, if $n=2$, the range of $i$ is $i \in \{0,1,2,3\}$, where we may say the interest rate of 0, 1, 2, or 3%.

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

??? note "Why does measuing $|1\rangle$ in the ancilla gives $\mathbb{E}[f(X)]$?"
    After we applying for the opeartion $A$, our quantum state becomes
    $$
    |\psi\rangle = \sum_{i=1}^{N-1}|i\rangle_{n}\bigg(\sqrt{1-f(i)}\sqrt{p_i}|0\rangle + \sqrt{f(i)}\sqrt{p_i}|1\rangle\bigg)
    $$
    the **total amplitude on $|1\rangle$** in the ancilla qubit is 
    $$
    \sum_{i=0}^{N-1} \sqrt{p_i} \cdot \sqrt{f(i)}|i\rangle_{n}|1\rangle
    $$
    squaring this gives the **total probability of measuring** $|1\rangle$ in the ancilla
    $$
    \text{Pr}[\text{ancilla} = |1\rangle] = \sum_{i=0}^{N-1}p_{i}f(i) = \mathbb{E}[f(X)].
    $$
    The final probaliity of seeing $|1\rangle = $ average of $f(i)$ weighted by $p_i$.

### How to evaluate risk measures such as VaR and CVaR
For a given confidence level $\alpha \in [0,1]$. $\text{VaR}_{\alpha}(X)$ (X is a random variable, can be loss or value) can be defined as the smallest value $x \in \{0,\cdots,N-1\}$ such that $\mathbb{P}[X \leq x]\geq (1-\alpha)$, which simply implies that with $1-\alpha$ probability, losses will not exceed $x$. To find $\text{VaR}_{\alpha}(X)$ on a quantum computer, we define the function 

$$
f_l(i) = 
\begin{cases}
1 & \text{if } i \leq l \\
0 & \text{otherwise}
\end{cases}
$$

where $l \in \{0,\cdots,N-1\}$. Applying $F_l$, i.e. the operator corresponding to $fl$. to $|\psi\rangle_{n}|0\rangle$ leads to the state
$$
\sum_{i = l+1}^{N-1}\sqrt{p_i}|i\rangle_{n}|0\rangle+ \sum_{i = 0}^{l}\sqrt{p_i}|i\rangle_{n}|1\rangle.
$$
The probability of measuring $|1\rangle$ for the last qubit is 
$$
\sum_{i=0}^{l}p_{i} = \mathbb{P}[X \leq l].
$$
Then use the binary search on $l \in \{0,\cdots,N-1 \}$ in at most $n$ steps to find the smallest $l_{\alpha}$ such that:
$$
\mathbb{P}[X \leq l_{\alpha}] \geq 1 - \alpha
$$
This gives you the $\text{VaR}_{\alpha}(X)$. This allows us to estimate $\text{VaR}_{\alpha}(X)$ as before with accuracy $O(M^{-1})$.

### Conditional Expectation of $X$

$\text{CVaR}_{alpha}(X)$ is the conditional expectation of $X$ restricted to $\{0,\cdots,l_{\alpha}\}$, where we compute $l_{\alpha} = \text{VaR}_{\alpha}(X)$ as before. To estimate CVaR we apply the operator $F$ that corresponds to the function 
$$
f(i) = \frac{i}{l_{\alpha}} \cdot f_{l_{\alpha}}(i)
$$
to $|\psi\rangle|0\rangle$, which leads to the state
$$
\bigg(\sum_{i = l_{\alpha}+1}^{N-1}\sqrt{p_i}|i\rangle_{n}|0\rangle+ \sum_{i = 0}^{l}\sqrt{1 - \frac{i}{l_{\alpha}}}\sqrt{p_i}|i\rangle_{n}|1\rangle\bigg)|0\rangle + \sum_{i=0}^{l_{\alpha}}\sqrt{\frac{i}{l_{\alpha}}}\sqrt{p_i}|i\rangle_{n}|1\rangle.
$$
The probability of measuring $|1\rangle$ for the last qubit equals 
$$
\sum_{i=0}^{l_{\alpha}}\frac{i}{l_{\alpha}}p_{i},
$$
which we approcimate using amplitude estimation. However, we know that $\sum_{i=0}^{l_{\alpha}}p_{i} \neq 1$ but $\mathbb{P}[X\leq l_{\alpha}]$ as evaluated during the VaR estimation. Therefore, we mst normalize the probability of measuring $|1\rangle$ to get
$$
CVaR_{\alpha}(X) = \frac{l_{\alpha}}{\mathbb{P}[X \leq l_{\alpha}]}\sum_{i=0}^{l_{\alpha}}\frac{i}{l_{\alpha}}p_{i}.
$$
We also multiplied by $l_{\alpha}$, otherwise we would estimate $\text{CVaR}_{\alpha}(\frac{X}{l_{\alpha}})$. Even though we replace $\mathbb{P}[X \leq l_{\alpha}]$ by an estimation, the error on CVaR, shows that we still achieve a quadratic speed up compated to classical Monte Carlo methods.


## Construction of Quantum Circuit

Approximating $\mathbb{E}$ using amplitude estiamtion requires the operator $F$ for $f(x) = x/(N-1)$. In general, representing $F$ for the expected value or for the CVaR either additional ancillas to pre-compute the (discretized) function $f$ into qubits, using quantum arithmetic, before applying the rotation (ref)/ And the exact number of ancillas depends on the desired accuracy of the approximation of $F$. 

In the following, paper [[1].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) will show how to approximate $F$ without ancillas using polynomially many gates, at the cost of a lower - but still faster than classical - rate of convergence. Note that the operator required for estimating VaR is easier to construct and we can always achieve the optimal rate of convergence as discussed later.

### Polynomial Approximate Quantum Circuit without Ancillas

the contribution in paper paper [[1].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) rests on the fact that an operator 
$$
P: |x\rangle_{n}|0\rangle \mapsto |x\rangle_{n}(\text{cos}(p(x))|0\rangle + \text{sin}(p(x))|1\rangle)
$$
for a given polynomial $p(x) = \sum_{j=0}^{k}p_{j}x^{j}$ of order $k$, can be efficiently constructed using multi-controlled Y-rotations.

(image)
*Here, we use qiskit for illustration, since Qiskit doesnt have native Ry-controlled gate with 2+ controlls, we use $\text{MCX}(q_{0},q_{1}) \mapsto R_y(4a) \mapsto \text{MCX}(q_{0},q_{1})$. See [Qiskit Quantum library](https://docs.quantum.ibm.com/api/qiskit/circuit_library), for more infomation.

In the single qubit operations with $n-1$ control qubits, it can be exactly constructed using $O(n)$ gates and $O(n)$ ancillas or $O(n^{2})$ gates without ancilla. Here, peper [[1].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) uses $O(n)$ gates and $O(n)$ ancillas. Since the binary variable representation of $p$, leads to at most $n^{k}$ terms, the operator $P$ can be constructed using $O(n^{k+1})$ gates and $O(n)$ ancillas.

For every analytic function f, there exists a sequence of polynomials such that the approximation error converges exponentially fast to zero with increasing order of the polynomials [[4].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference). 


If we can find a polynomial $p(y)$ wuch that $\text{sin}^{2}(p(y)) = y$, then we can set $y=f(x)$, and the previous discussion provides a way to construct the operator $F$. Since [the expected value is linear](../../Math_Fundamentals/probability_theory.md#linearity-of-expectation), we may choose to estimate $\mathbb{E}[c(f(X) - \frac{1}{2}) + \frac{1}{2}]$ instead of $\mathbb{E}[f(X)]$ for a parameter $c\in(0,1]$, and then map the result back to an estimatior for $\mathbb{E}[f(X)]$. The rationale behind this choice is that $\text{sin}^{2}(y+\frac{\pi}{4}) = y+\frac{1}{2} + O(y^3)$.

??? note "What is $\mathbb{E}[c(f(X) - \frac{1}{2}) + \frac{1}{2}]$ and Why?"
    By the linearity of expected value, we may define a function $g(X)$ such that
    $$
    g(X) := c(f(X) - \frac{1}{2}) + \frac{1}{2} \Rightarrow \mathbb{E}[g(X)] = c\mathbb{E}[f(X)] + (\frac{1}{2}+\frac{c}{2})
    $$
    therefore, you can estiamte $\mathbb{E}[g(X)]$ (via amplitude estimation), you can solve for 
    $$
    \mathbb{E}[f(X)] = \frac{1}{c}(\mathbb{E}[g(X)]-\frac{1}{2})+\frac{1}{2}.
    $$
    Since paper [[1].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) wants to approximate 
    $$
    \text{sin}^{2}\bigg(cp(y) + \frac{\pi}{4} \bigg) \approx \bigg( y - \frac{1}{2}\bigg) + \frac{1}{2}
    $$

    1. The $\text{sin}^{2}$ term is how amplitude is encoded in a quantum circuit.
    2. We can map term $\bigg( y - \frac{1}{2}\bigg) + \frac{1}{2}$ without ancilla on a quantum circuit. 

**Thus, we want to find $p(y)$ such that $c(y - \frac{1}{2}) + \frac{1}{2}$ is sufficiently well approximated by $\text{sin}^{2}(cp(y)+\frac{\pi}{4})$.** Thus, we try to solve $p(y)$ for 
$$
c(y - \frac{1}{2}) + \frac{1}{2} = \text{sin}^{2}(cp(y)+\frac{\pi}{4})
$$
we have 
$$
p(y) = \frac{1}{c}\bigg(\text{sin}^{-1}\bigg(\sqrt{c\bigg( y - \frac{1}{2}\bigg) + \frac{1}{2}}\bigg)-\frac{\pi}{4}\bigg)
$$
and we choose $p(y)$ as Taylor approximation of above equation around $y = 1/2$. From Taylor approximation, term $y-1/2$ makes a approximation an odd function and thus all sum of even function are zero. This approximation of order $2u+1$ leads to a maximal approximation error for above equation of 

$$
\frac{c^{2u+3}}{(2u+3)2^{u+1}}+O(c^{2u+5}),
$$

for all $y\in[0,1]$.

??? note "Why are we constructing $p(x)$ like this?"
    Amplitude estimation measures the probability:
    $$
    a = \sin^2(\theta)
    \Rightarrow \theta = \sin^{-1}(\sqrt{a})
    $$
    To encode a target probability $a \in [0,1]$, we compute this angle $\theta$ and apply the rotation gate:
    $$
    R_y(2\theta)|0\rangle = \cos(\theta)|0\rangle + \sin(\theta)|1\rangle
    $$
    This creates a quantum state where the amplitude of $|1\rangle$ is $\sin(\theta)$, so measuring the ancilla gives:
    $$
    P(|1\rangle) = \sin^2(\theta) = a
    $$
    Therefore, to prepare an amplitude that reflects $f(x)$, we construct $p(x)$ such that:
    $$
    p(x) = 2 \cdot \sin^{-1}(\sqrt{f(x)})
    $$
    This lets amplitude estimation recover $\mathbb{E}[f(X)]$ through quantum measurement.

## Convergence

## Reference
1. Quantum Risk Analysis

2. Quantum Amplitude Amplification and Estimation, G. Brassard, P. Hoyer, M. Mosca, and A. Tapp,
arXiv:0005055 (2000). [https://arxiv.org/abs/quant-ph/0005055](https://arxiv.org/abs/quant-ph/0005055)

3. Quantum measurements and the Abelian Stabilizer Problem, A. Y. Kitaev (1995), [https://arxiv.org/abs/quant-ph/9511026](https://arxiv.org/abs/quant-ph/9511026)

4. L. N. L. N. Trefethen, Approximation theory and approximation practice (2013), ISBN 1611972396.

