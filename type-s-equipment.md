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
| **Casing/sealing setup** | Encases the plate in injection-molded waste plastic | 🆕 | The one piece with no real precedent — Type T/L's coin bodies aren't owner-sealed, so there's nothing to adapt from them. Needs its own sourcing: process, feedstock, and whether it's solo-operable or needs dedicated shared equipment (see `type-s-ceremony.md` Open Questions). |
| **QR printer/etcher** | Marks the xpub onto the vessel's exterior | 🆕 | Whatever produces the QR needs to be resolved alongside the casing/sealing setup, since both happen at the same late stage of the ceremony. |
| **Member's own phone + wallet app** | Independent balance-check (ceremony step 10) | ✅ | Not shared equipment — brought individually by each member, same as in btckeys' xpub/BlueWallet step. |
| **Member's own verified microSD card** | Runs the checksum/engraving device | ✅ | Individually owned, prepared once via Phase 0 in `type-s-ceremony.md` — not shared, not reused across people. |

---

## Ownership and access model

Three different categories, not one:

- **Individually owned, prepared once**: the microSD card (Phase 0) and, implicitly, the member's own phone. Never shared, never reused across people.
- **Shared, workshop-owned, used two ways**: the entropy drum/pill set, the pill holder, the checksum/engraving device body, and the plate engraver. For the first workshop events there is **one of each** — used collectively during group practice, and alone once a member enters the minting space. See `type-s-workshop.md` for the collective side and `type-s-ceremony.md` for the solo side and the equipment-reset check between the two.
- **Unresolved**: the casing/sealing setup and QR marking equipment don't yet have an ownership model at all, because they don't yet have a sourced process. This needs settling before the ceremony's later steps (11–12 in `type-s-ceremony.md`) can actually be run, practice or live.

---

## Open Questions

- Confirm Entropia word pills + lottery drum as Type S's entropy method (following btckeys' own comparison against dice), or document a different mechanical source
- Source the casing/sealing process and multi-color waste plastic feedstock; determine whether it's solo-operable or requires dedicated shared equipment
- Source the QR printer/etcher for the xpub marking
- Engraving technique for an individual working alone, versus what needs Phase-0-style shared setup prepared in advance
- Booking/scheduling protocol for solo access to the single shared equipment set, once workshop events grow beyond a size where informal turn-taking still works
- Physical integrity/recount cadence for the entropy source (matches the still-open item in `type-s-ceremony.md`'s risk register)
