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

**1. What Exists Today**

Two independent systems have been built and tested. Neither knows the other exists yet. The work in this spec is connecting them.

**1.1 Type L — SatsCASH (Lightning)**

**● EXISTS — production-tested end to end**

A Cloudflare Worker (~3,300 lines) serving both API and merchant webapp HTML. Built and tested on the Sunmi V2S with the Bridge APK.

| **Component**                    | **Status** | **Notes**                                                        |
|----------------------------------|------------|------------------------------------------------------------------|
| **Denomination system**          | ✅         | red/blue/green/white/black — 5k/10k/50k/100k/500k sats           |
| **Mint screen (Sunmi)**          | ✅         | Colour select → NFC poll via Bridge → NDEF write → success       |
| **Redeem screen (Sunmi)**        | ✅         | NFC poll → LNURL QR generated → Lightning payout via Blink       |
| **Customer verify page /c/:uid** | ✅         | Tap any phone → denomination, status, redeem option              |
| **Dashboard**                    | ✅         | Treasury balance, solvency, coin inventory by colour             |
| **Blink API / payInvoice**       | ✅         | LNURL encoder and payInvoice debugged and working — DO NOT TOUCH |
| **Bridge APK (Android/Kotlin)**  | ✅         | NFC poll, NDEF write, receipt print on Sunmi V2S                 |
| **Cloudflare KV ledger**         | ✅         | Coin records keyed by NFC UID                                    |

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

**2. Priority Build Order**

Ordered by dependency and critical path. Items 1–4 are needed before any Type T coin can be minted on the Sunmi. Items 5–6 are needed before coins can be sold. Items 7–8 complete the full verification stack.

**Priority 1 · Unified Routing — One Tap, Two Paths**

**2.1 UID-based coin type detection**

**● NEW BUILD**

A tap on either coin type hits the same domain. The server must route to the correct verify page based on chip type. The NFC UID prefix is the discriminator.

| **UID prefix**       | **Chip**                         | **Route to**                    |
|----------------------|----------------------------------|---------------------------------|
| **FF (7 hex chars)** | NTAG213 — Type L v1 software UID | /c/:uid → Lightning verify page |
| **04 (7 hex chars)** | NTAG424 — Type T v2 hardware UID | /t/:uid → Timelock verify page  |

Change required in the Cloudflare Worker routing logic: inspect the first two characters of the UID on the /c/ route — if '04', redirect to /t/:uid. Alternatively, keep routes separate from the start and write the correct URL to each chip at manufacture.

> *Recommended: write /t/:uid directly to NTAG424 at enrolment rather than relying on redirect. Cleaner and avoids a round-trip.*

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

**Priority 3 · Bridge APK — NTAG424 Write Support**

**2.3 Update NfcManager.kt to handle NTAG424 payload size**

**● NEEDS WORK**

The existing Bridge APK NFC write endpoint (/nfc/write) writes a simple NDEF URL to NTAG213 tags. NTAG424 requires the same NDEF write but with a larger payload and AES-128 authentication for full chip security features.

**Payload size check**

The Type T payload contains: private key (52 chars WIF) + address (~62 chars) + descriptor (~120 chars) + coin ID (14 chars) + JSON wrapper ≈ 320 bytes. NTAG424 NDEF capacity is 412 bytes. Fits with ~90 bytes to spare.

| **Item**                           | **Size**                                   |
|------------------------------------|--------------------------------------------|
| **WIF private key**                | ~52 chars                                  |
| **Bech32 address**                 | ~62 chars                                  |
| **Miniscript descriptor**          | ~120 chars                                 |
| **Coin ID (hex)**                  | 14 chars                                   |
| **JSON wrapper + data URI prefix** | ~60 chars                                  |
| **Total estimate**                 | ~308 chars — within 412 byte NDEF capacity |

**NTAG424 tooling**

Two Node.js libraries are available for NTAG424 interaction:

- ntag424 (npm: nikeee/node-ntag424) — Node.js, AGPL licensed, tested with Alcor Micro AU9540 reader, supports AES-128 authentication and NDEF write

- ntag424-js (MxAshUp) — designed for backend verification of SUN messages and chip personalisation

For the Bridge APK (Android/Kotlin), the standard Android NFC stack (NfcAdapter, IsoDep, APDU commands) can write NDEF to NTAG424 without a third-party library — NXP publishes the APDU command set. The existing NfcManager.kt already uses IsoDep for NTAG213. Extending it to NTAG424 requires adding AES-128 authentication before the NDEF write.

