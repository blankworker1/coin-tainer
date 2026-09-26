# Type S — Equipment

*What the ceremony and workshop actually run on. `type-s.md` specifies the coin; this specifies the tools that make one. See `type-s-workshop.md` for how this equipment is used collectively, and `type-s-ceremony.md` for how it's used solo.*

## Status key

| **Status key** | **Meaning** |
|---|---|
| **✅ EXISTS** | Sourced/built and in use |
| **🔧 NEEDS WORK** | Exists but requires changes or adaptation for Type S specifically |
| **🆕 NEW BUILD** | Does not exist yet — needs sourcing or building |
| **⚠️ DEPENDENCY** | Blocked on another decision |

---

## Equipment

| Item | Role | Status | Notes |
|---|---|---|---|
| **Entropy source** — lottery drum + word pill set | Draw 11 words of raw entropy | 🔧 | Inherited from [[seedcard-protocol\|btckeys]] (transparent acrylic drum, 2048-pill Entropia set). Proven design, not yet built/sourced for this repo specifically — see Open Questions on confirming this over dice or an alternative. |
| **Pill holder** | Stages the 11 drawn words; the single physical record between draw and transcription | 🔧 | Also from btckeys: two-part clear perspex case, rotates as a sealed unit, 11 numbered slots + printed CHECKSUM (12th) slot, word-hidden-by-default. Reused directly; no Type S-specific changes identified yet. |
| **Checksum/engraving device** (e.g. SeedSigner) | Computes the 12th word; displays the full phrase for engraving and xpub derivation | ✅ | Air-gapped, boots from microSD, no wireless hardware. Used solely for checksum calculation and display — never as an entropy source. |
| **Plate engraver** (e.g. Seed Hammer) | Stamps the 12-word phrase onto the steel plate | ✅ | Same tool already proven in btckeys. |
| **Laser cutter** | Cuts the Coin-tainer's layers — Face A, Face B (bare 2mm MDF discs), and the Core (2mm MDF, through-cutout matching the plate) — plus the QR, engraved post-assembly | ✅ | Fabrication method and cut geometry now settled: 150mm circles, core is a through-cutout (picture-frame ring), not a recess. |
| **Clamping/press rig** | Holds Face–Core–Face (and, upgrade tier, the veneer) under pressure while the PVA cures | 🆕 | Two different pressure profiles needed: edge/local clamping for the face-to-core glue-up (bonding is at the core's cut edges), and full-flat pressure (a proper press or clamping caul) for the veneer lamination, which needs even contact across the whole 150mm disc, not just the rim. |
| **Edge-cleaning tool** (fine sandpaper or similar) | Removes laser char from the core's cut edges before gluing | 🆕 | Small but load-bearing step — an uncleaned charred edge is a weaker bond surface and could become the actual failure plane instead of the MDF itself tearing. |
| **High-strength PVA glue** | Bonds Face–Core–Face (and, upgrade tier, the veneer) | 🆕 | Consumable, brought/stocked like any other raw material. Grade matters — needs to be strong enough to fail by tearing MDF fibre, not release cleanly; not yet tested against a specific product. |
| **Walnut veneer stock** (150mm discs) | The upgrade tier's anti-duplication layer, laminated onto Face B, left unmarked | 🆕 | Sourcing (thickness, supplier, consistent enough to cut cleanly but variable enough grain-to-grain) not yet settled. |
| **Kubo (IPFS node)** | Self-hosted pin of each upgrade-tier coin's birth photo | 🆕 | Runs as another service on the same box already running BosaTAZ — no new hardware. Available whenever that infrastructure is operating; see `type-s.md` §4. |
| **Backup pinning service** (free-tier, e.g. Pinata) | Redundant pin of the same photo hash, for when the truck's own node isn't reachable | 🆕 | Same content hash either way — no conflict, pure redundancy, never authoritative over the self-hosted copy. |
| **Member's own phone + wallet app** | Independent balance-check (ceremony step) | ✅ | Not shared equipment — brought individually by each member, same as in btckeys' xpub/BlueWallet step. |
| **Member's own verified microSD card** | Runs the checksum/engraving device | ✅ | Individually owned, prepared once via Phase 0 in `type-s-ceremony.md` — not shared, not reused across people. |
| **Raw materials** — steel plate blank, MDF sheet stock, walnut veneer disc (upgrade tier only) | What actually becomes the coin | ⚠️ | **Always brought by the minter. Never supplied by the workshop.** This is a hard rule, not a convenience — see Ownership model below. |

---

## Ownership and access model

**Hard rule: the workshop supplies tools, never materials.** Anything that becomes part of a finished coin — the steel plate, the MDF stock, anything else consumed into the object itself — is always brought by the minter, never provided by the workshop. This isn't a resourcing detail; it's the line that keeps the workshop a tool shed and not a supplier, consistent with the growth model in `type-s-workshop.md` and the reasoning that keeps Type S outside money-transmission territory in the first place.

Five different categories, not one:

- **Individually owned, prepared once**: the microSD card (Phase 0) and, implicitly, the member's own phone. Never shared, never reused across people.
- **Individually owned, brought fresh each time**: raw materials (steel plate, MDF stock, walnut veneer disc for upgrade-tier mints). Never workshop-supplied, per the hard rule above.
- **Shared, workshop-owned, used two ways**: the entropy drum/pill set, the pill holder, the checksum/engraving device body, the plate engraver, the laser cutter, and the clamping/press rig. For the first workshop events there is **one of each** — used collectively during group practice, and alone once a member enters the minting space. See `type-s-workshop.md` for the collective side and `type-s-ceremony.md` for the solo side and the equipment-reset check between the two.
- **Shared, infrastructure-owned, not tied to any one ceremony**: the Kubo node and backup pin. These run on their own schedule (whenever the BosaTAZ box is operating), independent of any individual minting session.
- **Unresolved**: veneer sourcing and the clamping rig itself don't have a sourced process yet, so neither has a full ownership model. This needs settling before upgrade-tier ceremony steps can actually be run, practice or live — basic-tier mints aren't blocked by this.

---

## Open Questions

- Confirm Entropia word pills + lottery drum as Type S's entropy method (following btckeys' own comparison against dice), or document a different mechanical source
- **Pull-test the face-to-core PVA joint** before trusting the design with real value — confirm failure is fibre tear-out, not a clean parting line (see `type-s.md` §2)
- Source the clamping/press rig — specifically the full-flat press needed for veneer lamination, distinct from the edge-clamping the core joint needs
- Source walnut veneer stock (thickness, supplier, consistency of cut vs. natural grain variation)
- Recess/cutout clearance tolerance between the plate and the core's through-cutout — tight enough not to rattle, loose enough to seat without force
- Cure time for the PVA joint, and whether that creates a natural pause point in the ceremony sequence
- Who administers the Kubo node's ongoing operation (updates, disk space, uptime) as part of the BosaTAZ box's other responsibilities
- Choice of backup pinning service
- Engraving and laser-cutting technique for an individual working alone, versus what needs Phase-0-style shared setup prepared in advance
- Booking/scheduling protocol for solo access to the single shared equipment set, once workshop events grow beyond a size where informal turn-taking still works
- Physical integrity/recount cadence for the entropy source (matches the still-open item in `type-s-ceremony.md`'s risk register)
