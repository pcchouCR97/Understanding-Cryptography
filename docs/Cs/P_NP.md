# P and NP Problems

In computational complexity, we classify problems based on how efficiently they can be solved or verified.

## P(Polynomial time): Polynomial-Time Solvable 

The class **P** consists of problems that can be solved in polynomial time. That is, there exists an algorithm whose runtime is bounded by $O(n^k)$ for some constant $k$, where $n$ is the size of the input.

> Example: Checking whether a number is even — this is solvable directly and efficiently, so it belongs to **P**.

## NP(Nondeterministic Polynomial time): Polynomial-Time Verifiable

A problem is in **NP** if, for every "yes" instance, there exists a **witness** (or certificate) such that the correctness of the answer can be verified in polynomial time. You might not know how to find the solution efficiently, but if someone gives you a proposed answer, you can verify it quickly.

> Example: Determining whether a large number has a small factor — solving this is hard, but given a factor, you can verify it quickly using multiplication.

## Witness

A **witness** is evidence that certifies a "yes" answer. In NP problems, it's the input that helps confirm the solution.

## Quantum Complexity and Examples

-   **Shor’s algorithm** solves the factoring problem in polynomial time on a quantum computer. Since factoring is in **NP**, and Shor's algorithm solves it efficiently, it also lies in **BQP** (Bounded-Error Quantum Polynomial time). However, factoring is *not* known to be NP-complete.

-   **Grover’s algorithm** provides a quadratic speedup for unstructured search, reducing $O(N)$ to $O(\sqrt{N})$, but this still remains exponential in $\log N$, so it does not solve NP-complete problems efficiently.

## Interpretation

-   If solving a problem takes $O(n^3)$, it belongs to **P**.
-   If verifying a solution takes $O(n^2)$, but finding it is hard, the problem lies in **NP**.

In short, the distinction between **P**, **NP**, and **BQP** is not about how to solve problems directly, but about how to classify their **difficulty** — whether solving or verifying solutions can be done efficiently.

## References 

[1] M. A. Nielsen and I. L. Chuang, *Quantum Computation and Quantum Information*, 10th Anniversary Ed., Cambridge: Cambridge University Press, 2010.