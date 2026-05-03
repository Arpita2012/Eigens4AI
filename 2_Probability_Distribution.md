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
