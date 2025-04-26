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

From paper [Regulatory Capital Modelling for Credit Risk](https://arxiv.org/abs/1412.1183), we know taht given a firm $k$, the default probabilites and the latent vairable can be linearlized as 

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
p_{k}(z) = \Phi\bigg(\frac{\Phi^{-1}(p_{k}^{0}) - \sqrt{\rho_k}z}{\sqrt{1-\rho+k}} \bigg)
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

where $\theta_{k}(0) = 2 \text{sin}^{-1}(\sqrt{p_{k}(0)})$ is the offset and $\frac{d\theta_k}{dz} \right$ is the slope computed using chain rule.

Here we go through the details of chain rule. We want to expand: 

$$
\theta_{k}(z) = 2 \text{sin}^{-1}\sqrt{p_{k}(z)}
$$

where 

$$
p_{k}(z) = \Phi\bigg(\frac{\Phi^{-1}(p_{k}^{0}) - \sqrt{\rho_k}z}{\sqrt{1-\rho+k}} \bigg)
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
\frac{dp_{k}}{dz} = \frac{d}{dz}\Phi\bigg(\frac{\Phi^{-1}(p_{k}^{0}) - \sqrt{\rho_k}z}{\sqrt{1-\rho+k}} \bigg) = -\sqrt{\rho_k} \times \frac{1}{1-\rho_k} \times \phi \bigg(\frac{\Phi^{-1}(p_{k}^{0}) - \sqrt{\rho_k}z}{\sqrt{1-\rho+k}} \bigg)
$$

where $\phi(\cdot)$ is the standard normal PDF. Since we expand our function around $z = 0$, 

$$
\psi = \frac{\Phi^{-1}(p_{k}^{0}) - \sqrt{\rho_k}0}{\sqrt{1-\rho+k}} = \frac{\Phi^{-1}(p_{k}^{0})}{\sqrt{1-\rho+k}}
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



# References

[1]. https://qiskit-community.github.io/qiskit-finance/tutorials/09_credit_risk_analysis.html

[2]. https://arxiv.org/abs/1412.1183

