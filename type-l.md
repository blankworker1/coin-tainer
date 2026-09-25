# Coin-tainer — Type L (Lightning)

*Handover document · everything needed to build, produce, and reason about Type L independently. Shared protocol material (pillars, verification website, terminal hardware) lives in `protocol-shared.md`. Where Type L's build touches shared infrastructure that Type T also depends on, see `integration.md`.*

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


---

## Life cycle

**Type L — Life Cycle**

- Minted empty — no value until loaded

- Loaded at merchant terminal via Lightning — sats arrive, chip written, coin active

- Circulates as physical Lightning bearer instrument

- Redeemed: holder taps coin to Lightning wallet — sats paid out — coin inactive

- Reloaded and recirculated, or retired


---

## Manufacture

**Type L Production**

- Single-colour injection-moulded recycled plastic disc

- Stainless steel rim fitted

- NTAG213 sticker mounted in rear recess

- Chip UID recorded — coin initialised as inactive in Cloudflare KV ledger

- No optical enrolment required


---

## Build status — what exists today

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


---

## Notes for anyone working on Type L in isolation

- Type L's NFC path (NTAG213, the Bridge APK's `/nfc/write` endpoint, the Blink LNURL encoder and `payInvoice` function) is **working, production-tested, and must not be broken** by Type T integration work. See `integration.md` → "What Must Not Be Touched" for the full list of components Type T work must route around.
- Type L and Type T share a single Cloudflare Worker domain and routing layer. See `integration.md` for UID-based routing (§2.1 in the original tech spec).
