# Quantum Postulates


## Postulate 1 - State space

!!! note "Postulate 1"
    Associated to any isolated physical system is a complex vector space with inner product (that is, a Hilbert space) known as the state space of the system. The system is completely described by its state vector, which is a unit vector in the system’s state space.

A qubit has two-dimensional state. Suppose $|0\rangle$ and $|1\rangle$ form an orthonormal basis for that state space. Then an arbitrary statevector in the state space can be written 

$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle
$$

where $\alpha$ and $\beta$ are complex numebrs; $\alpha^{2} + \beta^{2} = 1$. The condition that $|\psi\rangle$ be a unit vector. $\langle \psi|\psi\rangle =1$ is known as a *normalization condition* for state vectors.

We say that any linear combination $\sum_{i}\alpha_{i}|\psi_{i}\rangle$ is a superposition of the state $|\psi_{i}\rangle$ with *amplitude $\alpha_{i}$* for the state $|\psi_{i}\rangle$. For example,

$$
\frac{|0\rangle - |1\rangle}{\sqrt{2}}
$$

is a superposition of the state $|0\rangle$ and $|1\rangle$ with amplitude $1/\sqrt{2}$ for the state $|0\rangle$ and $-1/\sqrt{2}$ for the state $|1\rangle$.


## Postulate 2 - Evolution
!!! note "Postulate 2"
    The evolution of a closed quantum system is described by a unitary transformation. This is, the state $|\psi\rangle$ of the system at time $t_{1}$ is related to the state $|\psi'\rangle$ of the system at time $t_{2}$ by a unitary operator $U$ which depends only on the times $t_{1}$ and $t_{2}$.

## Postulate 3 - Quantum measurement
!!! note "Postulate 3"
    Quantum measurements are described by a collection $\{M_{m}\}$ of *measurement operators*. These are operators acting on the state space of the system being measured. The  $m$ refers to the measurement outcomes that may occur in the experiment. If the state of the quantum system is $|\psi\rangle$ immediately before the measurement then the probability that result $m$ occurs is given by 

    $$
    p(m) = \langle \psi |M_{m}^{\dagger}M_{m}|\psi\rangle,
    $$

    and the state of the system after the measurement is 

    $$
    \frac{M_{m}|\psi\rangle}{\sqrt{\langle \psi |M_{m}^{\dagger}M_{m}|\psi\rangle}}.
    $$

    The measurement operators satisfy the *completness equation*,

    $$
    \sum_{m}M_{m}^{\dagger}M_{m} = I.
    $$

A simple but important example of a measurement is the *measurement of a qubit in the computational basis*. This is a measurement on a single qubit with two outcomes defined by the two measurement opeartors $M_{0} = |0\rangle \langle 0|$, $M_{2} = |1\rangle \langle 1|$. Notice that each operator is Hermitian, and that $M_{0}^{2} = M_{0}, M_{1}^{2} = M_{1}$. From *completness equation*, $\sum_{m}M_{m}^{\dagger}M_{m} = I$, we have $I = M_{0}^{\dagger}M_{0} + M_{1}^{\dagger}M_{1} = M_{0}+M_{1}$. Suppose the state being measured is $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$. Then the probability of obtaining measurement outcome 0 is 
$$
p(0) = \langle \psi|M_{0}^{\dagger}M_{0}|\psi\rangle = \langle \psi|M_{0}|\psi\rangle = |\alpha|^{2}
$$
similarly, the probability of getting the result of $1$ is $p(1) = |\beta|^{2}$. The state after measurement in the two cases is therefore 
$$
\begin{array}{rl}
\frac{M_{0}|\psi\rangle}{|\alpha|} & = \frac{\alpha}{|\alpha|}|0\rangle\\
\frac{M_{1}|\psi\rangle}{|\beta|} & = \frac{\beta}{|\beta|}|1\rangle.
\end{array}
$$

## Postulate 4 - Distinguishing quantum states

An important application of Postulate 3 is to the problem of *distinguishing quantum states*. In the classical world, distinct states of an object are usually distinguishable, at least in principle. For example, we can always identify whether a coin has landed heads or tails, at least in the ideal limit. Quantum mechanically, the situation is more complicated. For example, we say a plausible argument that non-orthogonal quantum states cannot be distinguished.

Imagine a game between Alice and Bob. Alice has a known list of quantum states, and she picks one of them — say $|\psi_i\rangle$ — and sends it to Bob. Bob's task is to figure out which state she sent.

### Orthonormal States

If the states $|\psi_i\rangle$ are **orthonormal**, Bob can always identify the state with certainty. He constructs a quantum measurement with operators that project onto each basis state. For example, $(M_{i} \equiv |\psi_{i}\rangle \langle \psi_{i}|)$, one for each possible index $i$, and an addtional measurement operator $M_{0}$ defined as the positive square root of the positive operator $I - \sum_{i \neq 0} |\psi_{i}\rangle\langle\psi_{i}|$. These operators satify the *completeness relation*, and if the state $|\psi_{i}\rangle$ is prepared then $p(i) = \langle \psi_{i}|M_{i}|\psi_{i} \rangle = 1$, so the result $i$ occurs with certainty. Thus, it is possible to reliably distinguish the orthonormal states $|\psi_{i}\rangle$.

### Non-Orthogonal States

If the stateS $|\psi_{i}\rangle$ are not orthonormal then we can prove that there is *no quantum measurement capable of distinguishing the states*. The idea is that Bob will do a measurement described by measurement operators $M_{j}$, with outcome $j$. The key to why Bob cannot distinguish non-orthogonal states $|\psi_{1}\rangle$ and $|\psi_{2}\rangle$ is the observation that $|\psi_{2}$ can be decomposed into a (non-zero) component parallel to $|\psi_{1}\rangle$, and a component orthogonal to $|\psi_{1}\rangle$. Suppose $j$ is a measurement outcome such that $f(i) =1$, that is, Bob guesses that the sate was $|\psi_{1}\rangle$ when he observes $j$. but because of the component of $|\psi_{2}\rangle$ parallel to $|\psi_{1}\rangle$, these is a non-zero probability of getting outcome $j$ when $|\psi_{2}\rangle$ is prepared, so sometimes bob will make an error identifying which state was prepared.

## Postulate 5 - Projective measurements

Projective measurements actually turn out to be equivalent to the general measurement postulate, when they are augmented with the ability to perform unitary transformations, as described in Postulate 2.

!!! note "Projective measurements"
    A projective measurement is described by an *observable*, $M$, a Hermitian operator on the state space of the system being observed. The observable has a spectral decomposition,

    $$
    M = \sum_{m}mP_{m},
    $$

    where $P_{m}$ is the projector onto the eigenspace of $M$ with eigenvalue $m$/ The possible outcomes of the measurement correspond to the eigenvalues $m$, of the observable. Upon measuring the state $|\psi\rangle$, the probability of getting results $m$ is given by

    $$
    p(m) = \langle \psi|P_{m}|\rangle
    $$

    Given that outcome $m$ occurred, the state of the quantum system immediately after measurement is 

    $$
    \frac{P_{m}|\psi\rangle}{\sqrt{p(m)}}.
    $$

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