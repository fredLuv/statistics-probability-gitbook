# Chapter 3: Hypothesis Testing, p-Values, and Neyman-Pearson

## 🎯 Core Thesis
Hypothesis testing provides a formal mathematical framework to make decisions using data. Larry Wasserman details that we set up a default assumption, the **Null Hypothesis** ($H_0$), and test if the observed data deviates significantly enough to warrant rejecting it in favor of the **Alternative Hypothesis** ($H_1$). 

Every test carries risks of errors: rejecting a true null (Type I error, $\alpha$) or failing to reject a false null (Type II error, $\beta$). 

This chapter details the mathematical structure of hypothesis testing—including the three fundamental classical tests (Wald, Likelihood Ratio, and Score)—presents a rigorous proof of the **Neyman-Pearson Lemma** (defining the most powerful test), and details corrections for **Multiple Testing** to prevent backtest overfitting in quantitative research.

---

## 🔑 Key Mathematical Formulations

### 1. Error Rates and Power
Let $\alpha$ be the probability of committing a Type I error, and let $\beta$ be the probability of committing a Type II error.

| State of Nature | Decision: Fail to Reject $H_0$ | Decision: Reject $H_0$ |
| :--- | :--- | :--- |
| **$H_0$ is True** | Correct Decision ($1 - \alpha$) | **Type I Error ($\alpha$, Significance)** |
| **$H_0$ is False** | **Type II Error ($\beta$)** | Correct Decision ($1 - \beta$, Power) |

*   **Significance Level ($\alpha$)**: The maximum allowed probability of committing a Type I error.
*   **Power ($1 - \beta$)**: The probability of correctly rejecting the null hypothesis when it is false.

---

### 2. The Holy Trinity of Hypothesis Tests
We test the null hypothesis $H_0 : \theta = \theta_0$ against $H_1 : \theta \neq \theta_0$ using three classical formulations. As $n \to \infty$, all three test statistics converge to a **$\chi_1^2$ distribution** under $H_0$:

```
        Log-Likelihood
             ▲             Score Test (Slope at theta_0)
             │                 \      
             │              * * * * *
             │            *      \     * ◄─── Wald Test (Distance on x-axis)
             │          *         \      *
             │        *            \       *
             │      *               \        * ◄─── Likelihood Ratio Test 
             │                                      (Distance on y-axis)
             └────────────────────────────────────────────────────────► theta
                               theta_0     theta_MLE
```

*   **The Wald Test**: Measures the horizontal distance between the MLE and the null value:
    $$W = \frac{(\hat{\theta}_{MLE} - \theta_0)^2}{\text{Var}(\hat{\theta}_{MLE})} \xrightarrow{d} \chi_1^2$$
*   **The Likelihood Ratio Test (LRT)**: Measures the vertical distance between the log-likelihood curves:
    $$\Lambda = 2 \left( l(\hat{\theta}_{MLE}) - l(\theta_0) \right) \xrightarrow{d} \chi_1^2$$
