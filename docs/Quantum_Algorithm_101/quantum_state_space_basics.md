# Quantum State and Space Basics

Quantum computing is linear algebra in a complex vector space. This guide builds intuition through geometric and vector-based thinking.

## Don’t Overthink "Superposition" - Hilbert Space and State Vectors

> A quantum state is just a normalized vector, like $[x, y]^T$ in 2D Cartesian coordinates, but in complex space.

We can write a quantum state as:

$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle, \quad \text{with } |\alpha|^2 + |\beta|^2 = 1.
$$

**where $|\psi\rangle$, $|0\rangle$, and $|1\rangle$ are vectors**. very easy and simple. just like in 2D Cartesian coordinates. In quantum algorithms, we define:

$$
|0\rangle = 
\begin{bmatrix}
1\\0
\end{bmatrix}, \quad 
|1\rangle = 
\begin{bmatrix}
0\\1
\end{bmatrix}
$$

<div style="text-align: center;">
    <img src="../../images/state_vector_basic.jpg" alt="state_vector_basic" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        State vector representation
    </p>
</div>

We use **big-endian** convention, where the state $|0\rangle$ corresponds to index 0 (first), and $|1\rangle$ to index 1 (second). Similarly, if we have the basis states $|0\rangle, |1\rangle, |2\rangle, |3\rangle$, they map to binary-labeled states $|00\rangle, |01\rangle, |10\rangle, |11\rangle$, corresponding to indices 0 to 3:

$$
|0\rangle = 
\begin{bmatrix}
1\\0\\0\\0
\end{bmatrix}, \quad 
|1\rangle = 
\begin{bmatrix}
0\\1\\0\\0
\end{bmatrix}, \quad 
|2\rangle = 
\begin{bmatrix}
0\\0\\1\\0
\end{bmatrix}, \quad 
|3\rangle = 
\begin{bmatrix}
0\\0\\0\\1
\end{bmatrix}
$$

Let’s go back to our original idea.

**Put simply**:

> Superposition describes the position of the state vector in the 1-qubit, 2D computational basis space (spanned by $|0\rangle, |1\rangle$).

> Don’t overthink “superposition.” It’s just like describing a vector in regular Cartesian coordinates — but now the axes are $|0\rangle$ and $|1\rangle$; $\alpha$ and $\beta$ are just vector coefficients, a.k.a, amplitude in quantum language!


## Don't be fooled by "Computational basis" - Different Representation in Quantum Algorithm

As you dig into the quantum algorithm, you will constantly hear a term called **computational basis**. Don't be fooled by the name, it is just as similar as $X,Y$ axes in our friend Cartesian coordinates. We can describe this **computational basis** using two methods

-   Binary representation: $|00\rangle,|01\rangle,|10\rangle,|11\rangle$
-   Decimal representation: $|0\rangle,|1\rangle,|2\rangle,|3\rangle$

They are jsut different labes for the same computational basis vectors.

## Tensor Products? Never heard that before!

In quantum algorithm, you will have to deal with tensor product, which you will probally never heard this before. or maybve you just hear of inner product. But here is the thing. Tensor product in Hilbert space:


$$
\mathcal{H}_{A} \otimes \mathcal{H}_{B}
$$

combines two quantum system into a joint space. It's like stacks coordinates, no interaction or entanglement.

Let's do some exampels, 

$$
|0\rangle = 
\begin{bmatrix}
1\\0
\end{bmatrix}, \
|1\rangle = 
\begin{bmatrix}
0\\1
\end{bmatrix}
$$

we do the tensor product $|0\rangle \otimes |0\rangle$ and $|0\rangle \otimes |1\rangle$

$$
\begin{bmatrix}
1\\0
\end{bmatrix}
\otimes 
\begin{bmatrix}
1\\0
\end{bmatrix}
=
\begin{bmatrix}
1 \cdot 1 \\
1 \cdot 0 \\
0 \cdot 1 \\
0 \cdot 0 \\
\end{bmatrix}
=
\begin{bmatrix}
1\\0\\0\\0
\end{bmatrix}
= |00\rangle
$$

$$
\begin{bmatrix}
1\\0
\end{bmatrix}
\otimes 
\begin{bmatrix}
0\\1
\end{bmatrix}
=
\begin{bmatrix}
1 \cdot 1 \\
1 \cdot 1 \\
0 \cdot 0 \\
0 \cdot 1 \\
\end{bmatrix}
=
\begin{bmatrix}
0\\1\\0\\0
\end{bmatrix}
= |01\rangle
$$

