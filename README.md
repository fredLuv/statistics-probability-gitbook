# Mathematical Statistics & Probability: The Core Quantitative Engines

Welcome to the ultimate study GitBook combining two foundational pillars of quantitative finance and data science: **Larry Wasserman's "All of Statistics: A Concise Course in Statistical Inference"** and **Frederick Mosteller's "Fifty Challenging Problems in Probability with Solutions"**.

This manual is specifically designed for quantitative researchers, algorithmic traders, and data scientists seeking to bridge rigorous, measure-theoretic mathematical theory with practical modeling and testing.

---

## 🎯 The Core Thesis

Quantitative trading and machine learning are fundamentally exercises in **statistical inference under uncertainty**. We seek to extract trading signals (alpha) from noisy financial time series, validate these signals using rigorous hypothesis testing to avoid backtest overfitting, and model future outcomes using conditional probability.

*   **Larry Wasserman's *All of Statistics*** provides a highly condensed, mathematically rigorous exposition of parametric and non-parametric estimation, hypothesis testing, Bayesian decision theory, and statistical learning.
*   **Frederick Mosteller's *Fifty Challenging Problems in Probability*** builds the tactical, intuitive problem-solving skills needed to analyze probability systems and excel in quantitative research interview loops.

---

## 🔑 Core Frameworks & Theorems Covered

### 1. Measure-Theoretic Probability Spaces
A mathematically rigorous foundation of random variables built on probability triplets $(\Omega, \mathcal{F}, P)$, Borel $\sigma$-algebras, and Kolmogorov’s axioms.

### 2. Maximum Likelihood Estimation (MLE) & Fisher Information
Detailed mathematical derivations of parameter estimation, asymptotic normality, and the **Cramér-Rao Lower Bound**—proving the maximum possible efficiency of any unbiased estimator.

### 3. Hypothesis Testing & Neyman-Pearson
Type I and Type II error minimization, Wald and Likelihood Ratio tests, and a complete algebraic proof of the **Neyman-Pearson Lemma** to establish the most powerful statistical test.

### 4. Bayesian Decision Theory & Loss Functions
Posterior derivations for conjugate prior systems (Beta-Binomial, Normal-Normal) and Bayesian decision rules under Squared Error, Absolute Error, and 0-1 loss functions.

### 5. Regression, KDE & Bootstrap Resampling
Matrix derivations of OLS coefficients and covariance matrices, logistic regression log-likelihoods, Non-parametric Kernel Density Estimation (KDE), and the mathematical proof of Bootstrap variance convergence.

### 6. Challenging Probability Puzzles
An extensive, worked-out appendix containing step-by-step probability algebra solutions to classic quant brainteasers, including the Birthday Paradox, the Gambler's Ruin, Bertrand's Paradox, and continuous random walks.

---

## 📖 GitBook Structure

### 📂 [Part 1: Statistical Inference (Larry Wasserman)](chapters/)
*   **[Chapter 1: Probability Spaces, Random Variables, and Inequalities](chapters/ch1_probability_spaces.md)** — Measure-theoretic probability, CDF properties, expectations, and five core inequality proofs.
*   **[Chapter 2: Estimation Frameworks & Maximum Likelihood Estimation](chapters/ch2_mle_estimation.md)** — Bias-variance tradeoff, Fisher Information, Cramér-Rao proof, MLE asymptotic normality, and worked MLE solutions.
*   **[Chapter 3: Hypothesis Testing, p-Values, and Neyman-Pearson](chapters/ch3_hypothesis_testing.md)** — Wald/LR/Score tests, Neyman-Pearson Lemma proof, and Multiple Testing corrections in quant backtests.
*   **[Chapter 4: Bayesian Inference & Decision Theory](chapters/ch4_bayesian_inference.md)** — Conjugate priors, posterior derivations, Bayesian loss functions, and credible intervals.
*   **[Chapter 5: Regression Models, Non-Parametrics & Resampling](chapters/ch5_regression_bootstrap.md)** — Matrix OLS derivations, logistic regressions, Kernel Density Estimation, and Bootstrap resampling theory.

### 📂 [Part 2: Probability Appendix (Frederick Mosteller)](appendix/)
*   **[Worked Solutions to Challenging Probability Problems](appendix/challenging_probability.md)** — Exhaustive worked solutions to eight classic quantitative probability puzzles.
