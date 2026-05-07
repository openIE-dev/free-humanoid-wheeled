# Free Humanoid Wheeled

**An open hardware, open firmware, open data wheeled-base humanoid reference design — sibling morphology to the bipedal [Free Humanoid Platform](../free-humanoid-platform/) and the deployment partner of the [shoal](../clean_fish/) water-remediation fleet. Targeted at the shoal dock-B service mission: urban stormwater outfalls, harbor docks, and industrial discharge points where shoal docks are deployed. Shielded by the [Free Humanoid Corpus](https://github.com/openIE-dev/free-humanoid-corpus).**

> Status: scaffold (v0.1). This repository establishes the project's positioning, governance, and architectural commitments. It is not yet a buildable robot. The architectural decisions are resolved in [ARCHITECTURE.md §9](ARCHITECTURE.md#9-architectural-commitments); each commitment is shielded by a corpus-cited prior-art chain. Detailed CAD, vendor-pinned BOM, and physical-build documentation are the next deliverables.

---

## What this is

Free Humanoid Wheeled is the wheeled sibling to the [Free Humanoid Platform](../free-humanoid-platform/). It inherits the platform's torso, arms, hands, head, sensing, compute, and safety supervisor architecture verbatim. It replaces the bipedal legs with a wheeled differential-drive base for paved and concrete urban service environments. The design domain is intentionally narrower than the bipedal sibling — no stair climbing, no rough-terrain ambition — and the resulting mass, payload, and runtime budgets are correspondingly more generous.

The project sits at the intersection of four projects:

1. **[OpenLoco](https://openie-dev.github.io/openloco/)** — the substrate. The platform compiles from a single UDD JSON file to URDF, MJCF, STL, BOM, and assembly through OpenLoco. Free Humanoid Wheeled is a `humanoid_wheeled` morphology family in the same compiler.

2. **[Free Humanoid Corpus](https://github.com/openIE-dev/free-humanoid-corpus)** — the prior-art commons. Every non-trivial design choice cites a specific corpus entry, the same governance posture as the bipedal platform.

3. **[Free Humanoid Platform](../free-humanoid-platform/)** — the bipedal sibling. The upper body of this morphology *is* the upper body of the platform; this repo cross-links rather than duplicates that work.

4. **[shoal](../clean_fish/)** — the deployment partner. Free Humanoid Wheeled is designed to integrate with the shoal cartridge-swap protocol and waste-handling logistics at shoal dock-B sites. See [shoal ARCHITECTURE §3](../clean_fish/ARCHITECTURE.md) for the dock spec, especially `shoal-dock-B` (shore-tethered).

These are tenants of one another. The wheeled morphology is one of many possible service-environment tenants of the bipedal upper body and one of many possible service tenants of the shoal fleet.

---

## Design domain

**Paved and concrete urban service environments.** Stormwater outfalls. Harbor docks. Industrial discharge sites. Wastewater treatment plant grounds. Municipal water-utility yards.

The platform deliberately does not target stair climbing, off-road traversal, or unstructured outdoor terrain. Where stairs are required, the [bipedal sibling](../free-humanoid-platform/) is the correct platform. Where terrain is rough but flat (gravel, dirt access roads), the wheeled-balancing variant suggested by [`ascento`](https://openie-dev.github.io/openloco-corpus/) shielding remains a future-track option.

---

## Headline numbers

| Quantity | Target | Notes |
|---|---|---|
| **Total system mass** | **~70 kg** | 30 kg base (battery + drive + casters + tower stand) + 40 kg upper body (inherited from the bipedal platform's 50 kg minus its leg mass) |
| **Battery capacity** | **2 kWh** primary + 0.2 kWh UPS | Doubled from the bipedal's 1 kWh; no leg cavity constraint |
| **Sustained payload at extended reach** | **30 kg** | Six times the bipedal's 5 kg; possible because the wide wheeled base gives a much larger support polygon and the lower-mounted battery lowers the COM |
| **Peak ground speed** | **~1.5 m/s** | Half-again the bipedal's 1.0 m/s; safe upper bound for service-environment shared-space operation |
| **Base footprint** | **~600 mm × 800 mm × 350 mm tall** | Wide stance for stability under upper-body manipulation loads; clears standard 800 mm doorways |
| **Drive wheels** | **2× 15" diameter pneumatic** | Sized for outdoor curb tolerance at dock-B sites |
| **Caster wheels** | **2× passive swivel** | Positioned for differential-drive geometry; specifics flagged TBD in [ARCHITECTURE §2.2](ARCHITECTURE.md#22-wheeled-base) |

These numbers are scaffold targets, not validated specifications. Iterating them against physical CAD, the actuator-distribution architectural call, and the shoal dock-B protocol is the Phase 1 deliverable.

---

## Mission profile

The platform exists to handle the human-equivalent service tasks at shoal dock-B sites:

1. **Cartridge rotation.** Retrieve spent gut cartridges from the dock's cartridge port; deliver fresh cartridges from the dock's biology reservoir. See [shoal ARCHITECTURE §2.2](../clean_fish/ARCHITECTURE.md) for the cartridge spec.
2. **Waste hopper retrieval.** Empty the dock's collected-pollutant hoppers into transport bins or municipal waste-stream containers.
3. **Dock-side maintenance.** Visual and tactile inspection of dock-B mechanical components (mooring, fluid manifolds, biology-reservoir seals).
4. **Biology-reservoir status.** Confirm reservoir handshake; report nutrient-feed status; flag anomalies upstream to `shoal-fleet`.
5. **Municipal water-utility EAM integration.** The platform is designed to be a node in a municipal water utility's enterprise asset management workflow — work orders in, status updates out.

The deployment unit is the platform plus a transport vehicle that moves it between dock-B sites; the platform itself is not road-going.

---

## License posture

Same as the bipedal sibling, with the same release-blocker note on canonical license-text inlining:

| Artifact class | License | Files |
|---|---|---|
| Hardware (CAD, schematics, PCB layouts, mechanical designs) | **CERN-OHL-S 2.0** | [LICENSE-HARDWARE](LICENSE-HARDWARE) |
| Firmware, control software, simulation, tools | **Apache 2.0** | [LICENSE-SOFTWARE](LICENSE-SOFTWARE) |
| Documentation | **CC-BY-SA 4.0** | [LICENSE-DOCS](LICENSE-DOCS) |
| Descriptors (UDD JSON), datasets, BOM data | **CC0 1.0** | [LICENSE-DATA](LICENSE-DATA) |

CC0 on the descriptor is the load-bearing license choice. The license files in this repo currently contain SPDX-tagged headers; **inlining the canonical text is a release-blocker before any public push**.

---

## Governance posture

Inherits verbatim from [Free Humanoid Platform](../free-humanoid-platform/README.md#governance-posture): defensive-publication norms, examiner-citeable design rationale, no-patent-aggression contributor norm, reference-design-not-product, fail-safe-to-inert. See [CONTRIBUTING.md](CONTRIBUTING.md) for the full text of the contributor contract.

---

## Eventual hosting

Per the OpenIE hosting split, OSS projects live at `openie-dev.github.io/<project>/`. The wheeled morphology's eventual home:

- **Repository:** `github.com/openIE-dev/free-humanoid-wheeled`
- **Site:** `openie-dev.github.io/free-humanoid-wheeled/`

Local-only at v0.1; no public push until the canonical license text inlining and the architectural-call resolution gates pass.

---

## Repository layout

```
free-humanoid-wheeled/
  README.md                  this file
  ARCHITECTURE.md            canonical full-system spec, prior-art-cited
  CONTRIBUTING.md            quality bar, defensive-publication norms
  LICENSE-HARDWARE           CERN-OHL-S 2.0 (canonical text inlining is a release blocker)
  LICENSE-SOFTWARE           Apache-2.0
  LICENSE-DOCS               CC-BY-SA-4.0
  LICENSE-DATA               CC0-1.0

  descriptor/                UDD descriptor — source of truth
    free-humanoid-wheeled.udd.json
    README.md

  base/                      wheeled-base subsystem (the morphology-distinguishing work)
    README.md

  upper-body/                inherited from free-humanoid-platform
    README.md

  electronics/               PCB pointers, power tree, Joule SOM integration
  firmware/                  HAL, behaviors, MathGround safety supervisor, comms
  control/                   differential-drive MPC, manipulation policy
  sim/                       MuJoCo / Gazebo / Drake model pointers
  docs/                      design notes beyond root README/ARCHITECTURE

  prior-art/
    INDEX.md                 corpus entries this morphology depends on, by subsystem
```

Each subdirectory has its own README.md scoped to its concern.

---

## Reading order

1. [ARCHITECTURE.md](ARCHITECTURE.md) — canonical full-system spec.
2. [descriptor/free-humanoid-wheeled.udd.json](descriptor/free-humanoid-wheeled.udd.json) — UDD source of truth.
3. [prior-art/INDEX.md](prior-art/INDEX.md) — corpus citation map.
4. [Free Humanoid Platform ARCHITECTURE.md](../free-humanoid-platform/ARCHITECTURE.md) — for everything inherited (upper body, sensing, compute, safety, manipulation, learning policy).
5. [shoal ARCHITECTURE.md](../clean_fish/ARCHITECTURE.md) — for the dock-B mission integration target.

---

## Status and roadmap

Current state is **scaffold (v0.1)**. The morphology's shape, governance, architectural commitments, and prior-art chain are in place. Phase 1 is OpenLoco-compiler-buildable simulation (descriptor → MuJoCo → differential-drive controller → dock approach). Phase 2 is dock-side prototype. Phase 3 is the first municipal-utility pilot at a real shoal dock-B site. See [ARCHITECTURE.md §6](ARCHITECTURE.md#6-roadmap) for the full roadmap.
