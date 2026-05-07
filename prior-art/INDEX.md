# Prior Art Index — Free Humanoid Wheeled

This document maps every load-bearing design choice in the wheeled morphology to specific entries in the [Free Humanoid Corpus](https://github.com/openIE-dev/free-humanoid-corpus). Each entry is cited by its corpus `id`. Anyone reviewing this morphology — including USPTO examiners and invalidity-contention attorneys — can use this index to locate the specific defensive publications that anticipate any patent claim against the platform's architecture.

The corpus has 269 entries (as of 2026-05-06) spanning 1818 (Frankenstein) to 2025. This index cites the entries that are most directly load-bearing for the wheeled morphology.

The bipedal sibling [Free Humanoid Platform](../../free-humanoid-platform/prior-art/INDEX.md) is the canonical statement for the upper-body subsystems (kinematic topology of the arms, hands, head, torso; actuator distribution at shoulder, elbow, wrist, hand; sensing at every link; the safety-supervisor architecture; the compute architecture; the learning-policy stack). This document recapitulates those subsystems in summary form and **adds** the wheeled-base-specific subsystems (1, 2, 6, 9 below).

---

## Subsystem 1 — Wheeled-base topology

| Corpus id | Year | Shielding |
|---|---|---|
| `dlr-justin` | 2009 | **Foundational anchor.** DLR Rollin' Justin: the canonical academic disclosure of wheeled humanoid mobile manipulation with full impedance control. 17-year prior art on wheeled humanoid platforms for service tasks. |
| `pr2` | 2010 | Willow Garage PR2: omnidirectional wheeled mobile manipulation; the platform around which ROS was built. 16-year prior art on wheeled humanoid hardware *and* on robotics middleware. |
| `willow-pr1` | 2008 | Willow Garage PR1: cable-driven intrinsically-safe wheeled humanoid; anticipates several modern compliant-actuator humanoid claims. |
| `pepper-softbank` | 2014 | SoftBank Pepper: foundational prior art for wheeled-base humanoid social robots. The omnidirectional wheeled base design has been widely cited. |
| `nao` | 2006 | SoftBank/Aldebaran NAO: not wheeled, but the SoftBank-Aldebaran lineage anchor that culminates in Pepper. NAO's mechatronic-design publication is widely cited. |
| `toyota-hsr` | 2012 | Toyota Human Support Robot: telescoping-torso wheeled humanoid for domestic service. **The closest existing prior art to this morphology's design domain.** Anchors the tower-stand-with-torso-lift commitment. |
| `reachy-2-pollen-2023` | 2023 | Pollen Robotics Reachy-2: open-source mobile humanoid with VR teleop. Anchors the open-source wheeled-humanoid posture. |
| `ascento` | 2019 | EPFL Ascento: wheeled-balancing biped. Held as future-track for any wheeled-balancing variant of this morphology. |

### Fictional anchors for civic / service / civil-defense humanoid deployment

| Corpus id | Year | Shielding |
|---|---|---|
| `forbidden-planet-robby` | 1956 | Robby the Robot: 70-year fictional anchor for **wheeled service humanoid with multi-language voice-command interface**. The single most pointed fictional precedent for this morphology family — Robby is wheel-based, civic-deployed, and accepts speech commands in multiple languages. |
| `magnus-robot-fighter` | 1963 | Magnus, Robot Fighter: 63-year fictional anchor for mass-produced humanoid civic deployment, including job-function-specialized variants (police variant, industrial variant, transit variant). |
| `b-9-lost-in-space` | 1965 | B-9 (Lost in Space): 61-year fictional anchor for civil-defense / hazard-warning / family-companion humanoid platforms. The "Danger, Will Robinson!" pattern is a hazard-warning behavior anchor. |
| `silent-running-drones` | 1972 | Huey, Dewey, Louie (Silent Running): 54-year fictional anchor. The on-set drones were physically functional compact bipedal humanoid platforms; production team published behind-the-scenes mechanism documentation. Anchors compact-service-humanoid disclosure. |

**Implication:** wheeled humanoid platforms for service deployment in civic / utility environments are **fully exhausted as patentable subspace**. Any patent claim issued post-2010 on a "wheeled humanoid for service tasks" is anticipated by at least two academic entries (`dlr-justin`, `pr2`, `toyota-hsr`) plus a 70-year-deep fictional chain.

---

## Subsystem 2 — Drive train (wheeled-base specific)

### 2.1 Drive motor architecture (QDD direct-drive)

| Corpus id | Year | Shielding |
|---|---|---|
| `mit-cheetah-2` | 2014 | Foundational QDD architecture. Wensing 2017 T-RO paper "Proprioceptive actuator design in the MIT Cheetah" is the foundational actuator-design disclosure. |
| `mini-cheetah` | 2019 | Open MIT QDD reference. The most-cited modern QDD anchor. |
| `mit-cheetah` | 2009 | Original Cheetah; foundational QDD lineage. |

### 2.2 Drive motor controller

| Corpus id | Year | Shielding |
|---|---|---|
| `mjbots-moteus` | 2019 | Foundational prior art for compact open BLDC controllers in legged robotics. CAN-FD interface. |
| `odrive` | 2017 | Open BLDC controller (USB / CAN). |
| `simplefoc` | 2020 | Open BLDC controller (Arduino-tier). |

### 2.3 Wheel choice (15" pneumatic)

Pneumatic-wheel + outdoor-curb-tolerance is shielded by the universal mobile-robot literature; no specific corpus entry is load-bearing here. The 15" diameter target is shielded against `dlr-justin`'s smaller hard-rubber wheels (indoor-only) and against `pr2`'s 800-mm-class wheels (overengineered for the design domain).

### 2.4 Caster geometry

Caster-front + drive-rear is the universal differential-drive pattern, shielded by `dlr-justin`, `pr2`, `pepper-softbank`, and the broad mobile-robot lineage. No specific patent-thicket exposure.

---

## Subsystem 3 — Upper-body kinematic topology

Inherited from [Free Humanoid Platform prior-art INDEX, Subsystem 1](../../free-humanoid-platform/prior-art/INDEX.md). Summary:

| Corpus id | Year | Shielding |
|---|---|---|
| `wabot-1` | 1973 | Foundational anchor for full-scale anthropomorphic humanoid kinematic topology. |
| `honda-e0`–`honda-e6` | 1986–1993 | Honda E-series chain. Anchors arm + torso topology. |
| `honda-p1`, `honda-p2`, `honda-p3` | 1993–1997 | Honda P-series. Anchors balance and ZMP. |
| `asimo`, `hubo`, `hrp-2`–`hrp-5p` | 2000–2018 | Mass-produced humanoid lineage. |
| `nasa-valkyrie`, `robonaut-2`, `pal-talos` | 2011–2017 | Industrial / NASA full humanoids. |
| `darwin-op`, `poppy-humanoid`, `inmoov`, `reachy` | 2010–2020 | Open-source small/desk humanoid lineage. |
| `unitree-h1`, `unitree-g1`, `berkeley-humanoid`, `k-scale-os` | 2023–2024 | Recent academic / commodity humanoid disclosures. |

For the wheeled morphology, the leg-related subset of this chain is dropped (no `cassie-osu`, `digit-meta`, `atrias`); the arm/hand/torso/head subset is preserved.

---

## Subsystem 4 — Upper-body actuators

Inherited from [Free Humanoid Platform prior-art INDEX, Subsystem 2](../../free-humanoid-platform/prior-art/INDEX.md), with the leg-actuator chain dropped. Summary:

### 4.1 Harmonic-drive (shoulder, elbow)

| Corpus id | Year | Shielding |
|---|---|---|
| `honda-e0` | 1986 | First disclosed harmonic-drive use in a humanoid. 40-year prior art. |
| `dlr-toro` | 2014 | Torque-controlled humanoid; harmonic-drive disclosure. |
| `asimo`, `hubo`, `hrp-2`–`hrp-5p`, `robonaut-2`, `pal-talos`, `nasa-valkyrie` | 2000–2018 | Full chain. |

### 4.2 Tendon-driven (wrist, hand)

| Corpus id | Year | Shielding |
|---|---|---|
| `shadow-dexterous-hand` | 2002 | 24-year prior art on tendon-routed anthropomorphic hand. |
| `dlr-hand-ii` | 2001 | DLR's dexterous hand. |
| `dlr-justin` | 2009 | DLR's tendon-arm chain. (Same anchor as Subsystem 1.) |
| `pisa-iit-softhand` | 2012 | Underactuated soft hand (synergy-based). The recommended-hand anchor. |

### 4.3 QDD (neck, waist, drive wheels)

| Corpus id | Year | Shielding |
|---|---|---|
| `mit-cheetah-2`, `mini-cheetah` | 2014, 2019 | QDD architecture. |
| `mjbots-moteus` | 2019 | Open BLDC controller. |

### 4.4 Linear actuator (tower lift)

| Corpus id | Year | Shielding |
|---|---|---|
| `toyota-hsr` | 2012 | Telescoping-torso wheeled humanoid. The single direct anchor for the tower-lift commitment. |

---

## Subsystem 5 — Sensing

Inherited from [Free Humanoid Platform prior-art INDEX, Subsystem 3](../../free-humanoid-platform/prior-art/INDEX.md), plus the wheeled-specific additions:

### 5.1 Inherited (head stereo, IMU at every link, force-torque at wrists, GelSight tactile)

| Corpus id | Year | Shielding |
|---|---|---|
| `pomerleau-alvinn` | 1989 | Camera-to-control. |
| `howe-cutkosky-tactile-1989` | 1989 | Robotic tactile sensing. |
| `biotac-syntouch` | 2008 | Multimodal fingertip tactile. |
| `gelsight` | 2009 | High-resolution optical tactile. |

### 5.2 Wheeled-base additions (lidar, GPS, 5G, JANUS-receiver, base tilt)

| Corpus id | Year | Shielding |
|---|---|---|
| `dlr-justin` | 2009 | Lidar-equipped wheeled humanoid; tilt-supervised drive. |
| `pr2` | 2010 | Lidar additions to wheeled mobile manipulator. |
| `hyundai-boston-dynamics-spot` | 2015 | Outdoor lidar mobile platform. |

GPS, 5G modems, and acoustic-receivers in robots are commodity and not patent-thicket-dense; corpus shielding is light because the design choices are obvious-and-universal rather than novel-claimed. The JANUS-receiver pattern is shielded by [shoal ARCHITECTURE §4](../../clean_fish/ARCHITECTURE.md).

---

## Subsystem 6 — Safety supervisor (with wheeled-specific invariants)

The safety supervisor architecture is inherited verbatim from [Free Humanoid Platform prior-art INDEX, Subsystem 5](../../free-humanoid-platform/prior-art/INDEX.md) — the deepest chain in the corpus, 80 years of fictional anticipation plus 30 years of formal academic prior art. Summary chain:

| Corpus id | Year | Shielding |
|---|---|---|
| `frankenstein` | 1818 | Earliest fictional anchor. |
| `rur-rossums-robots` | 1920 | Three-Laws-anticipating constraint failure. |
| `asimov-positronic-robots` | 1940 | Three Laws. 86-year fictional anchor. |
| `williamson-folded-hands` | 1947 | Safety-constraint failure mode. |
| `forbidden-planet-robby` | 1956 | Three-Laws-compliant fictional disclosure (also Subsystem 1 anchor). |
| `magnus-robot-fighter` | 1963 | Mass-produced civic-deployed humanoid (also Subsystem 1 anchor). |
| `b-9-lost-in-space` | 1965 | Hazard-warning humanoid (also Subsystem 1 anchor). |
| `hal-9000` | 1968 | Supervisor failure modes. |
| `silent-running-drones` | 1972 | Maintenance drones with task-bound autonomy (also Subsystem 1 anchor). |
| `asimovs-zeroth-law` | 1985 | Extension of Three Laws. |
| `data-tng` | 1987 | Self-aware constraint adherence. |
| `robocop-1987` | 1987 | "Prime Directives" as Simplex-anticipating disclosure. |
| `sherman-simplex-architecture` | 1995 | Foundational academic. Verified safety controller as fallback. |
| `iso-10218-collaborative-robots` | 2006 | Industrial standards anchor. |
| `reachability-analysis-safe-control` | 2005 | Formal-method safe set computation. |
| `control-barrier-functions` | 2007 | Modern formal safety-controller standard. |
| `runtime-assurance-rta` | 2010 | AFRL Simplex-to-flight-control bridge. |
| `shielding-rl` | 2018 | Formal safety on learned policy. |

### Wheeled-specific invariants

The bipedal supervisor adds three invariants for the wheeled morphology — slope tilt, edge proximity, low-battery-on-shore-power. Their additional shielding:

| Corpus id | Year | Shielding |
|---|---|---|
| `dlr-justin` | 2009 | Slope-tilt-supervised drive control. Direct anchor for the slope-tilt invariant. |
| `pr2` | 2010 | Cliff-sensor / edge-proximity hard-stop pattern. |
| `toyota-hsr` | 2012 | Low-battery operational latch (per HSR operational logs cited in the corpus entry). |

**Implication:** the fictional chain is denser at the front for this morphology — `forbidden-planet-robby`, `magnus-robot-fighter`, `b-9-lost-in-space`, `silent-running-drones` all directly anticipate civic-deployed service-humanoid behavior, which is exactly the deployment context for a dock-B service humanoid.

---

## Subsystem 7 — Compute and learning policy

Inherited from [Free Humanoid Platform prior-art INDEX, Subsystem 4](../../free-humanoid-platform/prior-art/INDEX.md). Joule SOM as recommended-default within an abstract HAL. Imitation-learning-from-teleop manipulation policy. Locomotion policy is wheeled-specific (differential-drive MPC, no RL bipedal locomotion).

| Corpus id | Year | Shielding |
|---|---|---|
| `act-aloha`, `mobile-aloha`, `diffusion-policy` | 2023–2024 | Imitation learning. |
| `openai-rt-2`, `open-x-embodiment`, `physical-intelligence-pi-zero`, `skild-foundation-model`, `covariant-rfm` | 2023–2024 | VLA foundation models (optional upgrade path). |

For wheeled-base locomotion control, the standard differential-drive MPC literature plus `dlr-justin` and `pr2` shield the choice fully.

---

## Subsystem 8 — Comms

Inherited from [Free Humanoid Platform prior-art INDEX, Subsystem 6](../../free-humanoid-platform/prior-art/INDEX.md), plus the wheeled-specific 5G and JANUS-receiver additions cited in Subsystem 5 above. ROS 2, CAN-FD, gRPC, WebRTC. None patent-thicket-dense.

---

## Subsystem 9 — Mission integration with shoal

This subsystem is the load-bearing original engineering and its corpus-shielding chain is the lightest. The cross-product of *wheeled humanoid* + *microbial water-remediation cartridge fleet* + *municipal water-utility EAM integration* is not anticipated by any single corpus entry. The integration surface is shielded *element-by-element* but not as a composition:

| Element | Corpus shielding |
|---|---|
| Cartridge-swap protocol (manipulation surface) | `dlr-justin` (mobile manipulation), `shadow-dexterous-hand` (anthropomorphic hand), `pisa-iit-softhand` (underactuated grasp) |
| Waste-hopper retrieval (manipulation surface) | `dlr-justin` (dual-arm mobile manipulation) |
| Biology-reservoir handshake (comms protocol) | [shoal ARCHITECTURE §4](../../clean_fish/ARCHITECTURE.md) — the dock-burst optical protocol |
| EAM REST API (integration surface) | None required — REST-over-HTTP for asset-management integration is universal |
| JANUS-receiver coordination (acoustic comms) | [shoal ARCHITECTURE §4](../../clean_fish/ARCHITECTURE.md) plus the broader JANUS-protocol corpus tag |

The mission-integration protocol stack itself is original work and is the focus of v0.1 of this morphology. See [ARCHITECTURE §5](../ARCHITECTURE.md#5-what-needs-original-engineering).

---

## How to use this index

When you make a design choice in the platform — adding a feature, picking a vendor, modifying the descriptor, or writing a new firmware module — locate the relevant subsystem in this index and cite the most specific corpus entry that anticipates the choice.

If the corpus does not yet anticipate your choice:
1. Check the [corpus](https://github.com/openIE-dev/free-humanoid-corpus) directly (`corpus.jsonl`).
2. If still no anticipation, propose a new corpus entry (per the corpus's `CONTRIBUTING.md`).
3. If neither is possible, mark the design choice `draft` in the descriptor or architecture and document the invalidation gap.

Every commit that introduces a load-bearing design choice should reference at least one corpus entry id by name in the commit message. This is the operational form of the governance posture in [README.md](../README.md) and [CONTRIBUTING.md](../CONTRIBUTING.md).
