
# Deep Reinforcement Learning for Multi-User RF Charging with Non-linear Energy Harvesters

This repository reproduces the wireless power transfer (WPT) study presented in the *IEEE GLOBECOM 2024* paper on deep reinforcement learning (RL) for multi-user charging. It contains a self-contained Jupyter notebook that instantiates the simulation scenario, trains the learning agent, and evaluates performance metrics that match the publication.

The core experiment trains a Deep Deterministic Policy Gradient (DDPG) agent to coordinate near-field energy beamforming and transmit-power allocation so that several low-power devices can maintain their batteries while the access point expends minimal energy.

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
   The notebook also relies on standard Python libraries such as `math`, `collections`, and `itertools` that ship with CPython.

2. **Launch Jupyter**
   ```bash
   pip install notebook
   jupyter notebook
   ```
   Open `RL_charging.ipynb` from the Jupyter interface and execute the cells sequentially.

## Notebook roadmap

The notebook is organized into the following stages:

1. **Scenario definition** – Configures a 5 m × 5 m × 5 m simulation volume with an 8 × 8 planar transmit array and four users arranged on a circle. The code sets the 2.4 GHz carrier, geometric spacing, and per-user locations used for channel calculations.

2. **Wireless channel and energy-harvester modeling** – Computes near-/far-field regimes, per-element channel coefficients, and a nonlinear logistic energy-harvesting curve that maps received RF power to direct-current charging power.

3. **Traffic generation placeholders** – Initializes per-device demand traces (`traffic_dict`) for every episode and time slot. Replace the placeholder zero traces with application-specific arrivals to replicate the paper’s traffic patterns.

4. **Wireless charging environment (`WC_Env`)** – Encapsulates the battery dynamics, outage tracking, and reward shaping. The state stores each device’s energy level, the action vector contains fractional beam allocations along with a normalized transmit-power command, and the reward penalizes unsatisfied demand plus excessive power draw.

5. **DDPG agent** – Implements replay memory, an Ornstein–Uhlenbeck exploration process, and actor/critic networks with two hidden layers (128→64 neurons for the actor, 64→32 for the critic). Training uses soft target-network updates, Adam optimizers, and configurable exploration decay.

6. **Training loop** – Instantiates the environment and agent with default hyperparameters (memory size 10,000, batch size 64, 1,500 training episodes) before invoking `agent.train(num_frames)`. Training records per-episode reward, transmit power, and outage statistics for later visualization.

7. **Evaluation and baselines** – Provides plotting utilities as well as two heuristic charging strategies (a demand-prediction policy and a buffer-threshold policy) for comparison against the learned agent. Additional cells compute moving averages of reward, outage probability, and consumed power to mirror the figures reported in the paper.

## Reproducing the main experiment

1. Run all scenario, channel, and environment cells to populate the array geometry and energy-harvesting model.
2. Customize the traffic generation block if you need non-zero demand traces.
3. Execute the agent initialization cell, followed by `agent.train(num_frames)` to launch training.
4. After training completes, run the plotting and baseline-evaluation cells to generate reward, outage, and power comparison figures.


Training is compute-intensive: each episode simulates 100 time slots and requires repeated convex optimizations inside the environment step. GPU acceleration (if available to PyTorch) can speed up neural network updates, but CVXPY’s solvers currently run on CPU.

## Citing

If you use this code, please cite the associated article. A BibTeX entry is provided below.

```bibtex
@INPROCEEDINGS{RL_WPT_azarbahran,
  author={Azarbahram, Amirhossein and López, Onel L. A. and Popovski, Petar and Pandey, Shashi Raj and Latva-aho, Matti},
  booktitle={GLOBECOM 2024 - 2024 IEEE Global Communications Conference}, 
  title={Deep Reinforcement Learning for Multi-User RF Charging with Non-linear Energy Harvesters}, 
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
