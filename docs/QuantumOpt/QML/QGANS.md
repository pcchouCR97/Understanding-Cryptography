# Quantum Generative Adversarial Networks 

In the unsupervised learning, we will discuss quantum versions of the famous **Generative Adversaial Networks (GANs)** that are called **Quantum Generative Adversarial Networks (QGANs).**

We will cover 

*   GANs and their quantum counterparts
*   Quantum GANs in Qiskit

## GANs and their quantum counterparts

Quantum GANs are **generative models** that can be trained in a perfectly unsupervised manner. By the fact that they are generative models we mean that quantum GANs will be useful for generating data that can mimic a training dataset; for instance, if you had a large dataset with pictures of people, a good generative model would be able to generate new pictures of people that would be indiscernible from those coming from the original distribution.

### What actually is a GAN?
GANs were introduced in 2014 by Goodfellow et al. as a groundbreaking machine learning model designed to generate data that closely mimics a given dataset.

A GAN consists of two key components:

- **Generator**: A neural network that takes arbitrary seeds as input and produces outputs resembling the original dataset. Its primary objective is to generate realistic data.  

- **Discriminator**: A binary classifier that distinguishes between real data from the dataset and the generated data. Its role is to identify whether a given input is authentic or artificially created.

<div style="text-align: center;">
    <img src="../../images_QML/QGAN_1.png" alt="QGAN_1" style="width: 400px; height: 200px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        A schematic representation of the agents involved in a generative adversarial network. Ref[1]
    </p>
</div>

The GAN training process involves the following steps:

1.  **Initialization**: The generator and discriminator networks are initialized with random parameters.  
2.  **Discriminator Training**: The discriminator learns to differentiate real data from the generator’s output. At this stage, distinguishing real from fake data is relatively easy.  
3.  **Generator Training**: The generator is trained to produce outputs that deceive the discriminator into classifying them as real. Once trained, it generates synthetic data.  
4.  **Iterative Training**: The discriminator is retrained on both real and newly generated data, followed by updating the generator to better fool the improved discriminator. This process repeats over multiple iterations.  

Over time, the generator improves, making it increasingly difficult for the discriminator to distinguish real from generated data. Ideally, this process reaches an equilibrium where the generated data becomes nearly indistinguishable from the original dataset.

<div style="text-align: center;">
    <img src="../../images_QML/QGAN_2.png" alt="QGAN_2" style="width: 500px; height: 250px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        A schematic illustration of the training process of a GAN meant to generate pictures of cats. Ref[1]
    </p>
</div>

It's important to note that this description is a simplified overview of GAN training. In practice, the discriminator and generator are not trained in a fully alternating manner but are optimized iteratively.  

For instance, when using gradient descent with a given batch size, the training process typically follows these steps in each epoch:  

1.  **Discriminator Update**: For each batch, the discriminator's weights are adjusted in a single optimization step to improve its ability to differentiate real from generated data.  
2.  **Generator Update**: In the same epoch, the generator’s weights are updated in a single optimizer step to improve its ability to generate data that fools the discriminator.  

With this understanding of the training process, the term **GAN** becomes clearer.  

-   **Generative**: These models focus on generating new data that resembles the original dataset.  
-   **Adversarial**: The training process is framed as a competition between two networks—the generator and the discriminator—each trying to outsmart the other.  
-   **Networks**: As expected, these models rely on neural networks as their fundamental building blocks.  

So far, we have discussed how GANs utilize neural networks in both the generator and discriminator. However, the networks used in GANs are not always the standard dense neural networks we have covered.

Dense networks consist of fully connected layers, where every neuron in one layer is connected to all neurons in the next. While these networks work well for many applications, they are not always the best choice—especially for handling images.

When working with images, whether for generation, classification, or manipulation, **convolutional layers** are often used instead. These layers are specifically designed to process spatial information efficiently, making them a key component in GANs designed for image generation.

