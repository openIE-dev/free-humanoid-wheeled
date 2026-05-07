# base/

The wheeled-base subsystem — the morphology-distinguishing work for Free Humanoid Wheeled.

This directory will hold:

- **Mechanical CAD** for the chassis (600 × 800 × 350 mm aluminum monocoque), the drive-wheel mounts, the caster mounts, the battery-module bays, and the tower stand. FreeCAD or OpenSCAD source per the open-tooling buildability check in [CONTRIBUTING.md](../CONTRIBUTING.md).
- **Drive-wheel and caster specs** — pinned vendor part numbers, wheel-bore dimensions, motor-to-hub interface drawings.
- **Tower-stand and torso-lift mechanism** — prismatic actuator selection, vertical bearing arrangement, lidar mount at top.
- **Battery-bay mechanical interface** — single-module-at-a-time hot-swap mating geometry. Inherits the CC0 commons spec from the bipedal sibling's commitment #6.
- **Suspension** (TBD per [ARCHITECTURE §2.2](../ARCHITECTURE.md#22-wheeled-base)) — hard-mount in v0; spring-arm variant in a future descriptor variant.

## Cross-references

- Architectural argument: [ARCHITECTURE §2.2](../ARCHITECTURE.md#22-wheeled-base) (form factor and base topology)
- Corpus shielding: [prior-art/INDEX.md Subsystem 1, 2](../prior-art/INDEX.md#subsystem-1--wheeled-base-topology)
- Descriptor encoding: [descriptor/free-humanoid-wheeled.udd.json](../descriptor/free-humanoid-wheeled.udd.json) — the `chassis`, `L_drive_wheel`, `R_drive_wheel`, `L_caster`, `R_caster`, `battery_module_a`, `battery_module_b`, `tower_stand` links and their joints.

## Status

v0.1 scaffold. Detailed CAD is the Phase 1 deliverable, gated on the architectural-call resolution for differential-drive vs. omnidirectional, drive-wheel placement, and suspension.