A basic idea of tensor product of column vectors. Let

$$
u = 
\begin{bmatrix}
u_{1}\\ u_{2}
\end{bmatrix}, \
v = 
\begin{bmatrix}
v_{1}\\ v_{2}
\end{bmatrix}
$$

then 

$$
u\otimes v = 
\begin{bmatrix}
u_{1} \cdot v_{1} \\
u_{1} \cdot v_{2} \\
u_{2} \cdot v_{1} \\
u_{2} \cdot v_{2} \\
\end{bmatrix}
\in \mathbb{C}^{4\times 1}
$$

> Tensor of two $n \times 1$ vectors $\rightarrow$ $n^{2} \times 1$. The dimensional expands via [Kronecker product](../Quantum_Algorithm_101/kronecker_product.md).

The python realization will be:

```python
def tensor_product(a, b):
    return [ai * bj for ai in a for bj in b]

``` 

## Inner product

In Linear Algebra, Euclidean space, an inner product is 

$$
\langle v,w\rangle = ||v|| \cdot ||w|| \cdot \text{cos} \theta
$$

If **$w$** is a unit vector, then

$$
\langle w,v\rangle = \text{length of projection of} \ v \ \text{onto} \ w
$$

In Hilbert space $\mathbb{H}$, the inner product between quantum state $|\psi\rangle$ and $|\phi\rangle$ is:

$$
\langle \phi|\psi\rangle = \text{"overlap" or projection of} \ |\psi\rangle \ \text{onto} \ |\phi\rangle
$$

For example, let's say you have the state:

$$
|\psi\rangle = \frac{1}{\sqrt{5}} \begin{bmatrix} 2\\1 \end{bmatrix} = \frac{2}{\sqrt{5}}|0\rangle + \frac{1}{\sqrt{5}}|1\rangle,
$$

you want to find the projection onto $|0\rangle$. Next we compute

$$
\langle 0|\psi \rangle = [1 \ 0] \cdot \frac{1}{\sqrt{5}} \begin{bmatrix} 2\\1 \end{bmatrix} = \frac{1}{\sqrt{5}} \cdot (1 \cdot 2 + 0 \cdot 1) = \frac{2}{\sqrt{5}}
$$

and we want to know the probability of measuing $|0\rangle$

$$
|\langle 0|\psi\rangle|^{2} = \frac{4}{5}
$$

where $\langle0|\psi\rangle$ is the projection amplitude and $|\langle 0|\psi\rangle|^{2}$ is the porbability of observing $|0\rangle$.

Methematically, 

$$
\text{Amplitude along} \ v = \langle v, \psi \rangle
$$

In quantum mechanics:

-   $\langle v_i|\psi \rangle$ is the amplitude of $|\psi\rangle$ in direction $|v_i \rangle$
-   $|\langle 0|\psi\rangle|^{2}$ is the probability of measuring in tat basis


Let's compare a

## Observable
In quantum mechanics, an observable tells you *what* you are measuring, for example, energy, spin, or position. It defines which basis (eigenvector) you are projecting into and what outcome (eigenvalue) you can get.

> Measuring state $|\psi\rangle$ using observable with eigenvector $|0\rangle$ means computing $\langle 0 | \psi\rangle$. This is the projection of $\psi\rangle$ onto axis $|0\rangle$ with probability = $|\langle 0 |\psi\rangle|^{2}$

For an easy example, lets say you want to measure a room temperature and we will need a thomometer. The temperature is the *observable* in quantum analogy, and the *measurement process* is the tool. The observable is the operator (e.g. Hamiltonian) defining what you measure.


## Quantum Gates as Rotations

> Quantum gates = unitary matrices, like rotation matrics in Euclidean geometry - but in complex hilbert space.

When you talk about the rotation xyz, there are no xyz in hilbert space, its all computational basis?



- Gates = unitary transformations
- Example: Z gate, Hadamard gate
- Acts like rotation/reflection in complex space

## References 

[1]. M. A. Nielsen and I. L. Chuang, *Quantum Computation and Quantum Information*, 10th Anniversary Ed., Cambridge: Cambridge University Press, 2010.