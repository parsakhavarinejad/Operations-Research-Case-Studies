# 📦 Inventory Control MDP – Full Project Overview

This project provides a **complete, end-to-end implementation** of a classical  
**single-product inventory control problem** using **Markov Decision Processes (MDPs)**.

The repository (or notebook) includes:

- A fully functional **simulation environment** (`InventoryEnv`)
- A **heuristic base-stock policy** (order-up-to level policy)
- Simulation-based policy evaluation
- Exact **tabular MDP modeling** (transition probabilities + expected rewards)
- **Value Iteration** to compute the *provably optimal* ordering policy
- Visualizations and comparisons between heuristic and optimal policies

This README explains the theory, math, and code structure, so you can learn MDPs deeply.

---

# 🧠 1. Problem Summary

You manage the daily operations of a warehouse storing **one type of product**.

Every day:

1. You observe the **on-hand inventory**.
2. You choose an **order quantity**.
3. The order **arrives immediately** (no lead time).
4. Customer **demand occurs** (stochastic, Poisson).
5. You sell what you can; unmet demand is lost.
6. Holding costs, ordering costs, and stockout penalties apply.

Your goal:

> **Maximize long-run expected profit**  
> (equivalently, minimize long-run expected total cost)

---

# 🔧 2. MDP Formulation

We model this as a finite MDP.

### 👉 States
Inventory level at the beginning of a day:
\[
s \in \{0,1,2,\dots,S_{\max}\}
\]

### 👉 Actions
Order quantity:
\[
a \in \{0,1,2,\dots,A_{\max}\}
\]

### 👉 Stochastic Demand
Demand is random:
\[
D \sim \text{Poisson}(\lambda)
\]

### 👉 State Transition
Inventory before demand:
\[
\text{inv\_pre} = \min(S_{\max}, s + a)
\]

Sales:
\[
\text{units\_sold} = \min(\text{inv\_pre}, D)
\]

Next state:
\[
s' = \text{inv\_pre} - \text{units\_sold}
\]

### 👉 Reward (profit)
Revenue:
\[
\text{revenu}e = p_s \cdot \text{units\_sold}
\]

Ordering cost:
\[
\text{order\_cost} =
\begin{cases}
K + c_p a & a > 0 \\
0 & a = 0
\end{cases}
\]

Holding cost:
\[
h \cdot s'
\]

Stockout penalty:
\[
p_b \cdot (D - \text{units\_sold})
\]

Reward:
\[
r(s,a,D) = \text{revenue} - \text{order\_cost} - \text{holding\_cost} - \text{stockout\_cost}
\]

---

# 🧩 3. Project Components

### ✔️ 3.1 Simulation Environment  
A custom Gym-style environment (`InventoryEnv`) that implements:

- `reset()`  
- `step(action)`  
- `render()`  
- Poisson demand  
- Inventory updates  
- Reward computation  

---

### ✔️ 3.2 Heuristic Policy: Base-Stock / Order-Up-To Level

A very common industry policy:

\[
a(s) = \max(0, S - s)
\]

We evaluate this policy via simulation and compute average rewards.

---

### ✔️ 3.3 Tabular MDP Model (P, R)

We compute:

- Transition probabilities  
  \[
  P(s'|s,a) = \sum_{d : f(s,a,d)=s'} P(D=d)
  \]

- Expected reward:
  \[
  R(s,a) = \mathbb{E}[r(s,a,D)]
  \]

This creates a complete MDP model suitable for dynamic programming.

---

### ✔️ 3.4 Value Iteration

We solve the Bellman optimality equation:

\[
V_{k+1}(s) = \max_a \left[ R(s,a) + \gamma \sum_{s'} P(s'|s,a) V_k(s') \right]
\]

After convergence:

\[
\pi^*(s) = \arg\max_a \left[ R(s,a) + \gamma \sum_{s'} P(s'|s,a)V^*(s') \right]
\]

This yields the **optimal inventory policy**.

---

# 📈 4. Visualizations

The notebook plots:

- Average reward vs. order-up-to level \( S \)
- Optimal policy \( \pi^*(s) \) from dynamic programming

You’ll typically see:

- Low S → stockouts → bad performance  
- High S → holding cost dominates → bad  
- Optimal S in the middle  
- DP policy shows a **base-stock structure**

---

# 🏆 5. Key Outcomes

By the end of the project you will:

### ⭐ Understand how an industrial inventory problem is an MDP  
### ⭐ Learn to build a simulator and a tabular MDP model  
### ⭐ Implement value iteration from scratch  
### ⭐ Derive the **optimal** ordering policy  
### ⭐ Compare heuristic vs. optimal policies  
### ⭐ Gain deep intuition about DP & RL in operations research  

This project gives you a strong foundation for:

- Reinforcement Learning  
- Operations research  
- Stochastic control  
- Optimization in supply-chain systems  

---

---

# 🚀 6. Extensions (Optional)

You can extend this project by adding:

- Lead time  
- Backorders (instead of lost sales)  
- Multiple products  
- Approximate dynamic programming  
- Q-learning  
- Deep RL methods (DQN, PPO, SAC)  
- Continuous state approximations  

---

# 🙌 7. Credits

This project is designed as a **learning notebook** to build real intuition about MDPs  
with an industry-relevant example (inventory control).

Feel free to adapt, extend, and experiment!

