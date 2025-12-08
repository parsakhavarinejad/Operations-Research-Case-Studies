# Operations Research Case Studies

This repository contains hands-on case studies that apply operations research, reinforcement learning, and machine learning techniques to real decision problems. Each project includes a readable problem overview, mathematical formulation, and an executable Jupyter notebook so you can reproduce results or adapt the models to your own context.

## What you will find
- **Documentation:** Every case study has its own `README.md` explaining the business setting, modeling choices, and solution approach.
- **Executable notebooks:** Python notebooks walk through data setup, model construction, and experiment results step by step.
- **Optimization, RL, and ML tooling:** Projects mix mathematical programming (e.g., Pyomo), heuristic search, reinforcement learning, and supportive machine learning/deep learning techniques where appropriate.
- **Sample outputs:** Some studies include solver exports, plots, or decision tables to illustrate results.

## Case studies

### 🚢 Container Terminal Congestion Reduction
Balance storage block fill ratios to reduce truck congestion inside a container yard. Includes a Pyomo formulation, an example solver export (`model.lp`), and a notebook that evaluates how reassigning new containers can equalize yard occupancy.

### 📦 Inventory Control with MDP + RL
Model a single-product warehouse as a Markov Decision Process. The notebook builds a simulation environment, benchmarks a heuristic base-stock policy, and then computes an optimal ordering strategy via tabular value iteration.

### 📈 Marketing Budget Optimization via RL
Frame marketing spend as a stochastic control problem with state variables for demand, brand equity, and cash. The notebook compares SARSA and Expected SARSA policies, with accompanying heatmaps and learning-curve visualizations.

## Getting started
1. Install Python 3 and Jupyter (e.g., via `pip install jupyterlab`).
2. Open a notebook (e.g., `jupyter lab "Container Terminal Congestion Reduction Problem"/notebook.ipynb`).
3. Run the cells to reproduce the analysis. Each notebook lists the Python packages it imports; install any missing ones in your environment.

## Contributing
Contributions and extensions are welcome—feel free to open an issue or submit a pull request if you improve a model, add experiments, or contribute additional projects to expand the collection.

---
✍️ Maintained by Parsa Khavarinejad
