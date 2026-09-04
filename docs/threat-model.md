# Threat Model

##  Non-Goals 

This prototype is not an attempt to attack, clone, or interoperate with the real Bildungscampus CampusCard system. Everything below concerns our own system, built on our own hardware and accounts.

## Assets

| Asset | Why it matters |
|---|---|
| Credential secret key | If leaked, an attacker can impersonate that credential |
| Challenge generation | If predictable, defeats the whole point of challenge-response |
| Backend credential database | Contains who's enrolled and their roles |
| Audit log | Source of truth for "what happened" |
| Reader and backend network channel | Carries the verify request/response |
| Physical relay/lock | The actual thing being protected |

## Boundaries 

* **Boundary 1 (NFC):** attacker within a few cm with their own NFC hardware.
* **Boundary 2 (Network):** attacker on the same WiFi, or between reader and backend.
* **Boundary 3 (Physical):** attacker with physical access to the Pi or wiring.

## Threats, Likelihood, Mitigation

| # | Threat | Boundary | Likelihood (for us) | Impact | Mitigation | Status |
|---|---|---|---|---|---|---|
| T1 | **Replay** — attacker captures a valid tap's response and replays it later | 1 | Medium | High | Challenge-response with single-use, random nonce  | Mitigated |
| T2 | **Cloning** — attacker extracts the credential's secret key from the phone | 1 | Low | High | Key never transmitted | Mitigated |
| T3 | **Relay attack** — attacker relays NFC traffic over a network to open a door remotely while the real credential is elsewhere | 1 | Medium | High | Not mitigated | Accepted risk for a prototype |
| T4 | **Man-in-the-middle** — on reader to backend traffic | 2 | Medium | High | Use HTTPS/TLS for the verify call| Add TLS |
| T5 | **Backend compromise** — SQL injection | 2/3 | Low | High | Use JPA statements, never string-concatenate SQL and validate all inputs | Ongoing |
| T6 | **Stale revocation** — revoked credential still works | 2 | Low | Medium | No caching of allow/deny decisions, every tap hits the DB fresh | Mitigated by design (FR6) |
| T7 | **Audit log tampering** — log entries altered/deleted after the fact | 2/3 | Low | Medium | Append-only table | WIP |
| T8 | **DoS on backend** — flooding with bogus requests | 2 | Low  | Low | Acceptable for a local demo | Accepted risk |
| T9 | **Weak nonce randomness** | 1/2 | Low if done right | High | Make sure secured randomness used for nonce generation | Code review checklist item |

##  What "good enough" is for us

We are not trying to eliminate every row above. We're trying to know exactly which ones we solved and which we didn't, and be able to explain why.

