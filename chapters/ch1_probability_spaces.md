# Chapter 1: Probability Spaces, Random Variables, and Inequalities

## 🎯 Core Thesis
Probability theory is mathematically formalized not as simple counts of outcomes, but as a branch of measure theory. Larry Wasserman defines the foundations of probability using a rigorous mathematical triplet: the **Probability Space** $(\Omega, \mathcal{F}, P)$. 

Random variables act as measurable functions mapping outcomes from this sample space to real numbers. Understanding the exact probability distribution requires analyzing the properties of Cumulative Distribution Functions (CDFs) and Probability Density Functions (PDFs). 

Furthermore, **probability inequalities** provide the analytical bounds required to constrain random variables without knowing their exact distributions, serving as the foundational bounds for asymptotic convergence theorems (such as the Law of Large Numbers).

---

## 🔑 Key Mathematical Formulations

### 1. The Measure-Theoretic Probability Space Triplet $(\Omega, \mathcal{F}, P)$
A probability space is defined by three components:
1.  **$\Omega$ (Sample Space)**: The set of all possible outcomes of a random experiment.
2.  **$\mathcal{F}$ ($\sigma$-algebra / $\sigma$-field)**: A collection of subsets of $\Omega$ (events) that contains $\Omega$ itself and is closed under complements and countable unions:
    *   $\Omega \in \mathcal{F}$
    *   If $A \in \mathcal{F}$, then $A^c \in \mathcal{F}$
    *   If $A_1, A_2, \dots \in \mathcal{F}$, then $\bigcup_{i=1}^{\infty} A_i \in \mathcal{F}$
    *   *Borel $\sigma$-algebra $\mathcal{B}(\mathbb{R})$*: The smallest $\sigma$-algebra containing all open intervals in $\mathbb{R}$, mandatory for defining continuous random variables.
3.  **$P$ (Probability Measure)**: A function $P : \mathcal{F} \to [0, 1]$ satisfying **Kolmogorov's Axioms**:
    *   *Non-negativity*: $P(A) \ge 0$ for all $A \in \mathcal{F}$
    *   *Normalization*: $P(\Omega) = 1$
    *   *Countable Additivity*: If $A_1, A_2, \dots \in \mathcal{F}$ are mutually disjoint events ($A_i \cap A_j = \emptyset$ for $i \ne j$), then:

$$P\left( \bigcup_{i=1}^{\infty} A_i \right) = \sum_{i=1}^{\infty} P(A_i)$$

---

### 2. Random Variables & Cumulative Distribution Functions (CDFs)
A random variable $X$ is a measurable function $X : \Omega \to \mathbb{R}$ such that for every Borel set $B \in \mathcal{B}(\mathbb{R})$, the preimage is an event: $\{\omega \in \Omega : X(\omega) \in B\} \in \mathcal{F}$.

The **Cumulative Distribution Function (CDF)** $F_X(x)$ completely characterizes the distribution of $X$:

$$F_X(x) = P(X \le x)$$

*   **Properties of CDFs**:
    1.  **Monotonically Non-decreasing**: If $x_1 < x_2$, then $F_X(x_1) \le F_X(x_2)$.
    2.  **Normalized Limits**: 

$$\lim_{x \to -\infty} F_X(x) = 0 \quad \text{and} \quad \lim_{x \to \infty} F_X(x) = 1$$

    3.  **Right-Continuous**: For every $a \in \mathbb{R}$:

$$\lim_{x \to a^+} F_X(x) = F_X(a)$$

---

### 3. Expectation, Variance, and Covariance Matrices
*   **Expectation (Inner Product Formulation)**:

$$\mathbb{E}[X] = \int_{-\infty}^{\infty} x \, dF_X(x) = \int_{-\infty}^{\infty} x f_X(x) \, dx \quad (\text{for continuous } X)$$

*   **Variance**:

$$\text{Var}(X) = \mathbb{E}[(X - \mathbb{E}[X])^2] = \mathbb{E}[X^2] - (\mathbb{E}[X])^2$$

*   **Covariance Matrix**: For a random vector $\mathbf{X} = [X_1, \dots, X_n]^T \in \mathbb{R}^n$, the covariance matrix $\Sigma \in \mathbb{R}^{n \times n}$ is:

$$\Sigma_{ij} = \text{Cov}(X_i, X_j) = \mathbb{E}[(X_i - \mathbb{E}[X_i])(X_j - \mathbb{E}[X_j])]$$

    *   *Cauchy-Schwarz for Random Variables:*

$$|\text{Cov}(X, Y)| \le \sqrt{\text{Var}(X) \text{Var}(Y)}$$

---

