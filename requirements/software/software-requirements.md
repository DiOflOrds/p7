# Software Requirements — P7 "Teams im HMI" (extension of platform baseline)

*Extends SWR-001–052; numbering continues. Components: BCK/FRT (backend/frontend), OPS (Betrieb). Language: English (D011). Status `reviewed` = feasibility + verifiability per DoD checklist. Verification is UI/device acceptance checklist plus tests; test coverage lands with the implementation sprints. v1.0 Sprint 0, T-0002 (G1a/D001); v1.1 Sprint 2, T-0012 — +SWR-059 from the G4-sprint-1 finding (digest shown as raw text, p7/D002).*

## Team view (P7-E1)

| ID | Requirement | Trace | Verification | Prio | Status |
|---|---|---|---|---|---|
| SWR-053 | The backend shall serve team data per team repo (detected via `team.yaml`): charter text, team key data, configuration values, SLA list, and the digest history (list plus single digest content) via read API; remote (non-localhost) read access to team content shall require the configured PIN while localhost stays PIN-free (extension of the ADR-006 write rule to sensitive team reads). | STK-017 | API tests (team data served; remote read without/with PIN) + acceptance checklist | high | reviewed |
| SWR-054 | The frontend shall provide a "Team" tab for team projects showing digest history with readable digest detail, and charter/configuration/SLA as read-only sections, usable on desktop and Android/Chrome phone. | STK-017 | UI/device acceptance checklist (browser + phone) | high | reviewed |
| SWR-055 | The cockpit shall show a team tile for team repos including the date of the latest digest (or a hint that none exists yet). | STK-017 | API test (cockpit contains team info) + UI checklist | medium | reviewed |
| SWR-059 | The frontend shall render digest and charter markdown as structured, readable HTML — headings, paragraphs, bold and inline code, ordered/unordered lists, tables, horizontal rules — via DOM construction without external libraries (ADR-002), so the relevant information is scannable at a glance on desktop and phone (finding from G4 sprint 1: raw-text view). | STK-017 | UI acceptance checklist (formatted digest on desktop + phone) | high | reviewed |
| SWR-060 | The markdown renderer shall support inline links `[text](https://…)` rendered as safe links (http/https only, new tab, rel=noopener), so each digest item can link directly to its mail in the mailbox web UI (Gmail rfc822msgid deep link supplied by the digest tool). Operations CR from team-mail/N-0001 (2026-08-15), planned by PM as class B (pm/B005), tracked as p7/T-0014. | STK-017 | UI acceptance checklist (link from digest opens the mail) | medium | reviewed |
| SWR-061 | The backend shall offer a restart endpoint (POST, protected by the existing PIN write gate) that acknowledges the request and then exits the server process with a dedicated exit code; the start scripts shall relaunch the server automatically on that code; the frontend shall offer a restart button (header, plus inside the version banner) with confirmation and automatic page reload. Operations CR from pm/N-0002, tracked as p7/T-0015 (pm/B007). | STK-017 | Acceptance checklist (button restarts server, page reloads, version banner gone) | medium | reviewed |

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

STK-017 ← SWR-053–061 (complete; no orphans). v1.2: +SWR-060 (Betriebs-CR T-0014); v1.3: +SWR-061 (Betriebs-CR T-0015 aus pm/N-0002). DoD checklist applied per SWR (2026-08-15 RM). Security: PIN read gate for team content (SWR-053) documented as ADR-006 delta in Sprint 1; Guardrail 2 (no GitHub remote for sensitive repos) untouched — Mission Control reads locally. G1 pending (T-0003).
