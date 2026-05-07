# electronics/

PCB pointers, power tree, and Joule SOM integration for Free Humanoid Wheeled.

This directory will hold:

- **Power tree.** 2 kWh primary battery (two 1 kWh hot-swap modules at 48 V nominal) feeding a wheeled-base power-distribution PCB; 24 V and 12 V DC-DC rails for compute and sensors; isolated UPS bus (0.2 kWh, 24 V) for the safety supervisor and compute domains.
- **Joule SOM integration board.** Same as the bipedal sibling — abstract-HAL-conforming carrier for Joule SOM as recommended-default. Alternatives (Jetson Orin, Pi 5 + Coral, Intel NUC) swap in at the same physical interface.
- **Drive-motor controller boards.** mjbots Moteus n1 per drive wheel, plus the CAN-FD harness from the chassis to the upper body.
- **Sensor breakouts.** Lidar mount + power, GPS module, 5G modem (mPCIe or M.2), JANUS acoustic-receiver, base tilt sensor, wheel-axle encoders.
- **Shore-power umbilical interface.** 48 V DC from the dock-B charging station, locking connector, BMS handshake.

## Cross-references

- Architectural argument: [ARCHITECTURE §2.5 Compute](../ARCHITECTURE.md#25-compute), [§2.10 Power](../ARCHITECTURE.md#210-power), [§2.11 Comms](../ARCHITECTURE.md#211-comms)
- Inherited from bipedal sibling: [free-humanoid-platform/electronics/](../../free-humanoid-platform/electronics/)
- Corpus shielding: [prior-art/INDEX.md Subsystems 5, 8](../prior-art/INDEX.md#subsystem-5--sensing)

## Open-tooling buildability

Per [CONTRIBUTING.md](../CONTRIBUTING.md): all PCB layouts must be readable in KiCad. Closed-tool-only contributions do not meet the open-everything principle.

## Status

v0.1 scaffold. Detailed PCB layouts are Phase 1 / Phase 2 deliverables.
