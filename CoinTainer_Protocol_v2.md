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

**IV. Type L — The Circulation Instrument**

> **TYPE L · LIGHTNING**

Type L is the everyday object. It circulates within a community, moves between hands, and is designed to be spent. Its simplicity is deliberate — it carries value without ceremony.

**Physical Construction**

| **Attribute**      | **Specification**                                                                 |
|--------------------|-----------------------------------------------------------------------------------|
| **Material**       | Single-colour injection-moulded recycled plastic. Uniform colour signals utility. |
| **Diameter**       | 40mm — standard bullion coin diameter                                             |
| **NFC chip**       | NTAG213, surface-mounted in rear recess                                           |
| **Face A (front)** | Clean recycled plastic surface. No markings.                                      |
| **Face B (rear)**  | NFC chip recess. No other markings.                                               |
| **Rim**            | Stainless steel                                                                   |
| **Optical PUF**    | Not applicable — coin is not designed to be held long-term                        |
| **Denomination**   | None marked. Value held in digital ledger.                                        |

**How It Works**

- Coin is sold empty by the maker

- Merchant loads sats via Lightning invoice on the Sunmi V2S terminal — NFC chip is written with a redemption URL

- Coin enters circulation as a physical Lightning bearer instrument

- Holder taps any NFC-capable phone — browser opens showing denomination and redemption option

- Redemption: holder taps coin to Lightning wallet — sats pay out instantly — coin returns to inactive in ledger

- Coin can be reloaded and recirculated

**Verification**

Tap the coin to any NFC-enabled phone. No app required. The browser opens a verification page showing current value, status, and redemption option. The NTAG213 chip UID is the coin's sole identifier — looked up against the Cloudflare KV ledger in real time.

**V. Type T — The Preservation Instrument**

> **TYPE T · TIMELOCK**

Type T is the heirloom object. It holds Bitcoin locked by consensus until block 1,951,500 — approximately 22 May 2046, the 36th anniversary of Bitcoin Pizza Day, the first real-world Bitcoin transaction. It is a 21-year time capsule. It is not designed to be spent soon. It is designed to last.

**Physical Construction**

| **Attribute**      | **Specification**                                                                                                                  |
|--------------------|------------------------------------------------------------------------------------------------------------------------------------|
| **Top half**       | Multi-colour mottled recycled plastic. Unique surface per coin — no two identical. This is the optical PUF face.                   |
| **Bottom half**    | White plastic. Houses the QR code insert.                                                                                          |
| **Internal layer** | NTAG424 PET sticker sandwiched between the two halves at the bond line — invisible externally, physically protected.               |
| **Rim**            | Stainless steel. UID laser-engraved on outer circumference.                                                                        |
| **Face A (front)** | Mottled recycled plastic surface. No markings. The surface pattern is the only feature.                                            |
| **Face B (rear)**  | QR code insert (resin-filled laser engraving, black on white). Mintmark centred below QR.                                          |
| **QR code**        | Data URI encoding the full coin payload — private key, address, descriptor, OP_RETURN txid. No external URL. No server dependency. |
| **Mintmark**       | Maker's mark, laser engraved on Face B below QR insert. Small, centred, subordinate.                                               |
| **Denomination**   | None marked. Value is what the holder places on-chain.                                                                             |

**The Timelock**

Each Type T coin contains a unique Bitcoin keypair. The private key is written to the NFC chip. The Bitcoin address is a timelocked P2WSH output — the sats sent to it cannot be spent until block 1,951,500.

The private key is stored openly because the timelock makes it safe. Security derives entirely from Bitcoin consensus, not from key secrecy. This is not a flaw — it is the architecture. The coin is the key, visibly.

| **Attribute**                  | **Specification**                                                                               |
|--------------------------------|-------------------------------------------------------------------------------------------------|
| **Lock mechanism**             | OP_CHECKLOCKTIMEVERIFY (BIP-65)                                                                 |
| **Miniscript descriptor**      | wsh(and_v(v:pk(PUBKEY),after(1951500)))                                                         |
| **Target block**               | 1,951,500                                                                                       |
| **Target date**                | ~22 May 2046 — Bitcoin Pizza Day, 36th anniversary                                              |
| **Lock duration**              | ~21 years from minting                                                                          |
| **On-chain birth certificate** | OP_RETURN transaction at mint — records coin ID and address immutably on the Bitcoin blockchain |

**How It Works**

- Coin is sold empty by the maker — no sats loaded at point of sale

- Buyer sends Bitcoin directly to the coin's on-chain address

- Multiple deposits are possible — all UTXOs at the address are locked equally until block 1,951,500

- Coin circulates or is held — tapping opens the verification page showing balance and lock status

