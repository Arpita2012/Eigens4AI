# Surprisal and Entropy

## Surprisal (Self-Information)

Surprisal (also called **self-information**) is an information-theoretic measure of how unexpected an event is.

It quantifies the amount of **information gained** when an event occurs.

- Highly probable events carry little information (**low surprisal**).
- Rare events carry more information (**high surprisal**).

---

# Formula for Surprisal

The surprisal of an event $x$ is defined as:

$$
S(x)=-\log_b(P(x))
$$

Where:

- $P(x)$ is the probability of event $x$
- $b$ is the logarithm base
  - Base 2 → units are **bits**
  - Base $e$ → units are **nats**

---

# Intuition

The less likely an event is, the more surprising it becomes.

Examples:

| Event | Probability | Surprisal |
|---|---|---|
| Sun rises tomorrow | Very high | Very low |
| Winning the lottery | Very low | Very high |

---

# Important Cases

## 1. Certain Event

If an event is guaranteed:

$$
P(x)=1
$$

then:

$$
S(x)=-\log_2(1)=0
$$

A completely expected event carries **zero information**.

---

## 2. Impossible Event

If:

$$
P(x)=0
$$

then:

$$
S(x)\rightarrow\infty
$$

An impossible event would produce infinite surprisal.

---

## 3. Fair Coin Toss

For a fair coin:

$$
P(x)=0.5
$$

then:

$$
S(x)=-\log_2(0.5)=1\text{ bit}
$$

A fair coin toss therefore contains **1 bit of information**.

---

# Entropy

Entropy measures the **average surprisal** over all possible outcomes.

It represents the overall uncertainty of a random variable.

The entropy formula is:

$$
H(X)=-\sum_x P(x)\log_b(P(x))
$$

Where:

- $H(X)$ is the entropy of random variable $X$
- $P(x)$ is the probability of outcome $x$

---

# Relationship Between Surprisal and Entropy

Surprisal describes the information content of **one event**.

Entropy describes the **expected average information** across all events.

Mathematically:

$$
H(X)=\mathbb{E}[S(X)]
$$

This means:

$$
\text{Entropy}=\text{Average Surprisal}
$$

---

# Example: Fair vs Biased Coin

## Fair Coin

| Outcome | Probability | Surprisal |
|---|---|---|
| Heads | 0.5 | 1 bit |
| Tails | 0.5 | 1 bit |

Entropy:

$$
H(X)=1\text{ bit}
$$

The uncertainty is maximal.

---

## Biased Coin

Suppose:

- $P(\text{Heads})=0.99$
- $P(\text{Tails})=0.01$

Then:

- Heads has very low surprisal
- Tails has very high surprisal

But overall entropy is lower because the outcome is highly predictable.

---


| Concept | Meaning |
|---|---|
| Surprisal | Information from a single event |
| Entropy | Average information across all events |
| High Probability | Low surprisal |
| Low Probability | High surprisal |

Key idea:

> Rare events are more informative than common events.

# Cross Entropy
---

## Definition

Let $P$ and $Q$ be probability distributions over a finite alphabet $\mathcal{X}$. The **cross entropy** of $Q$ relative to $P$ is:

$$H(P, Q) = -\sum_{x \in \mathcal{X}} P(x) \log Q(x)$$

**Reading:** "the expected number of bits needed to encode samples drawn from $P$, using a code optimised for $Q$."

For the continuous case:

$$H(P, Q) = -\int p(x) \log q(x)\, dx$$

---

## Decomposition

Cross entropy decomposes cleanly into two terms:

$$H(P, Q) = H(P) + D_{\mathrm{KL}}(P \,\|\, Q)$$

| Term | Name | Meaning |
|---|---|---|
| $H(P)$ | Entropy of $P$ | Irreducible information content of the true distribution |
| $D_{\mathrm{KL}}(P \| Q)$ | KL Divergence | Extra cost from using the wrong distribution $Q$ |

Since $D_{\mathrm{KL}} \geq 0$, this immediately gives **Gibbs' inequality**:

$$H(P, Q) \geq H(P)$$

with equality iff $P = Q$.

---

## Cross Entropy as a Loss Function in ML

In machine learning, $P$ is the **true label distribution** and $Q$ is the **model's predicted distribution**. For a single sample with true class $y$ and predicted probabilities $\hat{y}$:

**Binary cross entropy** (two classes):

$$\mathcal{L} = -\bigl[y \log \hat{y} + (1-y)\log(1-\hat{y})\bigr]$$

**Categorical cross entropy** ($n$ classes):

$$\mathcal{L} = -\sum_{i=1}^{n} y_i \log \hat{y}_i$$

where $y$ is a one-hot vector. Minimising cross entropy loss is equivalent to minimising KL divergence from the true distribution, since $H(P)$ is fixed w.r.t. model parameters:

$$\arg\min_\theta H(P, Q_\theta) = \arg\min_\theta D_{\mathrm{KL}}(P \,\|\, Q_\theta)$$

---

## Key Properties

- **Not symmetric**: $H(P,Q) \neq H(Q,P)$ in general
- **Lower bounded**: $H(P,Q) \geq H(P) \geq 0$
- **Self cross entropy**: $H(P,P) = H(P)$ — reduces to ordinary entropy when both distributions are the same
- **Relation to log-likelihood**: maximising log-likelihood under $P$ is equivalent to minimising $H(P, Q_\theta)$
-  $\boxed{H(P,Q) = H(P) + D_{\mathrm{KL}}(P\|Q) \geq H(P)}$
