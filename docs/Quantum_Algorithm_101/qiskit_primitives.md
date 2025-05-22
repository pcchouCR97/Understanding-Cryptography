# What are Qiskit primitives?


*   `Statevectorsampler`: can sample the output quantum state from the quantum circuit in the computational basis. For example, if we have a circuit  represented by a unitary $U$, `Statevectorsampler` can sampler $U|0\rangle$.

*   `StatevectorEstimator`: can estimate expectation values of observables with respect to the output quantum state of the quantum circuit. For instance, if we have a circuit represented by a unitary $U$ and an observale $O$ we'd like to measure, `StatevectorEstimator` calculates $\langle 0|U^{\dagger}OU|0\rangle$.



---

## Reference 
1. PENNYLANE - Qiskit primitives: [https://pennylane.ai/qml/glossary/what-are-qiskit-primitives](https://pennylane.ai/qml/glossary/what-are-qiskit-primitives)
2. Qiskit 101