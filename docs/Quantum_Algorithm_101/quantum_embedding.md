# Quantum Embedding 

## Introduction
A quantum embedding represtnes classical data as quantum states in Hilbert Space via a quantum feature map. With the input $x$, we get $|x\rangle$ after quantum embedding, which is a set of gate parameters in a quantum circuit.

Let's consider classical input date with $m$ example with $N$ featuress each, 

$$
\mathcal{D} = x^{(1)}, \cdots, x^{(m)}, \cdots, x^{(M)},
$$

where $x^{(1)}$ is the $N$-dimensional vector for $m = 1, \cdots, M$. To embed this data into **$n$** quantum subsystems. We can use various type of embedding techniques like [**Basis Embedding**](../Math_Fundamentals/quantum_embedding.md#basis-embedding) and [**Amplitude Embedding**](../Math_Fundamentals/quantum_embedding.md#amplitude-embedding) we are going to talk about next.

## Basis Embedding
Basis Embedding associates each input with a computational basis state of a qubit subsystem. The classical data has to be in the form of binary bit strings. For example, $x = 1101$ is represented by the 4-qubit quantum state $|1001\rangle$.

> One bit of classical information is represented by one quantum subsystem.

Thus, $x^{(m)} = (b_{1},\cdots,b_{N})$ with $b_{i} \in [0,1]$ for $i=1,\cdots, N$. we have 

$$
x^{(m)} \mapsto |X^{(m)}\rangle.
$$

The number of quantum subsystem $(n)$ most at least equals to $N$.

## Amplitude Embedding
Data is encoded into amplitudes of quantum state. A normalized classical $N$-dimensional classical datapoint $x$ is represented by the *amplitude of a $n$-qubit quantum state $|\psi_{x}\rangle$ as*

$$
|\psi_{x}\rangle = \sum_{i = 1}^{N}x_{i}|i\rangle,
$$

where $N = 2^{n}$, $x_{i}$ is the *i-th element of $x$* and $|i\rangle$ is the *i-th computational basis state.* $x_{i}$ can be either float or integer. For example, we have a normalized classical data 

$$
x_{\text{norm}} = \frac{1}{\sqrt{31.25}}(1,0.0,-5.5,0.0),
$$

the corrosponding amplitude embedding is 

$$
\begin{array}{ll}
|\psi_{x_{\text{norm}}}\rangle & = \frac{1}{\sqrt{31.25}}[1|00\rangle + 0|01\rangle - 5.5|10\rangle+0|11\rangle]\\
\ & = \frac{1}{\sqrt{31.25}}[1|00\rangle - 5.5|10\rangle]
\end{array}
$$

Let's consider the classical $\mathcal{D}$ above. Its amplitude embedding can be easily understood if we concatenate all the input example $x$ together into one vector 

$$
\alpha = C_{\text{norm}}x^{(1)}_{1},\cdots,x^{(1)}_{N},x^{(2)}_{1},\cdots,x^{(2)}_{N},\cdots,x^{(M)}_{1},\cdots,x^{(M)}_{N},
$$

where $C_{\text{norm}}$ is the normalization constant, $|\alpha|^{2} = 1$. The input dataset can be represented as 

$$
|\mathcal{D}\rangle = \sum_{i=1}^{2^{n}}\alpha_{i}|i\rangle,
$$

where $\alpha_{i}$ are the elements of the *element vector* $\alpha$ and $|i\rangle$ are computational basis states. The number of amplitude to be encoded is $N\times M$.

-   $N$: N features for each $m \in 1,...,M$.
-   $M$: M examples.

> As a system of $n$ qubit provides $2^{n}$ amplitude, it requires $n \geq log_{2}(NM)$ qubits. 

That is 
  
$$
\text{number of qubits} = n = \lceil log_{2}(NM) \rceil.
$$

---
More embedding methodologies from [PENNYLANE - Embedding templates](https://docs.pennylane.ai/en/stable/introduction/templates.html)

-   Angle Embedding
-   Displacement Embedding
-   IQPE Embedding
-   QAOA Embedding
-   Squeezing Embedding

---

## Reference 
1. PENNYLANE - Quantum Embedding: [https://pennylane.ai/qml/glossary/quantum_embedding](https://pennylane.ai/qml/glossary/quantum_embedding)