# Coin-tainer — Type T (Timelock)

*Handover document · everything needed to build, produce, and reason about Type T independently. Shared protocol material (pillars, verification website, terminal hardware) lives in `protocol-shared.md`. Where Type T's build touches shared infrastructure that Type L also depends on, see `integration.md`.*

## Status key

**COIN-TAINER PROTOCOL**

Technical Specification

Working Document · v0.1

*Type L (Lightning) and Type T (Timelock) — Integration Roadmap*

| **Status key**    | **Meaning**                                  |
|-------------------|----------------------------------------------|
| **✅ EXISTS**     | Code written and tested                      |
| **🔧 NEEDS WORK** | Code exists but requires changes             |
| **🆕 NEW BUILD**  | Does not exist yet — needs writing           |
| **⚠️ DEPENDENCY** | Blocked on another item or external decision |


---

## Product specification

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


---

**Optical Capture Shoe**

A 3D-printed accessory clips onto the rear of the Sunmi V2S. It holds a Type T coin face-up at a fixed distance below the camera lens, shielding ambient light so the built-in flash is the sole light source. This produces consistent, repeatable captures for reliable PUF matching across all environments.


---

## Life cycle

**Type T — Life Cycle**

- Minted empty — OP_RETURN birth certificate broadcast at manufacture

- Buyer deposits Bitcoin directly to coin's on-chain address

- Coin held, gifted, passed on — balance verifiable by anyone with a tap

- At block 1,951,500: sats become spendable — holder sweeps to personal wallet


---

## Manufacture and enrolment

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


---

## Build status — what exists today

**1.2 Type T — Timelock (Bitcoin on-chain)**

**● EXISTS — tested on Mutinynet signet**

Four static HTML files (mint.html, verify.html, sweep.html, index.html). Client-side only. No server. Built with bitcoinjs-lib, vanilla JS. Hosted on GitHub Pages.

| **Component**                     | **Status** | **Notes**                                                            |
|-----------------------------------|------------|----------------------------------------------------------------------|
| **Keypair generation**            | ✅         | WIF private key, compressed public key                               |
| **Miniscript descriptor**         | ✅         | wsh(and_v(v:pk(PUBKEY),after(1951500)))                              |
| **Timelocked address derivation** | ✅         | P2WSH bech32 address                                                 |
| **OP_RETURN broadcast**           | ✅         | Client-side, fee wallet required — mainnet version TBD               |
| **Balance / lock verify page**    | ✅         | mempool.space API, shows UTXOs and lock status                       |
| **Sweep tool**                    | ✅         | Builds and signs sweep tx, RBF enabled — triggers at block 1,951,500 |
| **NFC write (NTAG424)**           | 🔧         | Web NFC API used in browser — not yet tested through Bridge APK      |
| **QR data URI encoding**          | 🆕         | Not yet built — needed for coin Face B engraving                     |
| **Optical PUF enrolment**         | 🆕         | No frontend exists yet                                               |


---

## Build order — Type T-specific work

The priorities below are Type T's own build items, in their original dependency order. Two further priorities (unified routing, and the Bridge APK NFC write update) are shared with Type L and specified in `integration.md` — Type T's mint screen (below) depends on that routing and write-support work being done first.

**Priority 2 · Type T Mint Screen on the Sunmi Terminal**

**2.2 New screen: Type T coin generation and NFC write**

**● NEW BUILD**

A new screen in the merchant webapp (served by the Cloudflare Worker) that:

1.  Generates a fresh keypair client-side using bitcoinjs-lib (already bundled in the existing Type T HTML files — the same bundle can be imported or inlined)

2.  Builds the Miniscript descriptor and derives the timelocked P2WSH address

3.  Encodes the full payload (private key, address, descriptor, coin ID) as a data URI JSON

4.  Calls the Bridge APK NFC write endpoint to write the payload to the NTAG424

5.  Triggers the OP_RETURN birth certificate broadcast (see section 2.4)

6.  Displays the coin address as a QR code for the buyer to send sats to

> *The keypair generation and address derivation code from mint.html can be extracted as a shared JS module and served from the Worker — no need to rewrite it.*


**Priority 4 · OP_RETURN Birth Certificate via Lightning**

**2.4 Replace client-side fee wallet with opreturnbot.com Lightning payment**

**● NEW BUILD**

The current Type T toolkit broadcasts the OP_RETURN using a client-side fee wallet (a separate keypair with mainnet Bitcoin for fees). This is awkward on a merchant terminal. The replacement uses opreturnbot.com — a service that accepts a Lightning invoice payment and broadcasts the OP_RETURN on the holder's behalf.

**How opreturnbot.com works**

opreturnbot.com accepts a message (up to 80 bytes), generates a Lightning invoice, and broadcasts the OP_RETURN transaction once the invoice is paid. The existing Blink merchant wallet pays the invoice — no separate fee wallet needed.

| **Step** | **Action**                                                                                                 |
|----------|------------------------------------------------------------------------------------------------------------|
| **1**    | Mint screen POSTs the coin payload string (CoinID:Address, ~80 chars) to opreturnbot.com                   |
| **2**    | opreturnbot.com returns a Lightning invoice                                                                |
| **3**    | Worker calls Blink payInvoice with the returned invoice — same function already used for Type L redemption |
| **4**    | opreturnbot.com broadcasts the OP_RETURN transaction on mainnet                                            |
| **5**    | Transaction ID returned and stored in the coin's KV record                                                 |

