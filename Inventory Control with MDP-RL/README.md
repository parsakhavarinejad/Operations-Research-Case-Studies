# 📦 Inventory Control MDP – Full Project Overview

This project provides a complete, end-to-end implementation of a classical  
single-product inventory control problem using Markov Decision Processes (MDPs).

The repository (or notebook) includes:

- A fully functional simulation environment (`InventoryEnv`)
- A heuristic base-stock (order-up-to) policy
- Simulation-based policy evaluation
- Exact tabular MDP modeling (transition probabilities + expected rewards)
- Value Iteration to compute the optimal ordering policy
- Visualizations and comparisons between heuristic and optimal policies

This README explains the theory, math, and code structure in a GitHub-safe way.

---

# 🧠 1. Problem Summary

We manage the daily operations of a warehouse storing one type of product.

Each day:

1. Observe the on-hand inventory  
2. Choose an order quantity  
3. Ordered units arrive immediately  
4. Customer demand occurs (Poisson-distributed)  
5. Sell up to available inventory; unmet demand is lost  
6. Holding costs, ordering costs, and stockout penalties apply  

Goal: Maximize long-run expected profit (or minimize long-run expected cost)

---

# 🔧 2. MDP Formulation

## State  
Inventory level at the beginning of a day:  
`state = 0, 1, 2, …, S_max`

## Action  
Order quantity:  
`action = 0, 1, 2, …, A_max`

## Stochastic Demand  
Demand follows a Poisson distribution with mean lambda.

## State Transition

Given:
- `s` = current inventory  
- `a` = order quantity  
- `D` = random demand  

We compute:

1. `inv_pre = min(S_max, s + a)`  
2. `units_sold = min(inv_pre, D)`  
3. `next_state = inv_pre - units_sold`

## Reward (profit)

Components:

- `revenue        = selling_price * units_sold`
- `order_cost     = fixed_cost (if a > 0) + purchase_cost * a`
- `holding_cost   = holding_cost_per_unit * next_state`
- `stockout_cost  = penalty_per_unit * (D - units_sold)`

Daily reward:
`reward = revenue - order_cost - holding_cost - stockout_cost`

---

# 🧩 3. Project Components

## ✔️ 3.1 Simulation Environment (`InventoryEnv`)

Implements:
- `reset()`
- `step(action)`
- `render()`
- Poisson demand
- Inventory updates
- Reward computation

## ✔️ 3.2 Heuristic Policy: Base-Stock (Order-Up-To)

`action(s) = max(0, S - s)`

We evaluate this policy via simulation over many episodes.

## ✔️ 3.3 Tabular MDP Model

For every `(state, action)` pair we compute:

- Transition probabilities `P[state, action, next_state]`
- Expected rewards `R[state, action]`

This is done by summing over all possible demand values.

## ✔️ 3.4 Value Iteration

Uses the Bellman optimality update:

`V_new(s) = max over a [ R(s,a) + gamma * Σ P(s'|s,a) * V(s') ]`

After convergence:
`policy*(s) = argmax over a [ R(s,a) + gamma * Σ P(s'|s,a) * V(s') ]`

This yields the exact optimal inventory policy.

---

# 📈 4. Visualizations

We generate plots for:

- Average reward vs order-up-to level S
- Optimal policy returned by value iteration

Expected pattern:

- Small S → too many stockouts → low reward  
- Large S → too much holding cost → low reward  
- Best S is somewhere in the middle  
- Optimal DP policy resembles a base-stock (order-up-to) structure  

---

# 🏆 5. Key Learning Outcomes

By completing this project you will:

- Understand how inventory problems map to MDPs  
- Build a Gym-like environment from scratch  
- Construct P(s'|s,a) and R(s,a) tables  
- Implement Value Iteration  
- Derive the optimal inventory policy  
- Compare heuristic policies to optimal DP policies  
- Gain intuition about DP, RL, and operations research  

Useful for:

- Reinforcement Learning  
- Operations Research  
- Supply Chain Optimization  
- Stochastic Control  
- Dynamic Programming  

---

# 🚀 6. Extensions

You can extend this project by adding:

- Lead times  
- Backorders (instead of lost sales)  
- Multiple products  
- Approximate dynamic programming  
- Q-learning / SARSA  
- Deep RL (DQN, PPO, SAC)  
- Continuous state MDPs  

---

# 🙌 7. Credits

This project is designed as a learning notebook to help you build strong intuition  
about MDPs in the context of real industrial inventory control.

Feel free to adapt, extend, and experiment!
