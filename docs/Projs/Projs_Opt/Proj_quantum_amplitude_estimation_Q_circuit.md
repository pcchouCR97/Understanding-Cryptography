## Constructing Quantum Circuit in Amplitude Estimation 

## Why This Quantum Circuit? Let's Start Over

You may wonder, in the Paper [[1].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference), why we are using such a specific structure for our quantum circuit. Let’s step back and unpack the logic from the ground up.

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

The form 
$$
c(f(x)-\frac{1}{2})+\frac{1}{2}
$$
is choosen carefully. We want to measure using $\text{sin}^{2}$, so we manipulate the sine function to behave linearly. Let's start with the identity of $\text{sin}^{2}(\theta)$:
$$
\text{sin}^{2}(\theta) = \frac{1-\text{cos}(2\theta)}{2}
$$
therefore,
$$
\text{sin}^{2}(y+\frac{\pi}{4}) = \frac{1-\text{cos}(2y+\frac{\pi}{2})}{2}
$$
and you must know that $\text{cos}(2y+\frac{\pi}{2}) = -\text{sin}(2y)$, therefore
$$
\text{sin}^{2}(\theta) = \frac{1-\text{cos}(2\theta)}{2} = \frac{1+\text{sin}(2y)}{2}.
$$
Now we expand $\text{sin}(2y)$ at $y=0$ using Taylor series expansion:
$$
\text{sin}(2y) = 2y - \frac{(2y)^{3}}{3!} + \frac{(2y)^{5}}{5!} - \cdots = 2y - \frac{4y^{3}}{3} + \frac{8y^{5}}{15} - \cdots
$$
therefore
$$
\frac{1+\text{sin}(2y)}{2} = \frac{1}{2} + y - \frac{2y^{3}}{3} + \frac{4y^{5}}{15}.
$$
now we only consider the linear term and omit the high order term, we have 
$$
\frac{1+\text{sin}(2y)}{2} = \frac{1}{2} + y
$$
By replacing $y$ into our $f$, we approximate 
$$
\frac{1+\text{sin}(2y)}{2} = \frac{1}{2} + y = f(x) + \frac{1}{2}
$$
### Ok but why do we shift function 
You might wonder — doesn't shifting by $\frac{1}{2}$ push things into a nonlinear region?

Actually, we do it on purpose.

Taylor expansion is a **local** approximation — it's most accurate around the point it's expanded at.  
Since we expand $\sin^2(y + \frac{\pi}{4})$ around $y = 0$, we need the input to stay close to 0.

But in our case, $f(x) \in [0, 1]$, so we center it by subtracting $\frac{1}{2}$:
$$
f(x) - \frac{1}{2} \in [-0.5, 0.5]
$$
Then, we scale with a small constant $c$ to shrink the range:
$$
c(f(x) - \frac{1}{2}) \in [-0.5c, 0.5c]
$$
This keeps the input near 0 and improves the accuracy of the Taylor expansion.

Finally, we add $\frac{1}{2}$ back to ensure the result is still within $[0,1]$.

<div style="text-align: center;">
    <img src="../../Projs_Opt/images/Taylor.png" alt="Taylor approximation comparison" style="width: 480px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
    Linearization of \(\sin^2(y + \pi/4)\) near \(y=0\)
    </p>
</div>

---
Now you may understand the key poltnominal approximation trick in paper [[1].](../../Projs/Projs_Opt/Proj_quantum_amplitude_estimation.md#reference).


## Reference
1. Quantum Risk Analysis