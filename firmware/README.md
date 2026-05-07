# firmware/

Firmware HAL, behaviors, MathGround safety supervisor, and comms for Free Humanoid Wheeled.

This directory will hold:

- **HAL layer.** The same hardware-abstraction-layer contract as the bipedal sibling — sensor reads, motor commands, safety hooks, perception subscriptions. The wheeled morphology adds drive-wheel velocity command and base-tilt subscription primitives.
- **Behavior tree (OpenLoco SkillGraphRuntime).** The six modes from [descriptor/free-humanoid-wheeled.udd.json](../descriptor/free-humanoid-wheeled.udd.json): `safe_pose`, `dock_approach`, `cartridge_swap`, `waste_retrieval`, `fleet_coordination`, `idle`.
- **MathGround safety supervisor.** Inherits the bipedal sibling's invariant set verbatim, adds three wheeled-base invariants:
  - **Slope tilt limit:** pitch ≤ 12°, roll ≤ 8°. Violation → brakes latched, upper body to low-COM safe pose.
  - **Edge-proximity hard-stop:** ≥ 80 mm vertical drop within projected stopping distance → brakes latched immediately.
  - **Low-battery-on-shore-power latch:** on shore power + battery < 20% → refuse motion commands that would un-tether.
  See [ARCHITECTURE §2.6](../ARCHITECTURE.md#26-safety-supervisor) for the full statement.
- **Comms.** ROS 2 + CAN-FD + WiFi 6 + 5G + WebRTC + JANUS-receiver. Inherited from the bipedal sibling plus the 5G and JANUS-receiver additions. See [ARCHITECTURE §2.11](../ARCHITECTURE.md#211-comms).
- **shoal-integration shim.** Cartridge-swap protocol state machine; waste-hopper retrieval state machine; biology-reservoir handshake parser; municipal-utility EAM REST client. This is the load-bearing original engineering and is enumerated in [ARCHITECTURE §5](../ARCHITECTURE.md#5-what-needs-original-engineering).

## Cross-references

- Architectural argument: [ARCHITECTURE §2.6 Safety supervisor](../ARCHITECTURE.md#26-safety-supervisor), [§2.8 Locomotion control](../ARCHITECTURE.md#28-locomotion-control), [§2.9 Mission integration with shoal](../ARCHITECTURE.md#29-mission-integration-with-shoal), [§2.11 Comms](../ARCHITECTURE.md#211-comms)
- Inherited from bipedal sibling: [free-humanoid-platform/firmware/](../../free-humanoid-platform/firmware/)
- shoal protocol surface: [clean_fish/ARCHITECTURE.md §4 The protocols](../../clean_fish/ARCHITECTURE.md)
- Corpus shielding: [prior-art/INDEX.md Subsystem 6](../prior-art/INDEX.md#subsystem-6--safety-supervisor-with-wheeled-specific-invariants), [Subsystem 9](../prior-art/INDEX.md#subsystem-9--mission-integration-with-shoal)

## Open-tooling buildability

All firmware must build with open toolchains (GCC / Clang / cargo / etc.).

## Status

v0.1 scaffold. The MathGround integration shim and the shoal-integration protocol stack are Phase 1 / Phase 2 deliverables.