## ✍️ Step-by-Step Mathematical Proofs of Core Inequalities

### 1. Markov's Inequality
#### Theorem:
If $Y$ is a non-negative random variable ($\mathbb{E}[Y] \ge 0$) and $a > 0$:

$$P(Y \ge a) \le \frac{\mathbb{E}[Y]}{a}$$

#### Step-by-Step Proof:
1.  Let $I(Y \ge a)$ be an indicator random variable:

$$I(Y \ge a) = \begin{cases} 1 & \text{if } Y \ge a \\ 0 & \text{if } Y < a \end{cases}$$

2.  Since $Y$ is non-negative and $a > 0$, we have the inequality:

$$a I(Y \ge a) \le Y$$

    *   *Proof of indicator inequality:*
        *   If $Y < a$, the LHS is $0 \le Y$ (which is true because $Y \ge 0$).
        *   If $Y \ge a$, the LHS is $a \cdot 1 = a \le Y$ (which is true by assumption).
3.  Take the expectation on both sides (preserving the inequality by monotonicity of expectation):

$$\mathbb{E}[a I(Y \ge a)] \le \mathbb{E}[Y]$$

4.  Since $a$ is a constant, factor it out:

$$a \mathbb{E}[I(Y \ge a)] \le \mathbb{E}[Y]$$

5.  Recall that the expectation of an indicator variable is simply the probability of the event:

$$\mathbb{E}[I(Y \ge a)] = 1 \cdot P(Y \ge a) + 0 \cdot P(Y < a) = P(Y \ge a)$$

6.  Substitute this back into the inequality:

$$a P(Y \ge a) \le \mathbb{E}[Y]$$

7.  Divide by $a$ ($a > 0$):

$$P(Y \ge a) \le \frac{\mathbb{E}[Y]}{a}$$

Markov's Inequality is proved.

---

### 2. Chebyshev's Inequality
#### Theorem:
For any random variable $X$ with mean $\mu$ and variance $\sigma^2$, and $k > 0$:

$$P(|X - \mu| \ge k) \le \frac{\sigma^2}{k^2}$$

#### Step-by-Step Proof:
1.  Define the non-negative random variable $Y = (X - \mu)^2$, and let $a = k^2 > 0$.
2.  Apply Markov's Inequality to $Y$ and $a$:

$$P(Y \ge a) \le \frac{\mathbb{E}[Y]}{a}$$

3.  Substitute $Y = (X - \mu)^2$ and $a = k^2$:

$$P((X - \mu)^2 \ge k^2) \le \frac{\mathbb{E}[(X - \mu)^2]}{k^2}$$

4.  Note that $(X - \mu)^2 \ge k^2$ is algebraically equivalent to $|X - \mu| \ge k$.
5.  Recall that by definition, $\mathbb{E}[(X - \mu)^2] = \text{Var}(X) = \sigma^2$.
6.  Substitute these equivalents back into the inequality:

$$P(|X - \mu| \ge k) \le \frac{\sigma^2}{k^2}$$

Chebyshev's Inequality is proved.

---

### 3. Jensen's Inequality
#### Theorem:
If $g$ is a convex function, then for any random variable $X$:

$$\mathbb{E}[g(X)] \ge g(\mathbb{E}[X])$$

#### Step-by-Step Proof:
1.  Let $\mu = \mathbb{E}[X]$.
2.  Since $g$ is a convex function, there exists a tangent line to the curve $g(x)$ at the point $x = \mu$. Let this line be $L(x) = a + bx$.
3.  By the mathematical definition of convexity, the function curve $g(x)$ lies entirely **above or on** its tangent line for all $x$:

$$g(x) \ge a + bx \quad \text{for all } x$$

4.  Evaluate the line at $x = \mu$: since the tangent touches the curve at this point:

$$g(\mu) = a + b\mu$$

5.  Substitute the random variable $X$ into the inequality:

$$g(X) \ge a + bX$$

6.  Take the expectation on both sides:

$$\mathbb{E}[g(X)] \ge \mathbb{E}[a + bX]$$

7.  By linearity of expectation:

$$\mathbb{E}[g(X)] \ge a + b\mathbb{E}[X]$$

8.  Recall $\mathbb{E}[X] = \mu$:

$$\mathbb{E}[g(X)] \ge a + b\mu$$

9.  Substitute $g(\mu)$ for $a + b\mu$:

$$\mathbb{E}[g(X)] \ge g(\mu) \implies \mathbb{E}[g(X)] \ge g(\mathbb{E}[X])$$

Jensen's Inequality is proved.

---

