# Coin-tainer — Type S

![Coin-tainer mintmark — rounded triangle with interior bar](./coin-tainer-mintmark.jpg)

*Self-mint. Single-sig. Sealed.*

*The mark above is the Italian community's mintmark — mint #2 in the Coin-tainer lineage, alongside the Welsh Dragon as Criccieth's founding mark. In Type T/L it's engraved onto Face B alongside the UID ring text; in Type S (§1) it's engraved onto Face A, alongside the xpub QR and fingerprint — same identifying role, different face, same three-community lineage.*

A third credential/toolchain variant alongside **Type T** (timelocked, NTAG424) and **Type L** (Lightning, NTAG424 + Blink API). Where T and L use an NFC chip as the credential, **Type S uses a BIP39 plate** — spun out of BOX's original technical Bitcoin layer once BOX itself was cut back to a pure barter platform.

This file is the protocol spec — the object and its rules. Four companion documents cover everything else: the philosophical case (self-mint as a third rung beyond self-custody, mastery, Slow Money) lives in [[anyone-can-mint-this-money]]; the tools and materials the ceremony actually runs on live in `type-s-equipment.md`; the communal training that builds the skill lives in `type-s-workshop.md`; the solo step-by-step checklist for an actual mint lives in `type-s-ceremony.md`.

A Type S coin doesn't need BOX to exist. A BOX locker is one possible place to store or settle one, exactly as incidental as a pocket or a safe — never a dependency.

---

## In plain terms

Seed, key, private address, Bitcoin — the technical vocabulary is easy to confuse, and a holder shouldn't need to untangle it. **The Coin-tainer holds Bitcoin.** That's the one sentence worth knowing. Everything it does reduces to:

> **MINT → HOLD ⇄ SWAP**
> **TOP-UP** — anytime, by anyone, regardless of hold/swap state
> **REDEEM** — always available, rarely needed

The Coin-tainer object is complete and transactable independent of its current balance — mintable, holdable, swappable at zero, fully ready to receive value whenever anyone chooses to add it.

"Key" is a real, precise word, but it describes the seed inside, one layer down — never the Coin-tainer itself. See §1 below for exactly how those layers relate.

**No mobile wallet app, and no internet connection, is ever required for HOLD, SWAP, or VERIFY** — small, purpose-built static tools (§4) cover checking a balance and confirming a coin is genuine, with nothing to download, no server ever seeing an xpub or a private key, and a fully offline pair of both tools running right at the truck. **REDEEM is the one deliberate exception**: breaking the seal means signing a real transaction, which needs the seed loaded into genuine, vetted wallet software — the same boundary the deferred SETTLE App drew for itself, and for the same reason. Every other step in the arc stays wallet-app-free and connectivity-free by design; that one doesn't, on purpose.

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
- **Basic**: the three-layer stack above. **Face A carries three marks, laser-engraved together in one pass, post-assembly:**
  1. The wallet's **xpub**, as a QR — watch-only, cannot spend, visible without breaking the seal.
  2. The wallet's **fingerprint**, 8 hex characters, directly beneath the QR: the standard BIP32 key fingerprint (first 4 bytes of HASH160(pubkey)) computed from the same xpub — nothing assigned, nothing extra to generate, it falls straight out of the derivation already done in the ceremony.
  3. The community's **mintmark** (following the Welsh Dragon/Italian triangle precedent from Type T/L) — identifies which community's mint the coin came from, nothing about who specifically minted it.

  All three sized and positioned against the QR's own readability first — the QR keeps whatever minimum size and quiet-zone margin it needs to scan reliably; the fingerprint and mintmark are fitted around it, not the other way round. No per-unit physical uniqueness beyond these three marks.
- **Upgrade**: a fourth layer, a **150mm disc of walnut veneer**, laminated onto Face B (the face opposite Face A), left otherwise unmarked. Grain figure and colour variation are a property of the specific wood, not something a fabricator or duplicator controls — genuine physical entropy, restoring the anti-duplication property the basic tier doesn't have. See §4 for how this pairs with the birth record.

**The fingerprint is independently recomputable — this is what makes it more than a label.** Unlike an assigned serial number, which only ever checks against a log, anyone holding the coin can recompute the fingerprint themselves straight from the printed xpub, using any standard wallet tool, with no log access needed. It does two jobs: it's the lookup key into the birth registry (§4), *and* it's a free cryptographic cross-check that the engraved xpub QR hasn't been mis-engraved or swapped — a mismatch between a recomputed fingerprint and the engraved one is caught by anyone, on the spot, without trusting any registry at all.

