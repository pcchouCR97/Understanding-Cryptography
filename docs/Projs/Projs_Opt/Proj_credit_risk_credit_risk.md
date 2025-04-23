# Credit Risk


Imagine you are manaing credit portfolio of 10 million, which has 100 bonds, each of them worth 100,000 dollar across 100 diffferent firms. And we and to estimate the loss under systemetic shock.

Assumptions:

- Each bond has:
    - Baseline default probability of $p_{k}^{0} = 1\%$.
    - Recovery rate of $40\%$.
    - Correlation to system factor $\rho_{k} = 0.2$.

- System factor realization (for example, recession) = $z = 1$.


## What is a Default?

A **default** occurs when a borrower fails to meet debt obligations, such as interest or principal payments on a bond or loan.

- In a portfolio, if a bond issuer goes bankrupt, it defaults.
- The loss is calculated as:

Loss = (1 - Recovery Rate) × Exposure


For example, with a 40% recovery on a \$100,000 bond:

Loss = (1 - 0.4) × 100,000 = 60,000


## What is a Credit Portfolio?

A **credit portfolio** consists of credit-risky financial assets, including:

- Corporate bonds
- Bank loans
- Credit derivatives (e.g. CDS)

Such portfolios are exposed to the risk of borrower default.

## What is Baseline Default Probability $p_k^0$?

This is the **unconditional probability** that a firm will default, assuming normal market conditions.

- Estimated from credit ratings, CDS spreads, or historical default rates.
- Example: A BB-rated firm may have $p_k^0 = 0.01$ (1% default probability).

## What is Systemic Factor Correlation $\rho_k$?

$\rho_k$ measures how strongly asset $k$'s default risk is correlated with the macroeconomy.

- If $\rho_k$ is close to 0 → risk is mostly firm-specific.
- If $\rho_k$ is close to 1 → risk is strongly tied to macro conditions.

Typical values:

- $\rho_k = 0.1–0.2$: Low to moderate sensitivity.
- $\rho_k = 0.3–0.5$: High sensitivity to economic downturns.

## What is Systemic Factor Realization $z$?

The variable $z ~ N(0,1)$ models the **macroeconomic environment**:

- $z = 0$: Normal conditions
- $z = -1$: Recession (1 standard deviation below mean)
- $z = -2$: Financial crisis or deep recession

This is used to simulate default probabilities under stress.

## Why Use This Model?

The Gaussian Conditional Independence (GCI) model allows:

- Simulation of losses across many economic scenarios.
- Estimation of expected loss, VaR, and CVaR.
- Modeling of systemic vs. idiosyncratic risks.
- Regulatory capital and stress-testing applications.


## Whats the CDF (Cumulatuve Distribution Function)?

The cumulative distribution function of a real-valued random variable 
$X$ is the function given by

$$
F(x) = \mathbb{P}[X\leq x].
$$

This means 
> The probability that the random variable $X$ is less than or equal to some value $x$.
It's the area under the probability curve to the left of $x$.

Let's say $X \sim \mathcal{N}(0,1)$, where 0 is the mean and 1 is the variance ,i.e., standard normal distribution where mean $\mu = 0$, symmetrical bell curve. We wonder what's the chance a standard normal vairable is $\leq 0$? That is, $F(1) = \mathbb{P}[X\leq 0]$? The answer is $50%$ since the curve is symmetric around 0.
$$
F(0) = \Phi(0) = 0.5
$$

A simple example that we can analogy is the exam scores. Let's say your exam score $X$, which is a random variable, you don't know yet. And we have $X \sim \mathcal{N}(70,10^{2})$, which means that the average score $\mu = 70$ and standard deviation $\sigma = 10$. Now, you want to know that 

> What's the chance your score is less than 85? 

That will be 
$$
\mathbb{P}[X\leq x] = \mathbb{P}[X\leq 85],
$$
where $X$ is your future score which you don't know yet, $x= 85$ is the specific score you are comparing to. And the result will be
$$
Z = \frac{X - \mu}{\sigma} = \frac{85 - 70}{10} = 1.5
$$
thus, we plug back to our CDF function 

$$
\Phi(z) = \frac{1}{\sqrt{2\pi}}\int_{\infty}^{z} e^{-\frac{t^{2}}{2}} dt = \Phi(1.5) \approx 0.9332
$$

> This means there's a $93.3\%$ chance your score $\leq 85$.

Here, we define a standardized variable from a normal distribution:
$$
Z = \frac{X - \mu}{\sigma}
$$
where $X$ is the raw value (this can be return, credit score), $\mu$ is the mean of the distrubtion, $\sigma$ is the standard deviation, $Z \sim \mathcal{N}(0,1)$: always stardard normal





## Reference
[1]. https://en.wikipedia.org/wiki/Cumulative_distribution_function