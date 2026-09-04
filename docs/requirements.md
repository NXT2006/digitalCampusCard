# Requirements

## Actors

| Actor | Description |
|---|---|
| Credential holder | A user with an enrolled credential |
| Reader | The device that initiates the NFC exchange |
| Backend | Makes the access decision, stores state and logs activity |
| Admin | Gives and revokes credentials, views the audit log  |

##  Functional Requirements

| ID | Requirement |
|---|---|
| FR1 | The system shall authenticate a credential using challenge-response, not a static identifier. |
| FR2 | The reader shall forward the credential's response to the backend for a decision the reader itself shall not decide allow/deny. |
| FR3 | The backend shall enforce role-based permissions. A credential belongs to one or more roles (e.g. institution), and a door belongs to a role, access is granted only if they intersect. | 
| FR4 | The backend shall log every access attempt regardless of outcome. |
| FR5 | The backend shall support enrollment and revocation of credentials without redeploying code. |
| FR6 | A revoked credential shall be denied on its very next attempt. |
| FR7 | The reader-to-backend interface shall always be identical.  |
| FR8 | On an "allow" decision, the system shall perform the desired action. | 

## Non-Functional Requirements

| ID | Requirement |
|---|---|
| NFR1 (Security) | Per-credential secret material never leaves the credential device. Nonces are single-use and unpredictable. |
| NFR2 (Performance) | End-to-end tap-to-decision latency under ~1.5s |
| NFR3 (Auditability) | Every decision is explainable after the fact from the log alone (who, what door, when, why denied if denied). |
| NFR4 (Maintainability) | Each of the three components (credential, reader, backend) can be developed, tested, and demoed independently before integration. |

## Acceptance Criteria (for our Personal Project)

- **Phase 1 done:** two Android phones exchange a credential token over NFC reliably.
- **Phase 2 done:** backend makes the allow/deny call, reader no longer decides locally.
- **Phase 3 done:** replayed taps are rejected, roles are enforced, revoked credentials fail, every attempt is logged.
- **Phase 4 done:** the same secure flow works through a PC/SC reader with zero backend changes.
- **Phase 5 done:** an authorized tap physically opens a lock via a controller, an unauthorized one does not.

