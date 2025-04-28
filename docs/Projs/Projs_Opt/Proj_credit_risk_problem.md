## Problem definition

Suppose we are managing a credit risk porfolio with $K$ assets. The default probability of every asset $k$ follows the *Gaussian Conditional Independnce* model, i.e., given a value $z$ sampled from a latent random variable $Z$ following a standard normal distribution, the default probability of asset $k$ can be described as

$$
p_{k}(z) = \Phi \bigg(\frac{\Phi^{-1}(p_{k}^{0} - \sqrt{\rho_k}z)}{\sqrt{1-\rho_{k}}} \bigg)
$$

where $F\Phi$ denotes the [cumulative distribution function](../Projs_Opt/Proj_credit_risk_glossary.md#whats-the-cdf-cumulatuve-distribution-function) of $Z$, $p_{k}^{0}$ is the default probability of asset $k$ for $z = 0$ and $\rho_{k}$ is the sensitivity of the default probability of asset $k$ with respect to $Z$. By giving a concrete realization of $Z$ the individual default events are assumed to be independent from each other. 

Then, the topic we are interested in is measuring the total loss 

$$
L = \sum_{k=1}^{K}\lambda_{k}X_{k}(Z)
$$

where $\lambda_k$ denotes the [*loss given default*](../Projs_Opt/Proj_credit_risk_glossary.md#loss-given-default) of asset $k$, and $X_{k}(Z)$ denotes a [Bernoulli variable](../Projs_Opt/Proj_credit_risk_glossary.md#bernoulli-variable) representing the default event of asset $k$.

In reality, we are insterested in the expected value $\mathbb{E}[L]$, the Value at Risk (VaR) of $L$ and the Conditional Value at Risk of $L$ (Expected Shortfall). The VaR and CVaR are defined as 

$$
\text{VaR}_{\alpha}(L) = \text{inf}\{x|\mathbb{P}[L\leq x] \geq 1 - \alpha\}
$$

with confidence level $\alpha \in [0,1]$, and 

$$
\text{CVaR}_{\alpha}(L) = \mathbb{E}[L|L \geq \text{VaR}_{\alpha}(L)].
$$

where:

-   number of qubits used to represent $Z$, denoted by $n_z$,
-   truncation value for $Z$, denoted by $z_\text{max}$, i.e., $Z$ is assumed to take $2^{n_z}$ equidistant values in $\{-z_\text{min}, \cdots, z_\text{max} \}$,
-   the base default probabilities for each asset $p_0^{k} \in (0,1), k = 1, \cdots, K$,
-   sensitivities of the default probabilities with respect to $Z$, denoted by $\rho_k \in [0,1)$,
-   loss given default for asset $k$, denoted by $\lambda_k$
-   confidence level for VaR/CVaR $\alpha \in [0,1]$


### Uncertainty model
An [uncertainty model](../Projs_Opt/Proj_credit_risk_glossary.md#uncentainty-models) is a methematical model that is used to quantify and manage unknowns espeically future outcomes that are not determinstic. Here, in our risk analysis, this uncertainty model can be achieved by creating a quantum state in a register of $n_z$ qubits that represents $Z$ following a standard normal distribution. This state is then used to control single qubit Y-rotations on a second qubit register of $K$ qubits, where a $|1\rangle$ state of qubit $k$ represents the default even of asset $k$. We can encode our quantum state $|\psi\rangle$ into 

$$
|\psi\rangle = \sum_{i=0}^{2^{n_z}-1}\sqrt{p_{z}^{i}}|z_{i}\rangle \bigotimes_{k=1}^{K}\bigg( \sqrt{1-p_{k}(z_{i})}|0\rangle + \sqrt{p_{k}(z_i)}|1\rangle \bigg),
$$

where we denote by $z_i$ the $i$-th value of the discretized and truncated $Z$.


See [Qiskit GCI](https://github.com/qiskit-community/qiskit-finance/blob/main/qiskit_finance/circuit/library/probability_distributions/gaussian_conditional_independence_model.py) for how to construct this model using Qiskit. 


### Linear Approximation

From [Amplitude Estimation](../Projs_Opt/Proj_quantum_amplitude_estimation.md), we know that we want to encode a function $p(x)$ 

$$
p_{k}(z) = \Phi \bigg(\frac{\Phi^{-1}(p_{k}^{0} - \sqrt{\rho_k}z)}{\sqrt{1-\rho_{k}}} \bigg)
$$

into a rotational angle in a quantum circuit

$$
\theta_{k}(z) = 2\text{sin}^{-1}(\sqrt(p_k)(z)).
$$

This angle is needed to prepare a qubit state:

$$
\sqrt{1-p_{k}(z)}|0\rangle + \sqrt{p_{k}(z)}|1\rangle
$$

since the quantum gate $Ry$ work with linear functions of the input register, we approximate around $z = 0$:

$$
\theta_{k}(\theta) \approx \text{slope}_k \cdot z + \text{offset}_k
$$

From [[2]](../Projs_Opt/Proj_credit_risk_problem.md#references), we can approximate using 

$$
\theta_k(z) = 2 \arcsin\left( \sqrt{p_k(z)} \right)
$$

Then you approximate this $\theta$ as:

$$
\theta_k(z) \approx \underbrace{\theta_k(0)}_{\text{offset}} + \underbrace{ \left. \frac{d\theta_k}{dz} \right|_{z=0} }_{\text{slope}} \cdot z
$$

### Step by step

#### 1. Linear Dependency Between Risk and Latent Factor

From paper [Regulatory Capital Modelling for Credit Risk](https://arxiv.org/abs/1412.1183), we know that given a firm $k$, the default probabilites and the latent vairable can be linearlized as 

$$
X_k = a_{k} Z_{k} + b_k
$$

where $X_k$ is the individual risk variables, $Z$ is the latent variable, $a_k, b_k$ are constants. 

#### 2. Default event Modeling
A firm defaults if its latent variable fall below a threshold:

$$
\text{Firm Default if } X_{k} \leq \Phi^{-1}(p_{k}^{0})
$$

where $p^{0}_{k}$ is the baseline (unconditional) default probability.

#### 3. Conditional Default Probability as a Function of Z
From above, the conditional probability that firm $k$ defaults given $Z = z$ is:

$$
p_{k}(z) = \Phi\bigg(\frac{\Phi^{-1}(p_{k}^{0}) - \sqrt{\rho_k}z}{\sqrt{1-\rho_{k}}} \bigg)
$$

where $\Phi(z)$ is a standard normal CDF. This models how worsening economy (negative $z$) increases default probability.

#### 4. Rotation Mapping for Quantum Circuit
In quantum, we want to prepare qubit amplitudes based on probabilities. Thus, we map $p_{k}(z)$ into a rotation angle:

$$
\theta_{k}(\theta) = 2 \text{sin}^{-1}(\sqrt{p_{k}(z)})
$$

we must let rotation angle build qubit state with correct probability $p_{k}(z)$.

#### 5. Problem with Angle Rotation
It's clear that above formula is nonlinear in $z$ and $\text{sin}^{-1}$ introduces another nonlinearity. This is too complex for us for direct quantum control.

#### 6. Linear Approximation Using Taylor Expansion
We want to approximate $\theta_{k}(z)$ by first-order Taylor expansion around $z = 0$:

$$
\theta_k(z) \approx \underbrace{\theta_k(0)}_{\text{offset}} + \underbrace{ \left. \frac{d\theta_k}{dz} \right|_{z=0} }_{\text{slope}} \cdot z
$$

where $\theta_{k}(0) = 2 \text{sin}^{-1}(\sqrt{p_{k}(0)})$ is the offset and $\frac{d\theta_k}{dz}$ is the slope computed using chain rule.

Here we go through the details of chain rule. We want to expand: 

$$
\theta_{k}(z) = 2 \text{sin}^{-1}\sqrt{p_{k}(z)}
$$

where 

$$
p_{k}(z) = \Phi\bigg(\frac{\Phi^{-1}(p_{k}^{0}) - \sqrt{\rho_k}z}{\sqrt{1-\rho_{k}}} \bigg)
$$

we compute the chain rule by 

$$
\frac{d\theta_{k}}{dz} = \frac{d\theta_{k}}{dp_k} \times \frac{dp_{k}}{dz}
$$

where

$$
\frac{d\theta_{k}}{dp_k}= \frac{d}{dp_k}\bigg( 2 \text{sin}^{-1}(\sqrt{p_{k}(z)}) \bigg) = \frac{1}{\sqrt{p_k(1- p_k)}}
$$

then $\frac{dp_{k}}{dz}$ is 

$$
\frac{dp_{k}}{dz} = \frac{d}{dz}\Phi\bigg(\frac{\Phi^{-1}(p_{k}^{0}) - \sqrt{\rho_k}z}{\sqrt{1-\rho_{k}}} \bigg) = -\sqrt{\rho_k} \times \frac{1}{1-\rho_k} \times \phi \bigg(\frac{\Phi^{-1}(p_{k}^{0}) - \sqrt{\rho_k}z}{\sqrt{1-\rho_{k}}} \bigg)
$$

where $\phi(\cdot)$ is the standard normal PDF. Since we expand our function around $z = 0$, 

$$
\psi = \frac{\Phi^{-1}(p_{k}^{0}) - \sqrt{\rho_k}0}{\sqrt{1-\rho_{k}}} = \frac{\Phi^{-1}(p_{k}^{0})}{\sqrt{1-\rho_{k}}}
$$

thus, 

$$
\left. \frac{d\theta_k}{dz} \right|_{z=0}  = -\sqrt{\rho_k} \times \frac{1}{1-\rho_k} \times \phi(\psi)
$$

the final form of $\frac{d\theta_{k}}{dz} = \frac{d\theta_{k}}{dp_k} \times \frac{dp_{k}}{dz}$ will be 

$$
\text{slope} = \bigg(\frac{1}{\sqrt{p_k(0)(1 - p_{k}(0))}} \bigg) \times \bigg( -\sqrt{\rho_k} \times \frac{1}{1-\rho_k} \times \phi(\psi)\bigg)
$$

note that $p_{k}(0) = \Phi(\psi)$ and $\phi(psi)$ is the standard normal pdf evaluated at $\psi$.


#### 7. Final Linear Form for Quantum Amplitude Encoding

After approximation:

$$
\theta_{k}(z) \approx \text{offset}_{k} + \text{slop}_{k} \cdot z
$$

The quantum circuit can easily implement rotation $\text{RY}(\theta_{k})$ using only a simple linear function of the quantum register represenattaing $z$. In short,

$$
Z \xrightarrow{ X_k = a_{k} Z_{k} + b_k} X_k \xrightarrow{\Phi(\cdot)} p_{k}(Z) \xrightarrow{2 \text{sin}^{-1}\sqrt{\cdot}} \theta_{k}(z) \xrightarrow{\text{Linear Approx}} \text{Quantum Rotatopm RY}
$$

#### 8. Scaling 

##### Motivation: Why Do We Shift and Scale?

In quantum circuits (like Qiskit's GCI model), quantum registers naturally hold **integer states**:

$$
i \in [0, 2^n-1]
$$

But the real-world credit risk model operates in a **continuous real domain**:

$$
z \in [-c, c]
$$

Our goal is to correctly map quantum integer states $i$ to real systemic risk factors $z$ and preserve the real-world meaning: systemic factor $z$ drives default probabilities and quantum rotations.

> Adjusting for integer to normal range ensures that quantum integers correctly simulate real-world systemic risk factors in $[-c,c].$


##### Step 1: Shifting (Centering)

**Problem:**

- Quantum integers start from 0.
- Real $z$ domain is centered at 0 ( $-c$ to $c$).

**Solution:**

- Shift $i$ so that the middle integer maps to $z = 0$.
- Use shift:

$$
\text{shift} = \frac{2^n-1}{2}
$$

- Adjust offset:

$$
\text{offset} += \text{real slope} \times (-\text{normal_max_value})
$$

##### Step 2: Scaling (Stretching/Compressing)

**Problem:**

- Integer domain $[0, 2^n-1]$ is not the same size as real $[-c, c]$.

**Solution:**

- Scale slope to match step sizes:

$$
\text{scale factor} = \frac{2c}{2^n-1}
$$

- Adjust slope:

$$
\text{slope} *= \text{scale factor}
$$

---

##### Super Simple Example

Given:

-   $c = 3$
-   $n = 3$ qubits → $2^3 = 8$ states
-   Real slope = $-0.02$
-   Real offset = $0.1$

**Correct Process:**

1.  **Shift:**

$$
\text{offset} += (-0.02) \times (-3) = 0.1 + 0.06 = 0.16
$$

2.  **Scale:**

$$
\text{scale factor} = \frac{6}{7} \approx 0.857
$$

$$
\text{new slope} = (-0.02) \times 0.857 \approx -0.01714
$$

Now $i=0$ maps to $z=-3$ and $i=7$ maps to $z=+3$ correctly. Makesure you always adjust offset using real (unscaled) slope and only scale slope once after shifting. Thus, real-world meaning is preserved while quantum integers are mapped correctly into physical systemic factors $z$. Combininig shift and scale:

$$
z(i) = \frac{2c}{2^{n}-1}\bigg(i - \frac{2^{n}-1}{2}\bigg)
$$

Now the ==quantum states $|i\rangle$==s behave as if they are sampling real $z \in [-c,c]$.


### Normal Distribution Quantum Circuit Modeling

The probability density function of a normal distribution is defined as 

$$
\mathbb{P}(X = x) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{(x - \mu)^2}{\sigma^2}}
$$

Note that $\sigma^{2}$ is the **variance**. This is fro consistency with multivariate distributions, where the uppercase sigma $\Sigma$ is associated with the covariance.

The circuit considers the discretized version of the normal distribution on $2^{\text{num_qubits}}$ equidistant points, $x_i$, truncated to $\text{bonds}$. For a one-dimensional random variable, which means the `num_qubits` is a single integer, it applies the operation 

$$
\mathcal{P}_X |0\rangle^n = \sum_{i=0}^{2^n - 1} \sqrt{\mathbb{P}(x_i)} |i\rangle
$$

 where $n$ is `num_qubits`. The circuit loads the **square root** of the probabilities into the qubit amplitudes such that the sampling probability, which is the square of the amplitude, equals the probability of the distribution.

In the multi-dimensional case, the distribution is defined as

$$
\mathbb{P}(X = x) = \frac{1}{\sqrt{2\pi\Sigma^{2}}} e^{-\frac{(x - \mu)^2}{\Sigma}}
$$

where $\Sigma$ is the covariance. To specify a multivariate normal distribution `num_qubits` is a list of integers, each specifying how many qubits are used to discretize the respective dimension. The arguments $\mu$ and $\sigma$ in this case are a vector and square matrix.
 
If for instance, `num_qubits = [2, 3]` then $\mu$ is a 2d vector and $\sigma$ is the $2 \times 2$ covariance matrix. The first dimension is discretized using 2 qubits, hence on 4 points, and the second dimension on 3 qubits, hence 8 points. Therefore the random variable is discretized on $4 \times 8 = 32$ points.

Since, in general, it is not yet known how to efficiently prepare the qubit amplitudes to represent a normal distribution, this class computes the expected amplitudes and then uses the `QuantumCircuit.initialize` method to construct the corresponding circuit.

This circuit is for example used in amplitude estimation applications, such as finance [1, 2], where customer demand or the return of a portfolio could be modeled using a normal distribution.


# References

[1]. https://qiskit-community.github.io/qiskit-finance/tutorials/09_credit_risk_analysis.html

[2]. https://arxiv.org/abs/1412.1183

