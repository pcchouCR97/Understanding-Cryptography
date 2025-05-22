# Matrices

## Column and Row vector
Here are our definitions of a column vector and a row vector:

$$
\text{column vector} = |y\rangle = 
\begin{bmatrix}
a_1 \\
a_2 \\
\vdots\\
a_n
\end{bmatrix}
$$

$$
\text{row vector} = \langle x| = 
\begin{bmatrix}
a_1,a_2,\cdots,a_n
\end{bmatrix}.
$$

The vector and row vectors can be represented by **one-dimenstional** matrices.

## Multiplying Vectors

A bracket $\langle x|y\rangle$, is essentially matrix multiplication of a row vector and a column vector. Here is our definition

$$
\langle x|y\rangle = 
\begin{bmatrix}
x_1,x_2,\cdots,x_n
\end{bmatrix}
\begin{bmatrix}
y_1 \\ y_2 \\ \vdots\\ y_n
\end{bmatrix}
=x_{1}\cdot y_{1}+x_{2}\cdot y_{2}+\cdots x_{n}\cdot y_{n}
$$

for example,

$$
\langle y|x\rangle = 
\begin{bmatrix}
3 \ 2 \ 1 \ 4
\end{bmatrix}
\begin{bmatrix}
1 \\ 2 \\ 3 \\
4
\end{bmatrix}
=16
$$

## Matrix-vector multiplication 

If the matrix and column vector are variables, we can write out the product this way:
$$
A|x\rangle
$$
This denotes the matrix $A$ multiplying the vector $|x\rangle$.

Let's say we have 

$$
A|x\rangle = 
\begin{bmatrix}
4 & 3 & 2\\ 4 & 1 & 3\\ 2 & 4 & 2
\end{bmatrix}
\cdot
\begin{bmatrix}
w\\ y\\ z
\end{bmatrix}
= 
\begin{bmatrix}
a\\ b\\ c
\end{bmatrix}, \quad
x = 
\begin{bmatrix}
w\\ y\\ z
\end{bmatrix}
$$

we can seperate matrix $A$ into row vectors

$$
\begin{array}{c}
\begin{bmatrix}
4 & 3 & 2
\end{bmatrix} = \langle R_{1}|, \quad
\begin{bmatrix}
4 & 3 & 3
\end{bmatrix} = \langle R_{2}|, \quad
\begin{bmatrix}
2 & 4 & 2
\end{bmatrix} = \langle R_{3}|. \quad
\end{array}
$$

and we can put things together as 

$$
A|x\rangle = 
\begin{bmatrix}
4 & 3 & 2\\ 4 & 1 & 3\\ 2 & 4 & 2
\end{bmatrix}
\cdot
\begin{bmatrix}
w\\ y\\ z
\end{bmatrix}
=
\begin{bmatrix}
\langle R_{1}|x\rangle \\ \langle R_{2}|x\rangle\\ \langle R_{3}|x\rangle
\end{bmatrix}
$$

In the same fashion, if we perform the product on the two following matrices $A$ and $B$ such that 

$$
A = 
\begin{bmatrix}
1 & 2 \\ 3 & 4
\end{bmatrix}
\quad
B = 
\begin{bmatrix}
1 & 2 & 3 \\ 4 & 5 & 6.
\end{bmatrix}
$$

Same, we rewrite matirx $A$ into row vectors $\langle R_{1}|,\langle R_{2},\langle R_{3}|$ and $B$ into column vectors $|C_{1}\rangle, |C_{2}\rangle, |C_{3}\rangle$. We can have the product of $A$ and $B$ matrix as 

$$
A \cdot B = 
\begin{bmatrix}
\langle R_{1}|C_{1}\rangle & \langle R_{1}|C_{2}\rangle & \langle R_{1}|C_{3}\rangle \\
\langle R_{2}|C_{1}\rangle & \langle R_{2}|C_{2}\rangle & \langle R_{2}|C_{3}\rangle
\end{bmatrix}
$$

## Quantum example

in quantum circuits, the inputs are qubits (vectors), and the gates are matrices. An example quantum logic gate, `NOT gate`, is show as 

The `NOT` gate in quantum copmuting is represented by the following matrix:

$$
X =
\begin{bmatrix}
0 & 1 \\ 1 & 0
\end{bmatrix}
$$

if our input qubits is a $|1\rangle$, then the output would be:

$$
X|1\rangle = 
\begin{bmatrix}
0 & 1 \\ 1 & 0
\end{bmatrix}
\begin{bmatrix}
0 \\ 1
\end{bmatrix}
=
\begin{bmatrix}
1 \\ 0
\end{bmatrix}
= |0\rangle
$$

## Hilbert space quantum 

## Hermitian matrix

A **Hermitian matrix** is a complex square matrix that is equal to its own conjugate transpose - that is, the element in the $i$-th row and $j$-th column is equal to the complex conjugate of the element in the $j$-th row and $i$-th column, for all indices $i$ and $j$. A is a Hermitian matrix $A$ contains elements $a_{ij}$, then 