!!! Note 
    If you decide are interested in quantum versions of convolutional layers and networks, please see [[2]](../QML/QGANS.md#reference) and [[3]](../QML/QGANS.md#reference).

A key detail in GAN training is that the generator is never directly exposed to the original data. Instead it learns soley through feedback from the discriminator. And this let GANs enable fully unsupervised training.

### Challenges

GANs, like other machine learning models, face challenges during training. One concern is ensuring that the generator creates truly new data rather than slightly altered copies of the original dataset. This issue, known as **overfitting** or **memorization**, can prevent the model from generalizing patterns effectively. Techniques such as adding **noise**, using **regularization**, applying **dropout**, or ensuring **diversity** in training samples help mitigate this risk and encourage the generator to produce novel yet realistic outputs. 

GAN training can fail if the generator produces only a limited set of variations from the dataset, a problem known as **mode collapse**. For example, a GAN trained on cat images might generate only a few similar-looking cats. To address this, various improvements have been proposed, such as **Wasserstein GANs (WGANs)** [[4]](../QML/QGANS.md#reference), which use the **Wasserstein metric** to refine the loss function and enhance training stability.

### Some technicalities about GANs

#### Discriminator Loss Function
The discriminator in a GAN is trained as a binary classifier using **binary cross-entropy loss**. It assigns a label of **1** to real data samples from $X$ and **0** to generated samples from $S$. Given a generator $G$ and a discriminator $D$, the discriminator's loss function $L_D$ is defined as:
$$
L_{D} = -\frac{1}{|X| + |S|}\bigg( \sum_{x\in X}\text{log}D(x)+ \sum_{s\in S}\text{log}(1-D(G(s)))\bigg),
$$
where we are using $|X|$ and $|S|$ to denote the sie of the sets $X$ and $S$, respectively. The job of the discriminator would be to minimize this loss.

#### Generator Loss Function

Our goal when training the generator is to fool the discriminator trying to get it to classify our generated data as real data. Hence, the goal in the training of the generator is to **maximize the loss function of the discriminator**, that is, to minimize
$$
-L_{D} = \frac{1}{|X| + |S|}\bigg( \sum_{x\in X}\text{log}D(x)+ \sum_{s\in S}\text{log}(1-D(G(s)))\bigg),
$$

We can see that the first term in the sum is constant in the generator training since it does not depend on $G(s)$. We may consider the goal of the generator training to be the minimization of the generator loss function
$$
L_{G}^{'} = \frac{1}{|S|}\sum_{s\in S}\text{log}(1-D(G(s))).
$$
However, in practice, it has been shown [[5]](../QML/QGANS.md#reference) that it is usually more stable to take the goal of the generator training to be the minimization of loss
$$
L_{G} = -\frac{1}{|S|}\sum_{s\in S}\text{log}(D(G(s)))
$$

At the optimal equilibrium between the generator and discriminator, the discriminator assigns equal probabilities to real and generated data, meaning $D(x) = D(G(s)) = \frac{1}{2}$. As a result, both the discriminator and generator losses converge to:
$$
L_D = L_G = -\log \frac{1}{2} = \log 2 \approx 0.6931.
$$
This means that at equilibrium, the discriminator is no longer able to differentiate real data from generated data, implying that the generator has successfully learned to produce realistic outputs. The loss value of approximately **0.6931** indicates that the model has reached a balanced state where the generator and discriminator are equally matched, preventing further improvement in distinguishing real from fake data. You can find the proof in the original GANs paper [[5]](../QML/QGANS.md#reference).

### Quantum GANs

A **quantum GAN (QGAN)** is a GAN where either the generator, discriminator, or both are implemented using a quantum neural network. Despite its quantum components, it follows the same adversarial training process as a classical GAN.

Broadly, QGANs can be categorized into three types:

1.  **Fully Quantum QGAN (Quantum Data + Quantum Generator & Discriminator)**  
   In this setup, both the generator and discriminator are quantum circuits, and the data consists of quantum states. This creates a purely quantum model where all components interact seamlessly without requiring feature maps or measurement operations.

2.  **Hybrid QGAN (Quantum Data + Quantum Generator + Classical Discriminator)**  
   Here, the generator is quantum, producing quantum states, but the discriminator remains classical. Since the discriminator processes classical data, measurement operations are required to convert both the generated quantum states and the original quantum data into classical form.

3.  **Hybrid QGAN (Classical Data + Quantum Generator or Discriminator)**  
   In this scenario, either the generator or discriminator (or both) is quantum while working with classical data. For a quantum discriminator, a **feature map** is used to encode classical data into quantum states. This setup closely resembles classical GANs but leverages quantum models to enhance performance or efficiency.

   Because the availability of classical data is much bigger than that of quantum data, this is the type of architecture that has been studied more widely by the quantum computing community.

## GANs in Qiskit

# Reference

1.  Combarro, E. F., & González-Castillo, S. (2023). A practical guide to quantum machine learning and quantum optimisation: Hands-on approach to modern quantum algorithms. Packt Publishing.
2.  I. Cong, S. Choi, and M. D. Lukin, “Quantum convolutional neural networks,” Nature
Physics, vol. 15, pp. 1273–1278, 2019.
3.  M. Henderson, S. Shakya, S. Pradhan, and T. Cook, “Quanvolutional neural networks:
Powering image recognition with quantum circuits,” Quantum Machine Intelligence,
vol. 2, no. 1, pp. 1–9, 2020.
4.  M. Arjovsky, S. Chintala, and L. Bottou, “Wasserstein generative adversarial networks,”
in International conference on machine learning, PMLR, 2017, pp. 214–223.
5.  I. Goodfellow, J. Pouget-Abadie, M. Mirza, et al., “Generative adversarial nets,”
Advances in neural information processing systems, vol. 27, 2014.