**The mintmark, by contrast, verifies nothing on its own** — it's a symbol, exactly as reproducible as the QR. Its value is what it signals (this coin passed through a real Self-Mint), not what it proves; unlike the fingerprint, nobody should treat a mintmark as anything more than a legibility aid.

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

> *"[Nome] ha coniato una moneta, [fingerprint], [breve descrizione della colata], oggi."*

The **fingerprint** (§1) is now a required field on every entry, both tiers — it's the lookup key that connects a specific physical coin to its specific log line, something the log couldn't do on its own before, since every basic-tier coin is otherwise identical.

The two tiers diverge from here, deliberately:

- **Basic tier**: text only, exactly as above. No photo, no IPFS, no pinning infrastructure — the zero-infrastructure promise stays intact for basic mints. This closes a narrower gap than the upgrade tier does: it stops the laziest possible attack (an engraved fingerprint that was never actually logged), but a photo of one basic coin's plain MDF face would prove nothing next to another's, so nothing is lost by skipping it.
- **Upgrade tier**: the log entry adds one field, a link to a photo of the coin's veneer face, with the fingerprint **overlaid directly onto the image** — so the photo is self-describing even if separated from its log line. This is where the fingerprint does its second job: grain can't be deliberately reproduced, so this photo is genuine anti-duplication evidence, not just a registry-existence check. Stored as an IPFS URL, not a hosted image — content-addressed, so the link itself can't be quietly swapped for a different photo after the fact. Pinned redundantly: primary copy self-hosted (a Kubo node run as another service on the same box running BosaTAZ, available whenever that infrastructure is operating), backed up on a free-tier pinning service so the link still resolves when the truck's own node isn't reachable.

