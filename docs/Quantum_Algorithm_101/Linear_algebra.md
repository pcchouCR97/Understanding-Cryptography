# Linear Algebra

This page gives some of the basic linear algebra fundation of a quantum information.

## Bases and linear independence

A spinning set for the vector space $\mathbb{C}^{2}$ is the set 

$$
|v_{1}\rangle \equiv \begin{bmatrix} 1\\0 \end{bmatrix}; \ |v_{2}\rangle \equiv \begin{bmatrix} 0\\1 \end{bmatrix};
$$

since any vector 

$$
|v\rangle = \begin{bmatrix} a_{1}\\a_{2} \end{bmatrix}
$$

in $\mathbb{C}^{2}$ can be written as a linear combination $|v\rangle = a_{1}|v_{1}\rangle + a_{2}|v_{2}\rangle$ of the vectors $|v_{1}\rangle$ and $|v_{2}\rangle$. We say that 
> Vector $v_{1}$ and $v_{2}$ *span* the vector space $\mathbb{C}^{2}$

A set of non-zero vectors $|v_{1}\rangle, ..., |v_{n}\rangle$ are *linearly dependent* if there exists a set of complex numbers $a_{1},...,a_{n}$ with $a_{i} \neq 0$ for at least one value of $i$, such that 

$$
a_{1}|v_{1}\rangle + a_{2}|v_{2}\rangle + ... + a_{n}|v_{n}\rangle = 0
$$

## Linear operators and matrices

A *linear operator* between vector spaces $V$ and $W$ is defined to be any function $A:V \mapsto W$ which is linear in its inputs,

$$
A\bigg(\sum_{i}a_{i}|v_{i}\rangle\bigg) = \sum_{i}a_{i}A (|v_{i}\rangle).
$$

We usually write $A|v\rangle$ instead of $A(|v\rangle)$. A is defeined on a vector space, $V$, we mean that $A$ is a linear operator from $V$ to $V$. 

-   An important linear operator on any vector space $V$ is the *identity operator*, $I_{V}$, defined by the equation $I_{V}|v\rangle \equiv |v\rangle$ for all vectors $|v\rangle$.
-   Another important linear operator is the *zero operator*, which we denote $0$, which maps all vectors to the zero vector, $0|v\rangle \equiv |0\rangle$.

Suppose $V,W$, and $X$ are vector spaces, and $A: V \mapsto W$ and $B: W\mapsto X$ are linear operators. Then we use the notation $BA$ to denote the *composition* of $B$ with $A$, defined by $(BA)|v\rangle \equiv B(A|v\rangle) \equiv BA|v\rangle$.

The most common way to understand linear operator is in terms of their matirx representation. In fact, the linear opreator and matrix viewpoints turns out to be common completely equivalant. more precisely, the claim that the matrix $A$ is a linear operator means

$$
A\bigg( \sum_{i}a_{i}|v_{i}\rangle\bigg) = \sum_{i}a_{i}A_{ij}|v_{i}\rangle
$$

is true as an equation where the operation is matrix multiplication of $A$ by column vectors. $A_{ij}$ is just a *matrix representation* of the operator $A$.

## The Pauli matrices 

Four extremely useful matrices which we should know are the *Pauli matrices*.

$$
\begin{array}{rr}
\sigma_{0} \equiv I \equiv \begin{bmatrix} 1 & 0 \\ 0 &1 \end{bmatrix} & 
\sigma_{1} \equiv \sigma_{x} \equiv X \equiv \begin{bmatrix} 0 & 1 \\ 1 &0 \end{bmatrix} \\
\sigma_{2} \equiv \sigma_{y} \equiv Y \equiv \begin{bmatrix} 0 & -i \\ i &0 \end{bmatrix} &
\sigma_{3} \equiv \sigma_{z} \equiv Z \equiv \begin{bmatrix} 1 & 0 \\ 0 &-1 \end{bmatrix} \\
\end{array}
$$

## Inner products
In quantum mechanical notation for the inner product $(|v\rangle,|w\rangle)$ is $\langle v|w\rangle$, where $|v\rangle$ and $|w\rangle$ are vectors in the inner product sapce. A function $(\cdot,\cdot)$ from $V\times V$ to $\mathbb{C}$ is an inner product if 

1.  $(\cdot,\cdot)$ is linear in the second argument,

