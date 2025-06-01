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
\begin{array}{c}
\frac{M_{0}|\psi\rangle}{|\alpha|} = \frac{\alpha}{|\alpha|}|0\rangle,\\
\frac{M_{1}|\psi\rangle}{|\beta|} = \frac{\beta}{|\beta|}|1\rangle.
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
    p(m) = \langle \psi|P_{m}|\psi\rangle
    $$

    Given that outcome $m$ occurred, the state of the quantum system immediately after measurement is 

    $$
    \frac{P_{m}|\psi\rangle}{\sqrt{p(m)}}.
    $$

A projection measurement can be understoof as a special case of [Postulate 3 - Quantum measurement](../Quantum_Algorithm_101/quantum_postulate.md#postulate-3---quantum-measurement). Suppose the measurement operator in Postulate 3, in addition to the completenss relation $\sum_{m}M^{\dagger}M_{m} = I$, also satisfy the conditions that $M_{m}$ are orthogonal projectors, that is, the $M_{m}$ are Hermitian, and 

$$
M_{m}M_{m'} = \delta_{m,m'}M_{m}.
$$

With these, it reduces to a projective measurement as defined.

Of course, since the projective measurement is a special case of Postulate 3. We can also define the projectors more restrictedly

$$
P_{m}^{2} = P_{m}, P_{m}^{\dagger} = P_{m}, \sum_{m}P_{m} = I
$$

Projective measuremnets have many useful properties. In particular, it is very easy to calculate average for projective measurements. By definition, the average value of the measurement ($[\mathbb{E}]$),

$$
\begin{array}{rl}
[\mathbb{E}(M)] & = \sum_{m}mp(m)\\
\ & = \sum_{m}m\langle \psi|P_{m}|\psi\rangle\\
\ & = \langle \psi|\bigg(m\sum_{m}P_{m}\bigg)|\psi\rangle\\
\ & = \langle \psi|M|\psi\rangle\\
\end{array}
$$

This is so useful that the average value of the observable $M$ is often written $\langle M \rangle \equiv \langle \psi|M|\psi\rangle$. From this formula, the standard deviation, a measure of the typical spread of the observed values upon measurement
of $M$, associated to observations of $M$,

$$
\begin{array}{rl}
[\Delta(M)]^{2} & = \langle (M^{2} - \langle M \rangle^{2}) \rangle \\
              \ & = \langle M^{2} \rangle - \langle M \rangle^{2}.
\end{array}
$$

For example, suppose we prepare a quantum system in an eigenstate $|\psi\rangle$ of some observable $M$, with corresponding eigenvalue $m$. What is the average observed value of $M$, and the standard deviation?

We have an observable $M$, state $|\psi\rangle$ is an eigenstate of $M$, so:

$$
M|\psi\rangle = m|\psi\rangle
$$

The expectation value is 

$$
\langle M \rangle = \langle \psi|M|\psi \rangle = m
$$

since $|\psi\rangle$ is an eignestate of $M$ with eigenvalue $m$, the measurement will always yield $m$. The standard deviation is 

$$
[\Delta(M)]^{2} = \langle m^{2} - \langle m \rangle^{2} \rangle = 0.
$$

Rather than giving an observable to describe a projective measurement, often people simply list a complete set of orthogonal projectors $P_{m}$ satisfying the relations $\sum_{m}mP_{m} = I$ and $P_{m}P_{m'} = \delta_{mm'}P_{m}$.

The coresponding observable implicit in this usage is $M = \sum_{m}mP_{m}$. Another widely used phrase, to 'measure in a basis $|m\rangle$', where $|m\rangle$ form an otrhonormal basis, simply means to perform the projective measurement with projectors $P_{m} = |m\rangle\langle m|$. Let's look at an example of projective measurements on single qubits. 

First is the measurement of the observable $Z$. This has rigenvalues $+1$ and $-1$ with corresponding eigenvectors $|0\rangle$ and $|1\rangle$. Thus, for example, measurement of $Z$ on the state $|\psi\rangle = (|0\rangle + |1\rangle)/\sqrt{2}$ gives the result $+1$ with probability $\langle \psi|0\rangle \langle 0|\psi\rangle = 1/2$, and similarly the result $-1$ with probability $1/2$. Suppose $\overrightarrow{v}$ is any real three-dimensional unit vector. Then we can define an observable:

$$
\overrightarrow{v} \cdot \overrightarrow{\sigma} \equiv v_{1}\sigma_{1} + v_{2}\sigma_{2} + v_{3}\sigma_{3}.
$$

> Measurement of this observable is sometimes referred to as a 'measurement of spin along the $\overrightarrow{v}$', for historical reasons.

Exercise 1. Suppose we have qubit in the state $|0\rangle$, and we measure the observable $X$. What is the average value of $X$? What is the standard deviation of $X$?

Exercise 2. Calculate the probability of botaining the result $+1$ for a measurement of $\overrightarrow{v} \cdot \overrightarrow{\sigma}$, given that the state prior to measurement is $|0\rangle$. What is the state of the system after the measurement if $+1$ is obtained?


## Postulate 6 - POVM measurements

## Postulate 7 - Phase
Phase is a commonly used term in quantum mechanics, with several different meanings depends upon context. At this point it is convenient to review a couple of these meanings. 

### Global phase
Consider, for example, the state $e^{i\theta}|\psi\rangle$, where $|\psi\rangle$ is a state vector, and $\theta$ is a real number. We say that the state $e^{i\theta}|\psi\rangle$ is equal to $|\psi\rangle$, up to the *global phase factor $e^{i\theta}$.* It is interesting that the statistics od measurement predicted for these two state are the same. 

> For this reason we may ignore global phase factors as being irrelevanr to the observed properties of the physical system.

> Global phase doesn't change the direction of the Bloch vector at all.

### Relative phase
Howeverm, relative phase, on the other hand, is not something we can ignore. It refers to the phase difference between parts of a quantum superposition. For example, we have two states

$$
\frac{|0\rangle + |1\rangle}{\sqrt{2}} \ \text{and} \ \frac{|0\rangle +- |1\rangle}{\sqrt{2}},
$$

Both has the same amplitudes in magnitude $\frac{1}{\sqrt{2}}$, but the second state has a relative phase of $-1$ between $|0\rangle$ and $|1\rangle$. More generally still, two states are said to *differ by a relative phase* in some basis if each of the amplitudes in that basis is realted by such a phase factor.

Suppose you have a qubit state 

$$
|\psi\rangle = \alpha|0\rangle + \beta|0\rangle, \ \alpha, \beta \in \mathbb{C}
$$

since each amplitude is a complex number, we write

$$
\alpha = |\alpha|e^{i\theta}
$$

from Eular identity. You already know that $|\alpha|^{2}$ is a measurement probability. The $\text{arg}(\alpha) = \text{phase}$. The relative phase is defined as 

$$
\phi = \text{arg}(\beta) - \text{arg}(\alpha)
$$

where $\text{arg}(\alpha)$ is a phase angle, $\text{tan}^{-1}(\frac{b}{a})$ from a complex number $\alpha = a + bi$. We can easily calculate that $\alpha = 1e^{i\pi/4}$ from $\alpha = \frac{1}{\sqrt{2}}+\frac{1}{\sqrt{2}}i$.

!!! example "Reltaive phase"
    Suppose you are given a normalized qubit state:

    $$
    |\psi\rangle = \frac{1}{\sqrt{2}}|0\rangle + \frac{i}{\sqrt{2}}|1\rangle
    $$

    we say $\alpha = 1/\sqrt{2}$ and $\beta = i/\sqrt{2}$. From Eular equation,

    $$
    \alpha = |\alpha|e^{i\theta_{0}}, \beta = |\beta|e^{i\theta_{1}}
    $$

    we have $|\alpha| = 1/\sqrt{2}$ with $\theta_{0} = 0$ and $|\beta| = 1/\sqrt{2}$ with $\theta_{1} = \pi/2$. The relative phase can be derived as 

    $$
    \phi = \theta_{1} - \theta_{0} = \frac{\pi}{2}
    $$

    so the state can be written as 

    $$
    |\psi\rangle = \frac{1}{\sqrt{2}}(|0\rangle + e^{i\frac{\pi}{2}}|1\rangle).
    $$

## Postulate 8 - Composite systems
!!! note "Postulate 4"
    The state space of a composite physical system is the tensor product of the state spaces of the component physical systems. Moreover, if we have system numbered $1$ through $n$, and system $i$ is prepared in the state $|\psi_{i}{\rangle$, then the joint state of the total system is $|\psi_{1}\rangle \otimes |\psi_{2}\rangle \otimes \cdots \otimes |\psi_{n}\rangle$

Postulate 4 also enables us to define one of the most interesting and puzzling ideas assocaited with composite quantum system - entanglement. Consider the two qubit state 

$$
|psi\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}
$$

There are no such states $|a\rangle$ and $|b\rangle$ that can make $|\psi\rangle = |a\rangle|b\rangle$.

!!! note "Proof $|\psi\rangle = |a\rangle|b\rangle$ doesn't exist"
    Suppose 

    $$
    |a\rangle \otimes |b\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}
    $$

    write 
    
    $$
    |a\rangle = \alpha|0\rangle + \beta|1\rangle, \ |b\rangle = \gamma|0\rangle + \delta|1\rangle,
    $$

    then

    $$
    |a\rangle |b\rangle = \alpha\gamma|00\rangle + \alpha\delta|01\rangle + \beta\gamma|10\rangle + \beta\delta|11\rangle
    $$

    there are no such coefficient combinations $\alpha,\beta,\gamma,\delta$ exist.

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

[1]. M. A. Nielsen and I. L. Chuang, *Quantum Computation and Quantum Information*, 10th Anniversary Ed., Cambridge: Cambridge University Press, 2010.

[2]. [https://en.wikipedia.org/wiki/Mathematical_formulation_of_quantum_mechanics](https://en.wikipedia.org/wiki/Mathematical_formulation_of_quantum_mechanics)