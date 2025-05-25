# Measurement in Quantum mechanics

##  Positive operator-valued measure (POVM)

A quantum measurement can be described by a set of measurement operators ${M_{m}}$ acting on the Hilbert space. The probability of getting result $m$ when measuring a state $|\psi\rangle$" 

$$
p(m) = \langle \psi|M_{m}^{\dagger} M_{m}|\psi\rangle
$$

and measurement operators must satisfies 

$$
\sum_{m}M_{m}^{\dagger}M_{m} = I.
$$

Thus, $1 = \sum_{m}p_{m} = \langle \psi|M_{m}^{\dagger} M_{m}|\psi\rangle$. After the measurement, our state becomes 

$$
\frac{M_{m}|\psi\rangle}{\sqrt{\langle \psi|M_{m}^{\dagger}M_{m}|\psi\rangle}}.
$$

## Projective measurement
Projective measurement is a special case of general quantum measurement, where the set of measurement operators $\{P_{m}\}$ consists entirely of projectors.

Given a set of projectors $\{P_{m}\}$, we can define a corrsponding obervable (measurable physical quantity):

$$
M = \sum_{m} m P_{m}
$$

where $m$ are the eigenvalue (measurement result) and $P_{m}$ the projection opreator. A projector $P$ can be expressed using an orthonotmal basis $\{|i\rangle\}$ spanning some subspace $W \subset \mathcal{H}$

$$
P = \sum_{i=1}^{k}|i\rangle\langle i|
$$

> This means that $P$ projects any state vector onto the subspace $W$ spanned by the basis vectors $\{|i\rangle\}$.

The porjection operator has the foloowing properties 

$$
P_{m}^{\dagger} = P_{m}, \ P_{m}^{2} = P_{m}.
$$

The probability of obtaining outcome $m$ when measuring state $|\psi\rangle$ is: 

$$
p(m) = \langle \psi |P_{m}|\psi\rangle
$$

After the measurement, the system state collapses to

$$
|\psi'\rangle = \frac{P_{m}|\psi\rangle}{\sqrt{p(m)}}
$$

This is a normalized projection of the original state onto the eigenspace corresponding to $P_{m}$. The expected value of observable $M$ is

$$
\langle \psi \bigg(\sum_{m}mP_{m}\bigg)\psi\rangle  = \langle \psi |M|\psi\rangle 
$$

## References 

[1]. [https://en.wikipedia.org/wiki/Measurement_in_quantum_mechanics](https://en.wikipedia.org/wiki/Measurement_in_quantum_mechanics)

[2]. [https://en.wikipedia.org/wiki/Mathematical_formulation_of_quantum_mechanics](https://en.wikipedia.org/wiki/Mathematical_formulation_of_quantum_mechanics)

[3]. [https://en.wikipedia.org/wiki/Born_rule](https://en.wikipedia.org/wiki/Born_rule)

[4]. [https://en.wikipedia.org/wiki/POVM](https://en.wikipedia.org/wiki/POVM)