*   **The Score (Rao) Test**: Measures the slope of the log-likelihood curve (the score) evaluated at the null value $\theta_0$:
    $$S = \frac{\left( l'(\theta_0) \right)^2}{I_n(\theta_0)} \xrightarrow{d} \chi_1^2$$

---

## ✍️ Step-by-Step Proof: The Neyman-Pearson Lemma

### Theorem:
Let $X_1, \dots, X_n$ be a sample from a distribution. Consider testing the simple null hypothesis $H_0 : \theta = \theta_0$ against the simple alternative $H_1 : \theta = \theta_1$. Let $L(x; \theta)$ represent the likelihood function.

For a fixed significance level $\alpha$, the **Likelihood Ratio Test** with rejection region $R$:

$$R = \left\{ x : \frac{L(x; \theta_1)}{L(x; \theta_0)} \ge k \right\}$$

is the **Most Powerful Test** of size $\alpha$. That is, for any other test with rejection region $S$ of size $\le \alpha$, the power of the likelihood ratio test is greater than or equal to the power of the second test:

$$\text{Power}(R) \ge \text{Power}(S)$$

---

### Step-by-Step Proof:
1.  Let $\phi_R(x)$ and $\phi_S(x)$ represent the critical indicator functions for the two tests, where $\phi(x) = 1$ if $x \in \text{Rejection Region}$ and $\phi(x) = 0$ otherwise.
2.  By the size constraint of the tests under $H_0$:
    $$\mathbb{E}_0[\phi_R(X)] = \int \phi_R(x) L(x; \theta_0) \, dx = \alpha$$
    $$\mathbb{E}_0[\phi_S(X)] = \int \phi_S(x) L(x; \theta_0) \, dx \le \alpha$$
    This implies:
    $$\int (\phi_R(x) - \phi_S(x)) L(x; \theta_0) \, dx \ge 0$$
3.  We want to show that the power under $H_1$ satisfies:
    $$\int (\phi_R(x) - \phi_S(x)) L(x; \theta_1) \, dx \ge 0$$
4.  Consider the product $(\phi_R(x) - \phi_S(x))(L(x; \theta_1) - k L(x; \theta_0))$ for any point $x$:
    *   **Case 1**: If $x \in R$, then $\phi_R(x) = 1$. Since $\phi_S(x) \in [0, 1]$, the term $(\phi_R(x) - \phi_S(x)) \ge 0$. Also, by the definition of the region $R$, we have $L(x; \theta_1) \ge k L(x; \theta_0) \implies L(x; \theta_1) - k L(x; \theta_0) \ge 0$.
        *   *Result:* The product of two non-negative terms is non-negative ($\ge 0$).
    *   **Case 2**: If $x \notin R$, then $\phi_R(x) = 0 \implies (\phi_R(x) - \phi_S(x)) \le 0$. By definition, $L(x; \theta_1) < k L(x; \theta_0) \implies L(x; \theta_1) - k L(x; \theta_0) < 0$.
        *   *Result:* The product of two negative terms is non-negative ($\ge 0$).
5.  Therefore, for all possible values of $x$:
    $$(\phi_R(x) - \phi_S(x))(L(x; \theta_1) - k L(x; \theta_0)) \ge 0$$
6.  Integrate this product over the entire space:
    $$\int (\phi_R(x) - \phi_S(x))(L(x; \theta_1) - k L(x; \theta_0)) \, dx \ge 0$$
    $$\int (\phi_R(x) - \phi_S(x)) L(x; \theta_1) \, dx - k \int (\phi_R(x) - \phi_S(x)) L(x; \theta_0) \, dx \ge 0$$
7.  Rearrange terms:
    $$\int (\phi_R(x) - \phi_S(x)) L(x; \theta_1) \, dx \ge k \int (\phi_R(x) - \phi_S(x)) L(x; \theta_0) \, dx$$
8.  From Step 2, we know that $\int (\phi_R(x) - \phi_S(x)) L(x; \theta_0) \, dx \ge 0$. Since $k > 0$, the RHS of the inequality is non-negative:
    $$k \int (\phi_R(x) - \phi_S(x)) L(x; \theta_0) \, dx \ge 0$$
9.  Therefore, the LHS must also be non-negative:
    $$\int (\phi_R(x) - \phi_S(x)) L(x; \theta_1) \, dx \ge 0 \implies \int \phi_R(x) L(x; \theta_1) \, dx \ge \int \phi_S(x) L(x; \theta_1) \, dx$$
10. Recall that the power of a test is the expectation of the critical function under $H_1$:
    $$\text{Power}(R) \ge \text{Power}(S)$$

The Neyman-Pearson Lemma is proved.

---

## 📈 Quant & Data Science Bridging

### The Multiple Testing Crisis & Backtest Overfitting
In quantitative trading and machine learning, researchers search for profitable strategies by testing thousands of candidates. Let $m$ be the number of distinct trading indicators tested. For each strategy, we test:
$$H_0 : \text{Strategy Sharpe Ratio } = 0 \quad \text{against} \quad H_1 : \text{Strategy Sharpe Ratio } > 0$$

*   **The Trap (Family-Wise Error Rate)**: If we test $m = 1,000$ strategies using a standard significance level $\alpha = 0.05$ (meaning a $5\%$ Type I error rate per test), the probability of finding **at least one** strategy that appears highly profitable purely by random chance is:
    $$\text{FWER} = 1 - (1 - \alpha)^m = 1 - (0.95)^{1,000} \approx 0.999999...$$
    You are guaranteed to discover strategies that look spectacular in the backtest but are actually completely unprofitable random noise. This is called **p-hacking** or **backtest overfitting**.

#### Tactical Mitigations:
1.  **The Bonferroni Correction**:
    To control the FWER at a target level $\alpha$, we adjust the significance threshold for each individual test to:
    $$\alpha_{adj} = \frac{\alpha}{m}$$
    *   *Example:* If $m = 1,000$ and target $\alpha = 0.05$, then $\alpha_{adj} = 0.00005$. A strategy is only accepted if its individual p-value is below $0.00005$.
2.  **False Discovery Rate (FDR) - Benjamini-Hochberg Procedure**:
    The Bonferroni correction is often too conservative, rejecting true alpha strategies. Instead, quants control the **False Discovery Rate** (the expected proportion of rejected nulls that are false discoveries):
    *   Sort the $m$ individual strategy p-values in ascending order: $p_{(1)} \le p_{(2)} \le \dots \le p_{(m)}$.
    *   Find the largest rank $k$ such that:
        $$p_{(k)} \le \frac{k}{m} q$$
        Where $q$ is the target FDR (e.g. $10\%$).
    *   Reject $H_0$ for all strategies ranked $1$ to $k$. This procedure controls the false discovery proportion while retaining statistical power to discover real trading anomalies.
