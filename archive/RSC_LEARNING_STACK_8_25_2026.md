# Robotics Simulation & Control Learning Stack

Each stage produces a portfolio artifact and reuses the previous stage's work.
The through-line: build a robot in sim, instrument it properly, and close a
control loop around it over ROS 2 — the same discipline as instrumenting and
controlling a physical plant, with the plant replaced by physics simulation.

*This is the control-engineering reorganization of Stage 5 from the
[original learning stack](LEARNING_STACK.md). Stages 1–4 (RL foundations through
GPU RL in Isaac Lab) are complete.*

---

## Stage 5A — Isaac Sim fluency & the standalone Python loop  ✅ complete

**Goal:** get past the GUI. Drive the simulator from a standalone Python script.

**Do:** load a stage, spawn an articulation, step physics, and read/write joint
state in a loop. Command a joint trajectory from pure Python and log the
response. No ROS yet.

**Learn:** the standalone app lifecycle, the articulation API, physics-step vs.
rendering-step timing, USD stage basics (prims, references).

**Artifact:** a repo that commands a trajectory and logs actual vs. commanded
joint response —
[franka-joint-tracking](https://github.com/jaustinUF/franka-joint-tracking).

*Proving you can drive the plant.*

---

## Stage 5B — Instrumenting the robot

**Goal:** add and characterize a full sensor suite — where instrumentation
background shows.

**Do:** attach IMU, contact, and force/effort sensors plus one camera at the
correct points in the stage hierarchy. Log every channel. Then characterize
them: noise, sampling rate vs. physics rate, coordinate-frame conventions, unit
conversions, and any calibration needed.

**Learn:** the split between physics-based sensors (contact, IMU, force, effort,
proximity) and RTX-rendered sensors (camera, lidar, radar); how ground truth is
available directly from sim for validation.

**Artifact:** a sensor-characterization report/notebook. Few people entering the
field do this rigorously — it reads as senior.

---

## Stage 5C — Closing the loop over ROS 2  *(keystone project)*

**Goal:** drive the Stage 5B robot from an external controller over ROS 2.

**Do:** enable the ROS 2 bridge. Publish joint states + sensor data out;
subscribe to joint commands in. Write a classical controller as an external
ROS 2 node (PID / state-feedback / cascade loop) and close the loop on the sim
robot. Adopt the canonical interface shape: a state topic in, a command topic
out (mirroring the topic_based_ros2_control pattern).

**Learn:** ROS 2 Humble + the bridge, OmniGraph vs. Python control paths,
sim-time synchronization, DDS domains, and disciplined loop timing.

**Artifact:** a ROS 2 package + Isaac Sim scene where an external control node
closes the loop.

*The keystone — a control-engineering career restated in the new stack.*

---

## Stage 5D — Control comparison / hardware-in-the-loop

**Goal:** make the RL from earlier stages earn its keep — as a baseline, not the
main event.

**Do:** take one task and compare a hand-designed controller against a learned
policy on identical metrics: settling time, overshoot, tracking error,
disturbance robustness.

**Artifact:** a benchmark writeup positioning the author as someone who
understands both paradigms and can reason about when each fits.

---

## Stage 5E — Perception-in-the-loop  *(optional bridge)*

**Goal:** bring the camera into the control loop.

**Do:** visual servoing, or feed a perception output back into the controller.
Opens a natural door to the synthetic-data / digital-twin lane later (Replicator,
domain randomization).

**Artifact:** a closed vision → control loop.

---

## Roadmap at a glance

*(Stages 1–4 — complete)*

- **5A** — standalone Python control loop ✅
- **5B** — instrument & characterize sensors
- **5C** — close the loop over ROS 2  ← keystone
- **5D** — classical vs. learned control benchmark
- **5E** — perception-in-the-loop
- **6** — sim-to-real on physical hardware
