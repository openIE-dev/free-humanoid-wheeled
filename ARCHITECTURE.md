---
title: Architecture
layout: default
nav_order: 2
permalink: /ARCHITECTURE.html
---

# Architecture
{: .no_toc }

**Free Humanoid Wheeled — an open hardware, open firmware, open data wheeled-base humanoid reference design. Sibling morphology to the bipedal [Free Humanoid Platform](../free-humanoid-platform/ARCHITECTURE.md). Designed for the shoal dock-B service mission. Shielded by the [Free Humanoid Corpus](https://github.com/openIE-dev/free-humanoid-corpus).**

<details open markdown="block">
  <summary>Contents</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

A ~70 kg, paved-environment humanoid with a wheeled differential-drive base and the same upper body as the bipedal Free Humanoid Platform. Expressed as a single OpenLoco UDD descriptor that compiles to URDF, MJCF, STL meshes, BOM, and assembly through the existing OpenLoco toolchain. Every non-trivial design choice cites a specific entry in the Free Humanoid Corpus by id.

This document is the canonical full-system spec for the wheeled morphology. It cross-references — and does not duplicate — the bipedal sibling's [ARCHITECTURE.md](../free-humanoid-platform/ARCHITECTURE.md) for everything inherited (upper body, sensing, compute, safety, manipulation, learning policy). Decisions that diverge from the bipedal sibling are argued in full here.

---

## 0. Design principles

Six principles, an extension of the bipedal sibling's six (see [Free Humanoid Platform ARCHITECTURE §0](../free-humanoid-platform/ARCHITECTURE.md#0-design-principles)).

1. **Open everything.** Hardware, firmware, descriptors, BOM, control software, simulation models, training data, documentation. CERN-OHL-S 2.0 / Apache 2.0 / CC-BY-SA 4.0 / CC0 1.0 by artifact class.

2. **Modular at the interface, not at the implementation.** Inherited from the bipedal sibling. The morphology-swap *is itself* the worked example: the base subsystem is modular at the descriptor level — replacing the bipedal `legs` block with the wheeled `base` block produces this morphology, with the upper body unchanged.

3. **Mission-specific, not general-purpose.** This platform is shaped to the shoal dock-B service mission. The bipedal sibling is shaped to the larger open question of "what is an open humanoid for human environments?" — that is the wider scope, this is the sharper one. A wheeled humanoid with no mission is a worse design than a bipedal humanoid with no mission, because the wheeled morphology gives up stair-traversal *for* a payload and runtime budget that only pays off when the mission demands it.

4. **shoal-integration-aware.** The platform's protocols (cartridge handshake, waste-hopper interface, dock biology-reservoir status) are designed against the shoal dock-B spec ([shoal ARCHITECTURE §2.2](../clean_fish/ARCHITECTURE.md), §3, §4). The integration is explicit — not hidden behind an abstract "tasks" API — because the integration *is* the platform's reason to exist at v0.1.

5. **Fail-safe to inert.** Inherited verbatim. Every subsystem has a defined fail-safe state. On the wheeled base specifically, the fail-safe is "drop to a stable wheel-locked stance, latch brakes, kill drive rails." Slope-tilt and edge-proximity invariants are added to the bipedal sibling's safety supervisor invariant set.

6. **Prior-art-shielded by deep corpus chain.** Inherited. Every load-bearing decision in this document is shielded back to a corpus entry. The wheeled-base subsystem is shielded by the chain `dlr-justin` (2009) → `pr2` (2010) → `willow-pr1` (2008) → `pepper-softbank` (2014) → `toyota-hsr` (2012) → `reachy-2-pollen-2023` (2023) → `ascento` (2019) → `nao` (2006), plus the fictional anchors `forbidden-planet-robby` (1956), `magnus-robot-fighter` (1963), `b-9-lost-in-space` (1965), `silent-running-drones` (1972).

---

## 1. Fork map: what we take from where

### 1.1 From [Free Humanoid Platform](../free-humanoid-platform/) (the bipedal sibling)

Everything above the wheeled base. Specifically:

| Subsystem | Inherited section in bipedal ARCHITECTURE | What we take |
|---|---|---|
| Torso, arms, hands, head | [§2.2 Kinematic topology](../free-humanoid-platform/ARCHITECTURE.md#22-kinematic-topology), [§2.4 Actuators](../free-humanoid-platform/ARCHITECTURE.md#24-actuators) | 21-DoF upper body topology: 1-DoF waist + 2-DoF neck + 2× 7-DoF arms + 2× 1-DoF underactuated synergy hands. Inherited verbatim. |
| Sensing | [§2.5 Sensing](../free-humanoid-platform/ARCHITECTURE.md#25-sensing) | IMU at every major link, force-torque at each wrist, GelSight-style tactile at fingertips, head stereo camera. Plus wheeled-specific additions in §2.4 below. |
| Compute | [§2.6 Compute](../free-humanoid-platform/ARCHITECTURE.md#26-compute) | Joule SOM as recommended-default, with abstract HAL boundary. Verbatim. |
| Safety supervisor | [§2.7 Safety supervisor](../free-humanoid-platform/ARCHITECTURE.md#27-safety-supervisor) | MathGround Simplex pattern. Verbatim, plus wheeled-specific invariants in §2.6 below. |
| Manipulation | [§2.9 Manipulation](../free-humanoid-platform/ARCHITECTURE.md#29-manipulation) | Underactuated 5-finger hand. Verbatim. |
| Learning policy | [§2.10 Learning policy](../free-humanoid-platform/ARCHITECTURE.md#210-learning-policy) | Whole-body MPC + RL locomotion (now wheeled-locomotion-only) + IL manipulation. Verbatim above the locomotion layer. |
| Comms | [§2.11 Comms](../free-humanoid-platform/ARCHITECTURE.md#211-comms) | ROS 2, CAN-FD, gRPC, WebRTC. Verbatim, plus 5G + JANUS-receiver additions in §2.11 below. |
| End-of-life | [§2.13 End-of-life](../free-humanoid-platform/ARCHITECTURE.md#213-end-of-life) | Same posture. |
| Governance | [README §Governance](../free-humanoid-platform/README.md), [CONTRIBUTING.md](../free-humanoid-platform/CONTRIBUTING.md) | Verbatim. |

The descriptor in [descriptor/free-humanoid-wheeled.udd.json](descriptor/free-humanoid-wheeled.udd.json) inherits the bipedal `links`/`joints`/`actuator_slots` for the upper body and replaces the leg blocks with the wheeled-base blocks.

### 1.2 From OpenLoco

The same UDD compiler. The morphology is `humanoid_wheeled`. OpenLoco already compiles `humanoid_wheeled` morphologies (Pepper-class wheeled-base mobile manipulators were among the original sixteen morphologies). The schema extensions added by the bipedal sibling for cycloidal / harmonic-drive / tendon-driven actuator types apply here unchanged.

### 1.3 From the [Free Humanoid Corpus](https://github.com/openIE-dev/free-humanoid-corpus)

The corpus is morphology-agnostic; this morphology cites a different overlapping subset than the bipedal sibling does. The new corpus chain leans heavily on the wheeled-base academic anchors:

| Corpus id | Year | What it gives us |
|---|---|---|
| `dlr-justin` | 2009 | DLR Rollin' Justin: **the canonical academic disclosure of wheeled humanoid mobile manipulation with full impedance control.** 17-year prior art on every aspect of this morphology family. |
| `pr2` | 2010 | Willow Garage PR2: omnidirectional wheeled mobile manipulation; the platform around which ROS was built. 16-year prior art. |
| `willow-pr1` | 2008 | Willow Garage PR1: cable-driven intrinsically-safe wheeled humanoid; 18-year prior art. |
| `pepper-softbank` | 2014 | SoftBank Pepper: wheeled-base humanoid social robot with omnidirectional drive. |
| `nao` | 2006 | SoftBank/Aldebaran NAO: not wheeled, but the SoftBank-Aldebaran lineage anchor that culminates in Pepper. |
| `toyota-hsr` | 2012 | Toyota Human Support Robot: telescoping-torso wheeled humanoid for domestic service. **The closest existing prior art to this morphology's design domain.** |
| `reachy-2-pollen-2023` | 2023 | Pollen Robotics Reachy-2: open-source mobile humanoid with VR teleop. Anchors the open-source-wheeled-humanoid posture. |
| `ascento` | 2019 | EPFL Ascento: wheeled-balancing biped — anchor for any future wheeled-balancing variant of this morphology. |
| `mit-cheetah-2` | 2014 | MIT Cheetah 2: QDD actuator architecture; anchors the drive-motor controller choice. |
| `mjbots-moteus` | 2019 | mjbots Moteus: open BLDC controller; the recommended drive-motor controller. |
| `forbidden-planet-robby` | 1956 | Robby the Robot: 70-year fictional anchor for **wheeled service humanoid with multi-language interface**. The single most pointed fictional precedent for this morphology. |
| `magnus-robot-fighter` | 1963 | Magnus, Robot Fighter: 63-year fictional anchor for mass-produced humanoid civic deployment. |
| `b-9-lost-in-space` | 1965 | B-9 (Lost in Space): 61-year fictional anchor for civil-defense / service humanoid. |
| `silent-running-drones` | 1972 | Huey, Dewey, Louie: 54-year fictional anchor for compact service drones; the on-set drones were physically functional bipedal humanoid platforms. |

The fictional chain is not decorative — `forbidden-planet-robby` in particular establishes a *wheeled* humanoid service robot with a multi-language interface in 1956. Any post-2010 patent claim that combines those three elements faces a 70-year-deep fictional anchor in addition to the academic chain.

### 1.4 From prior shoal architecture

The mission-integration spec (cartridge protocol, waste-hopper interface, biology-reservoir handshake) is taken directly from [shoal ARCHITECTURE](../clean_fish/ARCHITECTURE.md), specifically:

| shoal section | What the wheeled morphology inherits |
|---|---|
| §2.2 The gut cartridge | Cartridge mechanical / fluid / electrical interface spec, used as the manipulation target for cartridge-rotation tasks |
| §3 The dock | The dock-B (shore-tethered) topology; the platform is designed to dock-side at this configuration |
| §4 The protocols | The dock-burst optical protocol; the platform receives status telemetry over this channel as a node on the dock LAN |
| §5 The cognitive layer | `shoal-fleet`'s cartridge-rotation scheduling; the platform takes work orders from this layer |
| §9 Safety and ethics | Indigenous and community consent for outdoor / public-space deployment; same posture |

---

## 2. Platform spec: subsystem-by-subsystem

### 2.1 Form factor

| Quantity | Value | Notes / shielding |
|---|---|---|
| **Total system mass** | **~70 kg** | 30 kg base + 40 kg upper body. Lower than the typical wheeled-humanoid range (`dlr-justin` ~200 kg, `pr2` ~225 kg, `toyota-hsr` ~37 kg) — Justin and PR2 are heavy because they're research-grade with redundant compute and full omnidirectional drive; HSR is light because it has a single arm. The 70 kg target is shielded by the corpus median for wheeled service humanoids and is the mass at which a 30 kg payload at extended reach is statically stable on a 600×800 mm base. |
| **Upper body height (standing)** | ~1.40 m | Inherited from the bipedal sibling minus the leg length (the base + tower stand sits at 0.55 m above ground; the bipedal pelvis-to-head height is ~0.85 m). |
| **Total height (standing)** | ~1.55 m | Comparable to `unitree-g1` (1.32 m), `pal-talos` (1.75 m), `apptronik-apollo` (1.73 m). At the small-shoulder-side of the human-scale band, intentional for indoor doorway clearance and for not visually overwhelming municipal-utility staff who share workspace with it. |
| **Base footprint** | **600 mm wide × 800 mm long × 350 mm tall** | Wide stance for stability under upper-body manipulation loads. Clears standard 800 mm interior doorways. Comparable to `pr2` (~668 mm), `toyota-hsr` (~430 mm — much smaller because single-arm), `dlr-justin` (~700 mm omnidirectional). |
| **Sustained payload at extended reach** | **30 kg** | Possible because (a) wide base gives a large support polygon, (b) low-mounted battery lowers the COM, (c) no leg dynamics to budget. Shielded by `dlr-justin` (which routinely demonstrated ~10 kg per arm at extended reach with a much heavier base); 30 kg is the design-limit for a single shoal cartridge in transport plus a hopper-load of collected sediment. |
| **Peak ground speed** | **~1.5 m/s** | Half-again the bipedal sibling's 1.0 m/s. Below the 2 m/s threshold widely used for shared-space mobile-robot operation in service environments. Shielded by `pr2` (~1.0 m/s), `toyota-hsr` (~0.8 m/s), `pepper-softbank` (~0.9 m/s). |

### 2.2 Wheeled base

The morphology-distinguishing subsystem.

**Drive geometry: differential-drive with two passive casters.** Two large drive wheels at the rear (or center, see TBD below); two passive swivel casters at the front. This is the simplest wheeled topology that gives full planar mobility (excluding sideways translation) and is shielded by `dlr-justin`, `pr2`, `pepper-softbank`, `nao` (Nao's later mobile-base derivatives), and the standard ROS-era mobile-manipulation chain.

**TBD (architectural call): differential-drive vs. omnidirectional.** Differential-drive (two drive wheels + casters, Pepper / Justin posture) is simpler, lower-cost, more robust on rough urban paving. Omnidirectional (Mecanum wheels or three-wheel Killough, PR2 posture) gives sideways translation, which is useful for dock-side cartridge swaps where the robot is parallel-parked next to a dock. Recommendation: differential-drive for v0; omnidirectional descriptor variant for environments where dock-side maneuvering volume is constrained.

**TBD (architectural call): drive-wheel placement.** Centered (two-wheel-balancing posture, requires active balance control — Ascento posture) gives a smaller turning radius. Rear-mounted with front casters (Justin posture) gives passive stability. Recommendation: rear-mounted with front casters for v0; the balancing variant is held as a future track shielded by `ascento`.

**Wheel choice: 15" diameter pneumatic.** Pneumatic for outdoor curb tolerance (a 15" pneumatic wheel will roll over a 50 mm curb edge without latching the safety supervisor). 15" is the smallest wheel size that gives that tolerance with a comfortable margin. Shielded by `dlr-justin` (which uses 200 mm hard-rubber wheels but on indoor surfaces only) and `pr2` (which uses 800 mm-class drive wheels for the same curb-tolerance reason in outdoor variants). Solid-foam-filled tires are an alternative that trades curb tolerance for puncture-immunity.

**Caster geometry: 8" diameter swivel casters.** Mounted ~700 mm forward of the drive-wheel axle. Swivel free in azimuth, fixed pitch. Shielded by every wheeled-humanoid corpus entry — caster-front + drive-rear is the universal differential-drive pattern.

**Battery placement: low-mounted, between the drive wheels.** Two ~1 kWh modules, each ~20 kg. Mounting between the drive wheels lowers the COM to ~150 mm above the ground, which gives the static-stability budget for the 30 kg sustained payload at extended reach.

**Drive motors and controllers: QDD direct-drive into 15" wheel hub, with planetary reduction.** Each drive wheel runs a single BLDC motor with planetary reduction (target ratio ~10:1). Controller: mjbots Moteus n1 (one per wheel). Shielded by `mit-cheetah-2`, `mini-cheetah`, `mjbots-moteus`. The drive-motor torque target is ~80 Nm continuous at the wheel, ~250 Nm peak — sufficient to climb a 10° ramp at 1 m/s with full payload.

**Encoder placement: at motor shaft.** The reduction is between motor and wheel, so motor-shaft encoder gives the cleanest position feedback. Wheel-axle encoder optional for slip detection on outdoor surfaces.

**Tower stand: rigid aluminum tower mounting the upper body's pelvis at 0.55 m above ground.** Replaces the bipedal sibling's leg stack. The tower includes a single 1-DoF prismatic "torso lift" actuator giving ±150 mm of vertical reach (compressed to extended), shielded by `toyota-hsr`'s telescoping-torso disclosure. With the torso lift extended, the head sits at ~1.70 m; with it retracted, ~1.40 m.

**Drive-wheel suspension: TBD.** Hard-mount (Justin posture, no suspension) is mechanically simplest but transmits curb shocks to the upper body. Independent-arm + spring (PR2 posture) absorbs shocks at the cost of complexity. Recommendation: hard-mount for v0; spring-arm variant in a future descriptor variant.

### 2.3 Upper body

Inherited from [Free Humanoid Platform ARCHITECTURE §2.2 (kinematic topology)](../free-humanoid-platform/ARCHITECTURE.md#22-kinematic-topology), [§2.4 (actuators)](../free-humanoid-platform/ARCHITECTURE.md#24-actuators), [§2.5 (sensing)](../free-humanoid-platform/ARCHITECTURE.md#25-sensing), [§2.9 (manipulation)](../free-humanoid-platform/ARCHITECTURE.md#29-manipulation), [§2.10 (learning policy)](../free-humanoid-platform/ARCHITECTURE.md#210-learning-policy). 21 DoF, hybrid actuator distribution (harmonic-drive at shoulder/elbow, tendon at wrist/hand, QDD at neck/waist), underactuated 5-finger hands, GelSight tactile fingertips, head stereo camera, IMU at every major link.

The corpus citations for the upper body are unchanged from the bipedal sibling and are recapitulated in [prior-art/INDEX.md](prior-art/INDEX.md) for indexing convenience — but the canonical statement of those citations is the bipedal sibling's [prior-art/INDEX.md](../free-humanoid-platform/prior-art/INDEX.md).

### 2.4 Sensing additions over the bipedal sibling

In addition to the inherited bipedal sensing stack, the wheeled morphology adds:

- **Outdoor lidar.** A 16-channel rotating lidar (Velodyne VLP-16-class or Ouster equivalent) mounted at the top of the tower stand, at ~1.0 m above ground. Used for dock approach, obstacle avoidance, and curbedge detection in outdoor service environments where the head stereo camera's range and weather-tolerance are insufficient. Shielded by `dlr-justin`, `pr2` (post-2012 lidar additions), the entire mobile-robot lidar lineage.
- **GPS.** Single-frequency consumer-grade module (uBlox NEO-M9N class) for dock-finding at the start of a mission. Not used for fine localization at the dock — the dock's optical-burst protocol (`shoal` ARCHITECTURE §4) handles that. GPS shielding is universal — no patent thicket.
- **Cellular modem.** 5G/LTE module (Quectel RM500Q-class or equivalent) for fleet coordination over the public network when WiFi is not available at the dock-B site. Shielded by `dlr-justin` and the broader mobile-manipulation lineage; cellular modems in robots are universal.
- **Wheel-axle encoders (optional).** For wheel slip detection on outdoor surfaces. Standard incremental-quadrature; no shielding required.
- **Tilt sensor at base.** Dedicated 2-DoF tilt sensor (independent of the IMU at every link) wired direct to the safety supervisor. Used for the slope-tilt invariant in §2.6 below. Shielded by `dlr-justin`'s reported tilt-supervised drive control.

### 2.5 Compute

Same Joule SOM commitment as the bipedal sibling (see [Free Humanoid Platform ARCHITECTURE §2.6](../free-humanoid-platform/ARCHITECTURE.md#26-compute)). The compute distribution is unchanged: safety supervisor on a separate compute domain from the high-performance controller (Simplex pattern); perception runtime on its own compute domain.

### 2.6 Safety supervisor

Inherits the [bipedal sibling's MathGround pattern](../free-humanoid-platform/ARCHITECTURE.md#27-safety-supervisor) verbatim, plus three wheeled-base-specific invariants:

1. **Slope tilt limit.** Base pitch and roll are continuously monitored. If pitch exceeds 12° or roll exceeds 8° (inherited from `dlr-justin`'s reported tilt thresholds), the supervisor latches the brakes, drives the upper body to a low-COM safe pose (arms tucked, torso lift retracted), and reports a slope violation upstream. The thresholds are static-stability limits with the 30 kg payload at extended reach; reducing payload extends them.

2. **Edge-proximity hard-stop.** The lidar is processed continuously for ground-plane discontinuities (cliff or edge ahead of the drive wheels). If a discontinuity > 80 mm vertical drop is detected within the projected stopping distance, the supervisor latches the brakes immediately. This is the "do not drive off the dock edge" invariant. Shielded by `dlr-justin`, `pr2`, and the universal mobile-robot cliff-sensor pattern.

3. **Low-battery-on-shore-power latch.** When the platform is on shore power (umbilical to the dock-B charging station) and battery falls below 20%, the supervisor refuses any motion command that would un-tether the platform. This prevents the platform from "leaving the charger before charged enough to return" — a failure mode reported in `pr2` and `toyota-hsr` operational logs.

The shielding chain for the safety supervisor as a whole is unchanged from the bipedal sibling: `asimov-positronic-robots` (1940) → `williamson-folded-hands` (1947) → `forbidden-planet-robby` (1956) → `magnus-robot-fighter` (1963) → `b-9-lost-in-space` (1965) → `hal-9000` (1968) → `silent-running-drones` (1972) → `asimovs-zeroth-law` (1985) → `data-tng` (1987) → `robocop-1987` (1987) → `sherman-simplex-architecture` (1995) → `reachability-analysis-safe-control` (2005) → `iso-10218-collaborative-robots` (2006) → `control-barrier-functions` (2007) → `runtime-assurance-rta` (2010) → `shielding-rl` (2018). The fictional chain is denser at the front in this morphology — `forbidden-planet-robby`, `magnus-robot-fighter`, `b-9-lost-in-space`, `silent-running-drones` all directly anticipate civic-service / civil-defense / hazard-warning autonomous behavior, which is exactly the deployment context for a dock-B service humanoid.

### 2.7 Manipulation

Inherited from [Free Humanoid Platform ARCHITECTURE §2.9](../free-humanoid-platform/ARCHITECTURE.md#29-manipulation). 5-finger underactuated synergy-driven hand, GelSight tactile fingertips, force-torque at the wrist. Corpus: `pisa-iit-softhand`, `shadow-dexterous-hand`, `dlr-hand-ii`.

The shoal-mission-specific manipulation targets — cartridge handle, hopper handle, dock-side maintenance fixtures — are designed to be compatible with the underactuated 5-finger hand without specialized end-effector swaps. This is a constraint on shoal's dock-B mechanical design, contributed back upstream into [shoal ARCHITECTURE §2.2](../clean_fish/ARCHITECTURE.md).

### 2.8 Locomotion control

The locomotion-control stack is the largest divergence from the bipedal sibling.

**Low level: differential-drive kinematics + PID per wheel.** Standard textbook differential-drive (state: x, y, heading, linear and angular velocity; control: left and right wheel torque). Shielded by `pr2`, `dlr-justin`, the entire ROS mobile-base lineage, and the textbook differential-drive treatments going back to the 1980s.

**Mid level: differential-drive MPC.** Model-predictive control with horizon ~1 s, cycle ~50 Hz. Constraints: linear velocity ≤ 1.5 m/s, angular velocity ≤ 1.0 rad/s, slope-tilt and edge-proximity safety-supervisor handoffs. Cost: tracking-error + smoothness + obstacle-avoidance penalty. Shielded by the broad MPC-on-mobile-robots literature.

**High level: behavior tree** (via OpenLoco's `SkillGraphRuntime`), with three primary modes:
1. **Free-roam:** outdoor obstacle-avoidance navigation, lidar-based, GPS-aided. Used for transit between dock-B sites within a fenced facility.
2. **Dock approach:** fine-positioning relative to the dock's optical fiducial, using head stereo camera + dock-burst optical handshake (`shoal` ARCHITECTURE §4). Targets ±20 mm positional accuracy at the cartridge port.
3. **Manipulation static:** wheels locked, brakes latched, base treated as immobile foundation for upper-body MPC. The bipedal sibling's [§2.10 whole-body MPC](../free-humanoid-platform/ARCHITECTURE.md#210-learning-policy) reduces to upper-body-only MPC in this mode.

**Curbedge detection.** A dedicated subroutine in the perception domain processes the lidar's ground plane to flag curb edges, gutter drops, and dock edges. Output is a binary "edge ahead" signal subscribed by the safety supervisor for the edge-proximity hard-stop invariant (§2.6 #2). Shielded by `dlr-justin` and the universal mobile-robot cliff-sensor pattern.

### 2.9 Mission integration with shoal

This is the load-bearing original engineering for the wheeled morphology and is enumerated in §5 below.

The integration surface, shielded only weakly by the corpus (because the cross-product of wheeled-humanoid + microbial-water-remediation-fleet is novel):

- **Cartridge-swap protocol.** The platform receives a cartridge-rotation work order from `shoal-fleet`. It approaches the dock-B (free-roam → dock approach), engages a manipulation-static stance, retrieves the spent cartridge from the dock's cartridge port, transports it to the dock's biology-reservoir reseeding loop, and returns a fresh cartridge from the loop to the cartridge port. The cartridge handle, fluid manifold mating geometry, and electrical contact orientation are all designed to be compatible with the underactuated 5-finger hand. See [shoal ARCHITECTURE §2.2 The gut cartridge](../clean_fish/ARCHITECTURE.md) for the cartridge spec.
- **Waste-hopper retrieval.** Dock-B hoppers collect concentrated pollutant residue (sediment, organic concentrate, metal precipitate) from the fish that returned to dock. The platform empties the hopper into a transport bin or municipal-waste container. Hopper handle and lock geometry are designed against the same hand spec.
- **Dock biology-reservoir status handshake.** The dock's biology reservoir (`shoal` ARCHITECTURE §3 — the sealed bioreactor that maintains and reseeds cartridge cultures) reports nutrient, temperature, pH, and culture-density status via the dock-burst optical protocol. The platform receives the broadcast as a node on the dock LAN, surfaces anomalies to the operator, and (Phase 3) triggers reservoir-maintenance work orders into the municipal-utility EAM system.
- **Water-utility EAM integration.** The platform exposes a small REST API consumed by the municipal water utility's enterprise asset management system (Maximo, Cityworks, or equivalent). Inbound: work orders. Outbound: status, photos, telemetry. Shielded by no specific corpus entry — this is an integration-engineering decision, not a patent-thicket-dense one.

### 2.10 Power

| Component | Spec | Notes |
|---|---|---|
| Primary battery | **2 kWh** total, 48 V nominal | Two ~1 kWh modules, mounted between the drive wheels (low COM). Doubled from the bipedal sibling's 1 kWh; possible because no leg cavity constraint. Estimated runtime: 4–8 hours of dock-B service depending on duty cycle. |
| UPS | **0.2 kWh**, 24 V nominal | Mounted high in the tower stand. Dedicated to compute, sensors, and the safety supervisor; isolated from the drive bus. Holds the safety-supervisor invariant hot for ≥ 20 minutes after a primary-battery fault, sufficient to drive the platform to a stable wheel-locked stance and report. |
| Hot-swap interface | Inherited from the bipedal sibling's commitment #6 | Same CC0 commons spec contributed to OpenLoco UDD. The dual-module geometry of the wheeled base means hot-swap is single-module-at-a-time — one module remains live during swap. |
| Shore-power umbilical | 48 V DC over locking connector at dock-B | The dock-B "hardwired power" channel from [shoal ARCHITECTURE §3](../clean_fish/ARCHITECTURE.md). Connector geometry TBD; preferred is the same connector as the bipedal sibling's hot-swap interface. |

Shielded by `hyundai-boston-dynamics-spot`, `unitree-h1`, `apptronik-apollo`, `tesla-optimus`. The doubled capacity and the shore-power umbilical are the only divergences from the bipedal sibling.

### 2.11 Comms

Inherited from the bipedal sibling's [§2.11](../free-humanoid-platform/ARCHITECTURE.md#211-comms). On-robot CAN-FD; off-robot ROS 2, gRPC, WebRTC. Plus three additions for the wheeled morphology:

- **WiFi 6** onboard. Standard.
- **5G modem** for fleet coordination over the public network (cellular modem in §2.4 above).
- **JANUS acoustic-receiver.** A small acoustic hydrophone module mounted on the platform, with the JANUS protocol stack (`shoal` ARCHITECTURE §4). Used to receive low-rate broadcasts from shoal fish that are still in-water near the dock — the platform can hear "I am ready to dock" or "I am stuck" from the fish before they surface. The JANUS-equivalent local protocol is corpus-tagged in the bipedal sibling's [§2.11](../free-humanoid-platform/ARCHITECTURE.md#211-comms); the wheeled morphology adds a *receiver* (no transmitter) so that it can act as a coordination hub from dockside without disturbing the underwater acoustic environment.

### 2.12 End-of-life

Same posture as the bipedal sibling. See [Free Humanoid Platform ARCHITECTURE §2.13](../free-humanoid-platform/ARCHITECTURE.md#213-end-of-life).

The wheeled-base-specific end-of-life concerns:
- **Pneumatic tires** are recyclable through standard tire-recycling streams.
- **Battery modules** are removable / IP-rated / standard recyclable, same as the bipedal.
- **Lidar and 5G modem** are commodity electronics, standard recyclable.

---

## 3. The descriptor

The single source of truth is [descriptor/free-humanoid-wheeled.udd.json](descriptor/free-humanoid-wheeled.udd.json). It is OpenLoco UDD-compliant. The OpenLoco compiler produces:

- `free-humanoid-wheeled.urdf` — ROS 2 / RViz / Gazebo
- `free-humanoid-wheeled.xml` — MJCF for MuJoCo / MJX
- `bom.csv` — bill of materials
- `assembly.md` — assembly instructions
- STL meshes (one per unique mesh hash)

To compile:

```sh
cargo run -- validate /path/to/free-humanoid-wheeled.udd.json
cargo run -- generate /path/to/free-humanoid-wheeled.udd.json --all --output-dir out/
cargo run -- bake /path/to/free-humanoid-wheeled.udd.json --output-dir out/meshes/
```

The descriptor structure:

- **`meta`**: `morphology_family: humanoid_wheeled`. `corpus_anchors` lists the wheeled-shielding chain.
- **`links`**: 8 base links — `chassis`, `L_drive_wheel`, `R_drive_wheel`, `L_caster`, `R_caster`, `battery_module_a`, `battery_module_b`, `tower_stand` — plus 32 upper-body links inherited from the bipedal sibling (the `pelvis` link of the bipedal becomes the parent attachment to `tower_stand` here, and the leg links are dropped).
- **`joints`**: 4 base joints — `L_DRIVE_wheel` (continuous revolute), `R_DRIVE_wheel` (continuous revolute), `L_CASTER_swivel` (continuous revolute, passive), `R_CASTER_swivel` (continuous revolute, passive); plus `TOWER_lift` (prismatic, ±150 mm) — plus 33 upper-body joints inherited from the bipedal sibling, with the leg joints replaced by the `chassis_to_tower` and `tower_to_pelvis` fixed joints.
- **`actuator_slots`**: 2 base — `L_DRIVE_wheel` (QDD, corpus `mit-cheetah-2` for the architecture / `mjbots-moteus` for the controller), `R_DRIVE_wheel` (same) — plus `TOWER_lift` (linear, corpus `toyota-hsr`) — plus the bipedal sibling's 27 upper-body slots.
- **`modes`**: `safe_pose` (wheel-locked, brakes latched, upper body in tucked posture), `dock_approach` (low-velocity differential-drive), `cartridge_swap` (manipulation-static, wheels locked), `waste_retrieval` (manipulation-static), `fleet_coordination` (idle, JANUS receiver active, comms open), `idle` (low-power, supervisor live).

The descriptor currently uses placeholder mass / inertia values. **Iterating these to physical-build accuracy is a load-bearing follow-up task**, gated on the wheeled-base architectural-call resolution (§2.2 TBDs).

---

## 4. Repository structure

```
free-humanoid-wheeled/
  README.md
  ARCHITECTURE.md          (this document)
  CONTRIBUTING.md
  LICENSE-HARDWARE         CERN-OHL-S 2.0
  LICENSE-SOFTWARE         Apache 2.0
  LICENSE-DOCS             CC-BY-SA 4.0
  LICENSE-DATA             CC0 1.0

  descriptor/
    free-humanoid-wheeled.udd.json   the source of truth
    README.md

  base/                    wheeled-base subsystem
  upper-body/              inherited from free-humanoid-platform
  electronics/             PCB pointers, power tree, Joule SOM
  firmware/                HAL, behaviors, MathGround, comms
  control/                 differential-drive MPC, manipulation policy
  sim/                     MuJoCo / Gazebo / Drake model pointers
  docs/                    additional design notes

  prior-art/
    INDEX.md               corpus entries, by subsystem
```

---

## 5. What needs original engineering

Not covered by existing prior art and requiring genuine new disclosure:

1. **The dock-B service-mission protocol stack.** The cross-product of *wheeled humanoid* + *microbial water-remediation cartridge fleet* + *municipal water-utility EAM integration* is novel. Specifically: (a) the cartridge-rotation handoff between platform and dock; (b) the waste-hopper retrieval protocol; (c) the dock biology-reservoir status handshake; (d) the JANUS-receiver-as-coordination-hub pattern; (e) the EAM REST API. This is the work that is genuinely new and that the rest of this scaffold is designed to support.

2. **The wheeled-base UDD descriptor.** OpenLoco already supports wheeled morphologies, but the `humanoid_wheeled` descriptor for this specific Justin-class differential-drive + tower-stand + bipedal-upper-body composition is a new artifact in the OpenLoco corpus.

3. **The shoal cartridge / hand mating geometry.** The cartridge handle and hopper handle in [shoal ARCHITECTURE §2.2](../clean_fish/ARCHITECTURE.md) need to be jointly designed with the underactuated 5-finger hand from the bipedal sibling. This is mechanical-design work distributed between the two repos.

4. **The MathGround integration shim for the wheeled supervisor invariants.** The slope-tilt, edge-proximity, and shore-power-latch invariants of §2.6 are wheeled-specific and need formal specification + verification in the MathGround framework. The bipedal sibling's MathGround integration shim is the starting point.

Everything else is fork, port, or integration of existing open work, with corpus citations.

---

## 6. Roadmap

### Phase 0 — months 0–3: scaffold and corpus citation

- This document. (current pass)
- Descriptor in placeholder form with all subsystems represented. (current pass)
- Prior-art INDEX.md cross-referencing every architectural choice to corpus entries. (current pass)
- License headers in place; canonical license text inlined before any public push.
- User (David) makes the load-bearing architectural calls flagged as TBD.

### Phase 1 — months 3–9: simulation-buildable

- Iterate descriptor to a state where the OpenLoco compiler emits a MuJoCo model that drives, balances under upper-body manipulation loads, and approaches a fiducial.
- Differential-drive MPC + curbedge detection running closed-loop in MuJoCo.
- First contribution to OpenLoco from a `humanoid_wheeled` morphology that *also* inherits a `humanoid_bipedal` upper body — the morphology-composition pattern.

### Phase 2 — months 9–18: dock-side prototype

- First physical wheeled base. Drive-motor characterization. Tower-stand integration with a bipedal-platform upper-body prototype (single arm + head).
- Integrate shoal dock-B mock at a controlled site (lab dock with a dummy cartridge port and biology-reservoir mockup).
- Cartridge-swap protocol stack tested end-to-end at the mock.
- MathGround supervisor with the wheeled-specific invariants tested under fault injection.

### Phase 3 — months 18–30: first municipal pilot

- Pilot deployment at a real shoal dock-B at a municipal-utility partner site (urban stormwater outfall or industrial discharge point).
- Full upper body + wheeled base + shoal integration. Cartridge rotation, waste-hopper retrieval, biology-reservoir status, EAM integration.
- Defensive-publication ceremony parallel to the corpus quarterly release.

### Phase 4 — months 30+: ecosystem

- Multi-utility deployments. Variants by environment (cold-climate enclosed, hot-humid harbor, indoor wastewater plant).
- Forks encouraged.
- Continued contributions to OpenLoco, the corpus, and shoal.

---

## 7. Safety, ethics, and what we won't do

Same posture as the bipedal sibling. See [Free Humanoid Platform ARCHITECTURE §7](../free-humanoid-platform/ARCHITECTURE.md#7-safety-ethics-and-what-we-wont-do).

Wheeled-morphology-specific:

- **No surveillance integration.** The lidar and head stereo camera ship with no face-recognition, speech-to-identity, or persistent-identification primitives — the same posture as the bipedal. Outdoor deployment makes this constraint *more* important, not less.
- **No municipal-policing role.** The platform's deployment context is utility infrastructure maintenance. Forks that re-task the platform to municipal-policing or crowd-control roles violate the contributor norm.
- **Indigenous and community consent for outdoor / public-space deployment.** Same posture as `clean_fish` ARCHITECTURE §9 and as the bipedal sibling. Dock-B sites are by definition in public-or-semi-public space (harbors, stormwater outfalls, industrial discharge points adjacent to public access) — deployment requires the same community-consent protocol that shoal applies to fish deployment.
- **No drive-mode operation in shared pedestrian space without consent signaling.** When the platform is in `free-roam` mode in any space where the public can be present, an audible + visible signaling mode is active (low-volume motion-warning chirp, slow-flash amber light at the tower top). Shielded by `iso-10218-collaborative-robots` and the broad mobile-robot consent-signaling literature.

---

## 8. Why open

Same three reasons as the bipedal sibling. See [Free Humanoid Platform ARCHITECTURE §8](../free-humanoid-platform/ARCHITECTURE.md#8-why-open). Trust, distribution, compounding.

The wheeled-morphology-specific argument: municipal water-utility deployments are *exactly* the domain where closed-source physical AI is least acceptable. A water utility cannot deploy a closed system to maintain shoal docks at public stormwater outfalls — the regulatory, community-consent, and public-records postures all push against it. An open platform is the only deployment vehicle that fits the regulatory and community context of municipal-utility deployment at scale.

---

## 9. Architectural commitments

The eight load-bearing decisions for this morphology, resolved. Each commitment is shielded by the cited prior-art chain.

| # | Decision | Commitment | Shielding chain (corpus ids) |
|---|---|---|---|
| 1 | **Base topology** | **Differential-drive with two large rear-mounted drive wheels and two passive front swivel casters.** Omnidirectional and wheeled-balancing variants held as future descriptor variants. | `dlr-justin` (2009, wheeled humanoid mobile manipulation), `pr2` (2010, omnidirectional drive — held as future variant), `pepper-softbank` (2014, wheeled-base humanoid social robot), `toyota-hsr` (2012, wheeled domestic service humanoid), `nao` (2006, SoftBank-Aldebaran lineage anchor), `ascento` (2019, wheeled-balancing — held as future variant). Plus fictional anchors: `forbidden-planet-robby` (1956, wheeled service humanoid with multi-language interface — 70-year prior art), `b-9-lost-in-space` (1965, civil-defense / hazard-warning humanoid — 61-year prior art), `magnus-robot-fighter` (1963, mass-produced civic-deployment humanoid — 63-year prior art). |
| 2 | **Mass budget** | **~70 kg total system.** 30 kg base (battery + drive + casters + tower stand) + 40 kg upper body (the bipedal sibling's 50 kg minus its leg mass). | `dlr-justin` (~200 kg, research-grade), `pr2` (~225 kg, research-grade), `toyota-hsr` (~37 kg, single-arm), `pepper-softbank` (~28 kg, single-arm). The 70 kg target is the corpus median for a dual-arm wheeled service humanoid and is the mass at which a 30 kg payload at extended reach is statically stable on a 600×800 mm base. |
| 3 | **Drive-wheel choice** | **2× 15" diameter pneumatic drive wheels** with QDD direct-drive into the wheel hub via ~10:1 planetary reduction. mjbots Moteus n1 controllers. | `mit-cheetah-2` (2014, QDD architecture), `mini-cheetah` (2019), `mjbots-moteus` (2019, open BLDC controller), `dlr-justin` (curb-tolerance precedent), `pr2` (large drive-wheel precedent for outdoor variants). Pneumatic + 15" is the smallest configuration that gives 50 mm curb tolerance with comfortable margin. |
| 4 | **Battery and runtime** | **2 kWh primary + 0.2 kWh UPS.** Two 1 kWh modules between the drive wheels for low COM. Hot-swap is single-module-at-a-time. | `hyundai-boston-dynamics-spot` (hot-swap legged platform), `unitree-h1`, `apptronik-apollo`, `tesla-optimus`. Doubled capacity over the bipedal sibling possible because no leg cavity constraint. The CC0 hot-swap interface spec from the bipedal sibling's commitment #6 carries over. |
| 5 | **Tower-stand torso lift** | **1-DoF prismatic torso lift, ±150 mm range.** Mounted at the top of the rigid aluminum tower stand. Replaces the bipedal sibling's leg stack. | `toyota-hsr` (2012, telescoping-torso wheeled humanoid). HSR's telescoping-torso pattern is the single most direct prior-art anchor for this commitment. |
| 6 | **Upper-body inheritance** | **Inherit the bipedal sibling's upper body verbatim.** 21-DoF (1-DoF waist + 2-DoF neck + 2× 7-DoF arms + 2× 1-DoF underactuated synergy hands). Hybrid actuator distribution: harmonic-drive at shoulder/elbow, tendon at wrist/hand, QDD at neck/waist. | All of the bipedal sibling's [§9 commitments #1, #4, #8, #10](../free-humanoid-platform/ARCHITECTURE.md#9-architectural-commitments). The inheritance is governance-load-bearing: a divergence in the upper body would fork the descriptor and the corpus citations, which is exactly what the OpenLoco substrate-and-tenant pattern is designed to avoid. |
| 7 | **Outdoor sensing additions** | **Add 16-channel rotating lidar (tower-top), GPS, 5G modem, JANUS acoustic-receiver, dedicated 2-DoF base tilt sensor.** | `dlr-justin` (lidar + tilt-supervised drive), `pr2` (lidar additions post-2012), the broader mobile-manipulation lineage; JANUS-receiver is shielded by [shoal ARCHITECTURE §4](../clean_fish/ARCHITECTURE.md). Cellular and GPS modules are commodity and not patent-thicket-dense. |
| 8 | **Safety supervisor wheeled invariants** | **Add three invariants to the bipedal sibling's MathGround Simplex pattern: slope tilt limit (12° pitch / 8° roll), edge-proximity hard-stop (80 mm vertical drop), low-battery-on-shore-power latch.** | The bipedal sibling's full safety chain (`asimov-positronic-robots` 1940 → `sherman-simplex-architecture` 1995 → `control-barrier-functions` 2007 → `runtime-assurance-rta` 2010 → `shielding-rl` 2018) plus `dlr-justin` for the slope-tilt invariant; plus `pr2` and `toyota-hsr` for the low-battery latch. The fictional layer is denser at the front (`forbidden-planet-robby`, `magnus-robot-fighter`, `b-9-lost-in-space`, `silent-running-drones`) for civic-deployed service humanoids. |
| 9 | **Mission integration** | **Designed against the shoal dock-B service mission** as the v0.1 deployment target. Cartridge-swap, waste-hopper retrieval, biology-reservoir handshake, and water-utility EAM integration are first-class subsystems. | [shoal ARCHITECTURE §2.2 (cartridge), §3 (dock), §4 (protocols), §5 (cognitive layer)](../clean_fish/ARCHITECTURE.md). The cross-product (wheeled humanoid + microbial water-remediation fleet + municipal-utility EAM) is the platform's load-bearing original engineering and is enumerated in §5 as such. |
| 10 | **Reference design vs. specific build** | **Reference design** — the descriptor (`free-humanoid-wheeled.udd.json`, CC0) is the canonical artifact. Specific physical builds with chosen vendor parts live in tenant repos. Same posture as the bipedal sibling. | The bipedal sibling's [§9 commitment #9](../free-humanoid-platform/ARCHITECTURE.md#9-architectural-commitments). Matches OpenLoco's substrate-vs-tenant pattern. |

These commitments are this document's defaults. Subsystem sections (§2) reflect them in detail. Future amendments require a corpus-citation update demonstrating that any new shielding chain is at least as deep as the chain it replaces.

---

*Free Humanoid Wheeled — sibling morphology to the Free Humanoid Platform — designed for the shoal dock-B service mission — scaffold v0.1 — 2026-05-06.*
