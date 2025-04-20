# Quantum Amplitude Estimiation Introduction
Suppose a unitary operator $\mathcal{A}$ acts on a register of $(n+1)$ qubits such that

$$
\mathcal{A}|0 \rangle_{n+1} = \sqrt{1-a} |\psi_{0} \rangle_{n} |0\rangle + \sqrt{a}|\psi_{1}\rangle_{n}|1\rangle,
$$

for some normalized states $|\psi_{0}\rangle_{n}$ and $|\psi_{1}\rangle_{n}$, where $a \in [0,1]$ is unknown.

Amplitude estimation allows us to efficiently estimate the value of $a$, i.e., the probability of measuring $|1\rangle$ in the last qubit. [[2]](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) This is done using an operator $Q$ and Quantum Phase Estimation (QPE) [[3]](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference), which helps approximate certain eigenvalues of $Q$.

### 1. We need some qubits!
Quantum Phase Estimation requires $m$ additional qubits and $M = 2^{m}$ applications of the operator $Q$. These $m$ qubits are first put into an equal superposition using Hadamard gates. Then, this superposition is used to control different powers of $Q$—each counting qubit controls a different $Q^{2^j}$.

### 2. Let’s do the inverse Fourier transform
After applying the inverse Quantum Fourier Transform (QFT) to the $m$ counting qubits, we measure them. This gives an integer outcome $y \in \{0,\dots,M-1\}$, which we map classically to an estimate $\widetilde{a}$.

The estimator $\widetilde{a}$ satisfies:

$$
\begin{array}{ll}
|a - \widetilde{a}| &\leq \frac{2\sqrt{a(1-a)\pi}}{M} + \frac{\pi^{2}}{M^{2}} \\
&\leq \frac{\pi}{M} + \frac{\pi^{2}}{M^{2}} = O(M^{-1}),
\end{array}
$$

with probability at least $\frac{8}{\pi^2} \approx 0.811$. Below figure is the quantum circuit for constructing a Qauntum Amplitude Estimation.

<div style="text-align: center;">
    <img src="../../Projs_Opt/images/Qcurcuit_ae.png" alt="Quantum circuit for amplitude estimation" style="width: 550px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Quantum circuit for amplitude estimation. \(H\) and is the Hadamard gate and \(F^{\dagger}_{m}\) denotes the inverse Quantum Fourier Transform on m qubits.
    </p>
</div>

