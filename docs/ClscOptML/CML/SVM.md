# Support Vector Machine (SVM) Algorithm

Support Vector Machine (SVM) is a supervised machine learning algorithm used for classification and regression tasks. While it can handle regression problems, SVM is particularly well-suited for classification tasks. 

SVM aims to find the optimal hyperplane in an N-dimensional space to separate data points into different classes. The algorithm maximizes the margin between the closest points of different classes.

## SVM Terminology
*   Hyperplane: A decision boundary separating different classes in feature space, represented by the equation $wx + b = 0$ in linear classification.
*   Support Vectors: The closest data points to the hyperplane, crucial for determining the hyperplane and margin in SVM.
*   Margin: The distance between the hyperplane and the support vectors. SVM aims to maximize this margin for better classification performance.
*   Kernel: A function that maps data to a higher-dimensional space, enabling SVM to handle non-linearly separable data.
*   Hard Margin: A maximum-margin hyperplane that perfectly separates the data without misclassifications.
*   Soft Margin: Allows some misclassifications by introducing slack variables, balancing margin maximization and misclassification penalties when data is not perfectly separable.

## How does SVM works?
The key idea behind the SVM algorithm is to find the hyperplane that best separates two classes by maximizing the margin between them. This margin is the distance from the hyperplane to the nearest data points (support vectors) on each side.

The best hyperplane, also known as the **“hard margin,”** is the one that maximizes the distance between the hyperplane and the nearest data points from both classes. This ensures a clear separation between the classes. So, from the above figure, we choose L2 as hard margin.

## How does SVM classify the data?
It’s simple! The blue ball in the boundary of red ones is an outlier of blue balls. The SVM algorithm has the characteristics to ignore the outlier and finds the best hyperplane that maximizes the margin. SVM is robust to outliers. A soft margin allows for some misclassifications or violations of the margin to improve generalization.

We define the objective function of a SVM as 

$$
\text{objective function} = (\frac{1}{\text{margin}}) + \lambda \sum(\text{penalty})
$$

where the penalty 


## What is a Kernel?

Then we will have to use the technique called **kernel trick**, which maps the original space $\mathbb{R}^{n}$ into a higher dimensional space $\mathbb{R}^{N}$. This higher dimensional space is called **feature space** and we refer to the function $\phi : \mathbb{R}^{n} \rightarrow \mathbb{R}^{N}$ as a **feature map**.

<div style="text-align: center;">
    <img src="/QuantumOpt/images_QML/QSVM_4.png" alt="QSVM_4" style="width: 800px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. The original data cannot be separated by a hyperplane linearly. The separating hyperplane is represented by a dashed line in (b) after applying the kernel trick
    </p>
</div>

## Reference

[1]. Geeksforgeeks: [https://www.geeksforgeeks.org/support-vector-machine-algorithm/](https://www.geeksforgeeks.org/support-vector-machine-algorithm/)