# Type S — The Self-Mint Ceremony

*A solo checklist. Written for one person, alone, with nobody to ask if they're doing it right — that's the whole point of the ceremony, so the instructions have to be unambiguous in a way a taught session doesn't need to be. See `type-s.md` for why this has to be solo, and `type-s-workshop.md` for the practice this checklist assumes has already happened.*

---

Adapted from the [[seedcard-protocol|btckeys]] ceremony (Sun to Sats community treasury pilot) — the equipment chain and two-checkpoint verification model are reused directly; every step originally written for a facilitator/member pair has been rewritten for one person, alone. **Design principle carried over unchanged: every step should fail loudly, not silently.** The two highest-stakes moments — transcribing drawn entropy into the checksum tool, and verifying the engraved plate — each get their own dedicated step rather than being folded into busier ones, because that's where manual ceremonies actually go wrong, not at the randomness source itself.

---

## Phase 0 — device preparation (separate, prior session, not solo)

Unlike the live mint, this can be done communally, ahead of time, and doesn't need to be repeated per-ceremony:

1. Download the official release image for your checksum/engraving device (e.g. SeedSigner) from its official source.
2. **Verify authenticity — mandatory, not optional.** Treating this as a "nice to have" is exactly what leaves the rest of the ceremony resting on an unverified assumption.
3. Flash the verified image onto your own, individually-owned microSD card.
4. Confirm it boots correctly. This card is now yours — labelled, stored, and brought to your own future ceremonies, never shared or reused across people.

---

## The minting space

For the first workshop events there is **one shared equipment set** — one pill/entropy set, one SeedSigner, one Seed Hammer — used two ways: collectively, for group practice (see `type-s-workshop.md`), and alone, for a live mint. The same physical objects serve both purposes; what changes is who's in the room.

**A live mint's entropy is not "more real" than a practice run's — the pills don't know the difference.** What makes a collective session's output unusable as a real coin is never randomness quality, it's that it was witnessed. This is worth holding onto precisely because it cuts both ways: a solo run on this same equipment is a fully valid, mintable coin the moment it's run alone, with no additional step needed to make its entropy "count."

Before entering the minting space solo, confirm the shared equipment is in a clean state — entropy source count matches the canonical total, device holds no leftover state from the last group session or prior mint. This is a continuation of the reset already built into step 13 below, worth treating as a deliberate check at entry, not just something the previous user is trusted to have done.

## Before the live ceremony

- **You have completed the practice phase** in `type-s-workshop.md` — this is not the place to learn the sequence for the first time.
- **You are alone.** No one else is in the room, watching, or able to see your screen or workspace. This is not a courtesy — it's the entire security property. If anyone else could witness any step below, stop and reschedule.
- **You have everything you need in front of you**: the shared entropy source and pill holder, blank steel plate, engraving tool, your own verified device from Phase 0, casing materials, QR printer/etcher for the xpub.
- **You know what you're minting for** — is this coin for you to hold, or are you minting on someone else's behalf? *(If the latter — stop. Re-read `type-s.md` §3. Type S cannot be pre-minted for someone else without reintroducing the exact failure this whole design avoids.)*

---

## The ceremony

