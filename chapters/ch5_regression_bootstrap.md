# Chapter 5: Regression Models, Non-Parametrics & Resampling

## 🎯 Core Thesis
Much of quantitative modeling and data science focuses on estimating the relationships between variables and constructing non-parametric representations of data. Larry Wasserman details the transition from classical parametric **regression models** (Linear and Logistic) to **non-parametric density estimation** (Kernel Density Estimation). 

Furthermore, when the underlying data-generating distribution is unknown and analytical math is intractable, **resampling methods**—specifically the **Bootstrap**—provide a mathematically rigorous, computer-intensive framework to estimate standard errors and confidence intervals. 

This chapter details the algebraic derivations of OLS regression matrices, logistic regression optimizations, KDE bandwidth tradeoffs, and the statistical convergence properties of the bootstrap.

---

## 🔑 Key Mathematical Formulations

### 1. Matrix Derivation of OLS Regression
Let $y = X\boldsymbol{\beta} + \boldsymbol{\epsilon}$ be a linear regression model where:
*   $y \in \mathbb{R}^n$ is the vector of dependent observations.
*   $X \in \mathbb{R}^{n \times d}$ is the design matrix of features (with full column rank $d$).
*   $\boldsymbol{\beta} \in \mathbb{R}^d$ is the vector of coefficient weights.
*   $\boldsymbol{\epsilon} \in \mathbb{R}^n$ is the vector of random errors satisfying $\mathbb{E}[\boldsymbol{\epsilon} \mid X] = \mathbf{0}$ and $\text{Var}(\boldsymbol{\epsilon} \mid X) = \sigma^2 I_n$ (Gauss-Markov assumptions).

#### Derivation of the Coefficients ($\hat{\boldsymbol{\beta}}$):
We seek $\boldsymbol{\beta}$ that minimizes the Residual Sum of Squares (RSS):
$$\text{RSS}(\boldsymbol{\beta}) = \|\mathbf{e}\|_2^2 = (y - X\boldsymbol{\beta})^T (y - X\boldsymbol{\beta})$$
$$\text{RSS}(\boldsymbol{\beta}) = y^T y - \boldsymbol{\beta}^T X^T y - y^T X \boldsymbol{\beta} + \boldsymbol{\beta}^T X^T X \boldsymbol{\beta} = y^T y - 2\boldsymbol{\beta}^T X^T y + \boldsymbol{\beta}^T X^T X \boldsymbol{\beta}$$

To find the minimum, take the gradient with respect to $\boldsymbol{\beta}$ and set to zero:
$$\nabla_{\boldsymbol{\beta}} \text{RSS}(\boldsymbol{\beta}) = -2X^T y + 2X^T X \boldsymbol{\beta} = \mathbf{0}$$
$$X^T X \boldsymbol{\beta} = X^T y \implies \hat{\boldsymbol{\beta}}_{OLS} = (X^T X)^{-1} X^T y$$

#### Derivation of the Covariance Matrix ($\text{Var}(\hat{\boldsymbol{\beta}})$):
$$\text{Var}(\hat{\boldsymbol{\beta}} \mid X) = \text{Var}\left( (X^T X)^{-1} X^T y \mid X \right)$$
Since $(X^T X)^{-1} X^T$ is a constant matrix given $X$, we apply the linear covariance transformation rule $\text{Var}(A y) = A \text{Var}(y) A^T$:
$$\text{Var}(\hat{\boldsymbol{\beta}} \mid X) = \left( (X^T X)^{-1} X^T \right) \text{Var}(y \mid X) \left( (X^T X)^{-1} X^T \right)^T$$
$$\text{Var}(\hat{\boldsymbol{\beta}} \mid X) = (X^T X)^{-1} X^T \left( \sigma^2 I_n \right) X (X^T X)^{-1}$$
$$\text{Var}(\hat{\boldsymbol{\beta}} \mid X) = \sigma^2 (X^T X)^{-1} X^T X (X^T X)^{-1} = \sigma^2 (X^T X)^{-1}$$

---

### 2. Logistic Regression & Log-Odds
For a binary classification model, we model the probability $p_i = P(y_i = 1 \mid \mathbf{x}_i)$ using the logistic link function:

$$\log\left(\frac{p_i}{1-p_i}\right) = \boldsymbol{\beta}^T \mathbf{x}_i \implies p_i = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i}}$$

*   **Log-Likelihood**:

$$l(\boldsymbol{\beta}) = \sum_{i=1}^{n} \left[ y_i \log(p_i) + (1-y_i)\log(1-p_i) \right]$$

*   **Newton-Raphson Optimization**: Since there is no closed-form algebraic solution for $\hat{\boldsymbol{\beta}}$, we optimize iteratively using the Hessian matrix $H$:

$$\boldsymbol{\beta}^{(new)} = \boldsymbol{\beta}^{(old)} - H^{-1} \nabla_{\boldsymbol{\beta}} l(\boldsymbol{\beta})$$

    Where the gradient and Hessian are:

$$\nabla_{\boldsymbol{\beta}} l(\boldsymbol{\beta}) = X^T (y - p), \quad H = -X^T W X \quad (\text{with diagonal } W_{ii} = p_i(1-p_i))$$

---

### 3. Non-Parametric Kernel Density Estimation (KDE)
To estimate the probability density function $f(x)$ without assuming a parametric family (e.g. Gaussian), we use KDE:

$$\hat{f}_h(x) = \frac{1}{n h} \sum_{i=1}^{n} K\left( \frac{x - x_i}{h} \right)$$

