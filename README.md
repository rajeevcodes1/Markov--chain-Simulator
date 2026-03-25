# Markov--chain-Simulator




A complete implementation of a **Discrete-Time Markov Chain (DTMC)** that models weather transitions across three states — **Sunny, Rainy, and Cloudy**. The project demonstrates how to compute the **stationary distribution** using three different approaches and verifies their convergence both numerically and visually. 


## 📌 Table of Contents

* [What is a Markov Chain?](#what-is-a-markov-chain)
* [The Markov Property](#the-markov-property)
* [State Space & Transition Matrix](#state-space--transition-matrix)
* [How the Simulation Works](#how-the-simulation-works)
* [Three Methods to Find Stationary Distribution](#three-methods-to-find-stationary-distribution)

  * [1. Monte Carlo Simulation](#1-monte-carlo-simulation)
  * [2. Power Iteration](#2-power-iteration)
  * [3. Analytical Solution](#3-analytical-solution)
* [The Ergodic Theorem](#the-ergodic-theorem)
* [Results](#results)
* [Visualizations](#visualizations)
* [Tech Stack](#tech-stack)
* [How to Run](#how-to-run)

---

## 🔍 What is a Markov Chain?

A **Markov Chain** is a probabilistic model describing transitions between a finite set of states. The defining feature is that the **next state depends only on the current state**, not on the sequence of events that came before.

It is formally defined by:

* A finite **state space**
  ( S = {s_1, s_2, ..., s_n} )
* A **transition matrix** ( T ), where each entry
  ( T_{ij} = P(X_{t+1} = s_j \mid X_t = s_i) )

---

## 🧠 The Markov Property

The central assumption of Markov processes is **memorylessness**:

[
P(X_{t+1} = s \mid X_t, X_{t-1}, ..., X_0) = P(X_{t+1} = s \mid X_t)
]

In simpler terms:

> The future state depends only on the present state — past history has no influence.

This greatly simplifies modeling and computation.

---

## 🌤️ State Space & Transition Matrix

### States

[
S = {\text{Sunny},; \text{Rainy},; \text{Cloudy}}
]

### Transition Matrix

[
T =
\begin{pmatrix}
0.7 & 0.2 & 0.1 \
0.3 & 0.4 & 0.3 \
0.2 & 0.3 & 0.5
\end{pmatrix}
]

| From \ To | Sunny | Rainy | Cloudy | Row Sum |
| --------- | ----- | ----- | ------ | ------- |
| Sunny     | 0.70  | 0.20  | 0.10   | 1.0     |
| Rainy     | 0.30  | 0.40  | 0.30   | 1.0     |
| Cloudy    | 0.20  | 0.30  | 0.50   | 1.0     |

**Interpretation:**
If today is Sunny, tomorrow has:

* 70% chance of being Sunny
* 20% Rainy
* 10% Cloudy

### Key Properties

* **Irreducible** → Every state is reachable from every other state
* **Aperiodic** → Self-transitions exist, removing cyclic behavior
* **Ergodic** → Guarantees a **unique stationary distribution**

---

## ⚙️ How the Simulation Works

The system can be visualized as a directed graph where:

* Nodes = weather states
* Edges = transition probabilities

### Monte Carlo Simulation Workflow

1. Start from an initial state (e.g., Sunny)
2. For each time step:

   * Use the current state's transition probabilities
   * Randomly sample the next state
3. Record all visited states
4. Compute frequencies of visits

This yields an **empirical estimate** of the stationary distribution.

---

## 📊 Three Methods to Find Stationary Distribution

The stationary distribution ( \pi ) satisfies:

[
\pi T = \pi \quad \text{and} \quad \sum_i \pi_i = 1
]

It represents the long-run proportion of time spent in each state.

---

### 1️⃣ Monte Carlo Simulation

**Idea:** Approximate ( \pi ) by simulating the chain over many steps.

[
\hat{\pi}_i = \frac{\text{Number of visits to state } i}{N}
]

* Simple and intuitive
* Accuracy improves as simulation length increases

---

### 2️⃣ Power Iteration

**Idea:** Repeatedly multiply the transition matrix:

[
T^n \rightarrow
\begin{pmatrix}
\pi \
\pi \
\pi
\end{pmatrix}
\quad \text{as } n \to \infty
]

* Uses fast exponentiation
* Efficient for large powers
* Any row of ( T^n ) converges to ( \pi )

---

### 3️⃣ Analytical Solution

**Idea:** Solve a linear system:

[
\pi (T - I) = 0
]

With normalization constraint:

[
\sum \pi_i = 1
]

* Exact method
* Uses linear algebra (matrix solving)
* Most precise approach

---

## 📐 The Ergodic Theorem

For ergodic Markov chains:

[
\lim_{n \to \infty} \frac{1}{n} \sum_{t=1}^{n} \mathbf{1}(X_t = i) = \pi_i
]

### Meaning:

> Over time, the proportion of visits to each state converges to its stationary probability — regardless of the starting point.

This explains why all three methods converge to the same result.

---

## 📈 Results

### Stationary Distribution Comparison

| Method          | Sunny  | Rainy  | Cloudy |
| --------------- | ------ | ------ | ------ |
| Monte Carlo     | 0.4689 | 0.2822 | 0.2489 |
| Power Iteration | 0.4565 | 0.2826 | 0.2609 |
| Analytical      | 0.4565 | 0.2826 | 0.2609 |

### Interpretation

In the long run:

* 🌞 Sunny → ~45.65%
* 🌧️ Rainy → ~28.26%
* ☁️ Cloudy → ~26.09%

### Convergence Example

| Step | Sunny  | Rainy  | Cloudy |
| ---- | ------ | ------ | ------ |
| 0    | 1.0000 | 0.0000 | 0.0000 |
| 5    | 0.4685 | 0.2794 | 0.2521 |
| 10   | 0.4568 | 0.2825 | 0.2607 |
| 20+  | Stable |        |        |

> Convergence typically occurs within ~15–20 steps.

---

## 📊 Visualizations

The project generates a multi-panel dashboard including:

* Transition Matrix Heatmap
* Simulated Weather Timeline
* Convergence of Frequencies
* Distribution Evolution
* Method Comparison Bar Chart
* State Transition Sequences

These plots collectively demonstrate both **dynamic behavior** and **steady-state convergence**.

---

## 🛠️ Tech Stack

| Tool          | Role                                     |
| ------------- | ---------------------------------------- |
| `numpy`       | Matrix operations & probability sampling |
| `matplotlib`  | Data visualization                       |
| `collections` | Efficient counting of state visits       |

---

## 🚀 How to Run

### 1. Clone the repository

```bash
git clone https://github.com/sharpsalt/Weather_Markov_Chain_Simulator.git
cd Weather_Markov_Chain_Simulator
```

### 2. Install dependencies

```bash
pip install numpy matplotlib
```

### 3. Run the notebook

```bash
jupyter notebook Markov.ipynb
```

Or open in VS Code and run all cells.

---

