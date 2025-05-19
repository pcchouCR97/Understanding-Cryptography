## Estimating VaR with QAE

A state $\psi\rangle_{n}$ representing the distribution of a random variable $X \in \{0,cdots, N-1\}$. To estimate VaR, you want to find a threshold $l_\alpha$ such that:

$$
\mathbb{P}[X \leq l_{\alpha}] \geq \alpha
$$

so they define an operator $F_{l}$ that flips an ancilla qubit if $x\leq l$. Then we can use quantum amplitude etimation (QAE) to estimate:

$$
\mathbb{P}[X \leq l]
$$

and use a bisection search to find the smallest $l$ such that the probability $\leq \alpha$. The result gives you $\text{VaR}_{\alpha}(X)$


## Estimating CVaR using VaR

Once you found the cutoff $l_\alpha$, you now want 

$$
\text{CVaR}_{\alpha}(X) = \mathbb{E}[X|X\leq l_{\alpha}]
$$

you use the same circuit $F_{l}$ to mark the tail region (where $X \leq l_{\alpha}$) and conditionally apply the operator $F$ that encodes the function $X$ into an amplitude. Then use QAE to copmute 

$$
\mathbb{E}[X|X\leq l_{\alpha}]
$$

## Bisection Search

Suppose a university uses SAT scores to determine admissions. You want to estimate the cutoff SAT score such that 95% of applicants score below that value. This is essentially finding the 95th percentile in the score distribution. Your goal is to find the smallest SAT score such that 

$$
\text{admitted_perventage(score)} \leq 95%
$$

Let's start with an initial score range with `low_scroe = 1000` (konwn to admit ~0%) and `high_score = 1600` (known to admit ~ 100%). Then we compute the middle 

$$
\text{mid_score} = \frac{1000 + 1600}{2} = 1300
$$

then we plug in this `mid_score` into our Iterative Quantum Amplitude Estimation to evalute the probability. Suppose we have a out come of `80%`, which doesn't meet our requirement. Then we start a new search in upper half by updating the `lower_score = 1300`. Repeat this process untill, for example, we found a `mid = 1450` get `92%` and `mid = 1500` and get 96%. We then set the `high_score = 1500`. We repeat this process untill the interval is sufficiently small, for example, `score = 1492` and we get a probability of 95%.


## CVaR

From[Conditional Value at Risk (investopedia)](https://www.investopedia.com/terms/c/conditional_value_at_risk.asp) Since CVaR values are derived from the calcualtion of VaR itself, factors such as the shape of the distribution of retrun, the cut-off level used, and the periodicity of the data, and the assumptions about stochastic volatility, will affect the value of CVaR. Once the VaR has been calculated, we can derive our CVaR as 

$$
\text{CVaR}_{\alpha} = \frac{1}{1-\alpha}\int_{-1}^{\text{VaR}_{\alpha}} xp(x)dx
$$

where $p(x)dx$ is the probability density of getting a return with value $x$, $c$ is the cut-off point on the discritubtion where tha analyst sets the $\text{VaR}$ breakpoint, and lastly, $\text{VaR}$ is the agree-upon $\text{VaR}$ level. We can write this in a discrete form as 

$$
\text{CVaR}_{\alpha} = \frac{1}{1-\alpha}\sum_{x\geq \text{VaR}_{\alpha}} xp(x)dx
$$


---
**Definition:**  
Conditional Value at Risk is the **expected (average) loss**, assuming that the loss **has already exceeded the VaR** threshold.

**Interpretation:**  
> *“If the portfolio ends up in the worst 5% of outcomes, I expect an average loss of \$3,500.”*

**Example:**  
Continuing with the same portfolio:
- 1-day 95% VaR = \$2,000  
- 1-day 95% CVaR = **\$3,500**

This means:  
If your loss **does exceed \$2,000**, then on average, you could lose **\$3,500**.

---