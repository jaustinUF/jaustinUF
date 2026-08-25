# Jim Austin

**Robotics simulation, approached as a control and systems problem.**

Engineer (BSEE, MSEE, University of Florida) with 30+ years in software,
instrumentation, and control systems, now working in physics-based robotics
simulation — NVIDIA Isaac Sim / Isaac Lab / Omniverse. My background is in
classical control and instrumentation, and I use it as the way into robotics
simulation: treating the simulated robot as a plant to be instrumented and
controlled, with reinforcement learning as one tool in the kit rather than the
whole story.

---

## Current focus

Building a control-engineering track through the robotics simulation stack —
driving articulated robots from standalone code, instrumenting them, and closing
control loops around them, the same discipline as instrumenting and controlling a
physical plant with the plant replaced by physics simulation.

Most recent work:

**[isaac-ros2-external-control](https://github.com/jaustinUF/isaac-ros2-external-control)** —
external control of a simulated Franka arm over ROS 2. An external controller
process, in its own environment (native ROS 2 Jazzy, system Python), closes a
joint-space loop on a Franka arm in Isaac Sim — the two processes never share a
Python interpreter, communicating only over ROS 2 topics carried by DDS. That
seam, where an outside process drives and reads a simulated robot over standard
robotics middleware, is the competency: the same boundary that exists on real
hardware, with the physical robot replaced by simulation. Uses
`sensor_msgs/JointState` on both edges, mapped by joint name. The keystone of the
Stage 5 control track.

**[franka-sensor-characterization](https://github.com/jaustinUF/franka-sensor-characterization)** —
characterizes a Franka arm's full sensor suite in Isaac Sim (camera, contact,
IMU, joint force/effort): measuring each sensor's shape, units, rate, frame, and
noise, and validating readings against ground truth or hand-computed arithmetic.
Its core artifact is a **sim-to-hardware transfer ledger** that tags every
characterized property as *model*, *execution-bound*, or *mixed* — a
classification that predicts which signals survive transfer to real hardware and
which won't. Findings include a camera with no modeled sensor noise, an IMU with
none of the noise/bias/drift a real device is dominated by, and a per-joint 6-axis
wrench with no hardware analog at all.

**[franka-joint-tracking](https://github.com/jaustinUF/franka-joint-tracking)** —
standalone Isaac Sim scripts that command a Franka arm's joints, log
commanded-vs-actual response, and characterize the joints' tracking frequency
response. The result is a measured Bode plot: the joint controllers track as a
low-pass system with a −3 dB bandwidth of ~0.8 Hz, uniform across joints,
consistent with load-matched PD tuning. An instrument built to measure a robot's
joint servo bandwidth.

---

## The path here

This work sits on top of a staged learning path from reinforcement-learning
foundations up through GPU-accelerated RL in Isaac Lab, then a deliberate
reorientation toward the control-and-instrumentation track above.

- **Stages 1–4 — RL foundations → robotic RL in simulation.** Tabular methods →
  neural policies (DQN, REINFORCE, A2C, PPO) → continuous control in physics
  (Pendulum, BipedalWalker, MuJoCo) → vectorized GPU RL in Isaac Lab (Cartpole,
  Franka arm, quadruped locomotion, manipulation).
- **Stage 5 — the control track** (current), detailed below.

Full detail: [learning stack overview](LEARNING_STACK.md) ·
[Stage 5 control track](RSC_Learning_Stack.md)

---

## Stage 5 roadmap — the control track

A staged path, each stage producing a portfolio artifact and reusing the last:

- **5A — standalone control loop** ✅ *complete* —
  [franka-joint-tracking](https://github.com/jaustinUF/franka-joint-tracking).
  Drive the simulator from standalone Python; command joint trajectories; log
  and characterize the response.
- **5B — sensor instrumentation & characterization** ✅ *complete* —
  [franka-sensor-characterization](https://github.com/jaustinUF/franka-sensor-characterization).
  Attach and characterize IMU, contact, force/effort, and camera sensors — noise,
  sampling rate, frame conventions — each validated against ground truth or
  arithmetic, with a sim-to-hardware transfer ledger. The part where
  instrumentation background shows.
- **5C — closing the loop over ROS 2** *(keystone)* ✅ *complete* —
  [isaac-ros2-external-control](https://github.com/jaustinUF/isaac-ros2-external-control).
  An external controller in its own environment (native ROS 2 Jazzy) closes a
  joint-space loop on the sim robot over the ROS 2 bridge — a two-process /
  one-DDS-graph architecture where the sim and controller never share a Python.
  JointState on both edges, mapped by joint name. The transport competency
  proven end-to-end; the control-law depth (PID on a gravity-loaded joint)
  explored and scoped out deliberately, documented rather than forced.
- **5D — classical vs. learned control benchmark.** Hand-designed controller vs.
  a learned policy on identical metrics — where the earlier RL work earns its
  keep as a baseline.
- **5E — perception in the loop.** Bring the camera into the control loop
  (visual servoing).
- **5F — ER 2 grounding check** *(optional).* Verify a vision-language model can
  reason about the sim camera feed before building on it.
- **6 — ER-2-driven orchestration** *(in sim).* A vision-language reasoning layer
  (Gemini Robotics ER 2) plans multi-step tasks and drives the Stage 5C
  controller as a callable tool — the reasoning layer on top of the trusted
  control substrate.
- **7 — sim-to-real** on physical hardware (deferred pending hardware).

---

## Background

30+ years across software engineering, electronic hardware and instrumentation,
and control systems and automation. That perspective shapes how I approach
robotics simulation: as a problem of dynamics, feedback, sensing, and
interaction. I also build tool-using AI systems (supervisor/worker agent
patterns, MCP servers and tools).

---

📫 [LinkedIn](https://www.linkedin.com/in/jim-austin-a476b562/)
