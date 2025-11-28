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

This README explains the theory, math, and code structure so you can learn MDPs deeply.

---

# 🧠 1. Problem Summary

You manage the daily operations of a warehouse storing **one type of product**.

Every day:

1. You observe the **on-hand inventory**  
2. You choose an **order quantity**  
3. The order **arrives immediately** (no lead time)  
4. Customer **demand occurs** (Poisson)  
5. You sell what you can; unmet demand is lost  
6. Holding costs, ordering costs, and stockout penalties apply  

Goal:

**Maximize long-run expected profit**  
(or equivalently minimize long-run expected total cost)

---

# 🔧 2. MDP Formulation

## 👉 State
$$
s \in \{0,1,2,\dots,S_{\max}\}
$$

## 👉 Action
$$
a \in \{0,1,2,\dots,A_{\max}\}
$$

## 👉 Demand
$$
D \sim \text{Poisson}(\lambda)
$$

## 👉 Transition Dynamics

Inventory before demand:
$$
\text{inv\_pre} = \min(S_{\max},\, s + a)
$$

Units sold:
$$
\text{units\_sold} = \min(\text{inv\_pre}, D)
$$

Next state:
$$
s' = \text{inv\_pre} - \text{units\_sold}
$$

## 👉 Reward Function

Revenue:
$$
\text{revenue} = p_s \cdot \text{units\_sold}
$$

Ordering cost:
$$
\text{order\_cost}=
\begin{cases}
K + c_p a & a > 0 \\
0 & a = 0
\end{cases}
$$

Holding cost:
$$
h \cdot s'
$$

Stockout penalty:
$$
p_b \cdot (D - \text{units\_sold})
$$

Final reward:
$$
r(s,a,D)=
\text{revenue}
- \text{order\_cost}
- \text{holding\_cost}
- \text{stockout\_cost}
$$

---

# 🧩 3. Project Components

## ✔️ 3.1 Simulation Environment

A Gym-like environment that supports:

- `reset()`
- `step(action)`
- `render()`
- Poisson demand sampling
- Reward computation

## ✔️ 3.2 Heuristic Base-Stock Policy

$$
a(s) = \max(0, S - s)
$$

We evaluate many values of \( S \) using Monte-Carlo simulation.

## ✔️ 3.3 Tabular MDP Model (P, R)

Transition probabilities:
$$
P(s'|s,a)=\sum_{d: s' = f(s,a,d)} P(D=d)
$$

Expected rewards:
$$
R(s,a)=\sum_d P(D=d)\, r(s,a,d)
$$

## ✔️ 3.4 Value Iteration

Bellman optimality update:
$$
V_{k+1}(s)=\max_a \left[
R(s,a) + \gamma
\sum_{s'} P(s'|s,a)V_k(s')
\right]
$$

Optimal policy:
$$
\pi^*(s)=\arg\max_a \left[
R(s,a)+\gamma\sum_{s'}P(s'|s,a)V^*(s')
\right]
$$

---

# 📈 4. Visualizations

Plots include:

- Average reward vs order-up-to level \( S \)
- Optimal policy \( \pi^*(s) \)

Typical behavior:

- Low \( S \) → too many stockouts  
- High \( S \) → too much holding cost  
- Middle \( S \) → best  
- Value iteration policy looks like a **base-stock** policy  

---

# 🏆 5. Key Outcomes

You will learn:

- How to model real operations problems as MDPs  
- How to write a custom simulation environment  
- How to build \(P(s'|s,a)\) and \(R(s,a)\)  
- How to implement **Value Iteration**  
- How to extract the optimal policy  
- How to compare RL / DP solutions to industry heuristics  

This builds foundations in:

- Reinforcement Learning  
- Operations Research  
- Stochastic Control  
- Supply Chain Optimization  

---

# 🚀 6. Extensions (Optional)

You can extend this project with:

- Lead times  
- Backorders  
- Multi-item inventory  
- Q-learning / SARSA  
- Deep RL (DQN, PPO, SAC)  
- Continuous state MDPs  
- Approximate DP  

---

# 🙌 7. Credits

This project is designed as a **teaching notebook** to build  
strong intuition about MDPs in real-world inventory control.

Feel free to adapt, extend, and experiment!
