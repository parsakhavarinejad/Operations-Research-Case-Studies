# Microgrid Dynamic Pricing using RL (A2C & PPO)

This case study explores how reinforcement learning can align household electricity consumption with dynamic grid prices. Two companion notebooks build lightweight simulators, train agents, and visualize how pricing signals influence demand response decisions.

## Data snapshot

The notebooks load `household_energy_consumption.csv`, which contains timestamped household data:

- `Avg_Temperature_C`: ambient temperature for the home.
- `Energy_Consumption_kWh`: energy drawn during the interval.
- `Household_Size`: number of occupants.
- `Peak_Hours_Usage_kWh`: share of consumption during peak hours.

## Notebooks

### `DynamicPricing_AC_Learning.ipynb`
- Implements a single-household environment with normalized observations and a reward that balances bill savings against thermal discomfort.
- Trains an advantage actor-critic (A2C) policy that outputs a continuous shedding ratio between 0 and 1.
- Evaluates the learned policy on a held-out sequence to show how actions respond to changing prices and temperatures.

### `DynamicPricing_PPO.ipynb`
- Extends the simulator to five parallel households sharing one policy network.
- Uses Proximal Policy Optimization (PPO) with clipped objectives to stabilize training across agents.
- Visualizes how the multi-agent policy adapts load shedding as neighborhood demand drives price signals.

## How to run

1. Open either notebook in Jupyter or VS Code.
2. Ensure `gymnasium`, `numpy`, `pandas`, `torch`, and `matplotlib` are available in your environment.
3. Execute cells sequentially to train and visualize the policies. Training loops are configured for short episodes to keep runtime manageable on CPUs.

## Key takeaways

- Continuous action policies can express nuanced demand response decisions instead of binary load-shed events.
- Quadratic discomfort penalties discourage aggressive shedding during hotter periods.
- Sharing a PPO policy across households enables cooperative responses to neighborhood-level pricing.
