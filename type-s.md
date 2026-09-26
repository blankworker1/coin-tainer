# Coin-tainer — Type S

*Self-mint. Single-sig. Sealed.*

A third credential/toolchain variant alongside **Type T** (timelocked, NTAG424) and **Type L** (Lightning, NTAG424 + Blink API). Where T and L use an NFC chip as the credential, **Type S uses a BIP39 plate** — spun out of BOX's original technical Bitcoin layer once BOX itself was cut back to a pure barter platform.

This file is the protocol spec — the object and its rules. Four companion documents cover everything else: the philosophical case (self-mint as a third rung beyond self-custody, mastery, Slow Money) lives in [[anyone-can-mint-this-money]]; the tools and materials the ceremony actually runs on live in `type-s-equipment.md`; the communal training that builds the skill lives in `type-s-workshop.md`; the solo step-by-step checklist for an actual mint lives in `type-s-ceremony.md`.

A Type S coin doesn't need BOX to exist. A BOX locker is one possible place to store or settle one, exactly as incidental as a pocket or a safe — never a dependency.

---

## 1. The object

Two parts:

1. **The plate** — a stainless steel plate, engraved with a single, independently-generated 12-word BIP39 seed via a Seed Hammer engraving machine — one wallet, never a sub-account, never reused across owners.
2. **The Coin-tainer** — the enclosure. **Reference shape: a 150mm circle.** Three layers, laminated flat, one on top of the next:
   - **Face A** (2mm MDF, bare) — bottom layer.
   - **Core** (2mm MDF) — a through-cutout the exact profile of the plate, forming a picture-frame ring. This is where the plate sits, fully enclosed once the faces are on both sides — not a recess in a single face.
   - **Face B** (2mm MDF, bare) — top layer.
   - Bonded under pressure with high-strength PVA glue. Both face-to-core joints are full-disc bonds across the whole 150mm circle, not just at the plate's edges — real structural area on both sides of the cavity, not a single lid doing all the containing work.

Two tiers:
- **Basic**: the three-layer stack above. The wallet's **xpub is laser-engraved onto Face A, after assembly** (post-cure, directly onto the now-exterior MDF surface) — watch-only, cannot spend, visible without breaking the seal. No per-unit physical uniqueness beyond the QR's own content.
- **Upgrade**: a fourth layer, a **150mm disc of walnut veneer**, laminated onto Face B (the face opposite the QR), left **unmarked**. Grain figure and colour variation are a property of the specific wood, not something a fabricator or duplicator controls — genuine physical entropy, restoring the anti-duplication property the basic tier doesn't have. See §4 for how this pairs with the birth record.

Not a demo tier — the true bearer-object standard, the physical peer to a Casascius coin.

---

## 2. What the seal actually does — a physical property, not a cryptographic one

Bitcoin has no concept of a deposit-only address; anyone holding the 12 words can always spend. "Value can only be added, not withdrawn" holds only because withdrawal requires the seed, and the seed is physically inaccessible while the casing is intact. This depends on the face-to-core PVA bonds holding under an attempt to pry the disc apart. High-strength PVA, properly clamped, typically bonds stronger than MDF itself — failure should be fibre tear-out, not a clean parting line — but this depends on the cut edges of the core being cleaned of laser char before gluing (a laser-cut edge's sooty surface layer is a weaker bonding surface than clean MDF, and left uncleaned could become the actual failure plane). Not yet pull-tested; see `type-s-equipment.md`.

The owner retains the right to break their own seal and sweep the funds to their own wallet at any time; the seal is a convenience and a ritual, never a restriction on the legitimate holder's sovereignty.

**Stated precisely, a sealed coin admits exactly two operations to anyone in the community, and one further operation reserved to a single person:**
- **Increase** — anyone, at any time, can send more sats to the wallet the xpub belongs to. Permissionless, requires nothing from the coin's current holder.
- **Exchange without increasing** — the sealed coin can change hands at its current attributed value with no Bitcoin transaction at all; only physical possession moves, the on-chain balance doesn't.
- **Decrease** — reserved entirely to whoever currently holds the coin, and only by breaking their own seal. No one else, at any point in the coin's circulation, can reduce its balance.

---

## 3. The self-mint rule — learned from Casascius's one real failure

Casascius coins used the same public-address / hidden-key shape, and the only thing that ever undermined trust in them was that they were minted centrally — for a moment, the minter held every key, and trust rested on believing they'd destroyed their copies.

Type S avoids this by requiring the eventual owner to generate the entropy, engrave the plate, and seal it themselves, in one uninterrupted session, with nobody else present. **No pre-minting a batch of sealed coins for convenience** — every coin is minted only at the moment someone takes possession of it. This mirrors the same self-sovereign generation principle already used in [[sd-card-ceremony]], applied to a plate instead of a microSD card.

A practice phase — the whole generate/engrave/seal sequence run on a scrap plate with worthless test entropy — should precede anyone's first live mint. Learner's permit, then the real thing; see `type-s-workshop.md` for how that training actually runs, and `type-s-ceremony.md` for the live-mint checklist itself. See [[anyone-can-mint-this-money]] for why this matters beyond due diligence: mastery, not just correctness, is what eventually lets the maker's doubt disappear entirely.

---

## 4. Recording the birth — one line in a log, not a gallery

No dedicated infrastructure, no website, no relay. The precedent this borrows: [[il-custode]]'s chain object is placed on its plinth once, by its first Donatore, and the Remembrancer records its form at that moment — a single, durable entry marking that it now exists. A coin's birth is the same shape of fact, registered the same way, in whatever durable log a given mint's community already keeps (BosaNode's Remembrancer log, for instance, if minted at a Prova):

