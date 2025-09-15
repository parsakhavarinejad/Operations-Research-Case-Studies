# 🚢 Container Terminal Congestion Reduction Problem

This case study explores an optimization problem from **container terminal operations**, where congestion arises due to uneven distribution of containers across storage blocks. The goal is to design and test approaches that reduce congestion by **equalizing block fill-ratios**.

---

## 📌 Problem Motivation

During field observations at the HIT container terminal, it was noted that:

- **Occupancy (fill-ratio) of blocks** varied significantly:

  $$
  \text{Fill-ratio of block } i = \frac{\text{Number of containers in block } i}{\text{Number of storage positions in block } i}
  $$

- Blocks with **higher fill-ratios** experienced much heavier truck traffic, leading to **road congestion** inside the terminal.  
- Idea: **Equalize fill-ratios across blocks** at the **end of each planning period** (e.g., 4 hours).  
  - This ensures traffic is more evenly distributed.  
  - End-of-period snapshot avoids overly complex continuous balancing.

---

## 📊 Model Formulation

We define the following:

- $x_i$: decision variable = number of **new containers** sent to block $i$ in this period.  
- $a_i$: number of **stored containers** that remain in block $i$ at end of the period (if no new containers are added).  
- $N$: total number of new containers expected to arrive.  
- $B$: total number of blocks.  
- $A$: storage positions per block.  

At the end of the period, the **yard-wide fill-ratio** is:

$$
F = \frac{N + \sum_i a_i}{A \times B}
$$

If perfectly equalized, each block should have fill-ratio $F$.  

Thus, for each block $i$:

$$
a_i + x_i \approx A \times F
$$

---

## 🧮 Linear Programming (LP) Formulation

We introduce deviation variables $u_i^+, u_i^-$ to measure how far block $i$'s fill level deviates from the target:

**Objective:**  

$$
\text{Minimize } \sum_i (u_i^+ + u_i^-)
$$

**Constraints:**  

$$
a_i + x_i - (u_i^+ - u_i^-) = A \times F \quad \forall i
$$  

$$
\sum_i x_i = N
$$  

$$
x_i, u_i^+, u_i^- \geq 0
$$

This is a **small LP** (variables ≈ number of blocks, typically ~100), making it computationally efficient.

---

## 🔢 Combinatorial Solution

Thanks to the special structure of the LP, a simple greedy algorithm can produce the **optimal solution**:

1. **Sort blocks** by $a_i$ (increasing order).  
2. **Allocate containers** in order:
   - For block $1$:  
     $$
     x_1 = \min \Big\{ N, \max\{0, A \times F - a_1\} \Big\}
     $$
   - For block $i \geq 2$:  
     $$
     x_i = \min \Big\{ \max\{0, A \times F - a_i\}, N - \sum_{r=1}^{i-1} x_r \Big\}
     $$

This **greedy allocation** ensures fill-levels approach uniformity while respecting capacity and total arrivals.

---

## 🚀 Planned Implementations

This case study will provide two approaches for solving the problem:

1. **LP with Pyomo**  
   - Formulate the problem directly in Pyomo.  
   - Solve using an LP solver (e.g., GLPK, CBC, or Gurobi).  

2. **Combinatorial Algorithm**  
   - Implement the greedy allocation scheme in Python.  
   - Compare runtime and results to Pyomo.  

---


## ✅ Expected Outcomes

- **Balanced fill-ratios** across blocks.  
- **Reduced truck congestion** in terminal roads.  
- Comparison of **LP vs. Combinatorial approach**:
  - Pyomo: more general, flexible, but solver-dependent.  
  - Combinatorial: lightweight, fast, and exact for this problem structure.  

---

✍️ *Inspired by real-world container terminal operations, with a focus on bridging optimization models and practical implementability.*
