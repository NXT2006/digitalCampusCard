# Protocol Spec 

This spec for now has the **NFC/APDU layer** (credential to reader) 

---

## 1. NFC / APDU Layer (credential to reader)

### 1.1 AID (Application Identifier)

We register a custom AID so both the Android HCE app and the reader select the *our* app on the credential, not some other NFC service on the phone.

```
AID: F0 43 41 4D 50 55 53 31    (spells "F0 CAMPUS1" in hex/ASCII, must match exactly on both sides)
```

### 1.2 Message flow

### 1.2 Message flow

The reader and credential exchange three messages, in this order:

1. **SELECT** — the reader sends our AID (`F0 43 41 4D 50 55 53 31`). The credential replies with a success code (`90 00`) if it matches.

2. **GET_CHALLENGE** — the reader generates a random, one-time nonce (16 bytes) and sends it to the credential. This nonce is different on every single tap.

3. **VERIFY** — the credential answers by computing `HMAC(secretKey, nonce)` — mixing the reader's random question together with its own private key. It returns two things: its `credentialId` (8 bytes, so the backend knows who's claiming this) and the resulting 32-byte HMAC.

The reader relays `credentialId + nonce + HMAC` on to the backend, which recomputes the same HMAC on its own side  and checks whether the two match.


### 1.3 APDU command definitions

All commands follow ISO 7816-4 style: `CLA INS P1 P2 Lc [Data] Le`.

| Command | CLA | INS | P1 | P2 | Data | Response |
|---|---|---|---|---|---|---|
| SELECT | `00` | `A4` | `04` | `00` | AID (8 bytes) | `90 00` (success) or `6A 82` (not found) |
| GET_CHALLENGE | `80` | `84` | `00` | `00` | (Le = `10` for 16 bytes) | 16-byte nonce + `90 00` |
| VERIFY | `80` | `20` | `00` | `00` | 16-byte nonce | 8-byte credentialId + 32-byte HMAC + `90 00` |

Status words to implement 

| SW | Meaning |
|---|---|
| `90 00` | Success |
| `6A 82` | AID not found / app not selected |
| `69 85` | Conditions not satisfied 
| `6A 86` | Wrong P1/P2 |
| `6F 00` | Unknown error |

### 1.4 Crypto details

- **HMAC:** full 32 bytes
- **secretKey:** a per-credential 256-bit key, generated at enrollment time, stored on the phone and mirrored in the backend's credential table
- **nonce:** 16 bytes from `SecureRandom`, generated fresh by the reader for every single tap, never reused.

## Change control

Only change this file when all three of us approve something, so i.e. when we're on call or are together