**The fingerprint ties four independent layers together, worth naming as a deliberate structure rather than a side effect**: the ceremony (where it's computed, from the same derivation as the xpub), the steel plate (the seed it ultimately derives from), the engraving (printed on Face A, self-verifiable against the xpub by anyone), and the IPFS record (overlaid on the birth photo, upgrade tier). No single layer has to be trusted alone — a mismatch at any layer is visible without needing the others to vouch for it.

### Verifying a specific coin, without exposing the mint bank

The primary use case for looking anything up: confirming that *the specific upgrade-tier coin in your hand* has genuinely been photographed and entered into the community's mint bank — not browsing the mint bank itself, which stays deliberately unenumerable by design, not merely undiscoverable through the UI.

That distinction matters technically, not just as a policy: a pure static site (files sitting on IPFS, or listed in a GitHub repo) can't deliver "unsearchable by design" on its own. Anything a browser can fetch, a person can fetch directly too — hiding a "browse all" button doesn't stop someone reading the underlying data file or a repo's own file listing, which would amount to a full index of every coin ever minted. Refusing to enumerate requires something that can actually refuse a request, not just an interface that doesn't offer one.

The fix: a **single-lookup-only service**, matching Type T's existing Cloudflare Worker pattern (`/t/:uid → KV lookup → redirect`) rather than inventing something new —

```
GET /s/lookup/{fingerprint} → KV get → { found: true, ipfs_url } or { found: false }
```

— with no endpoint, ever, that returns a full list of keys. A fingerprint's 8-hex-character keyspace (~4.3 billion values) makes blind enumeration impractical; only someone who already holds a specific coin's fingerprint gets anything back.

Two backend instances of this same lookup, entirely independent of each other — neither depends on the other being up:
- **Remote**: a Cloudflare Worker plus a new KV namespace (`TYPE_S_COINS`), populated with one `{fingerprint: ipfs_url}` entry per upgrade-tier mint at the same moment the birth log gets its entry. Reachable generally, over ordinary internet — **this is the route that exists specifically for when the truck isn't operating.**
- **Local**: the same lookup shape added as one more small service on BosaTAZ's own local relay — naturally non-enumerable from the start, since nothing there was ever exposing raw files. Reachable over the truck's own wifi zone, no internet required.

Pin names on both IPFS services (self-hosted Kubo and the backup pinning service) should be set to the fingerprint at upload time — Pinata's own dashboard already supports search-by-name with zero extra building, and the same convention costs nothing on the self-hosted side.

### Why a stale local balance is a safe floor, not a risk

A sealed coin's balance can only move in directions the design already accounts for (§2): *increase*, permissionless, by anyone, anytime; *exchange without increasing*, no change to the balance at all; *decrease*, reserved entirely to the current holder, and only by breaking their own seal. As long as the seal is visibly intact — checkable by eye, no data connection needed — **the true balance cannot legitimately be lower than any honest past reading of it.** It can only be the same or higher.

This means a cached, timestamped balance isn't uncertain in the way stale data normally is. The only thing a holder can get wrong by trusting one is *underestimating* a coin — missing a top-up that happened after the snapshot was taken. Nobody is put at risk by a coin turning out to be worth more than expected. Staleness here fails safe, in one direction only.

*(A defeated seal — tamper-evidence beaten, funds swept, resealed convincingly — would be an illegitimate decrease, but that's the self-mint-rule risk already covered in §6, and it's orthogonal to connectivity: a live balance check wouldn't catch it any better than a cached one, since both depend on the physical seal having held in the first place, not on how fresh the data is.)*

### Three tools, making the whole holder arc offline-capable at the truck

- **Local genuineness-lookup** (BosaTAZ portal tab): checks the local KV, exactly as described above. Fully offline, no change.
- **Local balance-check** (BosaTAZ portal tab): a cached `{fingerprint: last-known-balance, timestamp}` table, populated whenever the box briefly has connectivity — tethered before loading the truck, say — via the same one-directional sync pattern BosaTAZ already uses for its outward GitHub archive, just running inward. Given the floor property above, this is a genuinely valid, complete answer at the truck, not a stopgap: a holder sees "at least X, as of [time]," which is exactly the guarantee they need to accept a coin in person, no internet required at the moment of the trade itself.
- **Remote balance-check** (Cloudflare Pages + Worker): the live version, for anywhere BosaTAZ's wifi isn't reachable — checks the actual current network state via client-side xpub derivation and per-address queries, as before.

Both balance-check tools are built for a novice, one action: **scan the coin's xpub QR → see the balance** (plus its as-of time, on the local version). Both genuineness-lookup tools: **type the coin's 8-character fingerprint → see the registered veneer photo with its fingerprint overlay**, side by side with the coin in hand.

Kept as separate single-purpose tools throughout, not merged, matching every other tool in this project (SETTLE App, `verify.html`) — each does exactly one job.

**When a holder should actually use these**: before accepting an unfamiliar upgrade-tier coin — one from a minter you don't already know well — check genuineness and balance both, whichever pair (local or remote) is reachable. For a basic-tier coin, or one from a minter you do know, the balance check alone is normally enough.

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
- Confirm the fingerprint is computed from the exact xpub engraved on the coin, with no intermediate derivation step between them — otherwise the self-verification cross-check in §1 breaks
- Finalize the Face A layout: QR size/quiet-zone minimum first, then fingerprint and mintmark placement and size fitted around it — needs a physical test engrave to confirm all three stay legible at 150mm scale
- Build the two backend lookup services (remote Cloudflare Worker/KV, BosaTAZ local relay equivalent), the remote frontend pair (Cloudflare Pages, genuineness + balance tools), and the local pair (BosaTAZ portal tabs for both genuineness and cached balance) — spec deferred, design and hosting settled
- Confirm Pinata (and the self-hosted Kubo node) actually support setting/searching pin names as described, and adopt fingerprint-as-pin-name as standard practice at upload time
- Decide how many receive addresses the balance-check tool derives and checks by default (a fixed depth vs. scanning until a run of empty addresses, as the LNbits watch-only pattern does)
- Build the inward sync job populating BosaTAZ's local balance cache — what triggers it (any connectivity window vs. a manual step), and how the "as of [time]" timestamp is surfaced clearly enough that nobody mistakes a cached figure for a live one
- Sequence for eventually surfacing Type S publicly, if at all, beyond the communities already minting it
- Add real EC wallet derivation, signing, and broadcast to the deferred SETTLE App, if 5a is ever revisited — needs a properly vetted Bitcoin library, not a rushed addition
- See `type-s-workshop.md` and `type-s-ceremony.md` for training- and mint-specific open questions (entropy method, cadence, graduation, materials)
- See `type-s-equipment.md` for construction/tooling open questions (recess clearance, veneer press tooling, Kubo node setup)