1. **Open and fill.** Draw 11 words, one at a time, from the entropy source's blind draw. Seat each in the pill holder's numbered slots, word hidden. Check the source's remaining count against the canonical total before continuing.
2. **Close.** Seal the pill holder as a single unit. From this point it's the sole physical record of your 11 drawn words until you next open it.
3. **Transcribe.** Rotate the holder to reveal the words. Read each aloud to yourself and enter it into your device, in order, position by position. Rotate back to hidden once entry is confirmed complete. *This is the step that carries the most procedural weight — an error here is the one thing that can't be recovered later.*
4. **Calculate the checksum word.** Your device computes the 12th word from the 11 you entered. This word is never independently drawn — if a mistake is ever found here, it's simply recalculated; a mistake in the 11 cannot be fixed the same way, which is exactly why step 3 gets the weight it does.
5. **Engrave.** Your device displays the full 12-word phrase. Engrave it onto the steel plate.
6. **Verify plate against holder.** Rotate the holder open again. Read each engraved word aloud and check it against the holder, position by position — the second and final read-and-verify pass. Rotate back to hidden once done.
7. **Verify plate against device.** A second, independent check: confirm the engraved plate matches your device's own on-screen display of the full phrase, word for word. Distinct from step 6 — one checks against your physical entropy record, the other against the device's own computation.
8. **Derive the xpub.** Using the now-confirmed seed, derive the wallet's extended public key on the same device. Do not photograph, screenshot, or export the private key or seed at this step — only the xpub leaves this device.
9. **Mark the xpub.** Print or engrave the xpub as a QR on the vessel's exterior face.
10. **Check your own balance access.** Before sealing, scan the xpub into your own separate wallet app, on your own phone — confirming independently, on a device only you control, that you can see this wallet's balance without depending on any tool used earlier in the ceremony.
11. **Seal.** Encase the plate in the casing. This is the point of no return — from here, opening the coin again means destroying it.
12. **Check the seal.** Confirm the casing is fully intact, no visible seam, no tamper channel. Last moment to flag a problem before the coin enters circulation.
13. **Reset the entropy source.** Return the 11 pills (or equivalent) to the drum. Recheck the remaining count against the canonical total before the source is used again — by you or anyone else.
14. **Register the birth.** Add the one-line entry to your community's durable log — see `type-s.md` §4 for the exact wording. One line, once.
15. **Clear the workspace.** Destroy or securely erase every draft, note, or working file from entropy generation and engraving — the ceremony isn't finished until nothing survives outside the sealed plate that could reconstruct the seed.

---

## If something goes wrong mid-ceremony

- **Interrupted by someone else entering the room** — stop immediately. Do not seal. Destroy the plate and any written entropy; restart at a later, genuinely solo session.
- **Mismatch found at step 6 or 7, before sealing** — the plate is scrap. Start over with a fresh plate. A normal, expected cost of the process, not a failure worth routing around. (No defined partial-correction path exists — see Open Questions.)
- **Anything found wrong at step 12, after sealing** — the coin cannot be trusted. Treat it as void and never enter it into circulation, even though the seal is now broken to check. Register nothing for it at step 14.

---

## Risk register

### Resolved
- **Plate loss, no backup.** By design, not an oversight: single-sig, no backup. A lost or destroyed coin is a bounded, expected loss — treated like losing cash — not a catastrophic single point of failure, provided balances are kept low relative to what any one person could tolerate losing. Worth periodically re-checking this ceiling still holds as coins circulate longer and accumulate more through repeated top-ups.
- **Device software/firmware provenance.** Resolved via Phase 0 above — verified once, individually, per person, rather than trusted fresh at every ceremony.

### Still open
- **Ceremony environment control.** No defined check that the room is actually free of cameras or observation before starting — "you are alone" above is a stated precondition, not something the ceremony verifies.
- **No error-handling / abort procedure beyond "start over."** For repeated use across many people over time, a mismatch is a "when," not an "if" — worth a more defined response than blanket restart, especially for step 6/7 mismatches specifically.
- **No recurring integrity check on the entropy source itself** — periodic verification that a shared drum/pill set is complete, undamaged, and correctly matched to the canonical wordlist has no defined cadence yet.
- **No duress/coercion consideration** — the ceremony has no duress-resistant option built in.

---

## Open Questions

- Confirm Entropia word pills + lottery drum as Type S's entropy method (following btckeys' own comparison against dice), or document a different mechanical source if Type S's format needs diverge
- Engraving tool and technique — what's actually usable by an individual alone, versus what needs Phase-0-style shared equipment prepared in advance
- Casing/sealing process solo-executable with the current shared equipment, or whether it needs its own separate setup
- Exact destruction method for step 15 (shredding, burning, secure wipe) worth specifying rather than left to individual judgement
- Booking/scheduling protocol for solo access to the single shared equipment set, once workshop events grow beyond a size where informal turn-taking still works
