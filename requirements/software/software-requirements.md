# Software Requirements — P7 "Teams im HMI" (extension of platform baseline)

*Extends SWR-001–052; numbering continues. Components: BCK/FRT (backend/frontend), OPS (Betrieb). Language: English (D011). Status `reviewed` = feasibility + verifiability per DoD checklist. Verification is UI/device acceptance checklist plus tests; test coverage lands with the implementation sprints. v1.0 Sprint 0, T-0002 — G1 pending (Inbox-DR T-0003).*

## Team view (P7-E1)

| ID | Requirement | Trace | Verification | Prio | Status |
|---|---|---|---|---|---|
| SWR-053 | The backend shall serve team data per team repo (detected via `team.yaml`): charter text, team key data, configuration values, SLA list, and the digest history (list plus single digest content) via read API; remote (non-localhost) read access to team content shall require the configured PIN while localhost stays PIN-free (extension of the ADR-006 write rule to sensitive team reads). | STK-017 | API tests (team data served; remote read without/with PIN) + acceptance checklist | high | reviewed |
| SWR-054 | The frontend shall provide a "Team" tab for team projects showing digest history with readable digest detail, and charter/configuration/SLA as read-only sections, usable on desktop and Android/Chrome phone. | STK-017 | UI/device acceptance checklist (browser + phone) | high | reviewed |
| SWR-055 | The cockpit shall show a team tile for team repos including the date of the latest digest (or a hint that none exists yet). | STK-017 | API test (cockpit contains team info) + UI checklist | medium | reviewed |

## Team configuration via HMI (P7-E2)

| ID | Requirement | Trace | Verification | Prio | Status |
|---|---|---|---|---|---|
| SWR-056 | The backend shall accept configuration changes (period 1/7/30, report sections on/off, mail delivery on/off) via a PIN-protected write endpoint, validate values, write them to the team's `konfiguration.yaml` and commit immediately (identity "Mensch via HMI"); invalid values yield a clear German 400 message; account entries are never changed via this endpoint (class-A only). | STK-017 | API tests (valid change → file+commit; invalid value → 400; wrong PIN → 403) + acceptance checklist | high | reviewed |
| SWR-057 | The frontend shall offer a configuration form in the Team tab (period selector, section toggles, mail-delivery toggle, accounts read-only with class-A hint) using the existing PIN field, with success and error feedback in German. | STK-017 | UI acceptance checklist (change period via HMI, wrong-PIN attempt) | high | reviewed |

## Digest mail delivery (P7-E3)

| ID | Requirement | Trace | Verification | Prio | Status |
|---|---|---|---|---|---|
| SWR-058 | A delivery script shall send digests of teams with mail delivery enabled to the registered team addresses via the existing SMTP channel (SWR-033), marking sent digests so repeated runs never resend (idempotent), never blocking the abschluss chain on mail errors; it shall be integrated into the abschluss run. | STK-017 | Unit tests (send once, idempotence, error tolerance, disabled teams skipped) + real-mail acceptance checklist | high | reviewed |

## Traceability

STK-017 ← SWR-053–058 (complete; no orphans). DoD checklist applied per SWR (2026-08-15 RM). Security: PIN read gate for team content (SWR-053) documented as ADR-006 delta in Sprint 1; Guardrail 2 (no GitHub remote for sensitive repos) untouched — Mission Control reads locally. G1 pending (T-0003).
