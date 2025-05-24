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

## Quantum Gates as Rotations

- Gates = unitary transformations
- Example: Z gate, Hadamard gate
- Acts like rotation/reflection in complex space

## Amplitudes and Measurement

- Amplitude = vector component = vector coefficient if we are talking about the conventional Cartesian coordinates.
- Probability = squared magnitude: $ |\alpha|^2 $

## Tensor Products? Never heard that before!

In quantum algorithm, you will faced 
Tensor prouct.

- Combine states: $ |0\rangle \otimes |1\rangle = |01\rangle $
- Grows basis: $[2^n]$ space
- Manual computation:

```python
def tensor_product(a, b):
    return [ai * bj for ai in a for bj in b]

``` 

## Inner product

In Linear Algebra, Euclidean space, an inner product is 

$$
<v,w> = ||v|| \cdot ||w|| \cdot \text{cos} \theta
$$

If **$w$** is a unit vector, then

$$
<w,v> = \text{length of projection of} \ v \ \text{onto} \ w
$$

In Hilbert space $\mathbb{H}$, the inner product between quantum state $|\psi\rangle$ and $|\phi\rangle$ is:

$$
\langle \phi|\psi\rangle = \text{"overlap" or projection of} \ |\psi\rangle \ \text{onto} \ |\phi\rangle
$$

For example, let's say you have the state:

$$
|\psi\rangle = \frac{1}{\sqrt{5}} \begin{bmatrix} 2\\1 \end{bmatrix} = \frac{2}{\sqrt{5}}|0\rangle + \frac{1}{\sqrt{5}}|1\rangle,
$$

you want to find the projection onto $|0\rangle$.



### Amplitude
