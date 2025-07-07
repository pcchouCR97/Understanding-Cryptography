# The Quantum Search Algorithm

## The Orcale

Suppose we wish to search through a search space of $N$ elements. Rather than search the elementes directly, we focus on the *index* to those elements, which is just a number in the range $0$ to $N-1$. For convenience we assume $N = 2^n$, so the index can be stored in $n$ bits, and that the search problem has exactly $M$ solutions, with $1 \leq M \leq N$. This can be represented by a function $f$, which takes as inpur an integer $x$, in the range $0$ to $N-1$. By definition, 

$$
f(x) = 
\begin{cases}
1 & \text{if } x \ \text{is a solution to the search problem} \\
0  & \text{if } x \ \text{is not a solution to the search problem}
\end{cases}
$$

!!! example
    For example, we have a search problem that has $N = 2^{3}$ elements, and a problem has $M = 2$ solutions of our interests, which meets the requiremnet $1 \leq M \leq N$. Then we encode our problem, by preparing $|x\rangle|0\rangle$, into a quantum circuit and run the oracle $\mathcal{O}$ and obtain solution $f(x = 2) = f(x = 4) = 1$.

Suppose we are supplied with a quantum *oracle* - a black box - with the ability to *recognize* solutions to the search problem. This recognition is signalled by making use of an *orcale qubit*. 

The orcale is a unitary operator, $O$, defined by its action on the computational basis:

$$
|x\rangle|q\rangle \xrightarrow{\mathcal{O}} |x\rangle|q \oplus f(x)\rangle,
$$

where $|x\rangle$ is the index register, $\oplus$ denotes addition modulo 2, and the orcale qubit $|q\rangle$ is a single qubit which is flipped if $f(x)=1$, and is unchanged otherwise.

In the quantum search algorithm it is useful to apply the orcal with the oracle qubit initially in the state $\frac{|0\rangle - |1\rangle}{\sqrt{2}}$, just as was donw in the Deutsch-Jozsa algorithm. If $x$ is not a solution to the search problem, applying the oracle to the state $|x\rangle\frac{|0\rangle - |1\rangle}{\sqrt{2}}$ doesn't change the state. On the other hand, if $x$ is a solution to the seach problem, then $|0\rangle$ and $|1\rangle$ are interchanged by the action of the oracle, giving the final state $-|x\rangle\frac{|0\rangle - |1\rangle}{\sqrt{2}}$. The action of the oracle is thus:

$$
|x\rangle\bigg(\frac{|0\rangle - |1\rangle}{\sqrt{2}} \bigg) \xrightarrow{\mathcal{O}} (-1)^{f(x)} |x\rangle\bigg(\frac{|0\rangle - |1\rangle}{\sqrt{2}} \bigg),
$$

where you can easily see that if solution, term becomes $-|x\rangle\bigg(\frac{|0\rangle - |1\rangle}{\sqrt{2}} \bigg)$. Notice that the state of the oracle qubit is not changed and can therefore be omitted from further discussion of the algorithm. Thus, for simplicity, we write:

$$
|x\rangle \xrightarrow{\mathcal{O}} (-1)^{f(x)}|x\rangle.
$$

We say that the oracle *marks* the solutions to the search problem, by shifting the phase of the solution. For an $N$ item search problem with $M$ solutions, it turns out that we need only apply the search oracle $O(\sqrt{N/M})$ times to obtain a solution on a quantum computer.

## The procedure 

The internal working of the oracle, including the possibility of it needing extra work qubits, are not important to the description of the quantum search algorithm proper. The goal of the algorithm is to find a solution to the search problem, using the smallest possible number of applications of the orcale. 

The algorithm begines with the computer in the state $|0\rangle^{\otimes n}$. The `H` gate is used to put the computer in the equal superposition state,

$$
|\psi\rangle = \frac{1}{N^{1/2}}\sum_{x=0}^{N-1}|x\rangle.
$$

or 

$$
|0\rangle^{\otimes n} \xrightarrow{H^{\otimes n}} \frac{1}{\sqrt{2^{n}}}\sum_{x=0}^{2^{n}-1}|x\rangle.
$$

> Quantum computing exploits quantum parallelism — the ability to evaluate a function on many inputs simultaneously using superposition. And that's why we put qubits in an equal superposition.

The quantum search algorithm then consists of repeated application of a quantum subroutine, known as a *Grover operator*, which we denote $G$. The Grover iteration may broken up into four steps.

1.  Apply the oracle $O$.
2.  Apply the `H` transform $H^{\otimes n}$.
3.  perform a conditional phase shift on the computer, with every computational basis state except $|0\rangle$ receiving a phase shift of $-1$,

    $$
    |x\rangle \rightarrow -(-1)^{\delta_{x}0}|s\rangle.
    $$
4.  Apply the `H` transform $H^{\otimes n}$. 