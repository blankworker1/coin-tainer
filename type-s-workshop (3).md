# Self-Mint — The Workshop

*How the skill gets built. The live mint is always solo (see `type-s-ceremony.md`), but mastery — the thing that eventually lets a maker's doubt disappear entirely — can only be built communally, through repetition and people watching each other practice. This document is the training; the ceremony document is the exam nobody grades. Philosophical grounding for why this split exists at all lives in [[anyone-can-mint-this-money]].*

## Positioning

Three names, three layers, not one project wearing three hats:

- **Slow Money** — the guiding philosophy. Patient, local, quality over scale — see [[anyone-can-mint-this-money]].
- **Self-Mint** — the course and the workshop. What this document, and `type-s-ceremony.md`, actually teach and run.
- **Coin-tainer Type S** — the handmade object that results. Specified in `type-s.md` and `type-s-equipment.md`.

Someone can encounter any one of these without the others — a person might learn about Slow Money with no idea Type S exists, or hold a Type S coin with no idea it came out of a course called Self-Mint. That's intentional, not a naming gap to close.

**Definition.** A Self-Mint is a place where people can come to learn and mint their own money. Both halves have to be true — a location with the right equipment but no one training there isn't one; a course taught with nothing to actually mint on isn't one either.

**The minimum for the name.** No charter, no certification, no central registry of instances — the only requirement to call a place a Self-Mint is holding the equipment specified in `type-s-equipment.md`. That's it, and it's checkable by looking, not by asking anyone's permission. Anything beyond that (cadence, group size, how graduation is judged) is local, per-instance, and free to vary — see Open Questions and Graduation below for how the first one has chosen to run it, not a rule the next one has to inherit.

---

## What the workshop is for

Self-mint removes doubt for the maker alone. It says nothing about how confidently anyone *else* should treat a coin they didn't personally seal. That confidence is built the way any craft's reputation is built — organically, through people who've watched each other work over time, not through a certificate. The workshop is where that happens: a shared place, a known circle, repeated practice, until "I know this person's hand" is something people can actually say about each other, first-hand.

No one is expected to get this right on their first attempt — the same way no one expects to throw a good vase on their first day at the wheel.

---

## Equipment for the first workshop events

One shared set, full stop: one pill/entropy set, one SeedSigner (or equivalent checksum device), one Seed Hammer (or equivalent engraver). Test runs are run **collectively** — the group works through the ceremony sequence together, on this shared equipment, one member at a time, everyone else present and watching.

**This means test-run entropy is not weaker or fake — it's identical in quality to a live mint's.** The pill set doesn't know it's a practice draw. What makes a session's output unusable as a real coin is never the randomness — it's that the draw was witnessed. Self-mint requires solitude by rule, not because witnessed entropy is somehow compromised. Keep this distinction explicit: **no phrase generated during a collective session is ever funded or sealed into real casing**, regardless of how clean the run was. The plate/vessel material used in group sessions should still be cheap and clearly distinct from a real casing — not because the entropy inside is lesser, but so nothing produced in the room could be mistaken for, or repurposed as, a real coin later.

## What happens in a session

- **Practice runs of the full ceremony sequence** — generate, engrave, seal — on the shared equipment above, start to finish, matching the steps in `type-s-ceremony.md`.
- **People watch each other work.** This is the one place observation is not just permitted but the point — the opposite condition from the live ceremony, which has to be unwitnessed. What's being built here is familiarity with technique: engraving legibility, entropy-method discipline, steadiness under the actual physical process.
- **Open discussion of mistakes.** A practice run that fails checksum, or an engraving error caught late, is useful material for the group, not something to hide — this is where those failure modes get felt once, cheaply, before they can happen for real.

---

## How reputation actually forms here

There's no test to pass and no certificate issued. What accumulates instead is the same thing any workshop produces: people who've trained alongside someone long enough to trust their hand, based on having actually seen it, repeatedly, not on a claim about it. This reputation is real precisely because it's personal and direct — and precisely because of that, it doesn't transfer well to someone who's never set foot in the room. That's a feature, not a gap; see [[anyone-can-make-this-money]] on why this can't scale into something more formal without quietly reintroducing the trust problem self-mint exists to avoid.

The protocol's own engineering (the seal, the xpub, the birth-log entry) is meant to carry most of the weight regardless of who made a given coin — reputation only has to close the small remainder even the strictest observer would still want reassurance on.

---

## Graduation

No exam, no formal sign-off. When a member feels ready, they stop practising collectively and **enter the minting space on their own** — the same shared equipment (pill set, SeedSigner, Seed Hammer), used alone, for the live ceremony in `type-s-ceremony.md`. The decision to make that move is entirely self-assessed; informally, it's also shaped by having trained alongside people who've watched them practice, without that ever being a formal sign-off from anyone.

---

## Growth model

One open-source protocol, one free course, one workshop as a physical place — and, over time, more workshops, each independent. **No service, no seller, anywhere in this.**

- **The workshop lends tools, never supplies materials.** A plate blank, casing feedstock, anything that becomes part of a finished coin — always brought by the minter themselves, never provided by the workshop. This is the line that keeps every interaction inside the workshop's walls a "person uses a shared tool," never a "person receives a thing of value from an operator" — the same boundary that keeps Type S outside money-transmission territory in the first place, held at the materials level too.
- **Repeat and replicate, not franchise.** A second workshop, wherever it starts, runs the same protocol under its own steam — its own tools, its own group, no fee back to anywhere, no central approval needed. The precedent for this working cleanly at scale, for decades, without anyone needing to untangle liability between instances: the Fab Lab network, and Slow Food's own thousands of independent local chapters. Protocol scales; instances don't answer to anything above them.
- **Friends and family first.** Growth is expected to be slow and mostly invisible from outside — consistent with everything [[anyone-can-mint-this-money]] argues about why this can't and shouldn't scale like a product.

---

## Open Questions

- Cadence — how often the workshop runs, and how many people constitute a working session
- Whether any informal mentor/most-experienced role exists, or the group stays genuinely flat — leaning toward flat, consistent with "no intermediary," but worth deciding explicitly rather than by default
- Handoff protocol between collective sessions and someone entering the minting space solo — the equipment needs to move from shared/group use to a genuinely solitary context; see `type-s-ceremony.md`'s equipment-reset note and `type-s-equipment.md` for the full equipment set and its ownership model
- Whether a session's practice plates get destroyed after use or kept as reference material for the group
