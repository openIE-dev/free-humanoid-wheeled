# upper-body/

Upper-body subsystems for Free Humanoid Wheeled. **Inherits verbatim from [free-humanoid-platform](../../free-humanoid-platform/)**, the bipedal sibling. This directory exists to (a) document the inheritance, (b) hold any wheeled-morphology-specific deviations from the bipedal upper body (currently: none), and (c) cross-reference the bipedal sibling's authoritative documentation.

## What's inherited

| Subsystem | Bipedal sibling location | What we take |
|---|---|---|
| Torso, arms, hands, head | [`free-humanoid-platform/chassis/`](../../free-humanoid-platform/chassis/) | The 21-DoF upper body kinematic topology and structural design |
| Actuators | [`free-humanoid-platform/actuators/`](../../free-humanoid-platform/actuators/) | Hybrid distribution: harmonic-drive at shoulder/elbow, tendon at wrist/hand, QDD at neck/waist |
| Hand | [`free-humanoid-platform/chassis/hand-v0.1-*`](../../free-humanoid-platform/chassis/) | Underactuated 5-finger synergy hand with v0.1 vendor-pinned BOM |
| Head + sensing | [`free-humanoid-platform/electronics/`](../../free-humanoid-platform/electronics/) | Stereo camera, IMU at every link, force-torque at wrists, GelSight tactile fingertips |

## Wheeled-morphology-specific deviations

None at v0.1. The wheeled morphology takes the bipedal upper body unchanged. If a deviation is identified during Phase 1 simulation or Phase 2 prototype, it is recorded here with a corpus-citation rationale and a cross-reference into [ARCHITECTURE.md](../ARCHITECTURE.md).

## Cross-references

- Architectural argument (this morphology): [ARCHITECTURE §2.3 Upper body](../ARCHITECTURE.md#23-upper-body)
- Architectural argument (canonical bipedal statement): [free-humanoid-platform/ARCHITECTURE.md §2.4–§2.10](../../free-humanoid-platform/ARCHITECTURE.md#24-actuators)
- Corpus shielding: [prior-art/INDEX.md Subsystems 3, 4, 5](../prior-art/INDEX.md#subsystem-3--upper-body-kinematic-topology)
- Descriptor encoding: [descriptor/free-humanoid-wheeled.udd.json](../descriptor/free-humanoid-wheeled.udd.json) — every upper-body link and joint carries `_metadata.inherited_from: "free-humanoid-platform"`.

## Status

v0.1 scaffold. The inheritance-by-reference approach means this subsystem's status tracks the bipedal sibling's; see [free-humanoid-platform/README.md](../../free-humanoid-platform/README.md) for current state.
