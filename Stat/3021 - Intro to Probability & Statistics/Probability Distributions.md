---
tags:
  - type/lecture
  - course/stat3021
  - subject/statistics
course: STAT 3021
topic(s): Discrete Probability Distribution, Continuous Probability Distribution, Joint Probability Distribution
---
# Probability Distributions

## Discrete Probability Distributions

A **discrete random variable** is a variable that can assume only a countable number of values.

### Probability Mass Function (PMF)
The probability distribution of a discrete random variable $X$ is described by its probability mass function:
$$f(x) = P(X = x)$$

**Properties:**
1. $f(x) \geq 0$ for all $x$
2. $\sum_x f(x) = 1$

### Cumulative Distribution Function (CDF)
$$F(x) = P(X \leq x) = \sum_{t \leq x} f(t)$$

### Expected Value and Variance
**Expected Value (Mean):**
$$\mu = E[X] = \sum_x x \cdot f(x)$$

**Variance:**
$$\sigma^2 = \text{Var}(X) = E[(X - \mu)^2] = \sum_x (x - \mu)^2 f(x) = E[X^2] - \mu^2$$

**Standard Deviation:**
$$\sigma = \sqrt{\text{Var}(X)}$$

### Common Discrete Distributions

#### Uniform Distribution
All outcomes equally likely over $\{a, a+1, \ldots, b\}$:
$$f(x) = \frac{1}{b - a + 1}, \quad x = a, a+1, \ldots, b$$

#### Binomial Distribution
Number of successes in $n$ independent Bernoulli trials with probability $p$:
$$f(x) = \binom{n}{x} p^x (1-p)^{n-x}, \quad x = 0, 1, \ldots, n$$

- $E[X] = np$
- $\text{Var}(X) = np(1-p)$

#### Geometric Distribution
Number of trials until first success:
$$f(x) = (1-p)^{x-1} p, \quad x = 1, 2, 3, \ldots$$

- $E[X] = \frac{1}{p}$
- $\text{Var}(X) = \frac{1-p}{p^2}$

#### Negative Binomial Distribution
Number of trials until $k$ successes:
$$f(x) = \binom{x-1}{k-1} p^k (1-p)^{x-k}, \quad x = k, k+1, \ldots$$

#### Hypergeometric Distribution
Sampling without replacement from finite population:
$$f(x) = \frac{\binom{K}{x}\binom{N-K}{n-x}}{\binom{N}{n}}$$

Where $N$ = population size, $K$ = successes in population, $n$ = sample size

#### Poisson Distribution
Number of events in fixed interval with rate $\lambda$:
$$f(x) = \frac{e^{-\lambda} \lambda^x}{x!}, \quad x = 0, 1, 2, \ldots$$

- $E[X] = \lambda$
- $\text{Var}(X) = \lambda$
- Approximates binomial when $n$ large, $p$ small, $np = \lambda$ moderate

## Continuous Probability Distributions

A **continuous random variable** can take any value in an interval or collection of intervals.

### Probability Density Function (PDF)

^3923bf

For a continuous random variable $X$ with density $f(x)$:

**Properties:**
1. $f(x) \geq 0$ for all $x$
2. $\int_{-\infty}^{\infty} f(x)\,dx = 1$
3. $P(a < X < b) = \int_a^b f(x)\,dx$
4. $P(X = c) = 0$ for any specific value $c$

### Cumulative Distribution Function (CDF)
$$F(x) = P(X \leq x) = \int_{-\infty}^x f(t)\,dt$$

$$f(x) = \frac{dF(x)}{dx}$$

### Expected Value and Variance
**Expected Value:**
$$\mu = E[X] = \int_{-\infty}^{\infty} x \cdot f(x)\,dx$$

**Variance:**
$$\sigma^2 = \text{Var}(X) = \int_{-\infty}^{\infty} (x - \mu)^2 f(x)\,dx = E[X^2] - \mu^2$$

### Common Continuous Distributions

#### Uniform Distribution
Constant probability over interval $[a, b]$:
$$f(x) = \frac{1}{b-a}, \quad a \leq x \leq b$$

- $E[X] = \frac{a+b}{2}$
- $\text{Var}(X) = \frac{(b-a)^2}{12}$

#### Normal (Gaussian) Distribution
$$f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-(x-\mu)^2/(2\sigma^2)}, \quad -\infty < x < \infty$$

Notation: $X \sim N(\mu, \sigma^2)$

**Standard Normal:** $Z \sim N(0, 1)$
$$\phi(z) = \frac{1}{\sqrt{2\pi}} e^{-z^2/2}$$

**Standardization:**
$$Z = \frac{X - \mu}{\sigma}$$

