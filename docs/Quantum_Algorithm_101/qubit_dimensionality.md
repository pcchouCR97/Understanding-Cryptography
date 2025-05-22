# Multi-Qubit Hilbert Space

## General 2ⁿ-Dimensional State

Any $n$-qubit quantum state is a linear combination of all computational basis states:

$$
|\psi\rangle = \sum_{i=0}^{2^n - 1} \alpha_i |i\rangle, \quad \text{with } \sum_{i=0}^{2^n - 1} |\alpha_i|^2 = 1
$$

This is represented as a normalized column vector in $\mathbb{C}^{2^n}$, where each amplitude $\alpha_i$ can encode probabilities, payoff values, or other structured data.

## Qubit Basis and Vector Representation

A Hilbert space is a complex vector space that represents the state of quantum systems. For a single qubit, the Hilbert space is $\mathbb{C}^2$, with the computational basis:

$$
|0\rangle = \begin{bmatrix}1 \\ 0\end{bmatrix}, \quad
|1\rangle = \begin{bmatrix}0 \\ 1\end{bmatrix}
$$

In the standard **big-endian** convention (most significant bit first), the binary basis state $|0\rangle$ corresponds to the first index, hence it is represented by $[1, 0]^T$. Similarly, $|1\rangle$ corresponds to the second index, represented by $[0, 1]^T$.

This mapping generalizes to multi-qubit systems. For example, with $n = 3$, the state space is $\mathbb{C}^{2^3} = \mathbb{C}^8$. The basis state $|001\rangle$ corresponds to index 1 in binary ordering and is represented as:

$$
|001\rangle =
\begin{bmatrix}
0 \\
1 \\
0 \\
0 \\
0 \\
0 \\
0 \\
0
\end{bmatrix}
$$

> Each basis state $|i\rangle$ maps to a one-hot vector of length $2^n$, ==with a 1 at position $i$== and 0 elsewhere.