> *"[Nome] ha coniato una moneta, [breve descrizione della colata], oggi."*

**Upgrade-tier coins add one field**: a link to a photo of the coin's veneer face, so its grain pattern can be checked against the physical object later — the actual anti-duplication mechanism the veneer exists for. Stored as an IPFS URL, not a hosted image — content-addressed, so the link itself can't be quietly swapped for a different photo after the fact. Pinned redundantly: primary copy self-hosted (a Kubo node run as another service on the same box running BosaTAZ, available whenever that infrastructure is operating), backed up on a free-tier pinning service so the link still resolves when the truck's own node isn't reachable. Basic-tier coins have nothing to photograph and skip this field entirely.

This is **not a transaction** — a promise or a trade needs two parties and a relationship between them; a birth has only one. It's a registration: this now exists, made by this person. **One entry, once, at mint** (plus the one photo for upgrade-tier coins). **Nothing further is tracked.**

That's deliberately sufficient, because the chain already carries everything else. Current balance and whether a coin's ever been opened are both permanently checkable by anyone via its public xpub — no outgoing transaction, presumably still sealed; any outgoing transaction, redeemed, whether by a thief or by the rightful owner exercising their own right to break the seal. Circulation and current-holder are never logged at all, consistent with the no-persistent-reputation-by-default principle already built into [[zona-permuta-taz]]'s local ledger. The one fact the chain can never supply — who actually made it — is the one fact this entry exists to preserve.

**Critical rule: one wallet per plate.** An xpub exposes every address its wallet has used or will use, not just one balance — already the intended and accepted trade-off here, since the xpub is deliberately public from the moment of minting. What must never happen is a *shared* parent wallet across multiple plates, which would let one coin's xpub expose activity across every other plate derived from it.

---

## 5. Locker/vessel as "ledger entry" — the analogy, precisely

- Wherever it's kept = one UTXO container, not an account: it holds one specific, fixed-amount bearer instrument until swept.
- Opening + taking the plate = withdrawal; on-chain settlement happens after physical custody changes hands, on the recipient's own device — wherever the coin was kept never touches the blockchain itself.
- Sealing a coin away = the "ledger entry" — the physical, sealed, occupied vessel, legible to nobody but whoever holds it.
- This is an analogy, not a literal ledger: nothing proves a coin's claimed contents match reality except opening it, the same trust model as any bearer cash.

---

## 6. The double-spend problem — and its fix

Whoever created the wallet always retains knowledge of the underlying secret, regardless of casing quality or physical settlement. No mechanism at this layer can close that gap entirely — it's the general Bitcoin custody-handoff problem. The coin is meant to circulate sealed, not be swept on every handoff, so its protection is the **self-mint rule**: if the ceremony is followed correctly, nobody but the current owner ever knew the seed in the first place, so there's no prior holder's knowledge to worry about until the owner themselves chooses to pass the coin on. The risk shifts entirely to whether that rule was actually followed at minting.

