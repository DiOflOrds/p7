# P7-Abschlussbericht — „Teams im HMI: Digest-Ansicht & Team-Konfiguration" (PL)

*2026-08-15. An: Auftraggeber. Zeitraum: ein Tag (Beauftragung bis Abnahme), Sprints 0–2, Baselines p7-req-v1.0, p7-v0.1, **p7-v1.0** (p7 + platform). Abnahme: G4a/D003 via Inbox.*

## Was gebaut wurde

Teams sind jetzt vollwertige Bewohner von Mission Control: Der **Tab „Team"** zeigt Digest-Verlauf mit formatiertem Volltext, Steckbrief (Profil, Datenklasse, Rollen, SLAs) und Charter; das **Cockpit** trägt eine Team-Kachel mit dem letzten Digest-Datum. Der **Konfigurator** stellt die Eckparameter (Tages-/Wochen-/Monatszusammenfassung, Rechnungs-Abschnitt, Mail-Zustellung) PIN-geschützt um — jede Änderung als Sofort-Commit „Mensch via HMI"; Konten bleiben Klasse A. Die **Digest-Zustellung per Mail** läuft idempotent im abschluss-Schritt [2c/5]. Sicherheitskern: das **PIN-Lesegate** (ADR-006-Delta, architektur v1.4) — sensible Team-Inhalte sind remote nur mit PIN lesbar. Besonders: Der Lesbarkeits-Befund aus deiner Sprint-1-Stichprobe wurde noch am selben Tag als **SWR-059** formalisiert und mit einem eigenen Markdown-Renderer (ADR-002-konform, keine Bibliothek) behoben.

## Abnahmekriterien — Ergebnis

| K | Kriterium | Bewertung | Evidenz |
|---|---|---|---|
| 1 | Team-Tab real (Browser + Handy), formatiert | **erfüllt** | D002-Stichproben 1–2 + D003-Stichprobe 1 (SWR-059) |
| 2 | Cockpit-Kachel letzter Digest | **erfüllt** | D002-Stichprobe 4 + API-Test |
| 3 | Konfigurator wirksam (Commit, Fehlerfälle, Konten Klasse A) | **erfüllt** | Realer Roundtrip (D002), Commits „Mensch via HMI" in team-mail, 2 Testfälle |
| 4 | Mail-Zustellung real, kein Doppelversand | **erfüllt mit Betriebs-Restpunkt** | 4 hermetische Tests (Einmaligkeit/Idempotenz/Fehlertoleranz); Realnachweis = erster Auto-Abschluss-Lauf nach Aktivierung im Konfigurator (Zustellvermerk dann sichtbar) |
| 5 | PIN-Lesegate, requirements-first, 0 € | **erfüllt** | D002-Stichprobe 3 real; SWR-053–059, Matrix 59/0; 0,00 € API |

## KPIs

Tests 166 + 42 grün · Matrix 59/0 · Projektlaufzeit: 1 Tag · 4 Entscheidungen via Inbox · Aufwand: 345 min geschätzt, ≈ 300 min Ist (−13 %) · 0,00 € API.

## Übergabe an den Betrieb

Mail-Zustellung aktivieren: Team-Tab → Konfigurator → „Digest zusätzlich per Mail" (der nächste Auto-Abschluss-Lauf stellt zu — K4-Realnachweis geschieht dabei von selbst, PM prüft ihn als Klasse-B-Stichprobe). CR-Kandidaten fürs LeLe: Markdown-Renderer auch für Briefe/Reports, mail_digest-Promotion in den Katalog (B003, Pilotreview ab 29.08.). Betrieb offen: BB-5 (PAT ab 2026-09-05).
