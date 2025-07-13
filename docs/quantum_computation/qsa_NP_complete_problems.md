# Quadratic Speedup for NP-Complete Problems

In this chapter, we show how quantum search can assist in solving the Hamiltonian cycle problem.

The Hamiltonian cycle problem asks whether a given graph contains a cycle that visits each vertex exactly once. This problem belongs to the class of **NP-complete** problems. Solving it involves searching through all possible orderings of the graph’s vertices:

1. Generate each possible ordering $(v_{1}, ..., v_{n})$ of the vertices. Repetitions are allowed to simplify the analysis without changing the essential result.
2. For each ordering, check whether it forms a Hamiltonian cycle. If not, continue checking the remaining orderings.

Since there are $n^n = 2^{n\log n}$ possible orderings, the classical algorithm requires $2^{n\log n}$ checks in the worst case.

## How Quantum Search Speeds Up NP Problems

A quantum algorithm can accelerate this process by speeding up the search. We use the method in [quantum counting](../quantum_computation/qsa_quantum_counting.md) to determine whether a solution to the search problem exists.

Let:

-   $n$: the number of vertices in the graph  
-   $m = \lceil \log n \rceil$: the number of qubits needed to represent a single vertex index

To find a Hamiltonian cycle, we consider all sequences of $n$ vertex labels, requiring $nm$ qubits. The computational basis state can be written as:

$$
|v_1, v_2, ..., v_n\rangle
$$

where each $|v_i\rangle$ is a string of $m$ qubits.

The oracle for the search algorithm applies the transformation:

$$
O|v_1, ..., v_n\rangle =
\begin{cases}
\ \ |v_1, ..., v_n\rangle & \text{if } (v_1, ..., v_n) \text{ is not a Hamiltonian cycle} \\
- |v_1, ..., v_n\rangle & \text{if } (v_1, ..., v_n) \text{ is a Hamiltonian cycle}
\end{cases}
$$

Given a description of the graph, we can construct a polynomial-size classical circuit that checks for Hamiltonian cycles. This circuit can be converted into a reversible one (still polynomial in size) that performs:

$$
(v_1, ..., v_n, q) \mapsto (v_1, ..., v_n, q \oplus f(v_1, ..., v_n)),
$$

where $f(v_1, ..., v_n) = 1$ if the sequence is a Hamiltonian cycle, and $0$ otherwise.

We initialize $q$ to $|1\rangle$ and apply a Hadamard gate to create the superposition state:

$$
\frac{|0\rangle - |1\rangle}{\sqrt{2}}
$$

This phase kickback enables the oracle to mark solutions.

Using [quantum counting](../quantum_computation/qsa_quantum_counting.md), we can determine whether a Hamiltonian cycle exists using only $O(2^{mn/2})$ applications of the oracle — giving a **quadratic speedup** over brute-force search.


To summerize:

$$
\begin{array}{|l|l|l|}
\hline
\textbf{Feature} & \textbf{Classical Algorithm} & \textbf{Quantum Algorithm} \\
\hline
\text{Time Complexity} & O(p(n) \cdot 2^{n \lceil \log n \rceil}) & O(p(n) \cdot 2^{n \lceil \log n \rceil / 2}) \\
\hline
\text{Oracle Cost} & \text{Polynomial } p(n) & \text{Polynomial } p(n) \\
\hline
\text{Success Probability} & 1 \text{ (deterministic)} & \text{At least } 5/6,\ \text{boosted by repeats} \\
\hline
\text{Exponential Term} & 2^{n \lceil \log n \rceil} & 2^{n \lceil \log n \rceil / 2} \\
\hline
\text{Error Handling} & \text{Not needed} & \text{Repeat } r\ \text{times} \Rightarrow \text{error } 1/6^r \\
\hline
\end{array}
$$

## References 

[1]. M. A. Nielsen and I. L. Chuang, *Quantum Computation and Quantum Information*, 10th Anniversary Ed., Cambridge: Cambridge University Press, 2010.