$$
\bigg(|v\rangle , \sum_{i}\lambda_{i}|w_{i}\rangle \bigg) = \sum_{i}\lambda_{i}(|v\rangle,|w]\rangle) = \sum_{i}\lambda_{i}\langle v|w\rangle.
$$

2.  $(|v\rangle,|w\rangle) = (|w\rangle,|v\rangle)^{*}$.
3.  $(|v\rangle,|v\rangle) \geq 0$ with equality if and only if $|v\rangle = 0$.

For example, $\mathbb{C}^{n}$ has an inner product defeined by 

$$
((y_{1},...,y_{n}),((z_{1},...,z_{n}))) \equiv \sum_{i}y_{i}^{*}z_{i} = \begin{bmatrix} y_{1}^{*} ... y_{n}^{*} \end{bmatrix} \begin{bmatrix} z_{1}\\ \vdots\\ z_{n} \end{bmatrix}.
$$

### Orthogonal
> Vectors $|w\rangle$ and $|v\rangle$ are *orthogonal* if their inner product is zero.

### Norm
The norm of a vector $|v\rangle$ is defined by 

$$
|||v\rangle|| = \sqrt{\langle v|v\rangle}.
$$

### Unit vector
> A *unit vector* is a vector $|v\rangle$ such that its *norm* is 1.
We also say taht $|v\rangle$ is *normalized* if its *norm* is 1.

Suppose $|w_{1}\rangle,...,|w_{d}\rangle$ is a basis set for some vector space $V$ with an inner product. The *GRAM-Schmidt* procedure can produce an orthonormal basis set $|v_{1}\rangle,...,|v_{d}\rangle$ for the vector space $V$. Define $|v_{1}\rangle \equiv |w_{1}\rangle/|||w_{1}\rangle||$, and for $1\leq k \leq d-1$ define $v_{k+1}$ by

$$
v_{k+1} = \frac{|w_{k+1}\rangle - \sum_{i=1}^{k}\langle v_{i}|w_{k+1}\rangle|v_{i}\rangle}{|||w_{k+1}\rangle - \sum_{i=1}^{k}\langle v_{i}|w_{k+1}\rangle|v_{i}\rangle||}.
$$

### Matrix representation
The inner product on a Hilbert space can given a convenient matrix representation. Let $|w\rangle = \sum_{i}w_{i}|i\rangle$ and $|v\rangle = \sum_{j}v_{j}|j\rangle$ be representations of vector $|w\rangle$ and $|v\rangle$ with respect to some orthonormal basis $|i\rangle$. Then, since $\langle i |j\rangle = \delta_{ij}$,

$$
\begin{array}{rl}
\langle v|w\rangle = & \bigg( \sum_{i}v_{i}|i\rangle, \sum_{j}w_{j}|j\rangle\bigg) = \sum_{ij}v_{i}^{*}w_{j}\delta_{ij} = \sum_{i}v_{i}^{*}w_{i} \\
\ = & \begin{bmatrix} v_{1}^{*} ... v_{n}^{*} \end{bmatrix} \begin{bmatrix} w_{1} \\ \vdots \\ w_{n}\end{bmatrix}
\end{array}
$$

The inner product of two vectors is equal to the vector inner product between two matrix representations of those vectors, provided the representations are written with respect to the same orthonormal basis.

### Outer product
We can also represent linear operators which makes use of the inner product, known as the *inner product* representation. Suppose $|v\rangle$ is a vector in an inner product spave $V$, and $|w\rangle$ is a vector in an inner product space $W$. Define $|w\rangle\langle v|$ to be the linear operator **from $V$ to $W$** whose action is defined by 

