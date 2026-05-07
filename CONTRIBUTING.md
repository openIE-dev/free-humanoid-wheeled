# Contributing to Free Humanoid Wheeled

Thank you for considering a contribution. This morphology is open hardware, open firmware, open data, and is governed by the same defensive-publication discipline as its bipedal sibling [Free Humanoid Platform](../free-humanoid-platform/CONTRIBUTING.md).

This document is the contract. **The contributor norms are inherited verbatim from the bipedal sibling**; the differences are highlighted below. If anything in this document is unclear, the canonical statement is in [free-humanoid-platform/CONTRIBUTING.md](../free-humanoid-platform/CONTRIBUTING.md).

---

## Quality bar

A contribution is acceptable for merge when:

1. **Every load-bearing design choice cites a [Free Humanoid Corpus](https://github.com/openIE-dev/free-humanoid-corpus) entry by id.** "Load-bearing" means: any choice that changes the descriptor, the BOM, the firmware HAL, the safety supervisor's invariants, the policy architecture, the wheeled-base topology, the shoal-mission protocol surface, or the comms protocol. Cosmetic changes do not require corpus citation.

2. **The cited entry actually anticipates the choice.** Same standard as the bipedal sibling.

3. **The commit message names the corpus entry id.** A commit that adds a new wheeled-base subsystem element reads `[base] add 5G modem mount; cite dlr-justin` rather than `[base] update electronics`.

4. **Documentation updates are part of the same commit.** [ARCHITECTURE.md](ARCHITECTURE.md), [prior-art/INDEX.md](prior-art/INDEX.md), and any subdirectory README that depends on the choice are updated together.

5. **Hardware contributions include an open-tooling buildability check.** Mechanical CAD must be readable in FreeCAD or OpenSCAD. PCB layouts must be readable in KiCad. Firmware must build with open toolchains.

6. **Safety-relevant contributions go through review by the safety supervisor maintainer.** Same posture as the bipedal sibling. The wheeled-base-specific invariants (slope tilt, edge proximity, low-battery-on-shore-power) are subject to additional scrutiny because they are new to this morphology.

7. **Mission-integration contributions go through review by the shoal maintainer.** Any change to the cartridge-swap protocol, waste-hopper retrieval interface, biology-reservoir handshake, or EAM API requires sign-off from the shoal architecture maintainer in addition to the wheeled-platform maintainer. The integration surface is shared between two repos and changes ripple.

---

## Defensive publication norms

Inherited from the bipedal sibling. Every commit is a public disclosure that, taken with its commit timestamp, becomes citeable as 102/103 prior art on the date of disclosure.

- **Don't strip rationale.**
- **Cite primary sources.**
- **Mark drafts as drafts.**

---

## No-patent-aggression contributor norm

Inherited from the bipedal sibling. By contributing, you agree that you will not assert any patent against this morphology, its derivatives, its users, its forks, or any other contributor, in connection with any matter covered by the design, BOM, firmware, descriptors, or documentation.

---

## Reference-design-not-product

Inherited. There is no warranty, no support contract, no service tier, no SLA, no certified configuration. Forks are encouraged.

If you are deploying the platform in a safety-critical environment — and a municipal water-utility deployment at a shoal dock-B *is* safety-critical — you are responsible for the safety case. The contributor base provides the architecture (see ARCHITECTURE §2.6) but cannot certify any specific deployment satisfies any specific regulatory regime.

---

## Pull request checklist

Before opening a PR:

- [ ] Every load-bearing design choice cites at least one corpus entry id.
- [ ] The cited entry actually anticipates the choice.
- [ ] Commit messages name the corpus entry id where applicable.
- [ ] Documentation (README, ARCHITECTURE, prior-art/INDEX, subdirectory READMEs) updated.
- [ ] Open-tooling buildability check passes for any hardware artifact.
- [ ] Safety-relevant changes flagged for safety-maintainer review.
- [ ] Mission-integration changes flagged for shoal-maintainer review.
- [ ] Drafts marked as drafts.
- [ ] Contributor agrees to the no-patent-aggression norm.

---

## Reporting security or safety issues

For safety-critical issues — supervisor bypass, fail-safe failure modes, undocumented deadlocks, edge-proximity invariant escapes, slope-tilt latch failures — please email the safety maintainer directly rather than opening a public issue, until the fix is in.

For non-safety security issues, open a regular issue.

---

## Style guide for documentation

Inherited from the bipedal sibling.

- Prose, not bullets, where the content is argued. Bullets where it is enumerated.
- Cite corpus entries inline as `` `entry-id` `` (backticks).
- Use markdown link syntax for cross-references.
- Architecture decisions: argued prose, with options, recommendation, and TBD callout where the user (David) needs to make the call.
- Subsystem READMEs: scoped to the concern, no architectural argument. Architectural argument lives in [ARCHITECTURE.md](ARCHITECTURE.md).

---

## Maintainer approval

Maintainership is currently held by the OpenIE / Free Humanoid project lead (David Charlot). Component-area maintainers (base, upper-body, electronics, firmware, control, sim, safety supervisor, shoal-integration) are appointed as the project grows.
