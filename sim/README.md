# sim/

MuJoCo, Gazebo, and Drake simulation model pointers for Free Humanoid Wheeled.

This directory will hold:

- **MuJoCo / MJX model.** Compiled from [descriptor/free-humanoid-wheeled.udd.json](../descriptor/free-humanoid-wheeled.udd.json) by the OpenLoco compiler. The reference simulation environment for differential-drive MPC and dock-approach controller development.
- **Gazebo model.** For ROS 2 stack integration and lidar / GPS / 5G simulation. Compiled from the same UDD descriptor.
- **Drake model.** For higher-fidelity contact and manipulation simulation, particularly for cartridge-swap and waste-hopper retrieval.
- **Test scenes.** Mock dock-B fiducial layouts, curb-edge environments, slope-tilt scenarios, simulated shoal-fleet work-order streams.

## Cross-references

- Architectural argument: [ARCHITECTURE §3 The descriptor](../ARCHITECTURE.md#3-the-descriptor), [§6 Roadmap Phase 1](../ARCHITECTURE.md#6-roadmap)
- Inherited from bipedal sibling: [free-humanoid-platform/sim/](../../free-humanoid-platform/sim/)

## Compiling models from the descriptor

```sh
cargo run -- generate /path/to/free-humanoid-wheeled.udd.json --all --output-dir out/
# produces:
#   out/free-humanoid-wheeled.urdf  (ROS 2 / Gazebo)
#   out/free-humanoid-wheeled.xml   (MJCF for MuJoCo / MJX)
#   out/meshes/*.stl                (one per unique mesh hash)
```

## Status

v0.1 scaffold. MuJoCo model and dock-B mock scene are Phase 1 deliverables.
