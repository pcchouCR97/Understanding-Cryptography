# Quantum Counting

So the question is how quickly can we determine the number of solutions, $M$, to an $N$ item search problem, if $M$ is not known in advance? Compare with classical computer, which requires $\Theta(N)$ consultations with an oracle to determin $M$, a quantum computer is able to estimate number of solutions much more quickly by combining Fourier transform.

Here are some key applications:

1.  If we can estimate the *number of solutions* quickly then it is also possible to find a solution quickly, even if the number of solutions is unknown. BY first counting the number of solutions, and then applying the quantum saerch algorithm to find a solution.

2.  To decide whether the solution even exist, depending on whether the numbert of solutions is zero, or non-zero. This is useful when we are facing NP-complete problems.

## Phase estimation

Quantum counting is an application of the phase estimation procedure to estimate the eigenvalues of the Grover iteration $G$. Suppose $|a\rangle$ and $|b\rangle$ are two eigenvectors of the Grover iteration in the space spanned by $|a\rangle$ and $|\beta\rangle$. Let $\theta$ be the angle of rotation determined by the Grover iteration. From equation,

$$G = 
\begin{bmatrix}
\text{cos}\theta & -\text{sin}\theta \\
\text{sin}\theta & \text{cos}\theta
\end{bmatrix}
$$

where $\theta$ is a real number in the range $0$ to $\pi/2$ (assuming for simplicity that $M\leq N/2$), where

$$
\text{sin}\theta = \frac{2\sqrt{M(N-M)}}{N},
$$

we see that the corresponding eigenvalues are $e^{i\theta}$ and $e^{i(2\pi - \theta)}$. We expoand our search space to $2N$ to ensure that $\text{sin}^{2}(\theta/2) = M/2N$.

## References 

[1]. M. A. Nielsen and I. L. Chuang, *Quantum Computation and Quantum Information*, 10th Anniversary Ed., Cambridge: Cambridge University Press, 2010.