$$
a_{ij} = \overline{a_{ij}}
$$

or in a quantum mechanics notation

$$
A^{H} = \overline{A^{T}} = A^{\dagger} .
$$

For example, we have a complex square matrix $A$ such that

$$
A = 
\begin{bmatrix}
0 & a-ib & c-id \\
a+ib & 1 & m-in \\
c+id & m+in & 2
\end{bmatrix}
$$

we apply transpose, it becomes

$$
{A^{T}} =
\begin{bmatrix}
0 & a+ib & c+id \\
a-ib & 1 & m+in \\
c-id & m-in & 2
\end{bmatrix} 
$$

then we find its cimplex conjugate, we have 

$$
\overline{A^{T}} =
\begin{bmatrix}
0 & a-ib & c-id \\
a+ib & 1 & m-in \\
c+id & m+in & 2
\end{bmatrix} 
= A^{\dagger} = A
$$

### Diagonal values 
The entries on the main diagonal of any Hermitian matrix are real.

### Symmetric 
A matrix that has only real entries is symmetric if and only if it is a Hermitian matrix. A real and symmetric matrix is simply a special case of a Hermitian matrix.

### Normal 
Every Hermitian matrix is a normal matrix. $AA^{\dagger} = A^{\dagger}A$.

### Properties
For any Hermitian matrix $A$ and $B$, we have 

1. $(A+B)_{ij} = A_{ij}+B_{ij} = \overline{A_{ij}}+\overline{B_{ij}} = \overline{A+B}_{ij}$ 
2. For an invertible Hermitian matrix. $A^{-1} = (A^{-1})^{H}$


## Normal matrix 
A complex square matrix $A$ is **normal**if it commutes with its conjugate transpose $A^{*}$. 

$$
A^{*}A = AA^{*}
$$

## Conjugate transpose

To maintain the consistancy, symbol $H$ has been replaced to $\dagger$.

A square matrix $A$ with entries $a_{ij}$ is called 

1. Hermitian or self-adjoint if $A = A^{\dagger}$; i.e., $a_{ij} = \overline{a_{ji}}$.
2. Skew Hermitian if $A = -A^{\dagger}$; i.e., $a_{ij} = -\overline{a_{ji}}$.
3. Normal if $A^{\dagger}A = AA^{\dagger}$.
4. Unitary if $A^{\dagger} = A^{-1}$; i.e., $AA^{\dagger} = A^{\dagger}A = I$.

## Unitary matrix 

In linear algebra, an invertible complex square matrix $U$ is **unitary** if its inverse matrix $U^{-1}$ equals its conjugate transpose $U^{*}$, that is, if 

$$
U^{*}U = UU^{*} = I
$$

In quantum, we use $\dagger$, that is

$$
U^{\dagger}U = UU^{\dagger} = I
$$

For real numbers, the analogue of a unitary matrix is an orthogonal matrix. Unitary matrices have significant importance in quantum mechanics because they preserve norms, and thus, probability.

## Orthogonal matrix
 
In liner algrbra, an **orthogonal matrix**, or **orthonormal matrix**, is a real square matrix whose columns and rows are orthonormal vectors.

Also, it holds that 

$$
Q^{T}Q = QQ^{T} = I
$$

where $Q^{T}$ is the transpose of $Q$ and $I$ is the identity matrix. As a result, it also holds that

$$
Q^{T} = Q^{-1}.
$$

## Orthonormal basis 
An orthonormal basis is a set of vectors have a norm of 1 and are pairwise orthogonal. [ref](matrices.md#references)

## References:
1.  Woody III, L. S. (2021). Essential mathematics for quantum computing. Packt Publishing. [https://www.packtpub.com/en-us/product/essential-mathematics-for-quantum-computing-9781801070188](https://www.packtpub.com/en-us/product/essential-mathematics-for-quantum-computing-9781801070188)
2.  Hidary, J. D. (2019). Quantum computing: An applied approach. Springer. [https://link.springer.com/book/10.1007/978-3-030-23922-0](https://link.springer.com/book/10.1007/978-3-030-23922-0)
3.  [Conjugate transpose (Wiki)](https://en.wikipedia.org/wiki/Conjugate_transpose)
4.  [Orthogonal matrix (Wiki)](https://en.wikipedia.org/wiki/Orthogonal_matrix)
5.  [Hermitian matrix (Wiki)](https://en.wikipedia.org/wiki/Hermitian_matrix)
6.  [Orthonormal basis](https://www.sciencedirect.com/topics/mathematics/orthonormal-basis#:~:text=A%20basis%20is%20orthonormal%20if,product%20spaces%20to%20orthonormal%20bases.)
7.  [Orthonormal basis](https://en.wikipedia.org/wiki/Orthonormal_basis)
8.  [Hilbert Space Quantum Mechanics](https://quantum.phys.cmu.edu/QCQI/qitd114.pdf)