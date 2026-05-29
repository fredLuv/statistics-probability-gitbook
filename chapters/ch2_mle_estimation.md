# Chapter 2: Estimation Frameworks & Maximum Likelihood Estimation

## 🎯 Core Thesis
Statistical inference aims to estimate the unknown parameters $\theta$ of a probability distribution $f(x; \theta)$ using observed data. Larry Wasserman outlines two primary estimation frameworks: classical frequentist parametric estimation and non-parametric estimation. 

Among frequentist estimators, the **Maximum Likelihood Estimator (MLE)** is the most widely used due to its optimal asymptotic properties. 

This chapter details the mathematical properties of estimators, provides rigorous algebraic proofs of the **Cramér-Rao Lower Bound** (defining the limit of estimator efficiency) and the **Asymptotic Normality of the MLE**, and provides fully worked MLE solutions for the most critical probability distributions.

---

## 🔑 Key Mathematical Formulations

### 1. Estimator Properties & Bias-Variance Decomposition
Let $\hat{\theta}_n$ be an estimator of parameter $\theta$ based on $n$ observations.
*   **Bias**: $\text{Bias}(\hat{\theta}_n) = \mathbb{E}[\hat{\theta}_n] - \theta$. An estimator is unbiased if $\text{Bias}(\hat{\theta}_n) = 0$.
*   **Mean Squared Error (MSE)**: Measures the overall quality of an estimator.
    *   *Derivation of MSE Decomposition:*
        $$\text{MSE}(\hat{\theta}_n) = \mathbb{E}[(\hat{\theta}_n - \theta)^2]$$
        We add and subtract $\mathbb{E}[\hat{\theta}_n]$ inside the expectation:
        $$\text{MSE}(\hat{\theta}_n) = \mathbb{E}\left[\left( (\hat{\theta}_n - \mathbb{E}[\hat{\theta}_n]) + (\mathbb{E}[\hat{\theta}_n] - \theta) \right)^2\right]$$
        $$\text{MSE}(\hat{\theta}_n) = \mathbb{E}\left[ (\hat{\theta}_n - \mathbb{E}[\hat{\theta}_n])^2 + 2(\hat{\theta}_n - \mathbb{E}[\hat{\theta}_n])(\mathbb{E}[\hat{\theta}_n] - \theta) + (\mathbb{E}[\hat{\theta}_n] - \theta)^2 \right]$$
        Apply linearity of expectation. Note that $(\mathbb{E}[\hat{\theta}_n] - \theta)$ is a constant, and $\mathbb{E}[\hat{\theta}_n - \mathbb{E}[\hat{\theta}_n]] = 0$:
        $$\text{MSE}(\hat{\theta}_n) = \mathbb{E}[(\hat{\theta}_n - \mathbb{E}[\hat{\theta}_n])^2] + 2(\mathbb{E}[\hat{\theta}_n] - \theta)\mathbb{E}[\hat{\theta}_n - \mathbb{E}[\hat{\theta}_n]] + (\mathbb{E}[\hat{\theta}_n] - \theta)^2$$
        $$\text{MSE}(\hat{\theta}_n) = \text{Var}(\hat{\theta}_n) + 0 + \text{Bias}^2(\hat{\theta}_n)$$
        $$\text{MSE}(\hat{\theta}_n) = \text{Var}(\hat{\theta}_n) + \text{Bias}^2(\hat{\theta}_n)$$

---

### 2. Fisher Information
The **Fisher Information** $I(\theta)$ measures the amount of information that an observable random variable $X$ carries about an unknown parameter $\theta$:

$$I(\theta) = \mathbb{E}\left[ \left( \frac{\partial}{\partial \theta} \log f(X; \theta) \right)^2 \right] = -\mathbb{E}\left[ \frac{\partial^2}{\partial \theta^2} \log f(X; \theta) \right]$$

For a sample of $n$ independent and identically distributed (i.i.d.) observations, the total Fisher Information is:
$$I_n(\theta) = n I(\theta)$$

---

## ✍️ Step-by-Step Proof: The Cramér-Rao Lower Bound

### Theorem:
Let $\hat{\theta}$ be an unbiased estimator of parameter $\theta$ ($\mathbb{E}[\hat{\theta}] = \theta$) from a probability density $f(x; \theta)$. Under mild regularity conditions, the variance of $\hat{\theta}$ is bounded below by the reciprocal of the Fisher Information:

