# Coin-tainer — Integration

*Handover document · the work that connects Type L and Type T, where a change made for one type can affect the other. Read this alongside `type-l.md` and `type-t.md`, not instead of them — this file covers only the coupled surface area: shared routing, the shared NFC-write path, the combined architecture, and the guardrails that keep Type T's build from breaking Type L's working system.*

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

## Why this file exists

**1. What Exists Today**

Two independent systems have been built and tested. Neither knows the other exists yet. The work in this spec is connecting them.


Two independent systems have been built and tested (see `type-l.md` and `type-t.md`). The work in this file is connecting them — the two build priorities below are the only ones that touch code both types depend on. Every other Type T build item lives entirely in `type-t.md` and carries no risk to Type L.

---

## Shared build priorities

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


---

## Combined architecture overview

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


---

## Open decisions

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


---

## What must not be touched

**5. What Must Not Be Touched**

These components are working and debugged. Any integration work must route around them, not through them.

| **Component**                                     | **Why it must not change**                                                                                                                  |
|---------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| **Blink LNURL encoder**                           | Took significant debugging to get Blink API and wallet to accept this — any change risks breaking Type L redemption                         |
| **payInvoice function**                           | Same — live and tested. New uses (paying opreturnbot.com invoice) should call this function, not replace it                                 |
| **NTAG213 NDEF write flow**                       | Type L minting is working end to end — Bridge APK changes for NTAG424 must not affect the NTAG213 path                                      |
| **Denomination map (red/blue/green/white/black)** | Set at protocol level — only changes annually on Pizza Day                                                                                  |
| **KV record schema for Type L coins**             | Dashboard, verify page, and redeem flow all depend on the existing schema — Type T records should use a separate KV namespace or key prefix |


---

## Immediate next steps

**6. Immediate Next Steps**

Suggested starting point before full spec is written for each priority item.

24. Test NTAG424 NDEF write via Bridge APK with a large payload (~300 bytes) — confirm the existing write endpoint handles the size, or identify exactly what needs changing in NfcManager.kt

25. Test opreturnbot.com API manually — send a test message, pay the Lightning invoice, confirm OP_RETURN appears on mempool.space — establish the exact API call format

26. Extract the bitcoinjs-lib keypair/address generation code from mint.html into a standalone JS module — this becomes the shared dependency for the Type T mint screen

27. Create a KV namespace for Type T coin records — separate from Type L — and define the schema

28. Define the coin ID format for Type T v1 (FF + 6 random bytes) and confirm it is written as the first field in the OP_RETURN payload

> *Starting with items 1 and 2 above gives the clearest picture of what the integration actually looks like in practice before any new screens are built.*

WORKING DOCUMENT · v0.1 · SUBJECT TO REVISION