## 7. What "risk-free" verification does and doesn't cover

The xpub gives risk-free **balance** auditing. It does not protect against the seed having been **copied before or during minting** — physical settlement proves nobody else can *open the seal* afterward, not that no copy was retained. This is the same limit every bearer seed instrument carries (see [[coin-tainer]]) — Bitcoin custody is defined by knowledge of the secret, not possession of any one physical object. At small-community scale this rests on the same character-based trust [[anyone-can-mint-this-money]] argues is the actual foundation, not a gap the protocol failed to close.

**Holder practice: spread holdings across minters.** This doesn't reduce the risk any single coin carries — a compromised minter's coin is exactly as compromised whether you hold one or fifty. What it reduces is *your own concentration*: exposure to any one bad-faith or careless minter scales with how much of your total holdings trace back to that person, not with how many other minters exist elsewhere in the network. The same logic that applies to savings across banks, or entropy across independent sources — don't let one point of failure account for a large share of what you hold. Worth stating as explicit guidance, not left as something holders are expected to discover on their own: a coin from a minter you don't yet know well is fine to hold in modest proportion; it shouldn't be where most of your value sits.

---

## 8. Deferred alternate: the fast-settlement mechanism ("5a")

A second, faster mechanism was designed and prototyped alongside Type S, then deliberately deferred rather than shipped. Kept here as a condensed record in case it's ever revisited.

**What it was:** a fixed, publicly-known plate (not owner-minted) offering instant, no-ceremony transfers — scan the plate's public 12 words, generate a fresh private half and passphrase via a dedicated tool (the **SETTLE App**), hand off screen-to-screen, sweep immediately.

**Why it mattered:** it was the one genuinely novel piece of engineering here — a public plate minting unlimited unrelated wallets is a pattern no existing scheme (BIP85, SLIP39, standard passphrase wallets) does — and it filled two real gaps: a fast, sats-sized *conguaglio* (the top-up that settles an uneven barter trade) without needing a whole Type S coin for a trivial amount, and a zero-ceremony on-ramp for someone not yet ready for the self-mint ceremony.

**Why it was deferred:** for coherence, not capability. Everything Type S rests on — self-mint, mastery, the workshop, Slow Money — describes patient, local, craft-based money. This mechanism was the opposite on every axis: instant, walk-up, no ceremony, available to someone who'd never touched the workshop. Keeping both would mean the project's own Bitcoin layer working against its stated values.

**Technical record:**
- Three data blocks — Locker (public, fixed), Generated (private half + computed 24th word, last 3 entropy bits fixed by convention so the 24th word never needs transmitting on its own), Passphrase (fresh per transaction) — each traveling on a different channel, none alone sufficient.
- A working prototype exists: real BIP39 entropy and SHA-256 checksum via native Web Crypto, QR-scan-driven UX on both the depositor and recipient sides, destination address scanned from the recipient's own wallet rather than typed (removing manual-entry error from the highest-consequence step). It compiles and displays the completed phrase for external-wallet import; it does not yet perform EC derivation, signing, or broadcast for true one-tap auto-sweep.
- Security rested on immediate-sweep discipline (a habit, not a hardware property) and a documented in-person/remote mode distinction — remote mode an explicit, named trade-off (no physical-presence guarantee) rather than a silent default.
- **Open question if revisited**: whether it can be reconciled with the Slow Money framing at all, or is better kept as a permanently separate, explicitly-labeled "fast lane" rather than folded back into Type S's main identity.

---

## Open Questions

- Pull-test the face-to-core PVA joints before trusting the design with real value — confirm failure is fibre tear-out, not a clean parting line
- Confirm the exact birth-entry wording with whoever maintains a given community's durable log (e.g. BosaNode's Remembrancer), and how/whether it's visually distinguished from other entry types
- Sequence for eventually surfacing Type S publicly, if at all, beyond the communities already minting it
- Add real EC wallet derivation, signing, and broadcast to the deferred SETTLE App, if 5a is ever revisited — needs a properly vetted Bitcoin library, not a rushed addition
- See `type-s-workshop.md` and `type-s-ceremony.md` for training- and mint-specific open questions (entropy method, cadence, graduation, materials)
- See `type-s-equipment.md` for construction/tooling open questions (recess clearance, veneer press tooling, Kubo node setup)
