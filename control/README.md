# control/

Differential-drive MPC, dock-approach controller, manipulation policy, and curbedge detection for Free Humanoid Wheeled.

This directory will hold:

- **Low-level differential-drive kinematics + per-wheel PID.** Standard textbook differential-drive (state: x, y, heading, linear and angular velocity; control: left and right wheel torque). Cycle 200–500 Hz on the control compute domain.
- **Differential-drive MPC.** Horizon ~1 s, cycle ~50 Hz. Constraints: linear velocity ≤ 1.5 m/s, angular velocity ≤ 1.0 rad/s. Cost: tracking-error + smoothness + obstacle-avoidance penalty. Hands off to the safety supervisor on slope-tilt or edge-proximity violations.
- **Dock approach controller.** Fine-positioning relative to the dock's optical fiducial (head stereo camera + dock-burst optical handshake from [shoal ARCHITECTURE §4](../../clean_fish/ARCHITECTURE.md)). Targets ±20 mm positional accuracy at the cartridge port.
- **Curbedge detection.** Lidar ground-plane processing on the perception domain; binary "edge ahead" signal subscribed by the safety supervisor.
- **Manipulation policy.** Inherited from the bipedal sibling — imitation-learning baseline (ACT or diffusion-policy variant) for cartridge-swap and waste-hopper retrieval. VLA upgrade path documented but not v0.1.
- **Whole-body MPC** for upper-body manipulation while the base is wheel-locked (`cartridge_swap`, `waste_retrieval` modes). Inherits the bipedal sibling's whole-body MPC reduced to upper-body-only.

## Cross-references

- Architectural argument: [ARCHITECTURE §2.7 Manipulation](../ARCHITECTURE.md#27-manipulation), [§2.8 Locomotion control](../ARCHITECTURE.md#28-locomotion-control)
- Inherited from bipedal sibling: [free-humanoid-platform/control/](../../free-humanoid-platform/control/)
- Corpus shielding: [prior-art/INDEX.md Subsystem 7](../prior-art/INDEX.md#subsystem-7--compute-and-learning-policy)

## Status

v0.1 scaffold. Differential-drive MPC + curbedge detection + dock-approach controller are Phase 1 deliverables (closed-loop in MuJoCo).
