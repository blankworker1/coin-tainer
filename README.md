# Coin-tainer Protocol — Type T Toolset

**VERITAS IN NUMERIS**

Three standalone HTML webapps for minting, verifying, and sweeping Type T timelocked Bitcoin coins. All tools run entirely client-side with no server dependency. Cryptography is handled by an inlined BitcoinLib bundle (bitcoinjs-lib v5.2.0 + noble/secp256k1 v1.7.1) — no CDN calls for crypto operations.

---

## The Three Tools

### `enrol.html` — Enrolment Tool

Mints a new Type T coin. Takes an NTAG424 NFC chip through a six-step workflow and produces a fully provisioned timelocked Bitcoin coin.

**Workflow**

1. **Tap NFC chip** — reads the NTAG424 UID via Web NFC
2. **Load mintmark** — upload an SVG; the tool strips backgrounds and flattens to black/transparent
3. **Generate coin** — derives a timelocked P2WSH address from a new random key; cross-verifies address via BitcoinLib
4. **Birth certificate** — opens [opreturnbot.com](https://opreturnbot.com/) with the coin payload pre-copied; operator pays Lightning invoice, pastes the returned txid
5. **Write chip** — writes dual NDEF records to the NTAG424: Record 1 (URL → verify page), Record 2 (JSON payload)
6. **Export QR** — generates a 30mm SVG QR at EC=M with the mintmark composited in; download for printing/engraving on Face B

**Key details**

- Mainnet only. Lock block hardcoded to **1,951,500** (~2046). Test mode offsets: +1/+5/+10 blocks
- Payload format: `{a, k, t}` — address, WIF private key, OP_RETURN txid
- WIF round-trip verified at generation time; address cross-verified via `bitcoin.payments.p2wsh`
- BitcoinLib bundle inlined — fully offline-capable after first load
- NFC write requires Web NFC API (Chrome on Android)

---

### `verify.html` — Verify Webapp

Public-facing coin verification page. Confirms a coin is genuine, shows its Bitcoin balance, and explains the timelock.

**Features**

- Live balance and total received from mempool.space API
- Lock status: LOCKED / UNLOCKED with block countdown and time estimate
- OP_RETURN birth certificate lookup — paste txid to reveal block created and date
- URL parameters: `?a=address`, `?i=uid`, `?t=txid` — NFC tap auto-populates all fields
- Three access routes explained: NFC tap, QR scan, direct web entry
- Collapsible explainer: timelock mechanics, key philosophy, material spec, anti-seigniorage rationale
- Mainnet only

**NFC tap flow (pending Cloudflare Worker)**

The NTAG424 chip writes `https://t.ct.io/{uid}` as NDEF Record 1. A Cloudflare Worker at that domain looks up the coin in KV storage and redirects to:

```
verify.html?a={address}&i={uid}&t={txid}
```

KV schema: `t:{uid}` → `{ address, txid, lockBlock, enrolledAt }`

---

### `sweep.html` — Sweep Tool

Spends the timelocked Bitcoin after block 1,951,500. Reconstructs the redeem script from the WIF key, builds a PSBT, pre-signs it, and arms an auto-broadcast trigger that fires the moment the lock expires.

**Input methods**

| Method | How |
|--------|-----|
| NFC tap | Web NFC reads NDEF Record 2 from the chip; extracts `{a, k, t}` silently |
| QR scan | Camera decodes the Face B QR code via jsQR |
| Manual | Paste address and WIF key directly; raw JSON paste also accepted |

**Signing flow**

1. WIF → private bytes → `BitcoinLib.fromPrivateKey` → public key
2. `buildCLTVScript(pubkey, 1951500)` → redeem script
3. `p2wshAddress(redeemScript)` → reconstructed address
4. Cross-verify: reconstructed address must match `a` from payload — aborts on mismatch
5. Fetch UTXOs from `mempool.space/api/address/{a}/utxo`
6. Build PSBT with `nLocktime = 1951500`, `sequence = 0xfffffffe` (RBF)
7. Sign all inputs; apply custom CLTV finalizer: `witness = [sig, redeemScript]`
8. Arm: poll network every 5 s; auto-broadcast at target block
9. RBF fee escalation fires automatically if a competing transaction is detected

**Fee strategy**

- Initial fee rate, max fee rate, and escalation multiplier are all configurable
- Fee history table shows each escalation step
- Manual bump button available while armed

---

## Payload Format

All three tools use the same compact JSON payload:

```json
{ "a": "bc1q…", "k": "K…", "t": "abcd1234…" }
```

| Field | Content | Size |
|-------|---------|------|
| `a` | Timelocked P2WSH address (bc1q…) | ~62 chars |
| `k` | WIF private key (K… or L…) | ~52 chars |
| `t` | OP_RETURN birth cert txid (64 hex chars) | 64 chars |

Total raw JSON: ~185 chars → fits a v8 QR at EC=M. The descriptor and UID are both omitted — they are reconstructable from `k` and the hardcoded lock block, and from `t` via OP_RETURN lookup respectively.

The private key is stored in NDEF Record 2 intentionally. Security comes from Bitcoin consensus timelock — the key is inert until block 1,951,500. There is no value in concealing it before then.

---

## Architecture Decisions

These are locked. Do not change them.

**Cryptography — BitcoinLib only**

| Operation | Implementation |
|-----------|---------------|
| Key generation | `crypto.getRandomValues(32 bytes)` → `BitcoinLib.fromPrivateKey` |
| Address derivation | `buildCLTVScript(pubkey, lockBlock)` → sha256 → bech32 → cross-verify via `bitcoin.payments.p2wsh` |
| WIF encoding | `toWIF(privBytes, mainnet=true)` → round-trip verify |
| Signing | `bitcoin.Psbt` + custom CLTV finalizer: `witness = [sig, redeemScript]` |
| **Never use** | The custom SECP object — produces different pubkeys than BitcoinLib |

**Redeem script reconstruction**

The sweep tool does not store the descriptor or redeem script. It reconstructs:

```
WIF → privBytes → BitcoinLib.fromPrivateKey → pubkey → buildCLTVScript(pubkey, 1951500) → P2WSH address
```

If the reconstructed address does not match `a`, the tool aborts. This is the primary integrity check.

**CLTV script structure**

```
<lockBlock> OP_CLTV OP_DROP <pubkey> OP_CHECKSIG
```

Encoded as P2WSH (pay-to-witness-script-hash). The witness stack at spend time is `[sig, redeemScript]`. The auto-finalizer in bitcoinjs-lib cannot handle custom scripts — the witness is built manually.

**NFC — dual NDEF**

NTAG424 capacity: 412 bytes. Estimated usage: ~240 bytes.

| Record | Type | Content | Purpose |
|--------|------|---------|---------|
| 1 | URL | `https://t.ct.io/{uid}` | OS dispatches on every tap — opens verify page |
| 2 | Text | `{a, k, t}` JSON | Silent — read by sweep tool via Web NFC API only |

---

## Network

- All three tools: **mainnet only**
- API: `https://mempool.space/api`
- Explorer links: `https://mempool.space/tx/{txid}`
- OP_RETURN broadcast: `https://opreturnbot.com/createRequest` (POST, form body `message=<text>`)
- opreturnbot fee: ~5,000–7,000 sats for a 77-char coin payload

---

## Browser Requirements

| Feature | Required by | Browser |
|---------|------------|---------|
| Web NFC API | Enrol (write), Sweep (read) | Chrome on Android only |
| `crypto.getRandomValues` | Enrol (key generation) | All modern browsers |
| Camera / `getUserMedia` | Enrol (QR scan), Sweep (QR scan) | All modern browsers |
| `navigator.clipboard` | All tools | All modern browsers (HTTPS required) |

The verify and sweep tools degrade gracefully without NFC — QR scan and manual entry are full fallbacks. The enrol tool requires NFC for the chip write step; the rest of the workflow runs in any browser.

---

## File Status

| File | Status | Notes |
|------|--------|-------|
| `enrol_fixed.html` | ✅ Complete | Awaiting NTAG424 tag for NFC write test |
| `verify.html` | ✅ Complete | Awaiting NFC tag + Cloudflare Worker routing |
| `sweep_typeT.html` | ✅ Complete | Full architecture; lock expires ~2046 |

---

## Outstanding Infrastructure

**Cloudflare Worker — `/t/:uid` routing (Priority 1)**

NFC taps hit `https://t.ct.io/{uid}`. A Worker must resolve the UID to coin data and redirect:

```
GET /t/:uid → KV lookup → redirect to verify.html?a={address}&i={uid}&t={txid}
```

KV namespace: `TYPE_T_COINS` (separate from Type L)
KV key format: `t:{uid}`
KV value: `{ address, txid, lockBlock, enrolledAt }`

**Bridge APK — NTAG424 write support**

Web NFC is used in the browser for development. The production Sunmi device needs a Bridge APK update to support NTAG424 dual NDEF write. Web NFC is sufficient until hardware arrives.

---

## Development Notes

- All three files are self-contained — open directly in a browser, no build step, no server
- The BitcoinLib bundle is ~1MB inlined. Minification would reduce this significantly if file size becomes a concern
- CORS blocks polling opreturnbot.com from the browser — the operator pastes the txid manually after payment. This is by design
- jsQR is loaded from `cdn.jsdelivr.net` in the sweep and enrol tools. For fully offline use, inline the jsQR bundle alongside BitcoinLib

---

*Coin-tainer Protocol · Working Document · Session 2 · May 2026*
