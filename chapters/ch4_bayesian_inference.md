# Chapter 4: Bayesian Inference & Decision Theory

## 🎯 Core Thesis
Frequentist inference treats the true parameter $\theta$ as a fixed, unknown constant, estimating it using long-run frequencies of data. In contrast, **Bayesian inference** treats $\theta$ as a random variable described by a probability distribution. 

By utilizing **Bayes' Theorem**, we incorporate our prior beliefs about the parameter, $f(\theta)$, and update them with the observed data likelihood, $f(x \mid \theta)$, to construct the **Posterior Distribution** $f(\theta \mid x)$:

$$f(\theta \mid x) = \frac{f(x \mid \theta) f(\theta)}{\int f(x \mid \theta) f(\theta) \, d\theta} \propto \text{Likelihood} \times \text{Prior}$$

By applying **Bayesian Decision Theory**, we choose an optimal point estimator that minimizes the expected value of a chosen **loss function**, providing a rigorous foundation for decision-making under uncertainty.

---

## 🔑 Key Mathematical Formulations

### 1. Conjugate Prior Systems
A prior $f(\theta)$ is **conjugate** to the likelihood $f(x \mid \theta)$ if the resulting posterior $f(\theta \mid x)$ belongs to the same probability family as the prior.

#### The Beta-Binomial Conjugate Model:
Let $X \sim \text{Binomial}(n, \theta)$ represent $x$ successes in $n$ trials. The likelihood is:
$$f(x \mid \theta) = \binom{n}{x} \theta^x (1-\theta)^{n-x}$$

Let the prior on $\theta$ be a Beta distribution with hyperparameters $\alpha$ and $\beta$:
$$f(\theta) = \frac{1}{\text{B}(\alpha, \beta)} \theta^{\alpha-1} (1-\theta)^{\beta-1}$$

The posterior distribution is:
$$f(\theta \mid x) \propto f(x \mid \theta) f(\theta) \propto \left( \theta^x (1-\theta)^{n-x} \right) \left( \theta^{\alpha-1} (1-\theta)^{\beta-1} \right) = \theta^{(\alpha + x) - 1} (1-\theta)^{(\beta + n - x) - 1}$$
This is the kernel of a Beta distribution. Therefore:
$$\theta \mid x \sim \text{Beta}(\alpha + x, \, \beta + n - x)$$

#### The Normal-Normal Conjugate Model:
Let $X \sim \mathcal{N}(\theta, \sigma^2)$ represent $n$ i.i.d. observations where variance $\sigma^2$ is known. Let the prior on $\theta$ be $\mathcal{N}(\mu_0, \sigma_0^2)$.
*   **The Posterior Parameters**: The posterior is also Gaussian, $\theta \mid x \sim \mathcal{N}(\mu_n, \sigma_n^2)$, where:
    $$\frac{1}{\sigma_n^2} = \frac{1}{\sigma_0^2} + \frac{n}{\sigma^2} \quad (\text{Posterior Precision} = \text{Prior Precision} + \text{Data Precision})$$
    $$\mu_n = \sigma_n^2 \left( \frac{\mu_0}{\sigma_0^2} + \frac{n\bar{X}}{\sigma^2} \right) \quad (\text{Posterior Mean} = \text{Weighted Average of Prior Mean and Sample Mean})$$

---

### 2. Intervals: Credible vs. Confidence
*   **Bayesian Credible Interval**: A $95\%$ credible interval $C$ is defined such that the probability that the parameter lies in $C$, **given the observed data**, is exactly $0.95$:
    $$P(\theta \in C \mid x) = \int_{C} f(\theta \mid x) \, d\theta = 0.95$$
*   **Frequentist Confidence Interval**: A $95\%$ confidence interval $I$ is a random interval constructed such that **across infinite hypothetical repetitions** of the experiment, $I$ will contain the fixed true parameter $\theta_0$ exactly $95\%$ of the time:
    $$P(X \in \{x : \theta_0 \in I(x)\}) = 0.95$$
    *   *Critical Disconnect:* Once the data is collected and the frequentist interval is calculated (e.g. $[12, 18]$), the probability $P(\theta_0 \in [12, 18])$ is either $0$ or $1$. The frequentist framework does not allow assigning a probability to the parameter itself.

---

## ✍️ Step-by-Step Proof: Deriving Bayes Estimators for Loss Functions

In Bayesian Decision Theory, let $L(\theta, a)$ be the loss incurred by choosing estimate $a$ when the true parameter is $\theta$. We select the **Bayes Estimator** $\hat{g}(x)$ that minimizes the **Posterior Expected Loss**:

$$\min_a \mathbb{E}[L(\theta, a) \mid x] = \min_a \int L(\theta, a) f(\theta \mid x) \, d\theta$$

---

### 1. Squared Error Loss $\implies$ Posterior Mean
#### Theorem:
Under Squared Error Loss, $L(\theta, a) = (\theta - a)^2$, the optimal Bayes estimator is the **Posterior Mean**:

$$\hat{\theta} = \mathbb{E}[\theta \mid x]$$

#### Step-by-Step Proof:
1.  Set up the posterior expected loss function $f(a)$ to minimize:
    $$f(a) = \mathbb{E}[(\theta - a)^2 \mid x] = \int (\theta - a)^2 f(\theta \mid x) \, d\theta$$
2.  Differentiate $f(a)$ with respect to $a$. Assuming regularity conditions allow passing the derivative inside the integral:
    $$f'(a) = \frac{\partial}{\partial a} \int (\theta - a)^2 f(\theta \mid x) \, d\theta = \int \frac{\partial}{\partial a} (\theta - a)^2 f(\theta \mid x) \, d\theta$$
