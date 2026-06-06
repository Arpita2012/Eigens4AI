# KL Divergence — Theory, Proofs & Gibbs' Inequality



## Table of Contents

1. [Concave & Convex Functions](#1-concave--convex-functions)
2. [Jensen's Inequality](#2-jensens-inequality)
3. [KL Divergence — Definition](#3-kl-divergence--definition)
4. [Proof: KL Divergence ≥ 0](#4-proof-kl-divergence--0)
5. [Gibbs' Inequality](#5-gibbs-inequality)
6. [Proof of Gibbs' Inequality](#6-proof-of-gibbs-inequality)
7. [Summary & Connection Map](#7-summary--connection-map)

---

## 1. Concave & Convex Functions

### Definitions

A function $f : \mathbb{R} \to \mathbb{R}$ is **convex** if for all $x, y$ in its domain and $\lambda \in [0,1]$:

$$f\bigl(\lambda x + (1-\lambda)y\bigr) \leq \lambda f(x) + (1-\lambda)f(y)$$

It is **concave** if the inequality is reversed:

$$f\bigl(\lambda x + (1-\lambda)y\bigr) \geq \lambda f(x) + (1-\lambda)f(y)$$

> **Geometric intuition:** A convex function "cups upward" — the chord between any two points lies *above* the curve. A concave function "cups downward" — the chord lies *below* the curve.

### Second-Derivative Test

| Condition | Function type |
|---|---|
| $f''(x) \geq 0$ for all $x$ | Convex |
| $f''(x) \leq 0$ for all $x$ | Concave |
| $f''(x) > 0$ for all $x$ | Strictly convex |
| $f''(x) < 0$ for all $x$ | Strictly concave |

### Key Examples Relevant to Information Theory

| Function | Convex / Concave | Why it matters |
|---|---|---|
| $f(x) = x \log x$ | **Convex** ($f'' = 1/x > 0$ for $x>0$) | Core of KL divergence |
| $f(x) = \log x$ | **Concave** ($f'' = -1/x^2 < 0$) | Used in KL ≥ 0 proof via Jensen |
| $f(x) = -\log x$ | **Convex** | Equivalent form |
| $f(x) = x^2$ | **Convex** | Variance, squared loss |

---

## 2. Jensen's Inequality

### Statement

Let $X$ be a random variable and $f$ a **convex** function. Then:

$$f\bigl(\mathbb{E}[X]\bigr) \leq \mathbb{E}[f(X)]$$

For a **concave** function $f$, the inequality flips:

$$f\bigl(\mathbb{E}[X]\bigr) \geq \mathbb{E}[f(X)]$$

### Discrete Form

For a discrete random variable with values $x_1, \dots, x_n$ and weights $\lambda_i \geq 0$, $\sum_i \lambda_i = 1$:

$$f\left(\sum_{i=1}^n \lambda_i x_i\right) \leq \sum_{i=1}^n \lambda_i f(x_i) \qquad (f \text{ convex})$$

### Proof Sketch (by induction on $n$)

**Base case** $n=2$: This is exactly the definition of convexity.

**Inductive step**: Assume it holds for $n-1$ points. Write:

$$\sum_{i=1}^n \lambda_i x_i = \lambda_n x_n + (1-\lambda_n)\underbrace{\sum_{i=1}^{n-1} \frac{\lambda_i}{1-\lambda_n} x_i}_{\displaystyle =: \bar{x}}$$

By convexity:

$$f\left(\sum_{i=1}^n \lambda_i x_i\right) \leq \lambda_n f(x_n) + (1-\lambda_n) f(\bar{x})$$

The weights $\mu_i = \lambda_i/(1-\lambda_n)$ satisfy $\sum_{i=1}^{n-1}\mu_i = 1$, so by the inductive hypothesis:

$$f(\bar{x}) \leq \sum_{i=1}^{n-1} \mu_i f(x_i) = \sum_{i=1}^{n-1} \frac{\lambda_i}{1-\lambda_n} f(x_i)$$

Combining:

$$f\left(\sum_{i=1}^n \lambda_i x_i\right) \leq \lambda_n f(x_n) + \sum_{i=1}^{n-1}\lambda_i f(x_i) = \sum_{i=1}^n \lambda_i f(x_i) \qquad \blacksquare$$

---

## 3. KL Divergence — Definition

### Discrete Case

Let $P$ and $Q$ be probability distributions over a finite alphabet $\mathcal{X}$. The **Kullback–Leibler (KL) divergence** from $Q$ to $P$ is:

$$D_{\mathrm{KL}}(P \| Q) = \sum_{x \in \mathcal{X}} P(x) \log \frac{P(x)}{Q(x)}$$

**Convention:** $0 \log 0 = 0$ and $p \log(p/0) = +\infty$ if $p > 0$.

### Continuous Case

$$D_{\mathrm{KL}}(P \| Q) = \int_{-\infty}^{\infty} p(x) \log \frac{p(x)}{q(x)} dx$$

### Important Properties

- **Not a metric**: $D_{\mathrm{KL}}(P\|Q) \neq D_{\mathrm{KL}}(Q\|P)$ in general (asymmetric)
- **Not a distance**: triangle inequality does not hold
- $D_{\mathrm{KL}}(P\|Q) = 0 \iff P = Q$ (almost everywhere)
- Related to **cross-entropy**: $D_{\mathrm{KL}}(P\|Q) = H(P,Q) - H(P)$

---

## 4. Proof: KL Divergence ≥ 0

### Theorem (Non-negativity of KL Divergence)

$$D_{\mathrm{KL}}(P \| Q) \geq 0$$

with equality if and only if $P = Q$ almost everywhere.

---

### Proof via Jensen's Inequality

We use the fact that $\log$ is a **concave** function, so Jensen's inequality gives:

$$\log\bigl(\mathbb{E}[X]\bigr) \geq \mathbb{E}[\log X]$$

**Step 1.** Rewrite KL divergence by flipping the log:

$$D_{\mathrm{KL}}(P \| Q) = \sum_x P(x)\log\frac{P(x)}{Q(x)} = -\sum_x P(x)\log\frac{Q(x)}{P(x)}$$

**Step 2.** Treat $\frac{Q(x)}{P(x)}$ as a random variable $Z$ under the distribution $P$. Then:

$$\mathbb{E}_P[Z] = \sum_x P(x) \cdot \frac{Q(x)}{P(x)} = \sum_x Q(x) = 1$$

**Step 3.** Apply Jensen's inequality with the concave function $f = \log$:

$$\mathbb{E}_P\left[\log \frac{Q(x)}{P(x)}\right] \leq \log\left(\mathbb{E}_P\left[\frac{Q(x)}{P(x)}\right]\right) = \log(1) = 0$$

**Step 4.** Therefore:

$$\sum_x P(x)\log\frac{Q(x)}{P(x)} \leq 0$$

$$\implies \quad -\sum_x P(x)\log\frac{Q(x)}{P(x)} \geq 0$$

$$\implies \quad \boxed{D_{\mathrm{KL}}(P \| Q) \geq 0} \qquad \blacksquare$$

**Equality condition:** Jensen's inequality is tight iff the argument of $\log$ is constant $P$-a.e., i.e., $Q(x)/P(x) = c$ for all $x$ with $P(x) > 0$. Since both sum to 1, we get $c = 1$, hence $P = Q$.

---

### Alternative Proof via $\ln t \leq t - 1$

This is the most direct algebraic route.

**Lemma:** For all $t > 0$:

$$\ln t \leq t - 1$$

with equality iff $t = 1$.

*Proof of Lemma:* Let $g(t) = t - 1 - \ln t$. Then $g'(t) = 1 - 1/t = 0$ at $t=1$, $g''(t) = 1/t^2 > 0$, so $t=1$ is a global minimum. Since $g(1) = 0$, we have $g(t) \geq 0$. $\square$

**Main proof:**

$$-D_{\mathrm{KL}}(P\|Q) = \sum_x P(x) \ln\frac{Q(x)}{P(x)} \leq \sum_x P(x)\left(\frac{Q(x)}{P(x)} - 1\right) = \sum_x Q(x) - \sum_x P(x) = 1 - 1 = 0$$

Therefore $D_{\mathrm{KL}}(P \| Q) \geq 0$. $\blacksquare$

---

## 5. Gibbs' Inequality

### Statement

Let $P = (p_1, \dots, p_n)$ and $Q = (q_1, \dots, q_n)$ be probability distributions over the same finite set. Then:

$$-\sum_{i=1}^n p_i \log p_i \leq -\sum_{i=1}^n p_i \log q_i$$

Equivalently:

$$H(P) \leq H(P, Q)$$

where $H(P) = -\sum_i p_i \log p_i$ is the **entropy** of $P$ and $H(P,Q) = -\sum_i p_i \log q_i$ is the **cross-entropy**.

### Reformulation

Gibbs' inequality is simply the statement:

$$D_{\mathrm{KL}}(P\|Q) = H(P,Q) - H(P) \geq 0$$

i.e., **cross-entropy is always at least as large as entropy**, and equality holds iff $P = Q$.

---

## 6. Proof of Gibbs' Inequality

We prove $H(P) \leq H(P,Q)$ directly using the $\ln t \leq t-1$ inequality.

**Proof:**

$$H(P,Q) - H(P) = -\sum_i p_i \log q_i + \sum_i p_i \log p_i = \sum_i p_i \log \frac{p_i}{q_i}$$

Using $\log t \leq (t-1)/\ln 2$ (the base-2 version of $\ln t \leq t-1$), equivalently using natural log throughout:

$$\sum_i p_i \ln \frac{p_i}{q_i} = -\sum_i p_i \ln\frac{q_i}{p_i}$$

Apply $\ln t \leq t-1$ with $t = q_i/p_i$:

$$\ln\frac{q_i}{p_i} \leq \frac{q_i}{p_i} - 1$$

So:

$$-\sum_i p_i \ln\frac{q_i}{p_i} \geq -\sum_i p_i\left(\frac{q_i}{p_i}-1\right) = -\sum_i q_i + \sum_i p_i = -1 + 1 = 0$$

Therefore:

$$H(P,Q) - H(P) \geq 0 \implies \boxed{H(P) \leq H(P,Q)} \qquad \blacksquare$$

**Equality:** Holds iff $q_i/p_i = 1$ for all $i$, i.e., $P = Q$.

### Corollary: Uniform Distribution Maximizes Entropy

Taking $Q$ to be the uniform distribution $q_i = 1/n$:

$$H(P) \leq H(P, Q_{\text{uniform}}) = -\sum_i p_i \log \frac{1}{n} = \log n$$

So $H(P) \leq \log n$, with equality iff $P$ is uniform. This is a direct consequence of Gibbs' inequality.

---

## 7. Summary & Connection Map

```
Concavity of log(x)
        │
        ▼
Jensen's Inequality:  E[log Z] ≤ log(E[Z])
        │
        │  Apply with Z = Q(x)/P(x) under P
        ▼
  E_P[Q/P] = 1  ──────────────────────────────┐
        │                                      │
        ▼                                      ▼
  log(E[Q/P]) = 0          ln(t) ≤ t-1  (alternative)
        │                          │
        └──────────┬───────────────┘
                   ▼
        D_KL(P || Q) ≥ 0
                   │
                   ▼
        H(P) ≤ H(P, Q)   ←──  Gibbs' Inequality
                   │
                   ▼
        H(P) ≤ log|X|    ←──  Uniform dist. max entropy
```

### Quick-Reference Table

| Result | Statement | Key tool used |
|---|---|---|
| **KL ≥ 0** | $D_{\mathrm{KL}}(P\|Q) \geq 0$ | Jensen's inequality (concavity of log) |
| **KL ≥ 0** (alt.) | $D_{\mathrm{KL}}(P\|Q) \geq 0$ | $\ln t \leq t-1$ |
| **Gibbs' Inequality** | $H(P) \leq H(P,Q)$ | KL ≥ 0 directly |
| **Max entropy** | $H(P) \leq \log n$ | Gibbs' + uniform $Q$ |

---

> **Note on base of logarithm:** All results hold for any log base. Use $\log_2$ for bits (information theory), $\ln$ for nats (math/ML). The inequality directions are unchanged.