$$\text{Var}(\hat{\theta}) \ge \frac{1}{I_n(\theta)}$$

### Step-by-Step Proof:
1.  Since $\hat{\theta}$ is an unbiased estimator:
    $$\mathbb{E}[\hat{\theta}] = \int \hat{\theta}(x) f(x; \theta) \, dx = \theta$$
2.  Differentiate both sides with respect to $\theta$. Assuming regularity conditions allow passing the derivative inside the integral:
    $$\frac{\partial}{\partial \theta} \int \hat{\theta}(x) f(x; \theta) \, dx = \frac{\partial}{\partial \theta} (\theta) \implies \int \hat{\theta}(x) \frac{\partial f(x; \theta)}{\partial \theta} \, dx = 1$$
3.  Use the logarithmic identity $\frac{\partial f}{\partial \theta} = f \frac{\partial \log f}{\partial \theta}$:
    $$\int \hat{\theta}(x) \left( \frac{\partial \log f(x; \theta)}{\partial \theta} \right) f(x; \theta) \, dx = 1 \implies \mathbb{E}\left[ \hat{\theta} \frac{\partial \log f(X; \theta)}{\partial \theta} \right] = 1$$
4.  Note that the expected value of the score function is zero:
    $$\mathbb{E}\left[ \frac{\partial \log f(X; \theta)}{\partial \theta} \right] = \int \frac{\partial \log f(x; \theta)}{\partial \theta} f(x; \theta) \, dx = \int \frac{\partial f(x; \theta)}{\partial \theta} \, dx = \frac{\partial}{\partial \theta} \int f(x; \theta) \, dx = \frac{\partial}{\partial \theta} (1) = 0$$
5.  Using step 4, we can subtract $\theta \mathbb{E}\left[ \frac{\partial \log f(X;\theta)}{\partial \theta} \right] = 0$ from step 3:
    $$\mathbb{E}\left[ (\hat{\theta} - \theta) \frac{\partial \log f(X; \theta)}{\partial \theta} \right] = 1$$
6.  Apply the Cauchy-Schwarz Inequality for random variables, $|\mathbb{E}[UV]| \le \sqrt{\mathbb{E}[U^2] \mathbb{E}[V^2]}$:
    $$\left( \mathbb{E}\left[ (\hat{\theta} - \theta) \frac{\partial \log f(X; \theta)}{\partial \theta} \right] \right)^2 \le \mathbb{E}[(\hat{\theta} - \theta)^2] \, \mathbb{E}\left[ \left( \frac{\partial \log f(X; \theta)}{\partial \theta} \right)^2 \right]$$
7.  Substitute the LHS value ($1^2 = 1$) and definitions:
    *   $\mathbb{E}[(\hat{\theta} - \theta)^2] = \text{Var}(\hat{\theta})$ (since $\hat{\theta}$ is unbiased)
    *   $\mathbb{E}\left[ \left( \frac{\partial \log f(X; \theta)}{\partial \theta} \right)^2 \right] = I_n(\theta)$
    $$1 \le \text{Var}(\hat{\theta}) \cdot I_n(\theta)$$
8.  Divide by $I_n(\theta)$ (which is positive):
    $$\text{Var}(\hat{\theta}) \ge \frac{1}{I_n(\theta)}$$

The Cramér-Rao Lower Bound is proved.

---

## ✍️ Step-by-Step Proof: Asymptotic Normality of the MLE

### Theorem:
Let $\hat{\theta}_n$ be the Maximum Likelihood Estimator of $\theta_0$. Under regularity conditions:

$$\sqrt{n}(\hat{\theta}_n - \theta_0) \xrightarrow{d} \mathcal{N}\left(0, \frac{1}{I(\theta_0)}\right)$$

### Step-by-Step Proof:
1.  By definition, the MLE $\hat{\theta}_n$ maximizes the log-likelihood $l(\theta) = \sum_{i=1}^{n} \log f(x_i; \theta)$. Therefore, the derivative (score function) at the MLE must equal zero:
    $$l'(\hat{\theta}_n) = 0$$