3.  Compute the derivative of the quadratic term:
    $$f'(a) = \int -2(\theta - a) f(\theta \mid x) \, d\theta = -2 \int \theta f(\theta \mid x) \, d\theta + 2a \int f(\theta \mid x) \, d\theta$$
4.  Recall that the posterior density integrates to 1: $\int f(\theta \mid x) \, d\theta = 1$. Also, by definition, the expected value of $\theta$ under the posterior is the posterior mean: $\int \theta f(\theta \mid x) \, d\theta = \mathbb{E}[\theta \mid x]$.
5.  Substitute these values back:
    $$f'(a) = -2\mathbb{E}[\theta \mid x] + 2a$$
6.  Set the derivative to zero to find the minimum:
    $$-2\mathbb{E}[\theta \mid x] + 2\hat{\theta} = 0 \implies \hat{\theta} = \mathbb{E}[\theta \mid x]$$
7.  Check the second derivative to verify it is a minimum:
    $$f''(a) = 2 > 0 \quad (\text{Minimum verified!})$$

The theorem is proved.

---

### 2. Absolute Error Loss $\implies$ Posterior Median
#### Theorem:
Under Absolute Error Loss, $L(\theta, a) = |\theta - a|$, the optimal Bayes estimator is the **Posterior Median** ($m$):

$$\int_{-\infty}^{m} f(\theta \mid x) \, d\theta = \frac{1}{2}$$

#### Step-by-Step Proof:
1.  Set up the posterior expected loss function $f(a)$ to minimize:
    $$f(a) = \mathbb{E}[|\theta - a| \mid x] = \int_{-\infty}^{a} (a - \theta) f(\theta \mid x) \, d\theta + \int_{a}^{\infty} (\theta - a) f(\theta \mid x) \, d\theta$$
2.  Apply **Leibniz's Rule** to differentiate $f(a)$ with respect to $a$:
    $$f'(a) = \int_{-\infty}^{a} \frac{\partial(a - \theta)}{\partial a} f(\theta \mid x) \, d\theta + (a - a)f(a \mid x) + \int_{a}^{\infty} \frac{\partial(\theta - a)}{\partial a} f(\theta \mid x) \, d\theta - (\hat{a} - a)f(a \mid x)$$
    $$f'(a) = \int_{-\infty}^{a} (1) f(\theta \mid x) \, d\theta + \int_{a}^{\infty} (-1) f(\theta \mid x) \, d\theta$$
3.  Simplify the integrals:
    $$f'(a) = P(\theta \le a \mid x) - P(\theta > a \mid x) = P(\theta \le a \mid x) - \left( 1 - P(\theta \le a \mid x) \right) = 2 P(\theta \le a \mid x) - 1$$
4.  Set the derivative to zero to find the minimum:
    $$2 P(\theta \le \hat{\theta} \mid x) - 1 = 0 \implies P(\theta \le \hat{\theta} \mid x) = \frac{1}{2}$$
    The optimal estimate $\hat{\theta}$ is the **posterior median**.

The theorem is proved.

---

## 📈 Quant & Data Science Bridging

### 1. Bayesian Regularization: Mapping Priors to Ridge/Lasso Penalties
In machine learning and quantitative research, we frequently regularize linear regressions to avoid overfitting. The Bayesian framework provides the exact probabilistic explanation for why Ridge and Lasso regularization function:

*   **Ridge Regression ($L_2$ Regularization) as a Gaussian Prior**:
    If we assume the regression weights $\beta_j$ follow a Gaussian prior centered at zero, $\beta_j \sim \mathcal{N}(0, \sigma_0^2)$, then the posterior log-likelihood is:
    $$\log f(\boldsymbol{\beta} \mid y, X) \propto \log f(y \mid X, \boldsymbol{\beta}) + \log f(\boldsymbol{\beta}) \propto -\frac{1}{2\sigma^2}\|y - X\boldsymbol{\beta}\|_2^2 - \frac{1}{2\sigma_0^2}\|\boldsymbol{\beta}\|_2^2$$
    Maximizing this posterior probability (MAP estimation) is mathematically equivalent to **Ridge Regression**:
    $$\min_{\boldsymbol{\beta}} \|y - X\boldsymbol{\beta}\|_2^2 + \alpha \|\boldsymbol{\beta}\|_2^2 \quad \text{where} \quad \alpha = \frac{\sigma^2}{\sigma_0^2}$$
*   **Lasso Regression ($L_1$ Regularization) as a Laplace Prior**:
    If we assume the regression weights $\beta_j$ follow a double exponential (Laplace) prior centered at zero, $f(\beta_j) \propto e^{-|\beta_j|/b}$, then the MAP estimation problem resolves to:
    $$\min_{\boldsymbol{\beta}} \|y - X\boldsymbol{\beta}\|_2^2 + \alpha \|\boldsymbol{\beta}\|_1 \quad \text{where} \quad \alpha = \frac{2\sigma^2}{b}$$
Lasso regression is the direct mathematical result of assuming a Laplace prior on your feature weights.

### 2. Bayesian Parameter Updating in Market Maker Order Books
Market makers (e.g. trading firms on exchanges) must continuously estimate the probability $\theta$ that an incoming order is driven by an **informed trader** (arbitrageur with non-public information) vs. an **uninformed noise trader**.
*   **The Update**: The firm establishes a prior distribution on $\theta$ (typically a $\text{Beta}(\alpha, \beta)$ distribution). As transactions cross the order book, they observe $x$ informed orders out of $n$ total trades.
*   The system updates its risk parameters in real-time using the **Beta-Binomial conjugate posterior**: $\text{Beta}(\alpha + x, \beta + n - x)$.
*   If informed trades spike ($x$ rises), the posterior distribution shifts right, causing the market maker to immediately widen their bid-ask spread to protect their capital from informed toxic order flows.
