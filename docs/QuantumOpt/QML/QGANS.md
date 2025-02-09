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

### Some technicalities about GANs

### Quantum GANs

## GANs in Qiskit



# Reference 
[1]. Combarro, E. F., & González-Castillo, S. (2023). A practical guide to quantum machine learning and quantum optimisation: Hands-on approach to modern quantum algorithms. Packt Publishing.