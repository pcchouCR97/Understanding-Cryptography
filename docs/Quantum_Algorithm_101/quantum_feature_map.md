# Quantum Feature Map

Let $\mathcal{X}$ be a set of input data, we have a quantum feature map such that 

$$
\mathcal{X} \rightarrow \phi \rightarrow \mathcal{F}
$$


In general, $\mathcal{F}$ is just a vector space. 

-   A quantum feature map $\phi: \mathcal{X} \rightarrow \mathcal{F}$ where $\mathcal{F}$ is a Hilbert space and the feature vectos are *quantum states*.
-   The map transforms $x \rightarrow |\phi(x)\rangle$ by way of a *unitary transformation $U_{\phi}(x)$*. Where $U_{\phi}(x)$ is a [variational circuit](../Math_Fundamentals/variational_circuit.md) whose parameters depend on the input data.


## Reference 
1. PENNYLANE - Quantum Feature Map: [https://pennylane.ai/qml/glossary/quantum_feature_map](https://pennylane.ai/qml/glossary/quantum_feature_map)