### 4. Hoeffding's Inequality (Chernoff Bound Derivation)
#### Theorem:
Let $X_1, X_2, \dots, X_n$ be independent random variables such that $X_i \in [a_i, b_i]$ almost surely. Let $S_n = \sum_{i=1}^{n} X_i$. For any $\epsilon > 0$:

$$P(S_n - \mathbb{E}[S_n] \ge \epsilon) \le e^{-2\epsilon^2 / \sum_{i=1}^{n}(b_i - a_i)^2}$$

#### Step-by-Step Proof Outline:
1.  **Apply the Chernoff Bound Technique**: For any $t > 0$, the event $S_n - \mathbb{E}[S_n] \ge \epsilon$ is equivalent to $e^{t(S_n - \mathbb{E}[S_n])} \ge e^{t\epsilon}$.
2.  Apply Markov's Inequality:

$$P(S_n - \mathbb{E}[S_n] \ge \epsilon) = P\left(e^{t(S_n - \mathbb{E}[S_n])} \ge e^{t\epsilon}\right) \le e^{-t\epsilon} \mathbb{E}\left[e^{t(S_n - \mathbb{E}[S_n])}\right]$$

3.  By independence, the expectation of the product factors:

$$\mathbb{E}\left[e^{t(S_n - \mathbb{E}[S_n])}\right] = \prod_{i=1}^{n} \mathbb{E}\left[e^{t(X_i - \mathbb{E}[X_i])}\right]$$

4.  **Hoeffding's Lemma**: For any random variable $Y \in [a, b]$ with $\mathbb{E}[Y] = 0$:

$$\mathbb{E}[e^{tY}] \le e^{t^2(b - a)^2 / 8}$$

5.  Substitute the Lemma back into the product:

$$P(S_n - \mathbb{E}[S_n] \ge \epsilon) \le e^{-t\epsilon} \prod_{i=1}^{n} e^{t^2(b_i - a_i)^2 / 8} = e^{-t\epsilon} e^{\frac{t^2}{8}\sum_{i=1}^{n}(b_i - a_i)^2}$$

6.  **Optimize for $t$**: To find the tightest possible bound, we minimize the exponent with respect to $t$. Let $C = \sum_{i=1}^{n}(b_i - a_i)^2$. The exponent is $f(t) = -t\epsilon + \frac{t^2}{8}C$.
7.  Take the derivative and set to zero:

$$f'(t) = -\epsilon + \frac{t}{4}C = 0 \implies t^* = \frac{4\epsilon}{C}$$

8.  Substitute $t^*$ back into the inequality:

$$P(S_n - \mathbb{E}[S_n] \ge \epsilon) \le e^{-t^*\epsilon + \frac{(t^*)^2}{8}C} = e^{-\frac{4\epsilon^2}{C} + \frac{16\epsilon^2}{8C^2}C} = e^{-\frac{4\epsilon^2}{C} + \frac{2\epsilon^2}{C}} = e^{-2\epsilon^2 / C}$$

Hoeffding's Inequality is proved.

---

## 📈 Quant & Data Science Bridging

### 1. Risk Bounds on Algorithmic Drawdowns (Hoeffding's Bound)
In quantitative trading, let $X_1, X_2, \dots, X_n$ represent the daily returns of an algorithmic trading strategy, bounded such that daily losses are capped at $a_i = -2\%$ and gains capped at $b_i = +3\%$. We want to find the probability that our cumulative return $S_n$ drops below its expected value by a massive margin $\epsilon$.

*   By utilizing **Hoeffding's Inequality**, risk managers can calculate the absolute worst-case drawdown probabilities **completely independent of the return distribution**. 
*   Unlike standard VAR (Value at Risk) models that assume a Gaussian return profile (which underestimates fat-tail drawdowns), Hoeffding provides a distribution-free mathematical ceiling on the probability of tail-risk drawdowns, protecting the fund from catastrophic leverage blowups.

### 2. Option Pricing Bounds via Jensen's Inequality
Option pricing relies heavily on the pricing function of call options, which is a **convex function** of the underlying asset price: $g(S_t) = \max(S_t - K, 0)$.

*   By applying **Jensen's Inequality**, the expected future payoff of the option satisfies:

$$\mathbb{E}[\max(S_t - K, 0)] \ge \max(\mathbb{E}[S_t] - K, 0)$$

*   In a risk-neutral pricing framework, $\mathbb{E}[S_t] = S_0 e^{rt}$. Substituting this shows that the price of a European call option must always be greater than or equal to its discounted intrinsic value:

$$\text{Call Option Price} \ge \max(S_0 - K e^{-rt}, 0)$$

Jensen's inequality establishes the physical lower bound for option pricing, preventing arbitrage opportunities in options markets.