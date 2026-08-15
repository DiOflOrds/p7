# Sprint-1-Report P7 — Team-Ansicht + Konfigurator

*2026-08-15, PL. Tickets T-0004–T-0008 done, G4-DR T-0009 in der Inbox.*

## Ergebnis

Teams sind jetzt Bürger erster Klasse in Mission Control: Der neue Tab **„Team"** zeigt für team-mail (und jedes künftige Team) den Digest-Verlauf mit Volltext-Detail, den Steckbrief (Profil, Datenklasse, Rollen, SLAs) und die Charter; das Cockpit trägt eine **Team-Kachel** mit dem Datum des letzten Digests. Die **Konfiguration** ist per Formular änderbar — Zeitraum Tag/Woche/Monat, Rechnungs-Abschnitt, Mail-Zustellung — jede Änderung landet als Sofort-Commit (Identität „Mensch via HMI") in `konfiguration.yaml`; Konten sind bewusst ausgenommen (Klasse A, Playbook Kap. 16). Sicherheitskern: das **PIN-Lesegate** (ADR-006-Delta, architektur v1.4) — sensible Team-Inhalte sind remote nur mit PIN lesbar, localhost bleibt frei, das Frontend sendet die PIN jetzt bei allen Anfragen.

## Verifikation

162 Tests grün (+6 neue in `test_teams.py`, gb-02-hermetisch mit Temp-Git-Repo und MC_PIN-Scrub) · Matrix 58 SWRs / 0 Lücken · arch_diagramm --check grün · node --check auf app.js grün. SWR-054/057-UI-Anteile: Abnahme-Checkliste = Stichproben in T-0009 (Team-Node-Gate beachten: erst nach grünem abschluss-Lauf entscheiden).

## Aufwand (E5)

Geschätzt 185 min · Ist ≈ 160 min (−14 %, im Kalibrierungsband).

## Retro (Kurzform)

**Gut:** teams.py folgt exakt den bewährten Mustern (briefkasten-Commit, inbox-Fehlerklassen, manueller YAML-Parser) — dadurch kein neues Konzept, nur neue Fläche; PIN-Lesegate war dank vorhandener `schreibschutz_pruefen` eine Zeile pro Route. **Beobachtung:** Der Konfigurations-Rewrite normalisiert Kommentare der YAML-Datei — akzeptiert (Quelle der Wahrheit sind die Werte), im Charter dokumentiert. **Für Sprint 2:** SWR-058 braucht einen Sende-Marker je Digest — Entwurf: Zustellvermerk-Zeile am Dateiende statt separater Statusdatei (ein Artefakt, git-sichtbar).
