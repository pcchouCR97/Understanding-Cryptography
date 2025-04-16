### Approximation Error 

The Taylor approximation of order $2u+1$ leads to a maximal approximation error for equation 

$$
p(y) = \frac{1}{c}\bigg(\text{sin}^{-1}\bigg(\sqrt{c\bigg( y - \frac{1}{2}\bigg) + \frac{1}{2}}\bigg)-\frac{\pi}{4}\bigg)
$$

to

$$
\frac{\pi}{M} + \frac{c^{2u+3}}{(2u+3)2^{u+1}} + O(c^{2u+5})
$$

for all $y\in [0,1]$.Using amplitude estimation to estimate $\mathbb{E}[c(f(x) - \frac{1}{2}) + \frac{1}{2}]$ leads to a maximal error 

$$
\frac{\pi}{M} + \frac{c^{2u+3}}{(2u+3)2^{u+1}} + O(c^{2u+5} + M^{-2})
$$

where we ignore the higher order terms in the follwoing. Also, $\pi/M$ is the QAE resolution limit, $\frac{c^{2u+3}}{(2u+3)2^{u+1}}$ is the polynomial approximation error, and $O(\cdot)$ is the higher-order terms. Since our estimation uses $cf(x)$, we have to consider the scaled error $c\epsilon$, where $\epsilon>0$ denotes the resulting estimation error for $\mathbb{E}[f(X)]$. Therefore,

$$
c\epsilon  = \frac{\pi}{M} + \frac{c^{2u+3}}{(2u+3)2^{u+1}}
$$

leads to

$$
c\epsilon  - \frac{c^{2u+3}}{(2u+3)2^{u+1}} = \frac{\pi}{M}.
$$

By minimizing the number of required $M$ (M is the number of quantum queries or evaluation steps used in amplitude estimation) to achieve a target error $\epsilon$, resulting in $c^{*} = \sqrt{2}\epsilon^{\frac{1}{2u+2}}$. Plugging $c^{*}$ back to above equation gives

$$
\sqrt{2}\bigg(1- \frac{1}{2u+3}\bigg)\epsilon^{1+\frac{1}{2u+2}} = \frac{\pi}{M}.
$$