# Law of Large Numbers

> **Eigens4AI** | Probability & Statistics Foundations

---

## Table of Contents

1. [Intuition](#1-intuition)
2. [Setup & Notation](#2-setup--notation)
3. [Weak Law of Large Numbers (WLLN)](#3-weak-law-of-large-numbers-wlln)
4. [Markov's Inequality](#4-markovs-inequality)
5. [Proof of WLLN via Chebyshev's Inequality](#5-proof-of-wlln-via-chebyshevs-inequality)
6. [Strong Law of Large Numbers (SLLN)](#6-strong-law-of-large-numbers-slln)
7. [Weak vs Strong — Key Distinction](#7-weak-vs-strong--key-distinction)
8. [Variants & Generalisations](#8-variants--generalisations)
9. [Relevance to ML & Information Theory](#9-relevance-to-ml--information-theory)
10. [Summary](#10-summary)

---

## 1. Intuition

Flip a fair coin 10 times — you might get 7 heads. Flip it 10,000 times — the fraction of heads will be very close to 0.5. Flip it infinitely many times — it converges to exactly 0.5.

**The Law of Large Numbers formalises this:** the sample mean of i.i.d. random variables converges to the true population mean as the sample size grows.

There are two versions — **weak** and **strong** — which differ in the *type* of convergence.

---

## 2. Setup & Notation

Let $X_1, X_2, X_3, \dots$ be a sequence of **independent and identically distributed (i.i.d.)** random variables with:

- Common mean: $\mathbb{E}[X_i] = \mu$
- Common variance: $\mathrm{Var}(X_i) = \sigma^2 < \infty$

Define the **sample mean** after $n$ observations:

$$\bar{X}_n = \frac{1}{n} \sum_{i=1}^n X_i$$

Both laws concern the behaviour of $\bar{X}_n$ as $n \to \infty$.

---

## 3. Weak Law of Large Numbers (WLLN)

### Statement

For any $\varepsilon > 0$:

$$\lim_{n \to \infty} P\left(\left|\bar{X}_n - \mu\right| > \varepsilon\right) = 0$$

This is called **convergence in probability**: $\bar{X}_n \xrightarrow{p} \mu$.

### What it says

For any fixed tolerance $\varepsilon$, no matter how small, the probability that the sample mean strays more than $\varepsilon$ away from the true mean becomes negligible as $n$ grows.

> It does **not** say the sample mean will *stay* close to $\mu$ forever — only that the probability of a large deviation shrinks to zero.

---

## 4. Markov's Inequality

### Statement

For a **non-negative** random variable $X$ and any $a > 0$:

$$P(X \geq a) \leq \frac{\mathbb{E}[X]}{a}$$

### Intuition

If the average value of $X$ is small, $X$ cannot be large very often. More precisely — the fraction of probability mass above threshold $a$ is bounded by the ratio of the mean to $a$.

Concretely: if the average income in a population is \$50,000, then **at most 50%** of people can earn ≥ \$100,000. If more than half earned that much, the average would have to exceed \$50,000 — a contradiction.

### Proof

Since $X \geq 0$, split the expectation over whether $X \geq a$ or not:

$$\mathbb{E}[X] = \mathbb{E}[X \cdot \mathbf{1}_{X \geq a}] + \mathbb{E}[X \cdot \mathbf{1}_{X < a}]$$

Drop the second (non-negative) term:

$$\mathbb{E}[X] \geq \mathbb{E}[X \cdot \mathbf{1}_{X \geq a}]$$

On the event $\{X \geq a\}$, we have $X \geq a$, so:

$$\mathbb{E}[X \cdot \mathbf{1}_{X \geq a}] \geq a \cdot \mathbb{E}[\mathbf{1}_{X \geq a}] = a \cdot P(X \geq a)$$

Combining:

$$\mathbb{E}[X] \geq a \cdot P(X \geq a)$$

Divide both sides by $a > 0$:

$$\boxed{P(X \geq a) \leq \frac{\mathbb{E}[X]}{a}} \qquad \blacksquare$$

### Why Non-negativity is Required

The inequality **fails** for variables that can be negative. Example: let $X = -100$ with probability 0.99 and $X = 100$ with probability 0.01. Then $\mathbb{E}[X] = -100(0.99) + 100(0.01) = -98$, giving a negative upper bound — meaningless for a probability.

### Markov as the Root of a Family of Bounds

Markov is the **foundation** of a chain of tighter inequalities, each derived by applying Markov to a transformed variable:

| Inequality | Derived by applying Markov to | Bound on |
|---|---|---|
| **Markov** | $X$ directly | $P(X \geq a)$ for $X \geq 0$ |
| **Chebyshev** | $(X - \mu)^2$ | $P(\|X - \mu\| \geq \varepsilon)$ |
| **Chernoff** | $e^{tX}$ for $t > 0$ | Exponential tail bounds |
| **One-sided Chebyshev** | $(X - c)^2$ optimised over $c$ | $P(X - \mu \geq \varepsilon)$ |

Chebyshev (used in the WLLN proof below) is Markov applied one level up — the entire WLLN proof ultimately rests on this single three-line argument.

---

## 5. Proof of WLLN via Chebyshev's Inequality

### Chebyshev's Inequality (prerequisite)

For any random variable $Z$ with mean $\mu_Z$ and variance $\sigma_Z^2$, and any $\varepsilon > 0$:

$$P\left(|Z - \mu_Z| \geq \varepsilon\right) \leq \frac{\sigma_Z^2}{\varepsilon^2}$$

*Proof of Chebyshev:* By Markov's inequality applied to $(Z - \mu_Z)^2$:

$$P\left(|Z - \mu_Z| \geq \varepsilon\right) = P\left((Z-\mu_Z)^2 \geq \varepsilon^2\right) \leq \frac{\mathbb{E}[(Z-\mu_Z)^2]}{\varepsilon^2} = \frac{\sigma_Z^2}{\varepsilon^2} \qquad \square$$

### Proof of WLLN

**Step 1.** Compute the mean and variance of $\bar{X}_n$:

$$\mathbb{E}[\bar{X}_n] = \frac{1}{n}\sum_{i=1}^n \mathbb{E}[X_i] = \mu$$

$$\mathrm{Var}(\bar{X}_n) = \frac{1}{n^2}\sum_{i=1}^n \mathrm{Var}(X_i) = \frac{\sigma^2}{n}$$

(independence used in the variance step)

**Step 2.** Apply Chebyshev's inequality to $\bar{X}_n$:

$$P\left(|\bar{X}_n - \mu| \geq \varepsilon\right) \leq \frac{\mathrm{Var}(\bar{X}_n)}{\varepsilon^2} = \frac{\sigma^2}{n\varepsilon^2}$$

**Step 3.** Take the limit:

$$0 \leq P\left(|\bar{X}_n - \mu| \geq \varepsilon\right) \leq \frac{\sigma^2}{n\varepsilon^2} \xrightarrow{n \to \infty} 0$$

By the squeeze theorem:

$$\lim_{n\to\infty} P\left(|\bar{X}_n - \mu| > \varepsilon\right) = 0 \qquad \blacksquare$$

**Note:** This proof requires only finite variance $\sigma^2 < \infty$. The WLLN holds under even weaker conditions (finite mean suffices — see Variants).

---

## 6. Strong Law of Large Numbers (SLLN)

### Statement

$$P\left(\lim_{n \to \infty} \bar{X}_n = \mu\right) = 1$$

This is called **almost sure convergence**: $\bar{X}_n \xrightarrow{a.s.} \mu$.

### What it says

With probability 1, the entire infinite sequence of sample means converges to $\mu$. Almost every realisation of the process eventually and permanently stays close to $\mu$.

### Proof (sketch — Kolmogorov's approach)

A full proof uses the **Kolmogorov Strong Law**, which requires only $\mathbb{E}[|X_i|] < \infty$. The key steps:

1. Apply the **Borel–Cantelli lemma**: show $\sum_n P(|\bar{X}_n - \mu| > \varepsilon) < \infty$ for all $\varepsilon > 0$, which implies the events $|\bar{X}_n - \mu| > \varepsilon$ occur only finitely often almost surely.

2. Use **Kolmogorov's maximal inequality** (a strengthening of Chebyshev for partial sums) to control the supremum of deviations over all $k \geq n$.

3. Conclude $\bar{X}_n \to \mu$ a.s. via a subsequence argument.

---

## 7. Weak vs Strong — Key Distinction

| | Weak LLN | Strong LLN |
|---|---|---|
| **Type of convergence** | In probability | Almost surely |
| **What it guarantees** | $P(\|\bar{X}_n - \mu\| > \varepsilon) \to 0$ for each fixed $n$ | $P(\bar{X}_n \to \mu) = 1$ over the whole sequence |
| **Intuition** | Each snapshot is likely close to $\mu$ | The path itself converges |
| **Required condition** | Finite mean (or finite variance for Chebyshev proof) | Finite mean $\mathbb{E}[|X|] < \infty$ |
| **Implies the other?** | No | Yes — a.s. convergence implies convergence in probability |

> **Strong implies Weak**, but not vice versa. A sequence can converge in probability without converging almost surely.

### Example of the gap

Consider random variables that keep "jumping" with diminishing probability. Formally, one can construct $X_n$ such that $P(X_n \neq 0) = 1/n \to 0$ (so $X_n \xrightarrow{p} 0$), yet by Borel–Cantelli (divergent series $\sum 1/n = \infty$), $X_n \neq 0$ infinitely often — so $X_n \not\xrightarrow{a.s.} 0$.

---

## 8. Variants & Generalisations

### 7.1 Khintchine's WLLN
**Condition:** $X_i$ i.i.d. with finite mean $\mu$ only (no variance assumption needed).  
**Result:** $\bar{X}_n \xrightarrow{p} \mu$.  
Proved using characteristic functions rather than Chebyshev.

### 7.2 Markov's LLN
**Condition:** $X_i$ need not be identically distributed. Requires only:
$$\frac{1}{n^2}\sum_{i=1}^n \mathrm{Var}(X_i) \to 0$$
**Result:** $\bar{X}_n - \mathbb{E}[\bar{X}_n] \xrightarrow{p} 0$.  
Generalises WLLN to non-identically distributed but uncorrelated variables.

### 7.3 Chebyshev's LLN
**Condition:** $X_i$ pairwise uncorrelated (independence not required), uniformly bounded variance: $\mathrm{Var}(X_i) \leq C < \infty$.  
**Result:** $\bar{X}_n \xrightarrow{p} \bar{\mu}_n$ where $\bar{\mu}_n = \frac{1}{n}\sum_i \mathbb{E}[X_i]$.

### 7.4 Kolmogorov's SLLN
**Condition:** $X_i$ i.i.d. with $\mathbb{E}[|X_1|] < \infty$.  
**Result:** $\bar{X}_n \xrightarrow{a.s.} \mu$.  
This is the definitive form — weakest condition for a.s. convergence of i.i.d. means.

### 7.5 LLN for Dependent Sequences (Ergodic Theorem)
When $X_i$ are **stationary and ergodic** (not necessarily independent):

$$\frac{1}{n}\sum_{i=1}^n X_i \xrightarrow{a.s.} \mathbb{E}[X_1]$$

This is **Birkhoff's Ergodic Theorem** — the foundation of ergodic theory and relevant to Markov chains in ML.

### 7.6 LLN for Triangular Arrays (Feller's Theorem)
For triangular arrays $\{X_{n,k}\}$ (where each row has $n$ variables that may depend on $n$), the WLLN holds under a **uniform integrability** condition. Used in asymptotic statistics.

### 7.7 $L^2$ LLN (Mean-Square Convergence)
Under finite variance:

$$\mathbb{E}\left[(\bar{X}_n - \mu)^2\right] = \frac{\sigma^2}{n} \to 0$$

This is **convergence in mean square** ($L^2$): $\bar{X}_n \xrightarrow{L^2} \mu$. Strictly stronger than convergence in probability, directly visible from the Chebyshev proof.

---

### Variants at a Glance

| Variant | Key condition | Convergence type |
|---|---|---|
| Classical WLLN (Chebyshev proof) | i.i.d., finite $\sigma^2$ | In probability |
| Khintchine's WLLN | i.i.d., finite $\mu$ only | In probability |
| Markov's LLN | Independent, $\frac{1}{n^2}\sum\mathrm{Var} \to 0$ | In probability |
| Chebyshev's LLN | Pairwise uncorrelated, bounded var | In probability |
| Kolmogorov's SLLN | i.i.d., finite $\mathbb{E}[|X|]$ | Almost surely |
| Ergodic Theorem | Stationary & ergodic | Almost surely |
| $L^2$ LLN | i.i.d., finite $\sigma^2$ | Mean square |

---

## 9. Relevance to ML & Information Theory

**Monte Carlo estimation:** $\mathbb{E}[f(X)] \approx \frac{1}{n}\sum_{i=1}^n f(x_i)$ — the entire foundation of Monte Carlo methods is the LLN.

**Empirical risk minimisation:** Training loss on $n$ samples converges to expected (population) loss. LLN justifies why training on finite data generalises.

**Maximum likelihood estimation:** The log-likelihood per sample $\frac{1}{n}\sum_i \log p_\theta(x_i) \xrightarrow{a.s.} \mathbb{E}[\log p_\theta(X)]$, which connects MLE to KL divergence minimisation.

**Asymptotic Equipartition Property (AEP):** For i.i.d. $X_i \sim p$:

$$-\frac{1}{n}\log p(X_1, \dots, X_n) \xrightarrow{a.s.} H(X)$$

This is a direct application of the SLLN to $\log p(X_i)$, and is the cornerstone of source coding theory.

---

## 10. Summary

```
i.i.d. samples X_1, ..., X_n,   E[X_i] = μ
                    │
                    ▼
         Sample mean X̄_n = (1/n)ΣXᵢ
                    │
          ┌─────────┴──────────┐
          ▼                    ▼
      Weak LLN             Strong LLN
  convergence in prob     almost sure
  P(|X̄_n - μ| > ε) → 0   P(X̄_n → μ) = 1
  needs: finite mean       needs: finite mean
          │                    │
          │    implied by      │
          └────────────────────┘
```

$$\boxed{\bar{X}_n \xrightarrow{p} \mu \quad \text{(Weak)} \qquad \bar{X}_n \xrightarrow{a.s.} \mu \quad \text{(Strong)}}$$
