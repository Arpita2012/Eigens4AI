# Finite Mean
---

## Definition

When we say a random variable $X$ has a **finite mean**, we simply mean:

$$\mathbb{E}[|X|] = \sum_x |x| \cdot P(X = x) < \infty$$

The expectation exists and does not blow up to infinity.

---

## Why It Is Not Always Guaranteed

For "nice" distributions like Gaussian or Binomial, the mean is obviously finite. But some distributions have such heavy tails that the mean literally does not exist as a finite number.

**The classic example — Cauchy distribution:**

$$f(x) = \frac{1}{\pi(1 + x^2)}$$

If you try to compute:

$$\mathbb{E}[X] = \int_{-\infty}^{\infty} \frac{x}{\pi(1+x^2)}\,dx$$

the integral diverges — equal infinite mass on both sides that never cancels cleanly. The mean is **undefined**. The LLN does not apply — if you average $n$ Cauchy samples, the sample mean does not converge to anything; it stays Cauchy regardless of $n$.

---

## The Hierarchy of Moment Conditions

| Condition | Meaning | What it enables |
|---|---|---|
| $\mathbb{E}[\|X\|] < \infty$ | **Finite mean** | Kolmogorov SLLN, Khintchine WLLN |
| $\mathbb{E}[X^2] < \infty$ | **Finite variance** | Chebyshev-based WLLN proof, $L^2$ convergence |
| $\mathbb{E}[\|X\|^k] < \infty$ | **Finite $k$-th moment** | Higher-order concentration bounds |

Finite variance **implies** finite mean, by Jensen's inequality:

$$\mathbb{E}[|X|] \leq \sqrt{\mathbb{E}[X^2]} < \infty$$

But the converse does not hold — a distribution can have a finite mean yet infinite variance.

**Example:** The Pareto distribution with shape parameter $1 < \alpha \leq 2$ has a finite mean but infinite variance.

So the Chebyshev-based WLLN proof uses a **stronger assumption** than strictly necessary. Khintchine showed finite mean alone suffices to get the WLLN.

---

## Summary

$$\mathbb{E}[X^2] < \infty \implies \mathbb{E}[|X|] < \infty \implies \mathbb{E}[X] \text{ exists}$$

> "Finite mean" is simply the requirement that $\mathbb{E}[X]$ is a well-defined real number — not $\pm\infty$ or undefined. For most distributions in ML this holds trivially; it only becomes a real constraint with heavy-tailed distributions like Cauchy or Pareto.