???+ note "How to read this quantum circuit?"
    There are three sections in this quantum circuit:
    
    1. **Top $m$ qubits:**  
        These are the counting qubits (labeled from 0 to $j$ to $m{-}1$ in the figure). They are used in the [Quantum Counting Algorithm](https://en.wikipedia.org/wiki/Quantum_counting_algorithm) and [Grover's Algorithm](https://en.wikipedia.org/wiki/Grover%27s_algorithm). Each qubit starts in the state $|0\rangle$ and is passed through a Hadamard gate. After the Hadamard transformation, the $m$ qubits form an equal superposition:
        
        $$
        \frac{1}{\sqrt{2^{m}}} \sum_{k=0}^{2^{m}-1} |k\rangle.
        $$

        For example, if $m = 2$, we have an equal superposition state $|\psi\rangle$ of:

        $$
        |\psi\rangle = \frac{1}{2}(|0\rangle+|1\rangle+|2\rangle+|3\rangle).
        $$

    2. **Middle $n$ qubits:**  
        This register is initialized as $|0\rangle^{\otimes n}$ (denoted as $|0\rangle_n$ in the figure). ==**It is used to encode computational basis states $|i\rangle$ drawn from a probability distribution $p_i$.**==

    3. **Bottom 1 qubit (ancilla):**  
        This single ancilla qubit **==encodes the value of the function $f(i) \in [0, 1]$ via a controlled rotation.==**


???+ note "What is operator $A$?"

    Operator $A$ acts on the bottom $n+1$ wires:

    $$
    A|0\rangle^{\otimes (n+1)} \mapsto \sum_{i} \sqrt{p_i}\, |i\rangle_{n} \left( \sqrt{1 - f(i)}\,|0\rangle + \sqrt{f(i)}\,|1\rangle \right)
    $$

    This operation ==**prepares the quantum probability distribution** and **encodes the function $f(i)$ into the amplitude of the ancilla qubit.**== $A$ simultaneously acts on the $n$ qubits representing the computational basis state $|i\rangle$ and the single ancilla qubit used to encode $f(i)$.


???+ note "What is operator $Q$?"

    Operator $Q$ is the **Grover diffusion operator**, used in amplitude amplification. Each $m$ qubit controls a different power of $Q$, which acts on the bottom $n+1$ register. Specifically:

    - $Q^{2^0}$ is controlled by counting qubit 0  
    - $Q^{2^j}$ is controlled by counting qubit $j$  
    - $Q^{2^{m-1}}$ is controlled by the last counting qubit $m{-}1$

    These controlled powers of $Q$ are part of the standard phase estimation procedure.

???+ note "Inverse QFT $F^{\dagger}_{m}$?"

    The inverse quantum Fourier transform, denoted $F^{\dagger}_{m}$, is applied to the top $m$ qubits. It is used to **decode the phase information** acquired during the controlled-$Q$ operations. The eigenphase encodes the amplitude:

    $$
    a = \mathbb{E}[f(X)]
    $$

    which is the expected value of the function $f(i)$ under the distribution $p_i$. This step extracts the amplitude estimate after phase accumulation.


### 3. Finding Expected Value, the Quantum Way

In this section, we demonstrate how to use amplitude estimation to approximate the expected value of a discrete random variable encoded in a quantum state.

Suppose we have the quantum state

$$
|\psi\rangle_{n} = \sum_{i = 0}^{N-1} \sqrt{p_{i}}\,|i\rangle_{n},
$$

where the probability of measuring the basis state $|i\rangle_{n}$ is $p_{i} \in [0,1]$, with $\sum_{i=0}^{N-1} p_{i} = 1$ and $N = 2^{n}$. The state $|i\rangle_{n}$ represents one of $N$ possible outcomes of a bounded discrete random variable $X$.

For example, $|i\rangle$ might represent a discretized interest rate or the value of a portfolio. If $n = 2$, then $i \in \{0, 1, 2, 3\}$, which could correspond to an interest rate of 0%, 1%, 2%, or 3%, respectively.

### 4. Apply a Function to Extract Useful Info

Let’s say we have a function $f: \{0, \dots, N{-}1\} \rightarrow [0, 1]$ and define an operator:

$$
F: |i\rangle_{n} |0\rangle \mapsto |i\rangle_{n} \left( \sqrt{1 - f(i)}\,|0\rangle + \sqrt{f(i)}\,|1\rangle \right),
$$

which applies a controlled rotation on an ancilla qubit based on the value of $f(i)$. 

Now apply this operator ($F$) to the quantum state $|\psi\rangle_{n} |0\rangle$ ==($|\psi\rangle_{n} = \sum_{i = 0}^{N-1} \sqrt{p_{i}}\,|i\rangle_{n}$)==, and we get:

$$
\sum_{i=0}^{N-1} \sqrt{p_i}\,|i\rangle_{n} \left( \sqrt{1 - f(i)}\,|0\rangle + \sqrt{f(i)}\,|1\rangle \right).
$$

At this point, the probability of measuring $|1\rangle$ in the last qubit is:

$$
\sum_{i=0}^{N-1} p_i f(i) = \mathbb{E}[f(X)],
$$

which is exactly what we want to estimate. To do this, we simply square the amplitude of the last qubit $|1\rangle$.

Now for a few useful choices of $f(i)$:

- If you use $f(i) = i / (N - 1)$, you’re estimating the **normalized mean** $\mathbb{E}[X / (N - 1)]$, and from that, you can recover $\mathbb{E}[X]$.
- If you use $f(i) = i^2 / (N - 1)^2$, you can estimate $\mathbb{E}[X^2]$ and then compute the **variance**:
  
  $$
  \text{Var}(X) = \mathbb{E}[X^2] - \left(\mathbb{E}[X]\right)^2
  $$

---

### 5. How to evaluate risk measures such as VaR and CVaR

#### VaR
For a given confidence level $\alpha \in [0,1]$. $\text{VaR}_{\alpha}(X)$ (X is a random variable, can be loss or value) can be defined as the smallest value $x \in \{0,\cdots,N-1\}$ such that $\mathbb{P}[X \leq x]\geq (1-\alpha)$, which simply implies that with $1-\alpha$ probability, losses will not exceed $x$. To find $\text{VaR}_{\alpha}(X)$ on a quantum computer, we define the function 

$$
f_l(i) = 
\begin{cases}
1 & \text{if } i \leq l \\
0 & \text{otherwise}
\end{cases}
$$

where $l \in \{0,\cdots,N-1\}$. Applying $F_l$, i.e. the operator corresponding to $f_l$. to $|\psi\rangle_{n}|0\rangle$ leads to the state
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

#### CVaR

$\text{CVaR}_{\alpha}(X)$ is the conditional expectation of $X$ restricted to $\{0,\cdots,l_{\alpha}\}$, where we compute $l_{\alpha} = \text{VaR}_{\alpha}(X)$ as before. To estimate CVaR we apply the operator $F$ that corresponds to the function 
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
\text{CVaR}_{\alpha}(X) = \frac{l_{\alpha}}{\mathbb{P}[X \leq l_{\alpha}]}\sum_{i=0}^{l_{\alpha}}\frac{i}{l_{\alpha}}p_{i}.
$$
We also multiplied by $l_{\alpha}$, otherwise we would estimate $\text{CVaR}_{\alpha}(\frac{X}{l_{\alpha}})$. Even though we replace $\mathbb{P}[X \leq l_{\alpha}]$ by an estimation, the error on CVaR, shows that we still achieve a quadratic speed up compated to classical Monte Carlo methods.


## Construction of Quantum Circuit

Approximating $\mathbb{E}$ using amplitude estiamtion requires the operator $F$ for $f(x) = x/(N-1)$. In general, representing $F$ for the expected value or for the CVaR either additional ancillas to pre-compute the (discretized) function $f$ into qubits, using quantum arithmetic, before applying the rotation (ref)/ And the exact number of ancillas depends on the desired accuracy of the approximation of $F$. 

In the following, paper [[1]](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) will show how to approximate $F$ without ancillas using polynomially many gates, at the cost of a lower - but still faster than classical - rate of convergence. Note that the operator required for estimating VaR is easier to construct and we can always achieve the optimal rate of convergence as discussed later.

### Polynomial Approximate Quantum Circuit without Ancillas

The contribution in paper paper [[1].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) rests on the fact that an operator 
$$
P: |x\rangle_{n}|0\rangle \mapsto |x\rangle_{n}(\text{cos}(p(x))|0\rangle + \text{sin}(p(x))|1\rangle)
$$
for a given polynomial $p(x) = \sum_{j=0}^{k}p_{j}x^{j}$ of order $k$, can be efficiently constructed using multi-controlled Y-rotations.

(image)
*Here, we use qiskit for illustration, since Qiskit doesn't have native Ry-controlled gate with 2+ controlls, we use $\text{MCX}(q_{0},q_{1}) \mapsto R_y(4a) \mapsto \text{MCX}(q_{0},q_{1})$. See [Qiskit Quantum library](https://docs.quantum.ibm.com/api/qiskit/circuit_library), for more infomation.

In the single qubit operations with $n-1$ control qubits, it can be exactly constructed using $O(n)$ gates and $O(n)$ ancillas or $O(n^{2})$ gates without ancilla. Here, peper [[1]](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) uses $O(n)$ gates and $O(n)$ ancillas. Since the binary variable representation of $p$, leads to at most $n^{k}$ terms, the operator $P$ can be constructed using $O(n^{k+1})$ gates and $O(n)$ ancillas.

For every analytic function $f$, there exists a sequence of polynomials such that the approximation error converges exponentially fast to zero with increasing order of the polynomials [[4]](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference). 

If we can find a polynomial $p(y)$ such that $\text{sin}^{2}(p(y)) = y$, then we can set $y=f(x)$, and the previous discussion provides a way to construct the operator $F$. Since [the expected value is linear](../../Math_Fundamentals/probability_theory.md#linearity-of-expectation), we may choose to estimate $\mathbb{E}[c(f(X) - \frac{1}{2}) + \frac{1}{2}]$ instead of $\mathbb{E}[f(X)]$ for a parameter $c\in(0,1]$, and then map the result back to an estimatior for $\mathbb{E}[f(X)]$. The rationale behind this choice is that $\text{sin}^{2}(y+\frac{\pi}{4}) = y+\frac{1}{2} + O(y^3)$.

???+ note "What is $\mathbb{E}[c(f(X) - \frac{1}{2}) + \frac{1}{2}]$ and Why?"
    By the linearity of expected value, we may define a function $g(X)$ such that
    $$
    g(X) := c(f(X) - \frac{1}{2}) + \frac{1}{2} \Rightarrow \mathbb{E}[g(X)] = c\mathbb{E}[f(X)] + (\frac{1}{2}+\frac{c}{2})
    $$
    therefore, you can estiamte $\mathbb{E}[g(X)]$ (via amplitude estimation), you can solve for 
    $$
    \mathbb{E}[f(X)] = \frac{1}{c}(\mathbb{E}[g(X)]-\frac{1}{2})+\frac{1}{2}.
    $$
    Paper [[1]](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) wants to approximate 
    $$
    \text{sin}^{2}\bigg(cp(y) + \frac{\pi}{4} \bigg) \approx \bigg( y - \frac{1}{2}\bigg) + \frac{1}{2}
    $$

    1. The $\text{sin}^{2}$ term is how amplitude is encoded in a quantum circuit.
    2. We can map term $\bigg( y - \frac{1}{2}\bigg) + \frac{1}{2}$ without ancilla on a quantum circuit. 


???+ note "Why this weird-looking expression?"
    Paper [[1]](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference) wants to approximate 
    $$
    \text{sin}^{2}\bigg(cp(y) + \frac{\pi}{4} \bigg) \approx \bigg( y - \frac{1}{2}\bigg) + \frac{1}{2}
    $$
    And the reason is that 1. Function $\text{sin}^{2}(y+\frac{\pi}{4} \approx y + \frac{1}{2})$ when $y$ is small (Taylor expression). By doing this,

???+ note "Why do we use $c(f(x) - \tfrac{1}{2}) + \tfrac{1}{2}$ and how does it relate to amplitude estimation?"
    Quantum amplitude estimation (QAE) measures probabilities of the form:
    $$
    a = \sin^2(\theta) \quad \Rightarrow \quad \theta = \sin^{-1}(\sqrt{a})
    $$
    To encode a desired probability $a \in [0,1]$, we prepare the state:
    $$
    R_y(2\theta)|0\rangle = \cos(\theta)|0\rangle + \sin(\theta)|1\rangle
    $$
    which yields:
    $$
    P(|1\rangle) = \sin^2(\theta) = a \ (\text{Born rule})
    $$
    So to encode a function $f(x)$ as a measurable amplitude, we must construct a function $p(x)$ such that:
    $$
    \sin^2(p(x)) = f(x)
    \quad \Rightarrow \quad p(x) = \sin^{-1}(\sqrt{f(x)})
    $$
    But since arbitrary functions are hard to implement on quantum circuits, we use a **polynomial approximation** of this $p(x)$. Instead of encoding $f(x)$ directly, we approximate a more convenient form:
    $$
    c(f(x) - \tfrac{1}{2}) + \tfrac{1}{2}
    $$
    Why this form? Because of the Taylor expansion:
    $$
    \sin^2(y + \tfrac{\pi}{4}) = \tfrac{1}{2} + y - \tfrac{4y^3}{6} + \cdots
    \Rightarrow \boxed{\sin^2(y + \tfrac{\pi}{4}) \approx \tfrac{1}{2} + y} \text{ for small } y
    $$
    This motivates encoding:
    $$
    \sin^2(cp(f(x)) + \tfrac{\pi}{4}) \approx c(f(x) - \tfrac{1}{2}) + \tfrac{1}{2}
    $$
    - $f(x) - \tfrac{1}{2}$: centers the value around 0
    - $c \in (0,1]$: scales down for small-angle accuracy
    - $+ \tfrac{1}{2}$: shifts result back to \([0,1]\)

    This form enables accurate encoding of $f(x)$ into amplitude using polynomially approximated quantum rotations.




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

???+ note "Why are we constructing $p(x)$ like this?"
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
1. Quantum Amplitude Amplification and Estimation. Brassard et al., 2002. [https://arxiv.org/abs/quant-ph/0005055](https://arxiv.org/abs/quant-ph/0005055)

2. Quantum Amplitude Amplification and Estimation, G. Brassard, P. Hoyer, M. Mosca, and A. Tapp,
arXiv:0005055 (2000). [https://arxiv.org/abs/quant-ph/0005055](https://arxiv.org/abs/quant-ph/0005055)

3. Quantum measurements and the Abelian Stabilizer Problem, A. Y. Kitaev (1995), [https://arxiv.org/abs/quant-ph/9511026](https://arxiv.org/abs/quant-ph/9511026)

4. L. N. L. N. Trefethen, Approximation theory and approximation practice (2013), ISBN 1611972396.

