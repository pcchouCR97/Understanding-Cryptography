# Quantum Hybrid Architectures

In a **hybrid quantum neural network (QNN)**, instead of manually reducing the number of features and potentially **losing important information**, we allow the **classical neural network** to learn which features are most relevant. This enables the classical network to **extract meaningful representations**, optimizing the input before passing it to the **quantum neural network**. The quantum component then focuses on classification or other complex tasks, leveraging its potential advantages in high-dimensional space processing and pattern recognition. This synergy between classical and quantum computing allows hybrid models to effectively balance efficiency and performance, making the most of both paradigms.

---

When we talk about **hybrid architectures** or **hybrid models**, we refer to systems that integrate classical and quantum-based models into a single, unified framework for training.  

In particular, we combine quantum neural networks (QNNs) with classical neural networks, as they naturally complement each other. Our approach embeds a QNN as a layer within a classical neural network, where the **quantum layer** processes inputs from the preceding layer or directly from the model’s input. It then passes its output to the next layer, if one exists. The quantum layer produces a numerical array of length $k$, functioning like a classical layer with $k$ neurons from the perspective of subsequent layers.  

In essence, a **hybrid QNN** is a classical neural network in which one or more layers are replaced by quantum layers. These quantum layers operate within the network just like classical layers, receiving inputs from previous layers and passing outputs forward. If no subsequent layer exists, the quantum layer's output serves as the final output of the network.  

## Example of a Hybrid QNN  

Let’s consider a simple example of a **hybrid quantum neural network (QNN):**  

1. **Classical Input:** The model starts by taking classical inputs. For example, an input of size **16**.  
2. **Classical Layer:** This input is fed into a standard classical layer with **8 neurons**, using the **sigmoid** activation function.  
3. **Quantum Layer:** Next, we introduce a **quantum layer** that accepts the **8 outputs** from the classical layer. For instance, we could use a **QNN with three qubits** employing **amplitude encoding**. The output of this quantum layer might be the **expectation values of the first and second qubits**, both measured in the computational basis.  
4. **Final Classical Layer:** A final classical layer with **a single neuron** and a **sigmoid activation function** takes the **two outputs** from the quantum layer as input.  

## Why Use Hybrid QNNs?  

You might wonder—what's the advantage of hybrid models? What are they useful for?  

In our exploration of **QNNs** ([see QNN](../QML/QNN.md)), we applied quantum neural networks to binary classification tasks. However, due to the **limitations of current quantum hardware and simulators**, we had to perform **dimensionality reduction** on our data before feeding it into the QNN.  

This is where hybrid QNNs shine: instead of using a **separate** dimensionality reduction step, why not integrate it **directly** into the model? A **hybrid QNN** allows us to use a **classical neural network** for dimensionality reduction while leveraging a **quantum neural network** for classification—combining the strengths of both classical and quantum computing within a single framework.  


## The Role of Hybrid QNNs  

By using a **hybrid QNN**, instead of first reducing the dimensionality of our data and then classifying it with a quantum neural network separately, we can design a model where:  

- **Classical layers** handle **dimensionality reduction** of the input data.  
- **A quantum layer** is responsible for **classification**.  

Since the entire network is trained as a single unit, the separation between **dimensionality reduction** and **classification** isn’t rigid. In practice, both the classical and quantum components will likely contribute to both tasks to some extent.  

## Important Considerations  

While hybrid QNNs are an exciting approach, there are some key points to keep in mind:  

1. **Quantum Layers Are Not a Guaranteed Performance Boost**  
   Quantum layers **aren’t magic**—they don’t automatically enhance the performance of a classical neural network. Simply swapping out a classical layer for a quantum one won’t necessarily yield improvements. Instead, you should carefully consider the **specific role** the quantum layer will play in your model.  

2. **Pay Attention to the Classical-Quantum Connection**  
   The way you integrate classical and quantum layers is crucial. The transition between these layers must be **carefully designed** to ensure smooth data flow and meaningful learning.  

3. **Neural Networks and Dimensionality Reduction**  
   Classical neural networks can **effectively reduce dimensionality**, a technique often implemented using **autoencoders**. At the same time, quantum neural networks have been shown to perform well on classification tasks, especially when working with data that has already undergone dimensionality reduction.  

Given this, it’s reasonable to assume that a well-structured hybrid QNN—properly tuned—can successfully achieve **both dimensionality reduction and classification** within a single model.  


## Questions about the hybrid architectures

!!! Question "Should You Perform Dimensionality Reduction Before Feeding Data into the Network?"
    Yes, you can perform dimensionality reduction before feeding data into a classical neural network. Techniques like PCA (Principal Component Analysis), t-SNE, or autoencoders are often used for this purpose. This approach reduces the amount of data that the network has to process, making it more efficient.

    However, if you apply dimensionality reduction beforehand, you are manually deciding what information to keep and what to discard before the neural network even starts learning. This can be a limitation, as the model loses the opportunity to learn the best way to process the data on its own.

!!! Question "What Happens If You Let the Classical Neural Network Handle Dimensionality Reduction?"
    A neural network—particularly one with layers designed for feature extraction—can automatically learn how to reduce the dimensionality of the data as part of its training process. This is commonly seen in autoencoders, convolutional networks, and attention-based architectures.
    
    If you let the classical neural network learn the best way to reduce dimensionality, rather than applying a manual reduction step beforehand, the model remains more flexible and adaptable to the task at hand.

!!! Question "Why Use Hybrid QNNs for Dimensionality Reduction and Classification Together?"

    The motivation behind hybrid QNNs is to integrate classical and quantum models in a way that leverages the strengths of both:

    *   Classical neural networks are good at handling large amounts of data and extracting useful features.
    *   Quantum neural networks have the potential to detect patterns and relationships that classical networks struggle with, especially in high-dimensional data spaces.
    
    By using a hybrid model, we allow the network to:

    1.  Learn how to reduce dimensionality automatically rather than applying a separate reduction step.
    2.  Let the classical part extract relevant features, while the quantum part focuses on classification (or potentially other tasks).

!!! Question "What’s the Best Approach?"
    It depends on the problem you're solving and available quantum resources:

    1.  If quantum resources are very limited, pre-reducing dimensionality manually might help because it reduces the complexity of the quantum component.
    2.  If you want a flexible, optimized model, letting the classical part of the hybrid QNN handle dimensionality reduction could be more effective.
    3.  In some cases, a combination works best—you might apply some initial dimensionality reduction (e.g., PCA to remove obvious redundancies) but still allow the classical neural network to fine-tune feature extraction before passing data to the quantum layer.

So, it’s not that you *shouldn’t* do dimensionality reduction beforehand—it’s that you don’t have to. If your dataset is large and high-dimensional, **some preprocessing might help**, but letting the classical network **learn** how to extract meaningful features before passing data to the quantum layer often leads to a more **adaptive** and **powerful model**.

# Reference 
[1]. Combarro, E. F., & González-Castillo, S. (2023). A practical guide to quantum machine learning and quantum optimisation: Hands-on approach to modern quantum algorithms. Packt Publishing.