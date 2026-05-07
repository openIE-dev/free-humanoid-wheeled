# descriptor/

The OpenLoco UDD descriptor is the source of truth for this morphology. Everything else — URDF for ROS 2, MJCF for MuJoCo, STL meshes, BOM, assembly instructions — falls out of [`free-humanoid-wheeled.udd.json`](free-humanoid-wheeled.udd.json) through the OpenLoco compiler.

## Structure

The descriptor at v0.1 contains:

- **`meta`**: morphology family is `humanoid_wheeled`. `corpus_anchors` enumerates the 21 corpus entries that shield the platform's load-bearing decisions. `siblings` cross-references the bipedal sibling descriptor and the shoal architecture document.
- **`robot.links`**: 8 base links (`chassis`, `L_drive_wheel`, `R_drive_wheel`, `L_caster`, `R_caster`, `battery_module_a`, `battery_module_b`, `tower_stand`) + 22 upper-body links inherited verbatim from the bipedal sibling. Each upper-body link carries an `_metadata.inherited_from: "free-humanoid-platform"` tag so the inheritance is traceable.
- **`robot.joints`**: 8 base joints (2× drive, 2× caster passive, 2× battery fixed, 1× chassis-to-tower fixed, 1× tower-lift prismatic) + 23 upper-body joints inherited verbatim. The bipedal `pelvis` becomes the child of `TOWER_lift` rather than a free root link.
- **`robot.actuator_slots`**: 3 base slots (2× QDD drive wheels, 1× linear tower lift) + 19 upper-body slots inherited.
- **`robot.modes`**: 6 modes — `safe_pose`, `dock_approach`, `cartridge_swap`, `waste_retrieval`, `fleet_coordination`, `idle`. Each annotated with corpus citations and shoal-integration cross-references.

## Compiling through OpenLoco

```sh
# from an OpenLoco checkout
cargo run -- validate /path/to/free-humanoid-wheeled.udd.json
cargo run -- generate /path/to/free-humanoid-wheeled.udd.json --all --output-dir out/
cargo run -- bake /path/to/free-humanoid-wheeled.udd.json --output-dir out/meshes/
```

## Status

Mass and inertia values are scaffold placeholders. Iterating to physical-build accuracy is gated on the wheeled-base architectural-call resolution (differential-drive vs. omnidirectional, drive-wheel placement, suspension — see [ARCHITECTURE §2.2](../ARCHITECTURE.md#22-wheeled-base)).

The descriptor is licensed CC0 1.0 — see [../LICENSE-DATA](../LICENSE-DATA).