2.  We perform a first-order Taylor expansion of $l'(\theta)$ around the true parameter value $\theta_0$:
    $$0 = l'(\hat{\theta}_n) \approx l'(\theta_0) + l''(\bar{\theta}) (\hat{\theta}_n - \theta_0)$$
    Where $\bar{\theta}$ lies between $\hat{\theta}_n$ and $\theta_0$.
3.  Isolate the parameter difference $(\hat{\theta}_n - \theta_0)$:
    $$\hat{\theta}_n - \theta_0 = -\frac{l'(\theta_0)}{l''(\bar{\theta})}$$
4.  Multiply both sides by $\sqrt{n}$ and scale the denominator and numerator by $\frac{1}{n}$:
    $$\sqrt{n}(\hat{\theta}_n - \theta_0) = \frac{\frac{1}{\sqrt{n}} l'(\theta_0)}{-\frac{1}{n} l''(\bar{\theta})}$$
5.  **Analyze the Numerator**: The term $\frac{1}{\sqrt{n}} l'(\theta_0)$ can be expressed as:
    $$\frac{1}{\sqrt{n}} l'(\theta_0) = \sqrt{n} \left( \frac{1}{n} \sum_{i=1}^{n} \frac{\partial \log f(x_i; \theta_0)}{\partial \theta} \right)$$
    Since $\mathbb{E}[\frac{\partial \log f(X; \theta_0)}{\partial \theta}] = 0$ and the variance of each term is the Fisher Information $I(\theta_0)$, by the **Central Limit Theorem (CLT)**:
    $$\frac{1}{\sqrt{n}} l'(\theta_0) \xrightarrow{d} \mathcal{N}(0, I(\theta_0))$$
6.  **Analyze the Denominator**: By the **Weak Law of Large Numbers (WLLN)**, as $n \to \infty$ and $\hat{\theta}_n \to \theta_0$ (consistency of the MLE), the average second derivative converges to the expected value:
    $$-\frac{1}{n} l''(\bar{\theta}) = -\frac{1}{n} \sum_{i=1}^{n} \frac{\partial^2 \log f(x_i; \bar{\theta})}{\partial \theta^2} \xrightarrow{p} -\mathbb{E}\left[ \frac{\partial^2 \log f(X; \theta_0)}{\partial \theta^2} \right] = I(\theta_0)$$
7.  **Apply Slutsky's Theorem**: Combine the convergence of the numerator (distribution) and denominator (probability):
    $$\sqrt{n}(\hat{\theta}_n - \theta_0) \xrightarrow{d} \frac{\mathcal{N}(0, I(\theta_0))}{I(\theta_0)} = \mathcal{N}\left( 0, \frac{I(\theta_0)}{I(\theta_0)^2} \right) = \mathcal{N}\left( 0, \frac{1}{I(\theta_0)} \right)$$

The theorem is proved.

---

## 🧮 Worked-Out Parametric MLE Solutions

### 1. The Gaussian Distribution ($\mu, \sigma^2$)
Given $n$ i.i.d. observations $X_i \sim \mathcal{N}(\mu, \sigma^2)$, find the MLE estimators for both parameters.

#### Log-Likelihood Formulation:
$$f(x; \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$
$$L(\mu, \sigma^2) = \prod_{i=1}^{n} \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{(x_i-\mu)^2}{2\sigma^2}} = (2\pi\sigma^2)^{-n/2} e^{-\sum_{i=1}^{n}\frac{(x_i-\mu)^2}{2\sigma^2}}$$
$$l(\mu, \sigma^2) = -\frac{n}{2}\log(2\pi) - \frac{n}{2}\log(\sigma^2) - \sum_{i=1}^{n}\frac{(x_i-\mu)^2}{2\sigma^2}$$

#### Solve for $\hat{\mu}_{MLE}$:
Take the partial derivative with respect to $\mu$ and set to zero:
$$\frac{\partial l}{\partial \mu} = \frac{2}{2\sigma^2} \sum_{i=1}^{n} (x_i - \mu) = \frac{1}{\sigma^2} \left( \sum_{i=1}^{n} x_i - n\mu \right) = 0 \implies \hat{\mu}_{MLE} = \frac{1}{n}\sum_{i=1}^{n} x_i = \bar{X}$$

#### Solve for $\hat{\sigma}^2_{MLE}$:
Take the partial derivative with respect to $\sigma^2$ and set to zero (let $v = \sigma^2$):
$$\frac{\partial l}{\partial v} = -\frac{n}{2v} + \frac{1}{2v^2} \sum_{i=1}^{n}(x_i - \mu)^2 = 0 \implies \frac{n}{2v} = \frac{1}{2v^2}\sum_{i=1}^{n}(x_i - \mu)^2 \implies \hat{\sigma}^2_{MLE} = \frac{1}{n}\sum_{i=1}^{n}(x_i - \bar{X})^2$$

---

### 2. The Bernoulli Trial ($p$)
Given $n$ i.i.d. binary observations $X_i \sim \text{Bernoulli}(p)$ where $X_i \in \{0, 1\}$.

#### Log-Likelihood Formulation:
$$f(x; p) = p^x (1-p)^{1-x}$$
$$L(p) = \prod_{i=1}^{n} p^{x_i} (1-p)^{1-x_i} = p^{\sum x_i} (1-p)^{n - \sum x_i}$$
$$l(p) = \left(\sum_{i=1}^{n} x_i\right) \log(p) + \left(n - \sum_{i=1}^{n} x_i\right) \log(1-p)$$

#### Solve for $\hat{p}_{MLE}$:
Take the derivative and set to zero (let $S = \sum x_i$):
$$\frac{\partial l}{\partial p} = \frac{S}{p} - \frac{n - S}{1-p} = 0 \implies S(1-p) = p(n - S) \implies S - Sp = np - Sp \implies \hat{p}_{MLE} = \frac{S}{n} = \bar{X}$$

#### Fisher Information & Variance:
$$\frac{\partial^2 l}{\partial p^2} = -\frac{S}{p^2} - \frac{n-S}{(1-p)^2}$$
$$I(p) = -\mathbb{E}\left[ \frac{\partial^2 \log f(X; p)}{\partial p^2} \right] = -\mathbb{E}\left[ -\frac{X}{p^2} - \frac{1-X}{(1-p)^2} \right] = \frac{p}{p^2} + \frac{1-p}{(1-p)^2} = \frac{1}{p} + \frac{1}{1-p} = \frac{1}{p(1-p)}$$
$$\text{Asymptotic Variance of } \hat{p}_{MLE} = \frac{1}{I_n(p)} = \frac{p(1-p)}{n}$$

---

## 📈 Quant & Data Science Bridging

### 1. Estimating Asset Return Volatility
In quantitative finance, the daily return series of an asset is often modeled using a Gaussian distribution $\mathcal{N}(\mu, \sigma^2)$. The MLE estimator for variance $\hat{\sigma}^2_{MLE} = \frac{1}{n}\sum (x_i - \bar{X})^2$ is highly efficient but has a minor bias: $\mathbb{E}[\hat{\sigma}^2_{MLE}] = \frac{n-1}{n}\sigma^2$.

*   **The Adjustment**: Risk models and trading platforms scale this to the unbiased sample variance $s^2 = \frac{1}{n-1}\sum (x_i - \bar{X})^2$ to avoid underestimating volatility during risk evaluation.

### 2. Maximum Likelihood in Logistic Trading Classifiers
When data scientists train a binary logistic classifier to predict whether a stock will rise ($Y=1$) or fall ($Y=0$) tomorrow based on a feature vector $\mathbf{x}$, the model output is parameterized using the sigmoid function:

$$P(Y=1 \mid \mathbf{x}) = \sigma(\boldsymbol{\beta}^T \mathbf{x}) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}$$

*   **Fitting the Model**: We cannot fit this model using linear regression because the output is binary. Instead, we compile the **Log-Likelihood** function (equivalent to the Bernoulli trial MLE):
    $$l(\boldsymbol{\beta}) = \sum_{i=1}^{n} \left[ y_i \log(\sigma(\boldsymbol{\beta}^T \mathbf{x}_i)) + (1-y_i) \log(1 - \sigma(\boldsymbol{\beta}^T \mathbf{x}_i)) \right]$$
*   By maximizing this log-likelihood (commonly implemented as minimizing the negative log-likelihood or **Binary Cross-Entropy Loss**), the optimizer fits the optimal weight vector $\boldsymbol{\beta}$ that maximizes the probability of generating the observed historical training sequences.
