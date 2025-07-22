# Phase kick-back

!!! info "Takeaway"
    Phase kickback is a quantum mechanical effect where applying a controlled gate transfers the phase from a target qubit to a control qubit. This happens when the target qubit is in an eigenstate of the unitary operation being applied.

## How It Works

The core principle of phase kickback relies on the setup of a controlled unitary operation (controlled-$U$ gate). This gate applies the unitary operator U to the target qubit only if the control qubit is in the state $|1\rangle$.

Let's represent the control qubit state as $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$  and the target (*ancilla*) qubit as $|t\rangle$. The combined initial state is:

$$
|\psi_{\text{in}}\rangle = (\alpha|0\rangle + \beta|1\rangle) \otimes |t\rangle = \alpha|0\rangle|t\rangle + \beta|1\rangle |t\rangle
$$

Applying the controlled-$U$ gate gives:

$$
|\psi_{\text{out}}\rangle = \alpha|0\rangle|t\rangle + \beta|1\rangle(U|t\rangle)
$$

Now, in order to perfrom the phase kick back, **the target qubit $|t\rangle$ must be the eigenstate of the operator $U$**. ==(An eigenstate is a state that, when acted upon by an operator, only changes by a scalar factor called the eigenvalue.)== For a unitary operator, this eigenvalue is of the form $e^{i\phi}$.

So, let's assume $U|t\rangle = e^{i\phi}|t\rangle$. We substitute this into our equation:

$$
|\psi_{\text{out}}\rangle = \alpha|0\rangle|t\rangle + \beta|1\rangle(e^{i\phi}|t\rangle)
$$

We can now factor out the target qubit state $|t\rangle$, which is unchanged:

$$
|\psi_{out}\rangle = (\alpha|0\rangle|t\rangle + e^{i\phi}\beta|1\rangle)|t\rangle
$$

Notice that 

-   The target qubit $|t\rangle$ is completely unaffected.
-   The control qubit state has changed from $\alpha|0\rangle|t\rangle + \beta|1\rangle$ to $\alpha|0\rangle|t\rangle + e^{i\phi}\beta|1\rangle$. A relative phase of $e^{i\phi}$ has been "kickback" from the target to the control qubit.

## Example - CNOT gate 

Let's consider a CNOT gate where the unitary operation is the Pauli-X gate `X`. We know that `X` has two eigenstates:

*   $|+\rangle = \frac{|0\rangle + |1\rangle}{\sqrt{w}}$, with eigenvalue $e^{i0} = +1$.
*   $|-\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{w}}$, with eigenvalue $e^{i\pi} = -1$.

Let's set state $|-\rangle$ as our target qubit. We are expecting to get $X|-\rangle = -|-\rangle$ when we apply `X` on state $|-\rangle$ due to its eigenvalue. First we set our initial state as  

$$
|\psi_{\text{in}}\rangle = |+\rangle|-\rangle
$$

the we apply the $CNOT$ gate,

$$
\begin{array}{ll}
CNOT(|+\rangle|-\rangle) & = CNOT \bigg(\frac{|0\rangle + |1\rangle}{\sqrt{2}} \otimes |-\rangle \bigg)\\
\ & = \frac{1}{\sqrt{2}}\bigg(CNOT|0\rangle|-\rangle + CNOT|1\rangle|-\rangle\bigg),
\end{array}
$$

remember that $CNOT$ clips the target if the control is $|1\rangle$. Thus,

-   For the $|0\rangle|-\rangle$ part, the control is $|0\rangle$, so nothing happens.
-   For the $|1\rangle|-\rangle$ part, the control is $|1\rangle$, so the `X` gate is applied to the target: $|1\rangle(X|-\rangle) = |1\rangle(-|-\rangle) = -|1\rangle|-\rangle$.

thus the final state will be 

$$
\begin{array}{ll}
|\psi_{\text{out}}\rangle & = \frac{1}{\sqrt{2}}\bigg(|0\rangle|-\rangle-|1\rangle|-\rangle\bigg) = \bigg(\frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)\bigg) \otimes |-\rangle\\
\ & = |-\rangle|-\rangle
\end{array}
$$

==The phase eigenvalue of $-1$ from the target qubit was kicked back, flipping the control qubit from $|+\rangle$ to $|-\rangle$, while the target qubit remained in the $|-\rangle$ state.==
