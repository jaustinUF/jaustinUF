# Robotics Simulation & Control Learning Stack

*Planning next steps in Isaac Lab robotics learning*

Each stage produces a portfolio artifact and reuses the previous stage's work. The through-line: build a robot in sim, instrument it properly, and close a control loop around it over ROS 2 — the same discipline as instrumenting and controlling a physical plant, with the plant replaced by physics simulation. From Stage 6 on, a vision-language reasoning layer (Gemini Robotics ER 2) is added on top of that trusted control substrate, and the whole stack points toward gratis robotics-feasibility work for small companies as the applied goal.

## Stage 5A — Isaac Sim fluency & the standalone Python loop

**Goal:** Get past the GUI. Drive the simulator from a standalone Python script.

**Do:** Load a stage, spawn an articulation, step physics, and read/write joint state in a loop. Command a joint trajectory from pure Python and log the response. No ROS yet.

**Learn:** The standalone app lifecycle, the articulation API, physics-step vs. rendering-step timing, USD stage basics (prims, references) carried over from your USD work.

**Artifact:** A repo that commands a trajectory and logs actual vs. commanded joint response.

*You're proving you can drive the plant.*

## Stage 5B — Instrumenting the robot (your home turf)

**Goal:** Add and characterize a full sensor suite — the part where 30 years of instrumentation shows.

**Do:** Attach IMU, contact, and force/effort sensors plus one camera at the correct points in the stage hierarchy. Log every channel. Then characterize them: noise, sampling rate vs. physics rate, coordinate-frame conventions, unit conversions, and any calibration needed.

**Learn:** The split between physics-based sensors (contact, IMU, force, effort, proximity) and RTX-rendered sensors (camera, lidar, radar); how ground truth is available directly from sim for validation.

**Artifact:** A sensor-characterization report/notebook. Almost nobody entering this field does this rigorously — it reads as senior.

## Stage 5C — Closing the loop over ROS 2 (keystone project)

**Goal:** Drive the Stage 5B robot from an external controller over ROS 2.

**Do:** Enable the ROS 2 Bridge. Publish joint states out; subscribe to joint commands in. Stand up an external controller as its own ROS 2 node — in its own environment (native Jazzy, system Python), talking to the sim ONLY over DDS topics — and close the loop on the sim robot. Adopt the canonical interface shape: JointState on both edges, mapped by joint name (mirroring the topic_based_ros2_control pattern). The core competency is THE SEAM: an external process closing a loop on Isaac Sim over the bridge. The control law is the vehicle, not the destination — a simple proportional loop is enough to prove the transport; a full PID is optional (see note below).

