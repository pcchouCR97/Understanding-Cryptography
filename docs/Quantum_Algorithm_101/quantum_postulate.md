# Quantum Postulates


## Postulate 1 - State space

!!! note "Postulate 1"
    Associated to any isolated physical system is a complex vector space with inner product (that is, a Hilbert space) known as the state space of the system. The system is completely described by its state vector, which is a unit vector in the system’s state space.

A qubit has two-dimensional state. Suppose $|0\rangle$ and $|1\rangle$ form an orthonormal basis for that state space. Then an arbitrary statevector in the state space can be written 

$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle
$$

where $\alpha$ and $\beta$ are complex numebrs.

## Postulate 2 - Evolution


## Postulate 3 - Quantum measurement

## Postulate 4 - Distinguishing quantum states


## Postulate 5 - Projective measurements

## Postulate 6 - POVM measurements

## Postulate 7 - Phase

## Postulate 8 - Composite systems

## Postulate 9 - Quantum mechanics: global view


### Composite system postualte

> The Hilbert space of a composite system is the Hilbert space tensor product of the state spaces associated with the component systems. For a non-relativistic system consisting of a finite number of distinguishable particles, the component systems are the individual particles.

## Measurement of a system

### Description of physical quantutites - Postulate II - a

> Every measurable physical quantity $\mathcal{A}$ is described by a Hermitian operator $A$ acting in the state space $\mathcal{H}$. This operator is an observable, meaning that its eigenvectors form a basis for $\mathcal{H}$. The result of measuring a physical quantity $\mathcal{A}$ must be one of the eigenvalues of the corrosponding observable $A$.

### Results of measurement - Postulate II - b

By spectral theory, we can associate a probability measure to the values of $A$ in any state ψ. We can also show that the possible values of the observable $A$ in any state must belong to the spectrum of $A$. The expectation value (in the sense of probability theory) of the observable A for the system in state represented by the unit vector $\psi \in \mathcal{H}$ is $\langle \psi |A|\psi \rangle$. ==If we represent the state ψ in the basis formed by the eigenvectors of $A$, then the square of the modulus of the component attached to a given eigenvector is the probability of observing its corresponding eigenvalue.==

> When the physical quantity $\mathcal{A}$ is measured on a system in a normalized state $|\psi\rangle$, the probability of obtaining an eigenvalue (denoted $a_{n}$ for discrete spectra and $\alpha$ for continuous spectra) of the corresponding observable $A$ is given by the *amplitude squared* of the appropriate wave function (projection onto corrosponding eigenvector)

Here are three cases:

$$
\begin{array}{ll}
\mathbb{P}(a_n) = |\langle a_{n}|\psi\rangle|^{2} & \text{Discrete, nondegenerate spectrum}\\
\mathbb{P}(a_n) = \sum_{i}^{g_n}|\langle a_{n}|\psi\rangle|^{2} & \text{Discrete, degenerate spectrum}\\
d\mathbb{P}(a_n) = |\langle a_{n}|\psi\rangle|^{2}d\alpha & \text{Continuous, nondegenerate spectrum}
\end{array}
$$

### Effect of measurement on the state - Postulate II - c 

> If the measurement of the physical quantity $\mathcal{A}$ on the system in the state $|\psi\rangle$ gives the result $a_n$, then the state of the system immediately after the measurement is the normalized projection of $|\psi\rangle$ onto eigensubspace associated with $a_n$

$$
|\psi\rangle \Rightarrow\frac{P_{n}|\psi\rangle}{\sqrt{\langle \psi |P_{n}|\psi\rangle}}
$$

## References 

[1]. [https://en.wikipedia.org/wiki/Mathematical_formulation_of_quantum_mechanics](https://en.wikipedia.org/wiki/Mathematical_formulation_of_quantum_mechanics)