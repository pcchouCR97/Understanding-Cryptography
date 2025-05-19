# Variational Circuit

*Variational circuits are also known as "parameterized quantum circuits"*

## Adaptable quantum circuits
Variational quantum circuit are "quantum algorithm" that depend on free parameters. Like standard quantum circuits, they consist of three ingredients:

-   Preparation of fixed **initial state**.
-   A quantum circuit $U(\theta)$, parameterized by a set of free parameters $\theta$.
-   *Measurement* of an observable $\widehat{B}$ at the output. 

The expectation values $f(\theta) = \langle 0|U^{\dagger}(\theta)\widehat{B}U(\theta)|0\rangle$ defines a scalar cost for a given task. The free parameters $\theta = (\theta_{1}, \theta_{2}, \cdots)$ of the citcuit(s) are tuned to optimzed this cost function.

Variational circuits are trained by a classical optimization algorithm that makes queries to the quantum device.

---
*Variational circuits have become popular as a way to think about quantum algorithms for near-term quantum devices. Such devices can only run short gate sequences, since without fault tolerance every gate increases the error in the output. Usually, a quantum algorithm is decomposed into a set of standard elementary operations, which are in turn implemented by the quantum hardware.*

*The intriguing idea of variational circuit for near-term devices is to merge this two-step procedure into a single step by "learning" the circuit on the noisy device for a given task. This way, the "natural" tunable gates of a device can be used to formulate the algorithm, without the detour via a fixed elementary gate set. Furthermore, systematic errors can automatically be corrected during optimization.*

## Building circuit
The variational parameters with a set of non-adaptable parameters $x = (x_{1},x_{2},\cdots)$ enter the quantum circuit as arguments for the circuit gates. 

-   This allows to convert "classical information ($\theta$ and $x$)" into quantum informtaion ($|U(x;\theta)|0\rangle$). 
-   The non-adaptable parameter usually plays a role of data inputs in quantum machine learning.

*"Quantum information"* is turned "back" into classical information by ==*evaluating the expectation value of the observable $\widehat{B}$.*==

$$
\begin{array}{ll}
f(x;\theta) & = \langle\widehat{B}\rangle\\
\ & = \langle 0|U^{\dagger}(x;\theta)\widehat{B}U(x;\theta)|0\rangle
\end{array}
$$

---

## Reference 
1. PENNYLANE - Variational Circuit: [https://pennylane.ai/qml/glossary/variational_circuit](https://pennylane.ai/qml/glossary/variational_circuit)