*   **$K(u)$ (Kernel)**: A symmetric, positive function integrating to 1 (often a standard Gaussian density).
*   **$h$ (Bandwidth)**: The smoothing parameter.
*   **The Bias-Variance Tradeoff of Bandwidth**:
    *   **Large $h$ (Over-smoothed)**: High bias, low variance. We wash out local details and peaks.
    *   **Small $h$ (Under-smoothed)**: Low bias, high variance. The density spikes at individual data points.
    *   *Asymptotic Mean Integrated Squared Error (AMISE)* is minimized at the optimal bandwidth $h^{\ast} \propto n^{-1/5}$.

---

## ✍️ Step-by-Step Mathematical Proof of Bootstrap Variance Convergence

Let $X_1, \dots, X_n \sim F$ be an i.i.d. sample from an unknown distribution $F$. We want to estimate a parameter $\theta = g(F)$ using a statistic $\hat{\theta}_n = s(X_1, \dots, X_n)$.

The frequentist objective is to find the variance of our estimator: $\text{Var}_F(\hat{\theta}_n)$. Since $F$ is unknown, we cannot compute this analytically. 

```
               POPULATION: F                                   SAMPLE: F_n
  [Draw n samples] ───► Standard sample               [Resample n with replacement] ──► Bootstrap sample
  [Compute variance] ──► True Var_F(theta)            [Compute variance] ───► Bootstrapped Var_Fn(theta*)
```

---

### The Bootstrap Formulation:
1.  We define the **Empirical Distribution Function (EDF)** $F_n$ as the discrete probability distribution that places probability weight $\frac{1}{n}$ on each observed data point $x_i$:

$$F_n(x) = \frac{1}{n}\sum_{i=1}^{n} I(X_i \le x)$$

2.  By the **Glivenko-Cantelli Theorem** (the fundamental theorem of statistics), as $n \to \infty$, the empirical distribution function $F_n$ converges **uniformly** and almost surely to the true data-generating distribution $F$:

$$\sup_x |F_n(x) - F(x)| \xrightarrow{a.s.} 0$$

3.  The **Bootstrap Principle** replaces the unknown true distribution $F$ with the observed empirical distribution $F_n$:

$$\text{True Variance: } \text{Var}_F(\hat{\theta}_n) \quad \approx \quad \text{Bootstrap Variance: } \text{Var}_{F_n}(\hat{\theta}_n^{\ast})$$

4.  To evaluate $\text{Var}_{F_n}(\hat{\theta}_n^{\ast})$ computationally, we draw $B$ bootstrap samples (size $n$ with replacement) from our empirical distribution $F_n$. Let $\theta^{\ast}_b$ be the statistic calculated on bootstrap sample $b$:

$$\hat{\sigma}^2_{boot} = \frac{1}{B-1}\sum_{b=1}^{B} \left( \theta^{\ast}_b - \bar{\theta}^{\ast} \right)^2 \quad \text{where} \quad \bar{\theta}^{\ast} = \frac{1}{B}\sum_{b=1}^{B} \theta^{\ast}_b$$

5.  By the **Weak Law of Large Numbers (WLLN)**, as the number of bootstrap simulations $B \to \infty$:

$$\hat{\sigma}^2_{boot} \xrightarrow{p} \text{Var}_{F_n}(\hat{\theta}_n^{\ast})$$

6.  By the **Glivenko-Cantelli Theorem** and the **Continuous Mapping Theorem**, as $n \to \infty$:

$$\text{Var}_{F_n}(\hat{\theta}_n^{\ast}) \xrightarrow{p} \text{Var}_F(\hat{\theta}_n)$$

Therefore, the simulated bootstrap variance is mathematically guaranteed to converge to the true, unknown population variance of our estimator.

---

## 📈 Quant & Data Science Bridging

### 1. Matrix Regression in Portfolio Hedging
In quantitative research, we hedge portfolio exposure to structural factors using multi-variate linear regression. Let $y \in \mathbb{R}^T$ represent the daily returns of our portfolio, and let $X \in \mathbb{R}^{T \times k}$ represent the daily returns of $k$ hedging factors.

*   The OLS matrix solution $\hat{\boldsymbol{\beta}} = (X^T X)^{-1} X^T y$ provides the exact portfolio allocation weights needed to construct the hedge.
*   The OLS covariance matrix $\text{Var}(\hat{\boldsymbol{\beta}}) = \sigma^2 (X^T X)^{-1}$ defines the **uncertainty bounds of our hedge weights**. If the hedging factors are highly correlated, $(X^T X)$ becomes near-singular, causing the diagonal entries of $(X^T X)^{-1}$ to spike. This warning indicates that our hedge weights are highly volatile and unstable, necessitating regularization.

### 2. Bootstrap Validation of Trading Strategy Sharpe Ratios
When a data scientist backtests a new quantitative trading strategy, they calculate its **Sharpe Ratio** based on $n$ daily return observations:

$$\text{Sharpe Ratio} = \frac{\bar{r} - r_f}{s_r}$$

*   **The Trap**: Standard formulas for standard errors of Sharpe Ratios assume that returns are independent and identically distributed (i.i.d.) Gaussian variables. However, physical financial returns have fat tails, volatility clustering, and serial autocorrelation, rendering the standard standard-error formulas completely invalid.
*   **The Bootstrap Solution**: Quants use **Stationary Block Bootstrap Resampling**. Instead of resampling individual days (which destroys time-series dependency), they resample contiguous blocks of daily returns of random lengths.
*   By generating 10,000 bootstrapped block return series, calculating the Sharpe Ratio on each, and constructing a $95\%$ credible percentile interval, they obtain a mathematically rigorous, distribution-free confidence bound on strategy performance, protecting the fund from deploying strategies driven by backtest randomness.