# Type S — The Workshop

*How the skill gets built. The live mint is always solo (see `type-s-ceremony.md`), but mastery — the thing that eventually lets a maker's doubt disappear entirely — can only be built communally, through repetition and people watching each other practice. This document is the training; the ceremony document is the exam nobody grades. Philosophical grounding for why this split exists at all lives in [[anyone-can-make-this-money]].*

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

## Open Questions

- Cadence — how often the workshop runs, and how many people constitute a working session
- Whether any informal mentor/most-experienced role exists, or the group stays genuinely flat — leaning toward flat, consistent with "no intermediary," but worth deciding explicitly rather than by default
- Handoff protocol between collective sessions and someone entering the minting space solo — the equipment needs to move from shared/group use to a genuinely solitary context; see `type-s-ceremony.md`'s equipment-reset note
- Whether a session's practice plates get destroyed after use or kept as reference material for the group
