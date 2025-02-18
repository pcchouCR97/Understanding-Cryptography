# Support Vector Machine (SVM)

Support Vector Machine (SVM) is a supervised machine learning algorithm used for classification and regression tasks. While it can handle regression problems, SVM is particularly well-suited for classification tasks. 

SVM aims to find the optimal hyperplane in an N-dimensional space to separate data points into different classes. The algorithm maximizes the margin between the closest points of different classes.

The key idea behind the SVM algorithm is to find the hyperplane that best separates two classes by maximizing the margin between them. This margin is the distance from the hyperplane to the nearest data points (support vectors) on each side.

A support vector machines takes inputs in an $n$-dimentional Euclidean space $(\mathbb{R}^{n})$ and classifies them according to which side of a hyperplane they are on. This hyperplane fully defines the behavior of the SVM, which can be defined by adjustable parameteres $\overrightarrow{w}$ and the constant $b$.

<div style="text-align: center;">
    <img src="../../images_CML/SVM_Margin.png" alt="SVM_Margin" style="width: 400px; height: 400px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        SVM Margin
    </p>
</div>

## SVM Terminology
*   **Hyperplane**: A decision boundary separating different classes in feature space, represented by the equation $wx + b = 0$ in linear classification.
*   **Support Vectors**: The closest data points to the hyperplane, crucial for determining the hyperplane and margin in SVM.
*   **Margin**: The distance between the hyperplane and the support vectors. SVM aims to maximize this margin for better classification performance.
*   [**Hard Margin**](../CML/SVM.md#hard-margin): A maximum-margin hyperplane that perfectly separates the data without misclassifications.
*   [**Soft Margin**](../CML/SVM.md#soft-margin): Allows some misclassifications by introducing slack variables, balancing margin maximization and misclassification penalties when data is not perfectly separable.
*   [**Kernel**](../CML/SVM.md#kernel): A function that maps data to a higher-dimensional space, enabling SVM to handle non-linearly separable data.

## Mathematical Theory
Let the datapoints in our training dataset be $\overrightarrow{w} \in \mathbb{R}^{n}$ and their expected label by $y_{j} = \{1, -1 \}$. Also, we assume these datapoints can be perfectly separated by a hyperplane.

When we train a classifier, we are interested in getting a low generalization error. In our case, we acheve this by looking into a hyperplane that can maximize the distance from itself to the training datapoints. For example, if our separating hyperplane is too close to one of the training datapoints, we risk another datapoint of the same class crossing to the other side of the hyperplane and being misclassified.

<div style="text-align: center;">
    <img src="../../images_CML/QSVM_1.png" alt="QSVM_1" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. Both hyperplanes separate the two categories, but the continuous line is closer to the datapoints than the dashed line.
    </p>
</div>
There are two ways that could help us achieve:

1. We can consider the distance from a separating hyperplane $H$ to all the points in the training dataset, and then try to find a way to tweak $H$ to maximize that distance while making sure that $H$ still separates the data properly.
2. Instead, we can associate to each data point a unique hyperplane that is parallel to $H$ and contains that datapoint. The parallel hyperplane that goes through the point that is closest to $H$ will itself be a separating hyperplane - and so will be its reflection over $H$.

<div style="text-align: center;">
    <img src="../../images_CML/QSVM_2.png" alt="QSVM_2" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. The solid black line represents a separating hyperplane \( H \). The red dashed line is parallel hyperplane which goes through the closest point to \( H \), and it's reflection over \( H \) is the other dashed line.
    </p>
</div>

This pair of hyperplanes — the parallel plane that goes through the closest point and its reflection — will be the two equidistant parallel hyperplanes, which are the furthest apart from each other while still separating the data. The distance between them is also know as the **margin** and it is what we aim to maximize.

The equation for the linear hyperplane can be written as:

$$
\overrightarrow{w} \cdot \overrightarrow{x} + b = 0.
$$

where:

*   $\overrightarrow{w}$ is the normal vector to the hyperplane.
*   $b$ is the offset or bias term,  representing the distance of the hyperplane from the origin along the normal vector $\overrightarrow{w}$.

Any hyperplane that is parallel to $H$ can be described as $\overrightarrow{w} \cdot \overrightarrow{x} + b = C$ for some constant $C$. And the reflection of this plane is $\overrightarrow{w} \cdot \overrightarrow{x} + b = -C$. That is, for some constant $C$, the hyperplane and its reflection can be characterized as 

$$
\overrightarrow{w} \cdot \overrightarrow{x} + b = \pm C.
$$

If we set $\tilde{w} = \overrightarrow{w}/C$ and $\tilde{b} = b/C$, the equation can be rewritten as 

$$
\tilde{w} \cdot \overrightarrow{x} + \tilde{b} = \pm 1,
$$

for some values of $\overrightarrow{w}$ and $b$. Also, we know that the distance between these two hyperplanes is $2/||w||$. Therefore, we can describe our problem as maximizing $2/||w||$ while subjecting to the constraint of $\tilde{w} \cdot \overrightarrow{x} + \tilde{b} = \pm 1$.

Since we assume that there are no points inside the margin, the hyperplane $\tilde{w} \cdot \overrightarrow{x} + \tilde{b} = 0$ will properly separate the data, if for all the positive entries, $\tilde{w} \cdot \overrightarrow{x} + \tilde{b} = \geq 1$, while all the negative ones will satisfy $\tilde{w} \cdot \overrightarrow{x} + \tilde{b} = \leq -1$. We conclude this as 

$$
y_{i}(\overrightarrow{w} \cdot \overrightarrow{x_{j}}) \leq 1,
$$

since we are considering $y_{j} = 1$ when the $j$-th example belongs to the positive class and $y_{j} = -1$ when it belongs to the negative one.

<div style="text-align: center;">
    <img src="../../Images_CML/QSVM_3.png" alt="QSVM_3" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. The hyperplane that could have been returned by an SVM is represented by a black solid line, and the lines in dashed lines are the equidistant parallel hyperplanes that are the furthest apart from each other while still separating the data. The margin is thus half of the thickness of the colored region
    </p>
</div>

### Hard Margin

Therefore we can write our optimization problem as 

$$
\begin{array}{ll}
\text{Minimize} & ||w||,\\
\text{Subject to} & y_{i}(\overrightarrow{w} \cdot \overrightarrow{x_{j}} + b) \geq 1,
\end{array}
$$

where each $j$ defines an individual constraint. We can also write our problem as 

$$
\begin{array}{ll}
\text{Minimize} & \frac{1}{2}||w||^{2},\\
\text{Subject to} & y_{i}(\overrightarrow{w} \cdot \overrightarrow{x_{j}} + b) \geq 1,
\end{array}
$$

this is also called as **hard-margin training** since we are allowing no elements in the training dataset to be misclassified or even to be inside the margin. This expression can save us from troubles due to Euclidean norm in most of the optimization algorithms.

### Soft Margin

With hard-margin training, we need our training data to be perfectly separable by a hyperplane because, otherwise, we will not find any feasible solutions to the optimization problem that we have just defined. Alternatively, we can use the technique called **soft-margin training**

In contrast to the **hard-margin training**, **Soft-margin** classification is more flexible, allowing for some misclassification through the use of slack variables ($\xi$). we will use 

$$
y_{i}({w} \cdot {x_{j}} + b) \geq 1 - \xi_{j}.
$$

When $\xi_{j} > 0$, we will allow $\overrightarrow{x_{j}}$ to be close to the hyperplane or even on the wrong side of the space. The larger the value of $\xi_{j}$ is, the further into the wrong side $\overrightarrow{x_{j}}$ will be.

Since we would these $\xi_{j}$ to be as small as possible, we need to include them into the cost function. Thus,

$$
\begin{array}{ll}
\text{Minimize} & \frac{1}{2}||w||^{2} + C\sum_{j} \xi_{j}, \\
\text{Subject to} & y_{i}(\overrightarrow{w} \cdot \overrightarrow{x_{j}} + b) \geq 1 - \xi_{j},\\
& \xi_{j} \geq 0,
\end{array}
$$

where $C>0$, a hyperparameter for a penalty term, is a hyperparameter that can be chosen by us. The larger the value of $C$ is, the less misclassification is allowed. If there is a hyperplane that can perfectly separate the data, setting $C$ to a huge value would be equivalent to doing hard-margin training. You may think to make $C$ as larger as possible, but this can make our model prone to overfitting.

The soft-margin training problem can be equivalently written in terms of some optimizable parameters $\alpha_{j}$ as follows:

$$
\begin{array}{ll}
\text{Maximize} & \sum_{j}\alpha_{j} - \frac{1}{2}\sum_{j,k}y_{i}y_{k}\alpha_{j}\alpha_{k}(\overrightarrow{x_{j}} \cdot \overrightarrow{x_{k}}),\\
\text{Subject to} & 0 \leq \alpha_{j} \leq C,\\
& \sum_{j}\alpha_{j}y_{j} = 0.
\end{array}
$$

This formulation of the SVM soft-margin training problem is, most of the time, easier to solve in practice. Once we get the $\alpha_{j}$ values, it is also possible to go back to the original formulation. For instance, it holds that 

$$
\overrightarrow{w} = \sum_{j}\alpha_{j}y_{j}\overrightarrow{x_{j}}.
$$

Notice that $\overrightarrow{w}$ only depends on the training point $\overrightarrow{x_{j}}$, for which $\alpha_{j} \neq 0$. These vectors are call **support vectors**.

### Kernel

Then we will have to use the technique called **kernel trick**, which maps the original space $\mathbb{R}^{n}$ into a higher dimensional space $\mathbb{R}^{N}$. This higher dimensional space is called **feature space** and we refer to the function $\phi : \mathbb{R}^{n} \rightarrow \mathbb{R}^{N}$ as a **feature map**.

<div style="text-align: center;">
    <img src="../../Images_CML/QSVM_4.png" alt="QSVM_4" style="width: 800px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. The original data cannot be separated by a hyperplane linearly. The separating hyperplane is represented by a dashed line in (b) after applying the kernel trick
    </p>
</div>

For example, figure above shows that a kernel trick is implemented to map 1-dimensional real line into a 2-dimensional plane with the function 

$$
F(x) = (x,x^{2})
$$

To use the kernel trick, the single and only computation that we need to perform in the feature space is 

$$
k(x,y) = \phi(\overrightarrow{x})\phi(\overrightarrow{y}).
$$

This is also know as a **kernel function**. The kernel functions are functions that can be represented as inner product in some space.

SVM transforms the data points into feature space such that they can be seperated linearly. Here are some types of kernels:

1.  Linear Kernel: Maps data into linear space.
2.  Polynomial Kernel: Maps data into polynimial space.
3.  Radial Basis Function (RBF) Kernel: Maps data into a space based on distance between data points.

!!! note
    The kernel trick allows computations involving a high-dimensional feature space through inner products $(\phi(x),\phi(y))$ without explicitly transforming the data, enabling efficient classification.

#### Types of Support Vector Machine
**Linear SVM:** Used when data is linearly separable, meaning a straight line (in 2D) or a hyperplane (in higher dimensions) can clearly separate the classes. It finds the optimal hyperplane that maximizes the margin between different classes.

**Non-Linear SVM:** Applied when data is not linearly separable. It uses kernel functions to map data into a higher-dimensional space where a linear boundary can be found. This allows for more complex decision boundaries.

## References

[1]. Geeksforgeeks: [https://www.geeksforgeeks.org/support-vector-machine-algorithm/](https://www.geeksforgeeks.org/support-vector-machine-algorithm/)

[2]. Combarro, E. F., & González-Castillo, S. (2023). A practical guide to quantum machine learning and quantum optimisation: Hands-on approach to modern quantum algorithms. Packt Publishing.

[3]. What are support vector machines (SVMs)? https://www.ibm.com/think/topics/support-vector-machine.

[4]. [Support vector machine](https://en.wikipedia.org/wiki/Support_vector_machine)By <a href="//commons.wikimedia.org/w/index.php?title=User:Larhmam&amp;action=edit&amp;redlink=1" class="new" title="User:Larhmam (page does not exist)">Larhmam</a> - <span class="int-own-work" lang="en">Own work</span>, <a href="https://creativecommons.org/licenses/by-sa/4.0" title="Creative Commons Attribution-Share Alike 4.0">CC BY-SA 4.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=73710028">Link</a>