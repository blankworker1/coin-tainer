# Coin-tainer — Type S

*Self-mint. Single-sig. Sealed.*

A third credential/toolchain variant alongside **Type T** (timelocked, NTAG424) and **Type L** (Lightning, NTAG424 + Blink API). Where T and L use an NFC chip as the credential, **Type S uses a BIP39 plate** — spun out of BOX's original technical Bitcoin layer once BOX itself was cut back to a pure barter platform. The philosophical case for this design — self-mint as a third rung beyond self-custody, mastery, Slow Money, the workshop — lives in its own document, [[anyone-can-make-this-money]]. This file is the protocol spec.

A Type S coin doesn't need BOX to exist. A BOX locker is one possible place to store or settle one, exactly as incidental as a pocket or a safe — never a dependency.

---

## 1. The object

- A steel plate, engraved with a single, independently-generated 12-word BIP39 seed — one wallet, never a sub-account, never reused across owners.
- Fully encased in **injection-molded, multi-color waste plastic**. The turbulent mixing of recycled material at injection produces a genuinely unique, non-repeating marbled pattern per unit — real physical entropy, not decorative uniqueness, and it uses waste material in keeping with the ecological grounding running through the rest of this work.
- The wallet's **xpub is printed on the outside** as a QR — watch-only, cannot spend, visible without breaking the seal.

Not a demo tier — the true bearer-object standard, the physical peer to a Casascius coin.

---

## 2. What the seal actually does — a physical property, not a cryptographic one

Bitcoin has no concept of a deposit-only address; anyone holding the 12 words can always spend. "Value can only be added, not withdrawn" holds only because withdrawal requires the seed, and the seed is physically inaccessible while the casing is intact. Full encasement makes this genuinely destructive to defeat — no seam to lift, no coating to peel — unlike a dip or hologram coating, which can in principle be removed and reapplied.

The owner retains the right to break their own seal and sweep the funds to their own wallet at any time; the seal is a convenience and a ritual, never a restriction on the legitimate holder's sovereignty.

**Stated precisely, a sealed coin admits exactly two operations to anyone in the community, and one further operation reserved to a single person:**
- **Increase** — anyone, at any time, can send more sats to the wallet the xpub belongs to. Permissionless, requires nothing from the coin's current holder.
- **Exchange without increasing** — the sealed coin can change hands at its current attributed value with no Bitcoin transaction at all; only physical possession moves, the on-chain balance doesn't.
- **Decrease** — reserved entirely to whoever currently holds the coin, and only by breaking their own seal. No one else, at any point in the coin's circulation, can reduce its balance.

---

## 3. The self-mint rule — learned from Casascius's one real failure

Casascius coins used the same public-address / hidden-key shape, and the only thing that ever undermined trust in them was that they were minted centrally — for a moment, the minter held every key, and trust rested on believing they'd destroyed their copies.

Type S avoids this by requiring the eventual owner to generate the entropy, engrave the plate, and seal it themselves, in one uninterrupted session, with nobody else present. **No pre-minting a batch of sealed coins for convenience** — every coin is minted only at the moment someone takes possession of it. This mirrors the same self-sovereign generation principle already used in [[sd-card-ceremony]], applied to a plate instead of a microSD card.

A practice phase — the whole generate/engrave/seal sequence run on a scrap plate with worthless test entropy — should precede anyone's first live mint. Learner's permit, then the real thing. See [[anyone-can-make-this-money]] for why this matters beyond due diligence: mastery, not just correctness, is what eventually lets the maker's doubt disappear entirely.

---

## 4. Recording the birth — one line in a log, not a gallery

No dedicated infrastructure, no website, no relay. The precedent this borrows: [[il-custode]]'s chain object is placed on its plinth once, by its first Donatore, and the Remembrancer records its form at that moment — a single, durable entry marking that it now exists. A coin's birth is the same shape of fact, registered the same way, in whatever durable log a given mint's community already keeps (BosaNode's Remembrancer log, for instance, if minted at a Prova):

> *"[Nome] ha coniato una moneta, [breve descrizione della colata], oggi."*

This is **not a transaction** — a promise or a trade needs two parties and a relationship between them; a birth has only one. It's a registration: this now exists, made by this person. **One entry, once, at mint. Nothing further is tracked.**

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

The xpub gives risk-free **balance** auditing. It does not protect against the seed having been **copied before or during minting** — physical settlement proves nobody else can *open the seal* afterward, not that no copy was retained. This is the same limit every bearer seed instrument carries (see [[coin-tainer]]) — Bitcoin custody is defined by knowledge of the secret, not possession of any one physical object. At small-community scale this rests on the same character-based trust [[anyone-can-make-this-money]] argues is the actual foundation, not a gap the protocol failed to close.

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

- Source the injection-molding process and multi-color waste plastic feedstock for the casing; prototype and stress-test tamper-evidence before trusting it with real value
- Confirm the exact birth-entry wording with whoever maintains a given community's durable log (e.g. BosaNode's Remembrancer), and how/whether it's visually distinguished from other entry types
- Write the self-mint ceremony script itself — a checklist for the moment of generating, engraving, and sealing a coin, one owner alone, including the practice-phase/learner's-permit step
- Sequence for eventually surfacing Type S publicly, if at all, beyond the communities already minting it
- Add real EC wallet derivation, signing, and broadcast to the deferred SETTLE App, if 5a is ever revisited — needs a properly vetted Bitcoin library, not a rushed addition
