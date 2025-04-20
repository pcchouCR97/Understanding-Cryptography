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

## Example

**Definition:**  
Value at Risk estimates the **maximum expected loss** over a specified time horizon at a given confidence level.

**Interpretation:**  
> *“I am 95% confident that I will not lose more than \$2,000 in one day.”*

**Example:**  
You have a \$100,000 portfolio.  
- 1-day 95% VaR = **\$2,000**

This means:  
There is a 95% chance your portfolio **will not lose more than \$2,000** in a single day.  
There’s a 5% chance it could lose more.

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