> *Decision needed: use full NTAG424 AES security features (SDM/SUN — adds server-side key management complexity) or write plain NDEF only (simpler, sufficient for this use case since security is the timelock not the chip). Recommend plain NDEF for v1 — AES features can be added in v2 if needed.*

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

**3. Architecture Overview**

After all eight priorities are complete, the system looks like this:

| **Layer**    | **Component**          | **Type L**                       | **Type T**                       |
|--------------|------------------------|----------------------------------|----------------------------------|
| **Hardware** | Sunmi V2S terminal     | Mint, load, redeem               | Mint, enrol, verify              |
| **Hardware** | Bridge APK             | NFC write (NTAG213), print       | NFC write (NTAG424), camera      |
| **Hardware** | Optical capture shoe   | Not used                         | Enrolment + verification         |
| **Network**  | Cloudflare Worker      | API + webapp HTML                | API + webapp HTML                |
| **Network**  | Cloudflare KV          | Coin records (UID → status/sats) | Coin records (UID → payload/PUF) |
| **Network**  | Blink Lightning wallet | Fund treasury, pay redemptions   | Pay OP_RETURN invoice            |
| **Network**  | opreturnbot.com        | Not used                         | OP_RETURN broadcast (v1)         |
| **Network**  | mempool.space API      | Not used                         | Balance + lock status            |
| **On-chain** | Bitcoin base chain     | Not used                         | Timelocked address, OP_RETURN    |
| **On-chain** | Lightning Network      | All coin value flows             | OP_RETURN fee only               |

**4. Open Decisions**

Items that need a decision before or during build. Flagged here to avoid assumptions in code.

| **\#** | **Decision**                 | **Options**                                                                               | **Recommendation**                                                                                       |
|--------|------------------------------|-------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| **1**  | NTAG424 AES security         | Plain NDEF write only vs full AES-128 SDM authentication                                  | Plain NDEF for v1 — simpler, no server-side key management needed                                        |
| **2**  | OP_RETURN service            | opreturnbot.com (third-party) vs self-hosted Bitcoin node                                 | opreturnbot.com for v1 — sovereign option documented for v2                                              |
| **3**  | Optical similarity threshold | Empirical calibration required on prototype coins                                         | Cannot be set until prototype testing — leave as env variable                                            |
| **4**  | KV vs SQLite node            | Cloudflare KV (current) vs PlasticCoin Core Node (Express/SQLite, partially built)        | Stay on KV for v1 — node migration is v2                                                                 |
| **5**  | Camera API on Sunmi          | Bridge APK camera endpoint (new) vs browser MediaDevices API (existing)                   | Test browser API first — if insufficient quality, add Bridge endpoint                                    |
| **6**  | opreturnbot.com reliability  | Service has been running ~4 years but is third-party — what if it goes down at mint time? | Retry logic in Worker — queue failed OP_RETURNs and retry. Birth cert is non-blocking for coin function. |

**5. What Must Not Be Touched**

These components are working and debugged. Any integration work must route around them, not through them.

| **Component**                                     | **Why it must not change**                                                                                                                  |
|---------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| **Blink LNURL encoder**                           | Took significant debugging to get Blink API and wallet to accept this — any change risks breaking Type L redemption                         |
| **payInvoice function**                           | Same — live and tested. New uses (paying opreturnbot.com invoice) should call this function, not replace it                                 |
| **NTAG213 NDEF write flow**                       | Type L minting is working end to end — Bridge APK changes for NTAG424 must not affect the NTAG213 path                                      |
| **Denomination map (red/blue/green/white/black)** | Set at protocol level — only changes annually on Pizza Day                                                                                  |
| **KV record schema for Type L coins**             | Dashboard, verify page, and redeem flow all depend on the existing schema — Type T records should use a separate KV namespace or key prefix |

**6. Immediate Next Steps**

Suggested starting point before full spec is written for each priority item.

24. Test NTAG424 NDEF write via Bridge APK with a large payload (~300 bytes) — confirm the existing write endpoint handles the size, or identify exactly what needs changing in NfcManager.kt

25. Test opreturnbot.com API manually — send a test message, pay the Lightning invoice, confirm OP_RETURN appears on mempool.space — establish the exact API call format

26. Extract the bitcoinjs-lib keypair/address generation code from mint.html into a standalone JS module — this becomes the shared dependency for the Type T mint screen

27. Create a KV namespace for Type T coin records — separate from Type L — and define the schema

28. Define the coin ID format for Type T v1 (FF + 6 random bytes) and confirm it is written as the first field in the OP_RETURN payload

> *Starting with items 1 and 2 above gives the clearest picture of what the integration actually looks like in practice before any new screens are built.*

WORKING DOCUMENT · v0.1 · SUBJECT TO REVISION