- At block 1,951,500: holder uses the private key (from chip, QR, or web) to sweep all sats to their wallet

**Private Key Access — Three Independent Routes**

The private key is accessible through three independent methods. None are controlled by the maker. Any one of the three is sufficient to sweep the coin in 2046.

| **Route**        | **Method**                                                                                 | **Notes**                                                            |
|------------------|--------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| **1. NFC tap**   | Tap coin to NFC-capable phone — chip payload includes private key, address, and descriptor | Requires chip to still be functional                                 |
| **2. QR scan**   | Scan QR code on Face B — data URI decodes full coin payload locally, no network required   | No server, no URL, no dependency — data encoded directly in the mark |
| **3. Web entry** | Enter UID on verification website — full payload available on the coin's web record        | UID visible on rim if chip fails — fallback to manual entry          |

**Optical PUF — The Provenance Record**

Every Type T coin's unique mottled surface is enrolled as a Physical Unclonable Function at manufacture. The random microstructure of the recycled plastic is photographed, feature descriptors are extracted, and stored in the verification database — permanently linked to the coin's NFC UID.

The optical database is the physical analogue of the blockchain — a permanent, queryable record of every coin's identity. It serves two purposes: anti-counterfeiting, and provenance. A Type T coin can be verified as genuine and as a specific, unique object — like a serial number, but unclonable.

- Verification: camera capture of Face A surface, compared against enrolled descriptors using ORB feature matching

- Minimum 200 keypoints required at enrolment — coins below threshold rejected

- Optical similarity threshold set empirically during prototype testing

- Dual-layer check: NFC identity + optical surface must both pass for full verification

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

**Optical Capture Shoe**

A 3D-printed accessory clips onto the rear of the Sunmi V2S. It holds a Type T coin face-up at a fixed distance below the camera lens, shielding ambient light so the built-in flash is the sole light source. This produces consistent, repeatable captures for reliable PUF matching across all environments.

**VII. The Life Cycle of Value**

Under this protocol, value can never be subtracted by an external authority through inflation. Each coin type has its own life cycle, but both share the same three terminal fates.

**Type L — Life Cycle**

- Minted empty — no value until loaded

- Loaded at merchant terminal via Lightning — sats arrive, chip written, coin active

- Circulates as physical Lightning bearer instrument

- Redeemed: holder taps coin to Lightning wallet — sats paid out — coin inactive

- Reloaded and recirculated, or retired

**Type T — Life Cycle**

- Minted empty — OP_RETURN birth certificate broadcast at manufacture

- Buyer deposits Bitcoin directly to coin's on-chain address

- Coin held, gifted, passed on — balance verifiable by anyone with a tap

- At block 1,951,500: sats become spendable — holder sweeps to personal wallet

**The Three Terminal Fates — Both Types**

**Utilisation**

The physical exchange of the bearer instrument, or the return of value to the network. For Type L: Lightning redemption. For Type T: the sweep at block 1,951,500.

**Destruction**

The obliteration of the container. The sats become permanently inaccessible, removed from the effective supply. A deflationary event.

**Loss**

The digital shipwreck. The coin exists but cannot be reached — lost at sea, buried, forgotten. The value remains part of the 21-million-unit truth, silently. For Type T, loss before 2046 is reversible if any one of the three key access routes survives.

**VIII. Manufacture and Enrolment**

**Type L Production**

- Single-colour injection-moulded recycled plastic disc

- Stainless steel rim fitted

- NTAG213 sticker mounted in rear recess

- Chip UID recorded — coin initialised as inactive in Cloudflare KV ledger

- No optical enrolment required

**Type T Production**

- Top half: multi-colour mottled recycled plastic — mixed feedstock, unique surface per coin

- Bottom half: white plastic — deep-engraved QR code, filled with black resin

- NTAG424 PET sticker placed on bottom half before bonding

- Two halves bonded — sticker sandwiched, invisible externally

- Stainless steel rim fitted — UID laser engraved on outer circumference

- Mintmark laser engraved on Face B below QR

**Type T Enrolment**

Every Type T coin must be enrolled before leaving the manufacturer. Enrolment binds the physical object irrevocably to its digital and on-chain record.

- Generate unique keypair — private key in WIF format

- Build Miniscript descriptor: wsh(and_v(v:pk(PUBKEY),after(1951500)))

- Derive timelocked Bitcoin address

- Broadcast OP_RETURN transaction: payload is CoinID:Address — immutable on-chain birth certificate

- Encode full payload as data URI — write to QR engraving and NTAG424 chip

- Photograph Face A with flash — extract ORB feature descriptors — store in KV ledger

- Minimum 200 keypoints required — coins below threshold rejected

- Coin initialised as inactive — no sats loaded

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
