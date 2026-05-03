# 2. Probability Distributions

This section introduces the core types of probability distributions used in machine learning: marginal, joint, and conditional distributions.

---

## 1. Probability Distribution

A **probability distribution** describes how probability is assigned to the values of a random variable.

Let:
- $(\Omega, \mathcal{F}, P)$ be a probability space
- $X: \Omega \rightarrow \mathbb{R}$ be a random variable

The distribution of $X$ is a probability measure $P_X$ on $(\mathbb{R}, \mathcal{B}(\mathbb{R}))$ defined as:

$$
P_X(B) = P(X^{-1}(B)), \quad \forall B \in \mathcal{B}(\mathbb{R})
$$

This means:
- We measure probabilities in the output space by mapping back to the original space

---
- **Discrete case (PMF)**:  
  $p(x) = P(X = x)$

- **Continuous case (PDF)**:  
  $P(a \leq X \leq b) = \int_a^b p(x)\,dx$

- **CDF (always defined)**:  
  $F(x) = P(X \leq x)$

## 2. Joint Distribution

The **joint distribution** describes the probability of two (or more) random variables together.

Let:
- $X: \Omega \rightarrow \mathbb{R}$
- $Y: \Omega \rightarrow \mathbb{R}$

Define the joint random variable:
$$
(X, Y): \Omega \rightarrow \mathbb{R}^2
$$

The joint distribution is:

$$
P_{XY}(A) = P\big((X, Y)^{-1}(A)\big), \quad A \in \mathcal{B}(\mathbb{R}^2)
$$

---

### Interpretation

- $P_{XY}$ tells us how $X$ and $Y$ vary together  
- It captures dependencies between variables  

---

### Density form (continuous case)

$$
p(x, y)
$$

such that:

$$
P((X,Y) \in A) = \iint_A p(x,y)\,dx\,dy
$$

---

## 3. Marginal Distribution

The **marginal distribution** is the distribution of a subset of variables obtained from the joint distribution.

From $P_{XY}$, we can recover:

- Distribution of $X$:
  $P_X(B) = P_{XY}(B \times \mathbb{R})$

- Distribution of $Y$:
  $P_Y(B) = P_{XY}(\mathbb{R} \times B)$

---

### Density form

If joint density $p(x,y)$ exists:

$$
p_X(x) = \int p(x,y)\,dy
$$

$$
p_Y(y) = \int p(x,y)\,dx
$$

---

### Interpretation

Marginalization means:
> “Ignore the other variable by integrating it out”

---

### Example: Marginal Distribution (Three Coin Tosses)

To understand marginal distributions, consider three fair coin tosses.

Define:
- $X$: number of Heads in the first two tosses ($0,1,2$)
- $Y$: number of Heads in the third toss ($0$ or $1$)

There are 8 equally likely outcomes:
HHH, HHT, HTH, HTT, THH, THT, TTH, TTT (each with probability $1/8$)

---

### Joint Distribution Table

| $X \backslash Y$ | $Y=0$ (Tails) | $Y=1$ (Heads) | Marginal $P_X$ |
|-----------------|--------------|--------------|----------------|
| $X=0$ (TT) | $1/8$ (TTT) | $1/8$ (TTH) | $2/8 = 1/4$ |
| $X=1$ (HT, TH) | $2/8$ (HTT, THT) | $2/8$ (HTH, THH) | $4/8 = 1/2$ |
| $X=2$ (HH) | $1/8$ (HHT) | $1/8$ (HHH) | $2/8 = 1/4$ |
| **Marginal $P_Y$** | $4/8 = 1/2$ | $4/8 = 1/2$ | **1** |

---


### How to Read the Marginals

**Marginal Distribution of $X$ (row sums):**

To compute $P(X=1)$, ignore the third toss and sum across the row:  
$P(X=1) = 2/8 + 2/8 = 4/8 = 1/2$

---

**Marginal Distribution of $Y$ (column sums):**

To compute $P(Y=1)$, ignore the first two tosses and sum down the column:  
$P(Y=1) = 1/8 + 2/8 + 1/8 = 4/8 = 1/2$

---

### Key Insight

Marginalization means:

> Ignoring one variable by summing over all its possible values.

- Rows → marginal over $X$
- Columns → marginal over $Y$



## 4. Conditional Distribution

The **conditional distribution** describes the distribution of one variable given another.

---

### Discrete case

$$
P(Y = y \mid X = x) = \frac{P(X = x, Y = y)}{P(X = x)}
$$

---

### Continuous case

$$
p(y \mid x) = \frac{p(x,y)}{p_X(x)}
$$

---

### Interpretation

- It tells us how $Y$ behaves when $X$ is fixed  
- This is the central object in supervised learning  

---

## 5. Key Relationship

All three distributions are connected:

$$
p(x,y) = p(y \mid x)\,p_X(x)
$$

This is the **factorization of the joint distribution**.

---

## 6. Machine Learning Perspective

In machine learning:

- Data is sampled from:
  $(X, Y) \sim P_{XY}$

- Learning aims to model:
  $P(Y \mid X)$

- Predictions are based on:
  $f(x) \approx \mathbb{E}[Y \mid X = x]$

---

## 7. Summary

- **Probability distribution**: describes a single variable  
- **Joint distribution**: describes multiple variables together  
- **Marginal distribution**: obtained by ignoring variables  
- **Conditional distribution**: describes dependence between variables  

---

## 8. Key Insight

Machine learning is fundamentally about:

> learning conditional distributions from samples of a joint distribution