**Learn:** ROS 2 Jazzy (Ubuntu 24.04) + the Isaac Sim Bridge; the two-process / one-DDS-graph architecture (Isaac's bundled Python 3.11 rclpy on the sim side vs. native Jazzy Python 3.12 on the controller side — they never share a Python, only DDS topics); plain-rclpy control from a standalone SimulationApp script (no OmniGraph needed); QoS compatibility (RxO); sim-time synchronization; and disciplined loop timing.

**Artifact:** An Isaac Sim scene + a pair of plain-rclpy scripts (a bidirectional sim-side plant node and an external controller node, each with its environment wrapper) where an external process closes the loop over ROS 2. Shipped as a GitHub repo; a colcon package was deemed unnecessary for the learning objective.

*This is the keystone — your career restated in the new stack. COMPLETE: the transport competency (external control over ROS 2, the two-process architecture) was proven end-to-end. A PID arc was explored on a single gravity-loaded joint but deliberately not carried to completion — it turned into a manipulator-dynamics problem (joint limits, pose-dependent gravity, pure P cannot hold a gravity-loaded joint) that taught plant dynamics, not the sim seam. The loop is closable on this rig with a cooperative plant (gravity feedforward, or a gravity-neutral joint); we chose not to spend the time, as the marginal lesson was low and the plant tax high. See Stage_5C4_PID_what_we_learned.md for the full record.*

### Current status & agreed next path

*Stages 1–4, 5A, 5B, and 5C are complete. The agreed path from here: (1) ship the 5C GitHub repo from the proven 5C.3 artifacts; (2) 5E — perception-in-the-loop: stream the 5B-characterized camera out over ROS 2, pull frames into an external process, close a visual loop (same seam, richer signal); (3) 5F — ER 2 read-only grounding check: send one Isaac Sim camera frame to Gemini Robotics ER 2, verify 2D object-pointing (cheap, de-risks Stage 6); (4) 5D — reframed as a decision, not a commitment: analyze whether to have Gemini ER use an Isaac Sim camera video stream to assist arm RL training, decided when reached; then (5) Stage 6 — ER-2-driven orchestration, the embodied-reasoning layer that is the north star. Guiding principle: the transport work is a trusted substrate for the reasoning layer, so "done well enough to build on" beats "polished to a mirror finish." Re-verify Gemini Robotics ER 2 / VLA availability when Stage 6 is reached — current as of Aug 2026.*

## Stage 5D — Control comparison / hardware-in-the-loop

**Goal:** Make the RL you already learned earn its keep — as a baseline, not the main event.

**Do:** Take one task and compare your hand-designed controller against a learned policy on identical metrics: settling time, overshoot, tracking error, disturbance robustness.

**Artifact:** A benchmark writeup positioning you as someone who understands both paradigms and can reason about when each fits.

## Stage 5E — Perception-in-the-loop (optional bridge)

**Goal:** Bring the camera into the control loop.

**Do:** Visual servoing, or feed a perception output back into the controller. Opens a natural door to the synthetic-data / digital-twin lane later (Replicator, domain randomization) if you choose it.

**Artifact:** A closed vision→control loop.

## Stage 5F — ER 2 read-only grounding check (optional capstone)

**Goal:** Verify that the frames leaving your perception pipeline are something a vision-language model can reason about — before any of Stage 6 depends on it. No motion, no control loop, no tool orchestration.

**Do:** Take the camera you already stream to a ROS 2 topic in 5B/5E. Grab a single frame, send it to Gemini Robotics ER 2 via the Gemini API / Google AI Studio, and ask it to point at an object (e.g. “point at the object that doesn't belong”). Draw ER 2's returned 2D point back onto the frame and confirm the grounding lands where it should. Repeat across a few scenes and lighting conditions.

**Learn:** The ER 2 API request/response shape and its 2D-pointing output format; how your Isaac Sim camera's resolution, encoding, and coordinate conventions map to what the model expects; where grounding is reliable vs. brittle. This isolates the perception-reasoning layer as a clean test point — the instrumentation instinct applied to an LLM.

**Artifact:** A short grounding-validation notebook: input frames, ER 2's returned points overlaid, and a pass/fail read on grounding accuracy. De-risks all of Stage 6.

*Optional and cheap — one afternoon. If it feels like scope creep, skip it; the full integration waits for Stage 6 either way. But doing it here means Stage 6 adds exactly one new unknown on top of a trusted base.*

## Roadmap at a glance

2D control intuition → neural policy learning → continuous control + physics → Isaac Lab GPU RL

*(Stages 1–4 — complete)*

↓

**5A** — standalone Python control loop

**5B** — instrument & characterize sensors

**5C — close the loop over ROS 2 ← keystone (COMPLETE)**

**5D** — classical vs. learned control benchmark

**5E** — perception-in-the-loop

**5F** — ER 2 read-only grounding check (optional)

↓

**6 — ER-2-driven orchestration in sim ← reasoning layer**

↓

**7** — sim-to-real on physical hardware (pending hardware)

## Stage 6 — ER-2-driven orchestration (still in sim)

*The reasoning layer, added on top of the trusted Stage 5 control substrate. Gemini Robotics ER 2 becomes the high-level brain; your ROS 2 controller from 5C remains the hands. Everything stays in Isaac Sim — no hardware yet.*

**Goal:** Close an agentic loop where ER 2 perceives the sim scene, plans a multi-step task, and drives your existing ROS 2 controller to execute it — with the model self-correcting from a continuous video feed.

**Do:** Stand up ER 2 as the orchestrator via the Gemini API. Declare your Stage 5C controller (and any navigation/grasp helpers) as tools ER 2 can call — this is the model's native “declare low-level control interfaces as tools” pattern. Stream camera frames from Isaac Sim into ER 2, let it decompose a task (e.g. “sort these parts into the correct bins”) into steps, and route each step's command back through the ROS 2 bridge to the sim robot. Keep the tasks deliberative (pick-and-place, inspection, sorting) to match ER 2's think-then-act, API-latency profile — not tight dynamic control loops.

**Learn:** ER 2's tool-orchestration workflow and how to expose a ROS 2 controller as a callable tool; streaming multimodal (video) input into the model; long-horizon task decomposition, progress estimation, and self-correction (ER 2 tracks when steps begin/end and adapts on failure); the latency and cost envelope of an API-in-the-loop agent, and which tasks fit inside it. The observability discipline from 5B carries directly — you're now instrumenting an agent's decisions, with the sim providing ground truth to check them against.

**Artifact:** A working ER-2-orchestrated task in Isaac Sim: the model plans and calls your controller as a tool, closes the loop over ROS 2, and recovers from at least one injected failure. Plus a writeup of where the reasoning was trustworthy vs. brittle — the exact evaluative lens a small company would pay (eventually) to have applied to their process.

*Note on model access: ER 2 (the embodied-reasoning / orchestration model) is publicly available via the Gemini API and Google AI Studio — this is the accessible half and the right spine for Stage 6. The full Gemini Robotics 2 VLA (whole-body / dexterous motor control) is largely gated to hardware partners; your path is ER-2-as-brain plus a controller you build, not the VLA driving end-to-end. Worth re-checking availability when you reach this stage — the field is moving fast.*

## Stage 7 — Sim-to-real on physical hardware (pending hardware availability)

*Deferred until suitable hardware is on hand. The competency being added here — physical deployment — is deliberately separated from the reasoning competency of Stage 6, so each is learned against one unknown at a time.*

**Goal:** Transfer the Stage 6 stack (ER 2 orchestration + ROS 2 controller) from sim onto a real robot, and confront the gap between the digital twin and the physical plant.

**Do:** Bring up the same ROS 2 interface on real hardware; re-run a Stage 6 task on the physical robot. Address the sim-to-real gap: real sensor noise vs. characterized sim noise (5B pays off here), latency, calibration, safety interlocks, and domain shift in the camera feed the model reasons over. Consider On-Device 2 for the local-inference path where API latency or connectivity is a constraint.

**Learn:** Sim-to-real transfer in practice; where the 5B sensor characterization predicted (or mispredicted) real behavior; hardware safety and the semantic + physical safety checks these models are built around; the real cost/time envelope of standing a robot up on an actual task.

**Artifact:** A sim-to-real case study: the same task in sim and on hardware, the gaps that appeared, and how they were closed. This is the capstone that turns the whole stack into a credible small-business feasibility offering — a real process, simulated, then physically demonstrated, with an honest cost/ROI read.

## Where to get unstuck

- **NVIDIA Developer Forums — Isaac Sim sub-forum:** sensor / ROS 2 / bridge questions, answered by NVIDIA engineers. Highest-signal spot for this track.

- **NVIDIA Omniverse Discord (isaac-sim channel):** day-to-day “I'm stuck” help.

- **Isaac Lab GitHub Discussions:** code-level questions on the learning framework.

- **MoveIt community (moveit.picknik.ai):** manipulation + control overlap; closer to your background than the RL crowd.

- **ROS Discourse (discourse.ros.org):** serious robotics-integration practitioners, many from controls/hardware backgrounds.

- **Google AI Studio + Gemini API docs (ER 2):** for Stages 5F–6 — the robotics model reference, pointing/orchestration examples, and tool-declaration patterns.

*Companion files: the Stage 5C.x handoff notes and Stage_5C4_PID_what_we_learned.md capture the as-built path. (The original Humble-era starter scaffold was superseded by the plain-rclpy path actually used and was never built.)*
