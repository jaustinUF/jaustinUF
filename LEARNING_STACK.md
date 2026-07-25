# RL → Robotics → Omniverse Learning Stack

*The original staged learning path, from reinforcement-learning foundations up
through Isaac Sim and sim-to-real. This is the backstory to the control-focused
[Stage 5 track](RSC_LEARNING_STACK.md); Stages 1–4 here are complete.*

---

## Stage 1 — Foundations of RL (2D control labs)

**Purpose:** build deep intuition about reward, dynamics, and the RL loop.

- CartPole
- MountainCar
- Acrobot
- Pendulum — first continuous-action problem (optional but recommended)

**Outcome:** an understanding of delayed rewards, stability, energy-based
control, continuous vs. discrete actions, and actor–critic intuition — the
foundations robotics builds on.

---

## Stage 2 — Neural policy learning (minimal depth, still 2D)

Neural policies, still in simple worlds:

- DQN (value-based control)
- REINFORCE (policy gradients)
- A2C (actor–critic)
- PPO (the algorithm behind many robotics papers)

Builds understanding of how neural networks represent policies, how value
functions stabilize learning, and why PPO is the standard in robotics
simulators.

---

## Stage 3 — Transition to robotics: continuous control + physics

The stage missing from most tutorials, and essential for Omniverse — RL
environments that reflect real robotics physics.

Key environments: Pendulum-v1 (basic torque control), MountainCarContinuous-v0,
LunarLanderContinuous-v2, BipedalWalker-v3 (first "robot-like" agent), and
MuJoCo locomotion (Hopper, HalfCheetah, Walker2d, Humanoid).

Skills gained: torque-based control, continuous action distributions, reward
shaping for real physics, multi-joint coordination, and stable training in noisy
environments. Directly preparatory for Isaac Sim.

---

## Stage 4 — Introduction to NVIDIA Isaac Lab (fast GPU RL)

Before full Omniverse — Isaac Lab is lightweight, pure Python, and extremely fast
(thousands of environments in parallel on GPU).

Trained: a 3D cartpole, a robotic arm, quadruped locomotion, and simple
manipulation tasks. Teaches vectorized environments, parallel rollouts,
GPU-accelerated RL, high-dimensional observations, and much faster feedback
cycles.

---

## Stage 5 — NVIDIA Omniverse + Isaac Sim (the robotics platform)

The transition to Omniverse / Isaac Sim: OpenUSD worlds, articulated robots,
physics-based manipulators, camera / LiDAR / IMU sensors, ROS 2 integration,
domain randomization, curriculum learning, and sim-to-real workflows.

*This stage was subsequently reorganized into a control-engineering track — see
[RSC_LEARNING_STACK.md](RSC_LEARNING_STACK.md) for the detailed 5A–5E plan.*

---

## Stage 6 — Sim-to-real robotics (long-term destination)

Train robots in a digital twin, deploy to physical hardware (Jetson,
microcontrollers, PLCs), evaluate, and iterate — designing policies that transfer
to the real world.

---

## Roadmap at a glance

2D control intuition (CartPole → MountainCar → Acrobot)
→ Neural policy learning (DQN → REINFORCE → A2C → PPO)
→ Continuous control + physics (Pendulum → MuJoCo)
→ Isaac Lab (fast GPU training)
→ Omniverse / Isaac Sim (3D robots, sensors, USD worlds)
→ Sim-to-real robotics
