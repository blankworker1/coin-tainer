# Coin-tainer Protocol — Shared Foundations

*Handover document · covers material common to both Type L and Type T. For type-specific construction and build status, see `type-l.md` and `type-t.md`. For the work that connects the two systems, see `integration.md`.*

---

**THE COIN-TAINER PROTOCOL**

*A Manifesto for the Return to Reality in Value*

v2.0 · Physical Product Specification

VERITAS IN NUMERIS

**I. Introduction**

Throughout history, physical money has served as a container of value. The maker imposed a denomination, but the user decided purchasing power through collective trust. In the digital age we have departed from this reality — trading physical labour for abstract numbers on a screen controlled by central authorities.

The Coin-tainer Protocol restores the continuity of belief by tethering digital scarcity to physical autonomy. The container is the bridge between the human mind and the physical world.

This document defines the complete product specification for two physical Bitcoin bearer instruments produced under the protocol: Type L and Type T.

**II. One Protocol, Two Tiers**

The Coin-tainer Protocol produces two distinct physical objects. They share a material philosophy, a maker ethos, and a verification platform. They differ in role, lifespan, and the relationship they create between the holder and Bitcoin.

The analogy is precise: Type L is the coin; Type T is the note. In traditional currency, coins circulate and notes are stored. The same logic applies here — one object is made to move, the other is made to last.

|                      | **Type L — Lightning**          | **Type T — Timelock**               |
|----------------------|---------------------------------|-------------------------------------|
| **Role**             | Circulation instrument          | Preservation instrument             |
| **Analogy**          | The coin                        | The note                            |
| **Bitcoin layer**    | Lightning Network               | Bitcoin base chain                  |
| **Spendable**        | Immediately                     | From block 1,951,500 (~22 May 2046) |
| **NFC chip**         | NTAG213                         | NTAG424 (50-year data retention)    |
| **Plastic**          | Single-colour recycled          | Multi-colour mottled recycled       |
| **Optical PUF**      | No                              | Yes                                 |
| **Sold**             | Empty — loaded at point of sale | Empty — buyer loads sats directly   |
| **Interoperability** | Cannot receive on-chain sats    | Cannot receive Lightning sats       |


**III. The Pillars of the Protocol**

**1. Finite Digital Scarcity**

The protocol utilises Bitcoin — a decentralised, proof-of-work ledger with a total supply capped at 21,000,000 units. This digital matter requires energy to create. The nothing is a representation of real-world work.

**2. Anti-Seigniorage**

The maker of the physical coin adds zero value. The maker is a craftsman providing a vessel for a fee. Only the user can infuse the container with value. This reverses the traditional power dynamic where states profit from the issuance of currency.

**3. The Physical Mirror**

The coin is a physical bearer instrument that reflects human existence. A tool for the sovereign individual to manifest stored energy in a form that requires no permission to hold, move, or verify.

**4. Material Honesty**

The substrate carries meaning. The Coin-tainer Protocol uses reclaimed waste plastic — a material society has written off. Transforming it into a bearer instrument for the hardest money ever created is the message of the object. The waste is the point.


---

**VI. Shared Infrastructure**

Both coin types are served by the same verification platform. A holder tapping either type is taken to a webpage appropriate to that coin. The experience is the same; the content differs.


**Verification Website**

- Type L tap: shows denomination, Lightning redemption option, ledger status

- Type T tap: shows on-chain balance, lock status, block countdown to 1,951,500, key access routes

- Manual UID entry: accessible on both — allows verification when chip is unresponsive

- No app required. No account required. Any NFC-capable phone.


**Merchant Terminal — Sunmi V2S**

The Sunmi V2S Android point-of-sale device handles Type L minting, loading, and redemption. It is also used for Type T optical verification via the capture shoe.

| **Attribute**       | **Specification**                                       |
|---------------------|---------------------------------------------------------|
| **Rear camera**     | 13MP with autofocus and flash LED — optical PUF capture |
| **NFC**             | ISO 14443 A/B — reads both NTAG213 and NTAG424          |
| **OS**              | Android 11                                              |
| **Role for Type L** | Mint, load, and redeem Lightning coins                  |
| **Role for Type T** | Optical PUF verification using capture shoe             |


*(The optical capture shoe accessory, used only for Type T enrolment and verification, is specified in `type-t.md`.)*

---

**VII. The Life Cycle of Value**

Under this protocol, value can never be subtracted by an external authority through inflation. Each coin type has its own life cycle, but both share the same three terminal fates.


**The Three Terminal Fates — Both Types**

**Utilisation**

The physical exchange of the bearer instrument, or the return of value to the network. For Type L: Lightning redemption. For Type T: the sweep at block 1,951,500.

**Destruction**

The obliteration of the container. The sats become permanently inaccessible, removed from the effective supply. A deflationary event.

**Loss**

The digital shipwreck. The coin exists but cannot be reached — lost at sea, buried, forgotten. The value remains part of the 21-million-unit truth, silently. For Type T, loss before 2046 is reversible if any one of the three key access routes survives.


*(Type L's and Type T's individual life cycles are specified in `type-l.md` and `type-t.md` respectively.)*

---

**IX. The Mirror**

At its deepest level, the Coin-tainer is a mirror. Identity is based on what we see in the mirror. When money is distorted, our perception of our labour and time is distorted. A protocol that cannot be manipulated creates a perfect economic reflection.

Type L reflects the present — the everyday exchange of stored energy between people who trust each other enough to transact. Type T reflects time itself — a commitment to the future, a 21-year wager that the value of work today will be honoured in 2046.

Both are made from waste — material that society discarded. Both hold Bitcoin — the only money whose supply cannot be inflated by a decision in a meeting room. The substrate and the contents mirror each other: written off, and then not.

The protocol asks for only one thing: understanding. Once the user understands that value is an idea made manifest through energy and stored in a physical container they control, the departure from reality ends.

**Document Status**

| **Attribute** | **Specification**                                                                                                              |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| **Version**   | 2.0                                                                                                                            |
| **Status**    | Draft — Product Specification                                                                                                  |
| **Type L**    | Architecture resolved. Technical specification to follow.                                                                      |
| **Type T**    | Architecture resolved. Technical specification to follow.                                                                      |
| **Pending**   | Optical similarity threshold (empirical calibration during prototype testing). Sweep tool specification. Webapp specification. |

**VERITAS IN NUMERIS**

