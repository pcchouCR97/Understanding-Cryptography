# Quantum Support Vector Machine
*This page focuses on the quantum implementation of the Support Vector Machine (SVM). Please see [**Support Vector Machine**](../../ClscOptML/CML/SVM.md) for classical SVM theory.*

## Going quantum
A Quantum Support Vector machines is a special case of SVMs which rely on the [**kernel trick**](../../ClscOptML/CML/SVM.md#kernel).

## General idea behind QSVM

In quantum support vector machine, we only need to use our quantum algorithm for the kernel function. This function will have to rely on a quantum computer to do the following:

1.  Take as input two vectors in the original space of data.
2.  Map each of them to a quantum state through a [**feature map**](../../ClscOptML/CML/SVM.md#kernel).
3.  Compute the inner product of the quantum states and return it.

Let's say we have a feature map $\phi$ such that for each $\overrightarrow{x}$, we will have a circuit $\Phi(\overrightarrow{x})$ such that the output of the feature map will be quantum state $\phi(\overrightarrow{x}) = \Phi{\overrightarrow{x}}|0\rangle$. With a feature map ready, we can then take our kernel function to be

$$
k(\overrightarrow{a},\overrightarrow{b}) = |\langle\phi(a)|\phi(b)\rangle|^{2} = |\langle 0|\Phi^{\dagger}(a)\Phi(b)|0\rangle|^{2}.
$$

This is nothing then the **probability** of measuring all zeros after preparing the state $\Phi^{\dagger}(a)\Phi(b)$. This follows from the fact that the computational basis is [**orthonormal**](../../Math_Fundamentals/matrices.md#orthonormal-basis).

Since quantum circuits are always represented by unitary operations, $\Phi^{\dagger}$ is just a inverse of $\Phi$. But $\Phi$ will be given by a series of quantum gates. So all you need to do is apply the gates in the circuit from right to left and invert each of them.

Let's review how to implement a quantum kernel function.

1. Take a feature map that will return a circuit $\Phi(\overrightarrow{x})$ for any input $\overrightarrow{x}$.
2. You prepare the state $\Phi^{\dagger}(a)\Phi(b)$ for the pair of vectors on which you want to compute the kernel.
3. Return the **probability** of measuring zero on all the qubits.

!!! Excercise 
    One of the conditions for a function $k$ to be a kernel is that it be symmetric. Prove that, for any quantum kernel, is symmetric. ($k(\overrightarrow{a},\overrightarrow{b}) = k(\overrightarrow{b},\overrightarrow{a})$ for any inputs.)

## Feature maps
A feature map is a parameterized circuit $\Phi(\overrightarrow{x})$ that depends on the original data and thus can be used to prepare a state depends on it.

### Angle encoding
The Angle encoding consists in the application of rotation gate on each qubit $j$ parameterized by the value $x_{j}$. In angle encoding, we are using the $x_{j}$ values as angles in the rotations hence the name of the encoding.

We are free to use any rotation gate of our choice. However, if we use $R_{Z}$ gates and take $|0\rangle$ to be our initial state. **The action of our feature map will have no effects**, as a result, we have to apply Hadamard gates on each qubit.

<div style="text-align: center;">
    <img src="../../images_QML/QSVM_5_angle_encoding.png" alt="QSVM_5_angle_encoding" style="width: 800px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. Angle encoding of an input \( (x_{1}, \cdots, x_{n}) \) using different rotation gates.
    </p>
</div>

These parameters should be properly normaliezd within a certian interval before feeding into angle encoding. For example, if they are normalized between $0$ and $4\pi$, then the data will be mapped to a wider region of the feature space then if they were normalized with in $0$ to $1$ interval.

### Amplitude encoding

Alghouth the Angle encoding can take $n$ inputs on $n$ qubits, the Amplitude encoding can take $2^{x}$ inputs when implemnted on an $n$-qubit circuit. If the amplitude encoding feature map is given an input $x_{0}, \cdots, x_{2^{n}-1}$, it simply prepares the state

$$
| \phi(\overrightarrow{a}\rangle) = \frac{1}{\sqrt{\sum_{k}x_{k}^{2}}}\sum_{k=0}^{2^{n}-1}x_{k} |k\rangle.
$$

As we were told in the basic quantum state, all quantum states need to be normalized vectors. And that's what we have done for this equation to make sure the output is, obviously, a quantum state. Also, we can tell from the formula, the amplititude encoding works for any inputs except for the zero vector since nothing can be divided by $0$.

Normally, we don't use all the $2^{n}$ parameters that amplititude encoding provides. Instead, we use some of them and fill the rest with zeros. If you want to use all $2^{n}$ inputs for $n$-qubits, you have to make sure that $n$ is big enough to avoid loss information in $2^{n}-1$ degree of freedom.

### ZZ feature map

**ZZ feature map** can take $n$ inputs $a_{1}, \cdots, a_{n}$ on $n$ qubits, just like angle embedding. Its parameterized circuit is constructed following these steps:

1.  Apply a Hadamard gate on each qubit.
2.  Apply a rotation $R_{Z}$ on each qubit $j$.
3.  For each pair of elements $\{j,k\}\subseteq\{1,\cdots,n\}$ with $j < k$, do
    -   Apply a CNOT gate targeting qubit $k$ and controlled by qubit $j$. 
    -   Apply a rotation $R_{Z} (2(\pi - x_{j})(\pi - x_{k}))$ on each qubit.
    -   Apply a CNOT gate targeting qubit $k$ and controlled by qubit $j$. 

<div style="text-align: center;">
    <img src="../../images_QML/QSVM_6_ZZ_feature.png" alt="QSVM_6_ZZ_feature" style="width: 800px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. \( ZZ \) feature map three qubits with inputs \(x_{1}, x_{2}, x_{3} \).
    </p>
</div>

To guarantee a healthy balance between separating the extrema of the dataset and using as big a region a possible in the feature space, the variables could be normalized to [0,1].

## Quantum support vector machines in PennyLane
    
## Quantum support vector machines in Qiskit

# References
[1]. Combarro, E. F., & González-Castillo, S. (2023). A practical guide to quantum machine learning and quantum optimisation: Hands-on approach to modern quantum algorithms. Packt Publishing.

[2]. What are support vector machines (SVMs)? https://www.ibm.com/think/topics/support-vector-machine.

[3]. [Support vector machine](https://en.wikipedia.org/wiki/Support_vector_machine)By <a href="//commons.wikimedia.org/w/index.php?title=User:Larhmam&amp;action=edit&amp;redlink=1" class="new" title="User:Larhmam (page does not exist)">Larhmam</a> - <span class="int-own-work" lang="en">Own work</span>, <a href="https://creativecommons.org/licenses/by-sa/4.0" title="Creative Commons Attribution-Share Alike 4.0">CC BY-SA 4.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=73710028">Link</a>