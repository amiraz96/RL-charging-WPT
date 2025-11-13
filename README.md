# Deep Reinforcement Learning for Multi-User RF Charging with Non-linear Energy Harvesters

This repository accompanies the paper published in *IEEE GLOBECOM 2024* on reinforcement learning (RL) for multi-user charging. It provides a self-contained Jupyter notebook that recreates the simulation environment, RL agent, and evaluation workflow described in the publication. The notebook focuses on training a Deep Deterministic Policy Gradient (DDPG) agent to jointly tune DMA beamforming weights and transmit power so that multiple low-power devices can replenish their energy buffers while minimizing outages and energy expenditure.

## Repository contents

| Path | Description |
| --- | --- |
| `RL_charging.ipynb` | Primary notebook that defines the WPT scenario, implements the RL environment and agent, runs training, and visualizes performance. |
| `README.md` | Project documentation (this file). |

## Getting started

1. **Install Python dependencies**
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   pip install --upgrade pip
   pip install numpy torch gymnasium matplotlib cvxpy
   ```
   The notebook also relies on standard Python libraries such as `math`, `collections`, and `itertools` that ship with CPython.【F:RL_charging.ipynb†L1】

2. **Launch Jupyter**
   ```bash
   pip install notebook
   jupyter notebook
   ```
   Open `RL_charging.ipynb` from the Jupyter interface and execute the cells sequentially.

## Notebook roadmap

The notebook is organized into the following stages:

1. **Scenario definition** – Establishes a 5 m × 5 m × 5 m simulation volume containing a 64-element DMA transmitter and four user devices arranged on a circle. It configures the 2.4 GHz carrier, propagation constants, and DMA geometry used for channel calculations.【F:RL_charging.ipynb†L1】【F:RL_charging.ipynb†L1】

2. **Wireless channel and energy harvester modeling** – Computes near/far-field regions, per-element channel coefficients, and a nonlinear logistic energy-harvesting model that maps received RF power to direct-current charging power at each device.【F:RL_charging.ipynb†L1】【F:RL_charging.ipynb†L1】

3. **Traffic generation placeholders** – Initializes per-device demand traces (`traffic_dict`) for each episode and time slot. Replace the placeholder zero traces with application-specific arrival processes to reproduce the traffic patterns analyzed in the paper.【F:RL_charging.ipynb†L1】

4. **Wireless charging environment (`WC_Env`)** – Encapsulates battery dynamics, outage tracking, and reward shaping. The state captures each device’s stored energy, the action vector allocates fractional beamforming weights plus a normalized transmit-power command, and the reward penalizes unsatisfied demand as well as excessive power draw.【F:RL_charging.ipynb†L1】

5. **DDPG agent** – Implements replay memory, an Ornstein–Uhlenbeck exploration process, and actor/critic networks with two hidden layers (128→64 neurons for the actor, 64→32 for the critic). Training uses soft target-network updates, Adam optimizers, and configurable exploration decay.【F:RL_charging.ipynb†L1】【F:RL_charging.ipynb†L1】

6. **Training loop** – Instantiates the environment and agent with default hyperparameters (memory size 10,000, batch size 64, 1,500 training episodes) before invoking `agent.train(num_frames)`. Training records per-episode reward, transmit power, and outage statistics for later visualization.【F:RL_charging.ipynb†L1】【F:RL_charging.ipynb†L1】

7. **Evaluation and baselines** – Provides plotting utilities as well as two heuristic charging strategies (a demand-prediction policy and a buffer-threshold policy) for comparison against the learned agent. Additional cells compute moving averages of reward, outage probability, and consumed power to mirror the figures reported in the paper.【F:RL_charging.ipynb†L1】

## Reproducing the main experiment

1. Run all scenario, channel, and environment cells to populate the DMA geometry and energy-harvesting model.
2. Customize the traffic generation block if you need non-zero demand traces.
3. Execute the agent initialization cell, followed by `agent.train(num_frames)` to launch training.
4. After training completes, run the plotting and baseline-evaluation cells to generate reward, outage, and power comparison figures.

Training is compute-intensive: each episode simulates 100 time slots and requires repeated convex optimizations inside the environment step. GPU acceleration (if available to PyTorch) can speed up neural network updates, but CVXPY’s solvers currently run on CPU.【F:RL_charging.ipynb†L1】【F:RL_charging.ipynb†L1】

## Citing

If you use this code, please cite the associated article. A BibTeX entry is provided below.

```bibtex
@INPROCEEDINGS{RL_WPT_azarbahran,
  author={Azarbahram, Amirhossein and López, Onel L. A. and Popovski, Petar and Pandey, Shashi Raj and Latva-aho, Matti},
  booktitle={GLOBECOM 2024 - 2024 IEEE Global Communications Conference}, 
  title={Deep Reinforcement Learning for Multi-User RF Charging with Non-linear Energy Harvesters}, 
  year={2024},
  volume={},
  number={},
  pages={3075-3080},
  keywords={Radio frequency;Performance evaluation;Energy consumption;Power demand;Array signal processing;Markov decision processes;Wireless power transfer;Probability;Power system reliability;Internet of Things;Radio frequency wireless charging;energy beamforming;near-field channels;charging scheduling},
  doi={10.1109/GLOBECOM52923.2024.10901606}}
```

## License

The repository inherits the usage rights granted by the original authors. Refer to the paper or contact the authors for explicit licensing terms.
