## Constructing Quantum Circuit in Amplitude Estimation 

## Why This Quantum Circuit? Let's Start Over

You may wonder, in the paper, why we are using such a specific structure for our quantum circuit. Let’s step back and unpack the logic from the ground up.

### Measuring the Amplitude

In quantum mechanics, consider a single-qubit state of the form
$$
|\psi\rangle = \alpha |0\rangle + \beta |1\rangle
$$
where $\alpha, \beta \in \mathbb{C}$ and the normalization condition holds:
$$
|\alpha|^2 + |\beta|^2 = 1.
$$
According to the Born rule, the amplitude of the state $|1\rangle$ is $\beta$, but what we can actually *measure* is the probability:
$$
P(|1\rangle) = |\beta|^2.
$$
Probabilities must be real and non-negative. So we compute the square of the magnitude:
$$
P = |\text{amplitude}|^2 = (\text{Re}[\beta])^2 + (\text{Im}[\beta])^2.
$$
---

### Encoding $f(x)$ into a Quantum State

Now here’s the key idea. We want to encode a classical function $f(x)$ into a quantum circuit in such a way that we can extract information about it via measurement. Specifically, we want the measurement probability of a certain basis state (say $|1\rangle$) to be equal to $f(x)$.

So we aim to construct a quantum state such that:
$$
|\beta|^{2} = \sin^2(p(x)) = f(x)
$$
In this setup, $p(x)$ is some function that encodes $f(x)$ via the sine-squared map. Inverting this, we get:
$$
p(x) = \sin^{-1}\left(\sqrt{f(x)}\right)
$$
This is what we mean when we say "encoding" $f(x)$ — we are constructing a rotation angle $p(x)$ such that the probability of measuring $|1\rangle$ becomes exactly $f(x)$.

---

### But Why Polynomial Approximation?

The challenge is: arbitrary functions $p(x)$ are generally hard to implement in quantum circuits directly. Quantum gates are discrete, hardware has finite resolution, and arbitrary-angle rotations are not feasible.

So instead of directly implementing:
$$
p(x) = \sin^{-1}(\sqrt{f(x)})
$$
we approximate it with a low-degree polynomial — something we *can* construct in a circuit using basic gates.

That’s the whole idea: translate classical functions into quantum-measurable quantities through amplitude encoding, using inverse trigonometric maps and approximations that play nicely with quantum hardware.

---

### Ok Polynomial Approximation, but why it looks ugly?

You certainly will have this type of question, why its must be this form $c$