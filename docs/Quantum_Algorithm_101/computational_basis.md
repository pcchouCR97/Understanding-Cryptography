# Computational Basis

The **computational basis** for an $n$-qubit Hilbert space $\mathcal{H} \in \mathbb{C}$ is the set of orthonormal vectors:

$$
\mathcal{B} = \{ |i\rangle : i = 0, 1,...,2^{n}-1\}
$$

Let $|i\rangle$ cprresponds to a **binary string** $|q_{0}q_{1}...q_{n-1}\rangle$ such that:

$$
|q_{0}\rangle \otimes |q_{1}\rangle \otimes \cdots \otimes |q_{n-1}\rangle = \text{1-hot vector in} \ \mathbb{C}^{2^{n}}.
$$

This basis forms a complete orthonomal basis for the hilbert space and it's **used in measurement** outcomes are reported in computational basis.