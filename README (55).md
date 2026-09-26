# Coin-tainer Protocol

**VERITAS IN NUMERIS**

*Temporary README — this exists to orient anyone opening the repo while the multi-file structure is still settling. Expect it to be rewritten once Type S has its own working tools and the file layout stabilises.*

---

## What this is

Coin-tainer is a protocol for physical Bitcoin bearer instruments: objects that hold a claim on real Bitcoin value, verifiable by anyone, and designed so the maker adds no value of their own — only the holder's trust in Bitcoin gives the object worth. Materials vary by type (see below). Full philosophy and shared material lives in `protocol-shared.md`.

The protocol currently produces **three** distinct types. They share a material ethos and a verification logic, but differ completely in credential, mechanism, and the relationship they create between holder and Bitcoin.

| | **Type S** | **Type L** | **Type T** |
|---|---|---|---|
| **Role** | Self-minted store of value | Circulation instrument | Preservation instrument |
| **Analogy** | The vessel you make yourself | The coin | The note |
| **Credential** | BIP39 seed plate | NTAG213 | NTAG424 |
| **Bitcoin layer** | Base chain, single-sig | Lightning | Base chain, timelocked |
| **Spendable** | Any time — owner breaks own seal | Immediately | From block 1,951,500 (~2046) |
| **Minted by** | The eventual owner, alone | The maker/merchant | The maker/merchant |
| **Status** | Concept + protocol spec drafted | Built, production-tested | Built, awaiting NFC hardware test |

---

## Type S — the self-minted coin

Type S is the odd one out by design: it's the only type minted by its own eventual owner rather than by a maker or merchant. A steel plate is generated, engraved, and sealed by one person, alone, in a single uninterrupted ceremony — no pre-minting, no third party ever holding the key. The plate sits inside a Coin-tainer: three 150mm MDF discs laminated Face–Core–Face (the core a through-cutout matching the plate), bonded under pressure with high-strength PVA glue — meant to fail by tearing the MDF, not by separating cleanly, though not yet pull-tested. The xpub is laser-engraved onto the exterior after assembly, for watch-only verification without breaking the seal. An upgrade tier adds a fourth, unmarked walnut veneer layer — natural grain variation restores a per-unit anti-duplication property the basic tier doesn't have, paired with a birth-log photo pinned to IPFS for later comparison.

Where Type L and Type T are made to be sold, Type S is made to be *learned* — the self-mint ceremony is treated as a craft, built up through practice the way any skill is, with a maker's reputation for care and consistency earned over time rather than certified. It has no software toolset — the equivalent of Type T's HTML tools is a physical equipment set (entropy drum, pill holder, SeedSigner, Seed Hammer) plus the training and checklist that use it.

**Three names, three layers**: *Slow Money* is the guiding philosophy; *Self-Mint* is the course and the workshop; *Coin-tainer Type S* is the handmade object that results. Growth is repeat-and-replicate, not franchise — one open protocol, any number of independent workshops, each supplying tools only, never materials, no fee back to anywhere.

→ `type-s.md` — full protocol spec (object, seal mechanics, self-mint rule, birth registration, deferred fast-settlement alternative)
→ `type-s-equipment.md` — the tools and materials the ceremony runs on, build/sourcing status, ownership model
→ `type-s-workshop.md` — communal training that builds the skill collectively, on one shared equipment set
→ `type-s-ceremony.md` — the solo, step-by-step checklist for an actual live mint

---

## Type T — the preservation instrument

Type T is the heirloom object. Bitcoin sent to its timelocked address can't be spent until block 1,951,500 — roughly 22 May 2046, Bitcoin Pizza Day's 36th anniversary. The private key lives openly on the coin (chip, QR, and web record) because the timelock itself is the security: there's nothing to gain by extracting a key that can't spend for 21 years. Multi-colour mottled plastic gives every coin an optically unique, unclonable surface (a Physical Unclonable Function), enrolled and matched via camera for anti-counterfeiting and provenance.

Type T is the most technically developed of the three: three working, self-contained HTML tools (enrol, verify, sweep) already exist, client-side only, with inlined Bitcoin cryptography and no server dependency for the crypto itself.

→ `type-t.md` — product spec, physical construction, life cycle, manufacture, build-priority status
→ `type-t-intro.md` — the three working tools (`enrol.html`, `verify.html`, `sweep.html`): workflows, payload format, locked architecture decisions, outstanding infrastructure

---

## Type L — the circulation instrument

Type L is the everyday object — a Lightning-loaded coin meant to move between hands and be spent without ceremony. Sold empty, loaded with sats at a merchant terminal, tapped to redeem. No optical PUF, no timelock, no denomination marked on the coin itself; a single NTAG213 chip and a Cloudflare-Worker-backed ledger track status. Production-tested end to end on the Sunmi V2S terminal.

→ `type-l.md` — product spec, life cycle, manufacture, build status

---

## Shared and cross-cutting material

- `protocol-shared.md` — the manifesto, the four pillars (finite digital scarcity, anti-seigniorage, the physical mirror, material honesty), shared verification infrastructure, life-cycle framework, closing philosophy
- `integration.md` — **Type L and Type T only.** The build work that touches both systems at once (shared UID routing, the Bridge APK NFC-write path both types depend on), the combined architecture table, and the guardrails protecting Type L's working code while Type T is built. Type S has no integration surface with the other two yet — it doesn't share hardware, a domain, or a codebase with either.

---

## Repo map

```
README.md              ← you are here (temporary)
protocol-shared.md      shared philosophy + infrastructure
type-s.md                Type S — self-minted coin (protocol spec)
type-s-equipment.md      Type S — tools and materials
type-s-workshop.md       Type S — communal training
type-s-ceremony.md       Type S — solo live-mint checklist
type-l.md                Type L — circulation instrument (spec + build status)
type-t.md                Type T — preservation instrument (spec + build status)
type-t-intro.md          Type T — the three working tools (enrol/verify/sweep) — currently behind the actual build; see below
integration.md          Type L ↔ Type T shared build work
```

**Known gap, not yet fixed here:** `type-t-intro.md` documents an earlier state of the Type T toolchain. The real build has since added Cloudflare Worker calls in `enrol.html` (edition/coin-number allocation), a separate mintmark PNG→SVG converter, a credit-card-plate generator for sweep-tool distribution, and possibly a second, dependency-free sweep variant (`sweep_minimal.html`) alongside the one currently in the repo. `verify.html` itself — confirmed built, with a funded gate, tap counter, and PUF image panel — isn't in the repo at all yet. Worth its own pass once Type S's structure is settled.