$$
(|w\rangle \langle v|)(|v'\rangle) \equiv |w\rangle \langle v|v'\rangle = \langle v|v' \rangle|w\rangle.
$$

> When the *operator* $|w\rangle \langle v|$ acts on $|v'\rangle$, it results of multiplying $|w\rangle$ by the complex number $\langle v|v' \rangle$.

### Completness relation

Let $|i\rangle$ be any orthonormal basis for the vector space $V$, so an arbitrary vector $|v\rangle$ can be written $|v\rangle = \sum_{i}v_{i}|i\rangle$ for some set of complex number $v_{i}$. Since $\langle i|v\rangle$,

$$
\bigg( \sum_{i}|i\rangle\langle i|\bigg)|v\rangle = \sum_{i}|i\rangle\langle i|v\rangle = \sum_{i}v_{i}|i\rangle = |v\rangle.
$$

since the last equation is true for all $|v\rangle$ it follows that 

$$
\sum_{i}|i\rangle\langle i | = I, \ \text{completness relation}.
$$

Suppose $A: V \rightarrow W$ is a linear operator, $|v_{i}\rangle$ is an orthonormal basis for $V$, and $|w_{j}\rangle$ and orthonormal basis for $W$. Using the completeness reltaion twice we obtain

$$
\begin{array}{rl}
A = & I_{W}AI_{V}\\
\ = & \sum_{ij}|w_{j}\rangle\langle w_{j}|A|v_{i}\rangle\langle v_{i}|\\
\ = & \sum_{ij}\langle w_{j}|A|v_{i}\rangle|w_{j}\rangle\langle v_{i}|
\end{array}
$$

which is the outer product representation for $A$. An A has matrix element $\langle w_{j}|A|v_{i}\rangle$ in the $i$-th column and $j$-th row, respect to the inpur basis $|v_{i}\rangle$ and output basis $|w_{j}\rangle$.

### Cauchy-Schwarz inequality
%TODO

## Eigenvectors and eigenvalues 

## Adjoints and Hermitian operators 
Suppose $A$ is any linear opeartor on a Hilbert space, $V$. It turns out that there exist a unique linear operator $A^{\dagger}$ on $V$ such that for all vectors $|v\rangle, |w\rangle \in V$,

$$
(|v\rangle,A|w\rangle) = (A^{\dagger}|v\rangle,|w\rangle).
$$

This operator is known as the *adjoint* or *Hermitian conjugate* of the operator $A$. 

> $(AB)^{\dagger} = B^{\dagger}A^{\dagger}$. 

By convention, if $|v\rangle$ is a vector then we know $|v\rangle^{\dagger} \equiv \langle v|$. Then we know that $(A|v\rangle)^{\dagger} = \langle v|A^{\dagger}$.

### Projectors

An operator $A$ whose adjoint is $A$ is known as a *Hermitian* or *self-adjoint* operator. An important class of Hermitian operators is the *projectors*. Suppose $W$ is a $k$-dimensional vector subspace of the $i$-dimentional vector space $V$.

By using Gram-Schmidt procedure it is possible to construct an orthonormal basis $|1\rangle,...,|d\rangle$ for $V$ such that $|1\rangle,...,|k\rangle$ is an orthonormal basis for $W$. By definition,

$$
P \equiv \sum_{i=1}^{k}|i\rangle\langle i|
$$

is the *projector* onth the subspace $W$. 

The *orthogonal complement* of $P$ is the operator $Q\equiv I-P$. $Q$ is a projector onto the vector space spanned by $|k+1\rangle,...,|d\rangle$, which we also refer to as the *orthogonal complement* of $P$, and may denote by $Q$. 

> Any projector $P$ satisfies $P^{2} = P$.

### Normal 

An operator $A$ is said to be *normal* if $AA^{\dagger} = A^{\dagger}A$. A *spectral decomposition* theorm states that an operator is a normal operator if and only if it is diagonalizable.

> A normal matrix is Hermitian if and only if it has real eigenvales.


### Unitary

A operator/matrix $U$ is said to be *unitary* if $U^{\dagger}U = I$. A unitary operator also satisfies $UU^{\dagger} = I$, and therefore $U$ is normal has a spectral decompostion. An unitary operators are important since they preserve inner products between vectors. To see this, let $|v\rangle$ and $|w\rangle$ be any two vectors. Then the inner product of $U|v\rangle$ and $U|w\rangle$ is the same as the inner product of $|v\rangle$ and $|w\rangle$,

$$
(U|v\rangle,U|w\rangle) = \langle v|U^{\dagger}U|w\rangle = \langle v|I|w\rangle = \langle v|w\rangle.
$$

since $(U|v\rangle)^{\dagger} = \langle v|U^{\dagger}$. This result suggests the following outer product representation of any unitary $U$. Let $|v_{i}\rangle$ be any orthonormal basis set. Define $|w_{i}\rangle \equiv U|v_{i}\rangle$, so $|w_{i}\rangle$ is also an orthonormal basis set, since unitary operators preserve inner products. Note that $U = \sum_{i}|w_{i}\rangle\langle v_{i}|$.

### Positive operator
A positive operator $A$ is defined to be an operator such that for any vector $|v\rangle$, $(|v,\rangle, A|v\rangle)$ is a real, non-negative number. If $(|v,\rangle, A|v\rangle)$ is *strictly* greater than zero for all $|v\rangle \neq 0$ then we say $A$ is *positive defininte*. 

> Any positive operator is automatically Hermitian, and therefore by the spectral decomposition has diagonal representation $\sum_{i}\lambda_{i}|r\rangle\langle i|$. with non-negative eigenvalues $\lambda_{i}$.

> Any operator $A$, $A^{\dagger}A$ is positive.

## Tensor products 
The *tensor product* is a way of putting vector spaces together to form larger vector spaces. 

Suppose $V$ and $W$ are vector spaces of dimension $m$ and $n$ respectively. For convenience we also suppose that $V$ and $W$ are Hilbert spaces. Then $V\otimes W$ is an $mn$ dimensional vector space. In particular, if $|i\rangle$ and $|j\rangle$ are orthonormal bases for the spaces $V$ and $W$ then $|i\rangle \otimes |j\rangle$ is a basis for $V\otimes W$. Be often use the abbriviationd notations for $|v\rangle|w\rangle$ as $|vw\rangle$. Here are some of the basic tensor properties:

1.  For an arbitrary scaler $z$ and elements $|v\rangle$ of $V$ and $|w\rangle$ for $W$,
$$
z(|v\rangle\otimes|w\rangle) = (z|v\rangle)\otimes |w\rangle = |v\rangle \otimes (z|w\rangle).
$$

2.  For arbitrary $|v_{1}\rangle$ and $|v_{2}\rangle$ in $V$ and $|w\rangle$ in $W$,
$$
(|v_{1}\rangle + |v_{2}\rangle) \otimes |w\rangle = |v_{1}\rangle \otimes |w\rangle + |v_{2}\rangle \otimes |w\rangle.
$$

3.  For arbitrary $|v\rangle$ in $V$ and $|w_{1}\rangle$ and $|w_{2}\rangle$ in $W$,
$$
|v\rangle \otimes (|w_{1}\rangle + |w_{2}\rangle) = |v\rangle \otimes |w_{1}\rangle + |v\rangle \otimes w_{2}\rangle.
$$

4.  Suppose we have operators $A$ and $B$ and $v\rangle$ and $|w\rangle$ are vectors in $V$ and $W$, respectively. Then we can defiine a linear operator $A\otimes B$ on $V\otimes W$ by the equation
$$
(A\otimes B)(|v\rangle \otimes |w\rangle) \equiv A|v\rangle \otimes B|w\rangle.
$$
The definition of $A\otimes B$ is then extended to all elements of $V\otimes W$, that is,
$$
(A\otimes B)\bigg(\sum_{i}a_{i}|v_{i}\rangle \otimes |w_{i}\rangle\bigg) \equiv \sum_{i}a_{i}A|v_{i}\rangle \otimes B|w_{i}\rangle.
$$

%TODO, Adding an exmple 

An arbitrary linear opeartor $C$ mapping $V\otimes W$ to $V'\otimes W'$ can be represented as a linear combination of tensor products of operators mapping $V$ to $V'$ and $W$ to $W'$,
$$
C = \sum_{i}c_{i}A_{i}\otimes B_{i}
$$
by definition
$$
\bigg(\sum_{i}c_{i}A_{i}\otimes B_{i} \bigg)|v\rangle \otimes |w\rangle \equiv \sum_{i}c_{i}A_{i}|v\rangle \otimes B_{i}|w\rangle.
$$

### Kronecker product
Suppose $A$ is an $m$ by $n$ matrix, and $B$ is a $p$ by $q$ matrix. Then we have the matrix representation for $A\otimes B$:

$$
A\otimes B = 
\begin{bmatrix}
A_{11}B & A_{11}B & ... & A_{1n}B \\
A_{11}B & A_{11}B & ... & A_{2n}B \\
\vdots & \vdots & \vdots & \vdots \\
A_{m1}B & A_{m1}B & ... & A_{mn}B 
\end{bmatrix}
$$

For example, the tensor product of vectors $(1,2)$ and $(2,3)$ is the vector 

$$
\begin{bmatrix} 1\\ 2 \end{bmatrix} \otimes \begin{bmatrix} 2\\ 3 \end{bmatrix} = 
\begin{bmatrix} 1 \times 2 \\ 1\times 3\\ 2 \times 2\\ 2\times 3 \end{bmatrix} =
\begin{bmatrix} 2\\ 3\\ 4\\ 6 \end{bmatrix} 
$$

The tensor of the Pauli-X and Pauli-Y is

$$
X\otimes Y = \begin{bmatrix} 0\cdot Y & 1\cdot Y \\ 1\cdot Y & 0\cdot Y  \end{bmatrix}
= \begin{bmatrix} 0 & 0 & 0 & -i \\ 0 & 0 & i & 0 \\ 0 & -i & 0 & 0 \\ i & 0 & 0 & 0 \\ \end{bmatrix}
$$

Finally, We introduce the useful notation $|\psi\rangle^{\otimes k}$, which means $|\psi\rangle$ tensored with itself $k$ times. For example, $|\psi\rangle^{\otimes 2} = |\psi\rangle |\psi\rangle$.

> $(A\otimes B)* = A* \otimes B*$; $(A \otimes B)^{T} = A^{T} \otimes B^{T}$; $(A\otimes B)^{\dagger} = A^{\dagger} \otimes B^{\dagger}$.

> The tensor product of two unitary operators is unitary.

> The tensor product of two Hermitian operators is Hermitian.

> The tensor product of two positive operators is positive.

> The tensor product of two projectors is a projector.

A Hadamard operator on one qubit may be written as 

$$
H = \frac{1}{\sqrt{2}} \bigg[ (|0\rangle +|1\rangle) \langle 0| + (|0\rangle -|1\rangle) \langle 1|\bigg].
$$

The Hadamard transform on $n$ qubits, $H^{\otimes n}$, can be written as 

$$
H^{\otimes n} = \frac{1}{\sqrt{2^{n}}}\sum_{x,y}(-1)^{x\cdot y}|x\rangle \langle y|.
$$


## Operator functions

%TODO
Given a function $f$ from the complex numbers to the complex numbers, it is possible to define a corresponding matrix function on normal matrices by the following construction. Let $A = \sum_{a}a|a\rangle\langle a|$ be a spectral decomposition for a normal operator $A$. Define 
$$
f(A)\equiv \sum_{a}f(a)|a\rangle \langle a|
$$
Since $f(A)$ is uniquely defined. 

### Trace
The trace of $A$ is defined to be the sum of its diagonal elements,
$$
\text{tr}(A) \equiv \sum_{i} A_{ii}.
$$

The trace is easily seen to be *cyclic*, $\text{tr}(AB) = \text{tr}(BA)$, and *linear*, $\text{tr}(A+B) = \text{tr}(A) + \text{tr}(B), \text{tr}(zA) = z\text{A}$, where $A$ and $B$ are arbitrary matrices and $z$ is a complex number. From the cyclic property it follows that the trace of a matrix is invariant under the unitary *similarity transformation* $A \mapsto UAU^{\dagger}$, as $\text{tr}(UAU^{\dagger}) = \text{tr}(UU^{\dagger}A) = \text{tr} = \text{tr}(A)$. Thus, it makes sense to define the trace of an *operator* $A$ to be the trace of any matrix representation of $A$. 

Suppose $|\psi\rangle$ is a unit vector and $A$ us an arbitrary operator. To evaluate $\text{tr}(A|psi\rangle\langle\psi|)$ use the Gram-Schmidt procedure to extend $|\psi\rangle$ to an orthonormal basis $|i\rangle$ which includes $|\psi\rangle$ as the first element. Then we have 

$$
\begin{array}{rl}
\text{tr}(A|\psi\rangle\langle\psi|) & = \sum_{i}\langle i|A|\psi\rangle\langle\psi|i\rangle\\
\ & = \langle \psi|A|\psi\rangle.
\end{array}
$$

The result $\text{tr}(A|\psi\rangle\langle\psi|) = \langle \psi|A|\rangle$ is useful in evaluating the trace of an operator.

## The commutator and anti-commutator

## The polar and singular value decomposition