#### Exponential Distribution
Time between events in Poisson process with rate $\lambda$:
$$f(x) = \lambda e^{-\lambda x}, \quad x \geq 0$$

- $E[X] = \frac{1}{\lambda}$
- $\text{Var}(X) = \frac{1}{\lambda^2}$
- **Memoryless property:** $P(X > s+t \mid X > s) = P(X > t)$

#### Gamma Distribution
Sum of $\alpha$ independent exponential random variables:
$$f(x) = \frac{\lambda^\alpha}{\Gamma(\alpha)} x^{\alpha-1} e^{-\lambda x}, \quad x \geq 0$$

Where $\Gamma(\alpha) = \int_0^\infty t^{\alpha-1} e^{-t}\,dt$

- $E[X] = \frac{\alpha}{\lambda}$
- $\text{Var}(X) = \frac{\alpha}{\lambda^2}$

**Special case:** $\alpha = 1$ gives exponential distribution

#### Chi-Square Distribution
Special case of gamma with $\alpha = \nu/2$, $\lambda = 1/2$:
$$f(x) = \frac{1}{2^{\nu/2}\Gamma(\nu/2)} x^{\nu/2-1} e^{-x/2}, \quad x \geq 0$$

Where $\nu$ = degrees of freedom

#### Beta Distribution
Defined on $[0, 1]$:
$$f(x) = \frac{\Gamma(\alpha + \beta)}{\Gamma(\alpha)\Gamma(\beta)} x^{\alpha-1}(1-x)^{\beta-1}, \quad 0 \leq x \leq 1$$

## Joint Distributions

### Joint Probability Distributions
For two random variables $X$ and $Y$:

**Discrete case:**
$$f(x, y) = P(X = x, Y = y)$$

Properties:
- $f(x, y) \geq 0$
- $\sum_x \sum_y f(x, y) = 1$

**Continuous case:**
$$P((X,Y) \in A) = \iint_A f(x,y)\,dx\,dy$$

Properties:
- $f(x, y) \geq 0$
- $\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(x,y)\,dx\,dy = 1$

### Marginal Distributions
**Discrete:**
$$g(x) = \sum_y f(x, y), \quad h(y) = \sum_x f(x, y)$$

**Continuous:**
$$g(x) = \int_{-\infty}^{\infty} f(x, y)\,dy, \quad h(y) = \int_{-\infty}^{\infty} f(x, y)\,dx$$

### Conditional Distributions
**Conditional PMF/PDF:**
$$f(x \mid y) = \frac{f(x, y)}{h(y)}, \quad h(y) > 0$$

$$f(y \mid x) = \frac{f(x, y)}{g(x)}, \quad g(x) > 0$$

**Conditional expectation:**
$$E[X \mid Y = y] = \sum_x x \cdot f(x \mid y) \quad \text{(discrete)}$$
$$E[X \mid Y = y] = \int_{-\infty}^{\infty} x \cdot f(x \mid y)\,dx \quad \text{(continuous)}$$

### Independence
Random variables $X$ and $Y$ are **independent** if and only if:
$$f(x, y) = g(x) \cdot h(y)$$

for all $(x, y)$, where $g(x)$ and $h(y)$ are the marginal distributions.

**Consequence:** If independent, then:
$$E[XY] = E[X] \cdot E[Y]$$
$$\text{Var}(X + Y) = \text{Var}(X) + \text{Var}(Y)$$

### Covariance and Correlation
**Covariance:**
$$\text{Cov}(X, Y) = E[(X - \mu_X)(Y - \mu_Y)] = E[XY] - E[X]E[Y]$$

**Correlation coefficient:**
$$\rho = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y}$$

where $-1 \leq \rho \leq 1$

- $\rho = 0$: uncorrelated
- $\rho = \pm 1$: perfect linear relationship
- If $X$ and $Y$ are independent, then $\rho = 0$ (converse not always true)

### Linear Combinations
For $X_1, X_2, \ldots, X_n$ random variables and constants $a_1, a_2, \ldots, a_n, b$:

$$E\left[\sum_{i=1}^n a_i X_i + b\right] = \sum_{i=1}^n a_i E[X_i] + b$$

$$\text{Var}\left(\sum_{i=1}^n a_i X_i\right) = \sum_{i=1}^n a_i^2 \text{Var}(X_i) + 2\sum_{i<j} a_i a_j \text{Cov}(X_i, X_j)$$

If $X_i$ are independent:
$$\text{Var}\left(\sum_{i=1}^n a_i X_i\right) = \sum_{i=1}^n a_i^2 \text{Var}(X_i)$$