> *opreturnbot.com is a third-party service. It has been running since ~2021 and has a Lightning node on 1ML. For a production system, self-hosting the OP_RETURN broadcast (a small Bitcoin node with a fee wallet) is the sovereign alternative. opreturnbot.com is suitable for v1 testing and early production.*
>
> *The coin ID + address string is 14 + 1 + 62 = 77 characters — within the 80-byte OP_RETURN limit.*


**Priority 5 · Type T Customer Verify Page**

**2.5 New route: /t/:uid — Timelock verify page**

**● NEW BUILD**

When a Type T coin is tapped, the holder sees a page showing the coin's on-chain status. This is a new Worker route serving an HTML page, using the mempool.space API to fetch live data.

| **Element**                 | **Data source**                      | **Notes**                                                |
|-----------------------------|--------------------------------------|----------------------------------------------------------|
| **On-chain balance (sats)** | mempool.space /address/:address/utxo | Sum of all UTXOs at the address                          |
| **Lock status**             | Current block height vs 1,951,500    | LOCKED / UNLOCKED                                        |
| **Blocks remaining**        | mempool.space /blocks/tip/height     | 1,951,500 minus current height                           |
| **Estimated unlock date**   | Blocks remaining × 10 min            | Approximate — displayed with caveat                      |
| **OP_RETURN txid**          | KV record                            | Link to mempool.space for birth certificate verification |
| **Key access routes**       | Static                               | Instructions for NFC tap, QR scan, UID entry             |
| **Coin ID**                 | KV record                            |                                                          |

The verify page does not expose the private key. Key access is explained with instructions — the key is retrieved via chip tap, QR scan, or UID entry on the sweep page.


**Priority 6 · QR Code Generation for Coin Engraving**

**2.6 Per-coin QR data URI generation and print output**

**● NEW BUILD**

Every Type T coin requires a unique QR code laser-engraved into its white plastic base. This QR encodes a data URI containing the full coin payload — no URL, no server, no external dependency.

**QR content**

> data:application/json;base64,\<base64({"id":"FF3A7C2E9B1D44","address":"bc1q...","wif":"Kx...","descriptor":"wsh(...)","txid":"..."})}

**Generation workflow**

7.  At mint time, after keypair generation and OP_RETURN broadcast, encode the payload as a base64 data URI

8.  Generate a QR code image from the data URI string using a JS QR library (qrcode.js — already available client-side)

9.  Display the QR on screen for immediate visual check

10. Send the QR image to the Bridge APK print endpoint for receipt printing — this gives the maker a paper record at manufacture

11. Store the data URI string in the KV record so it can be regenerated if needed

> *The QR code for engraving is produced separately from the minting flow — the maker uses the stored data URI to generate a high-resolution QR for laser engraving at manufacture. This can be a simple standalone HTML tool: enter coin ID, fetch payload from KV, generate print-resolution QR.*

**QR engraving toolchain**

Outside the scope of the software spec — this is a hardware/manufacturing workflow. The output needed from software is a clean, high-contrast QR image at sufficient resolution for the laser engraver (typically 300–600 DPI minimum for a ~25mm QR on a 40mm coin). A dedicated print page served by the Worker is sufficient.


**Priority 7 · Optical PUF Enrolment Screen**

**2.7 New screen: Type T optical enrolment at manufacture**

**● NEW BUILD**

After a Type T coin is physically assembled and the NFC chip written, the coin's unique surface must be photographed and enrolled. This is a new screen in the merchant webapp, used at the manufacturer's workstation (not at point of sale).

12. Operator places coin face-up in the optical capture shoe on the Sunmi V2S

13. Webapp triggers camera capture with flash via Android camera API (through a new Bridge APK endpoint, or via the browser's MediaDevices.getUserMedia API directly)

14. OpenCV.js (loaded client-side) extracts ORB feature descriptors from the captured image

15. If keypoint count ≥ 200: descriptors stored in KV alongside the coin's NFC record — enrolment complete

16. If keypoint count \< 200: coin rejected — surface insufficient for reliable PUF matching

> *OpenCV.js is a ~7MB WASM bundle. It should be cached via service worker on the Sunmi so enrolment does not require a full download each time.*

| **KV record field added at enrolment** | **Content**                                         |
|----------------------------------------|-----------------------------------------------------|
| **optical_descriptors**                | ORB feature descriptor array (JSON, ~2–10KB)        |
| **optical_keypoints**                  | Integer — keypoint count at enrolment               |
| **optical_enrolled_at**                | ISO timestamp                                       |
| **optical_image_hash**                 | SHA-256 of the enrolment image — for audit purposes |


**Priority 8 · Type T Optical Verification at Terminal**

**2.8 Dual-layer check: NFC identity + optical PUF match**

**● NEW BUILD**

For high-value Type T transactions, the merchant terminal performs a full dual-layer verification. This reuses the capture shoe and the optical pipeline from enrolment.

17. Operator taps coin — NFC read returns UID — Worker fetches KV record

18. If optical_descriptors present in record: prompt for optical verification

19. Operator slots coin into capture shoe — webapp triggers camera capture

20. OpenCV.js extracts live ORB descriptors — sends descriptor array to Worker

21. Worker runs BFMatcher comparison: live descriptors vs enrolled descriptors

22. Worker returns similarity score and pass/fail against threshold

23. Terminal displays: NFC ✓ Optical ✓ PASS — or shows which layer failed

> *The optical similarity threshold is not yet defined — must be calibrated empirically with prototype coins. The threshold lives in a Cloudflare environment variable so it can be adjusted without code deployment.*


