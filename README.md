# RL-Adaptive-Scheduler-for-Real-Time-Cyber-Physical-Systems
RL-PPO scheduler that reduces timing side-channel leakage from 99.3% → 22.2% while improving real-time schedulability.
# RL-Adaptive Scheduler for Real-Time CPS [![Paper](https://img.shields.io/badge/Paper-IEEE%20Blue-blue)](https://github.com/aalme034/RL-Adaptive-Scheduler/releases/latest/download/RTS_PAPER.pdf) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> **RL-Adaptive Scheduling to Mitigate Timing Side-Channel Attacks in Real-Time CPS**  
> Alejandro Almeida, Daniel Aviles Rueda — Florida International University  
> (To be published; preprint available)

Fully reproducible RL-based adaptive scheduler for mitigating timing side-channels in real-time CPS, using PPO on Gymnasium.

### Key Results (500 hyperperiods, 8-task set, U≈1.29)
| Metric                        | Baseline (EDF) | RL-Adaptive (PPO) | Improvement |
|-----------------------------|----------------|-------------------|-------------|
| Attack Success Rate         | 99.3%          | 22.2%             | 77.1% reduction |
| Deadline Miss Rate          | 24.8%          | 19.6%             | 5.2 pp better |
| Training Time (Laptop CPU)  | N/A            | <20 seconds       | N/A |
| Multi-Core Attack Success   | N/A            | 1.8% (aggressive) | N/A |
| Multi-Core Miss Rate        | N/A            | 58.7% (aggressive)| Trade-off noted |
| FPGA Validation Fidelity    | N/A            | High (Zynq-7000)  | Confirmed |

Dynamically calibrates jitter (0-9%) based on state, balancing security and schedulability.

## Features
- **RL Agent**: PPO for state-dependent jitter (utilization, latency variance, miss rate, etc.).
- **Gym Environment**: Custom Gymnasium for secure RT scheduling with KS-test attack simulation.
- **Simulator**: SimPy-based discrete-event simulator for uniprocessor/multi-core.
- **FPGA Deployment**: C-exported policy on Xilinx Zynq-7000 with FreeRTOS; timer interrupts for jitter.
- **Safety Shaping**: Conservative rewards (-200 for misses); EDF fallback for certifiability (DO-178C/ISO 26262).
- **Task Model**: 8 periodic tasks (Table I); secret/public classes with WCET variations.
- **Applications**: Autonomous vehicles, drones, medical devices, industrial robots.

## Certification Impact
While RL complicates traditional WCET analysis,
our bounded action space (4 discrete jitter levels)
enables hybrid verification—static analysis for worst-
case paths, learned policy for average-case security.
Future deployments must include updated timing analysis and re-certification artifacts to maintain DO-
178C/ISO 26262 compliance in safety-critical domains.
## Baseline EDF: attack success 99.3%, miss rate 24.8%.
<img width="600" height="600" alt="57232bd8feee47326f8e73c257d71e8221b5dfa1" src="https://github.com/user-attachments/assets/d7fc2887-6bbf-4007-a140-00e781d2dbd8" />

## Multi-core Trade-offs: 4-core drops attack to 1.8% but miss rate jumps to 58.7%.
<img width="600" height="600" alt="b55f7e5216ed892a2f02474c71cf99a44e8ff8ba" src="https://github.com/user-attachments/assets/39eeb472-0e1f-467f-8eb3-eed084d68086" />

## PPO learns state-dependent jitter (mean 12 ms) vs. uniform static.
<img width="600" height="600" alt="88e7b96dbf7daf5fd3f8b6ad49db0f7cc448313b" src="https://github.com/user-attachments/assets/c1eb6da4-e0ab-4fda-911f-83f45c677538" />


## PPO maintains high timing entropy even at high utilization.
<img width="600" height="600" alt="ab531a7710e8e1a37027761329d37a9c04a304f3" src="https://github.com/user-attachments/assets/90b77cc5-4c21-40d8-8a3e-91d6e42c222c" />

## Attack success rate (n=500 traces). RL reduces attack by 77.1%.
<img width="600" height="600" alt="eed9a34a4a55105c2d0cc5fe8005e48756365727" src="https://github.com/user-attachments/assets/460f016e-9564-4948-bade-9a0e4b9cfe89" />

## RL improves real-time performance by 5.2 pp. 
<img width="600" height="600" alt="f28809ef500028a98209fa47e28738cc2bd9e836" src="https://github.com/user-attachments/assets/8d1c33b4-8a73-41ca-b925-7d65ea87d59c" />

# Future Work
Our evaluation uses periodic tasks and discrete
jitter levels. Future work should extend to Aperiodic/sporadic workloads with DAG dependencies.
Continuous jitter actions via SAC or PPO-Clip and
Integration with formal timing analysis.






