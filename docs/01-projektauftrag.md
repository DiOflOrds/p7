# Projektauftrag P7 — „Teams im HMI: Digest-Ansicht & Team-Konfiguration" (v1.0, G0 übernommen)

*2026-08-15, PL. **G0-Äquivalent: pm/T-0005, Option P7b, entschieden via Inbox (pm/D001)** — der Auftraggeber hat Umfang und Optionen bereits am DR entschieden; ein zweiter G0 wäre Doppelverwaltung (verbucht als p7/D000). Auslöser: Auftraggeber-Fragen vom 15.08. („wo gibts die HMI … Teams sollen konfigurierbar sein … das muss dargestellt werden"). Der team-lokale Sofortteil (konfiguration.yaml + mail_digest v0.2) ist bereits geliefert; P7 ist der Plattformteil.*

## Was und Warum

Teams sind seit P5 in Mission Control sichtbar (Cockpit/Board/Chat), aber ihr **Inhalt** nicht: Digests sind nur Dateien, die Konfiguration nur YAML. P7 macht Teams im Leitstand erlebbar und bedienbar: ein **Team-Tab** (Digest-Verlauf, Charter, Konfiguration, SLA), eine **Cockpit-Kachel** (letzter Digest), ein **Konfigurations-Formular** (PIN-geschützt schreibend, wie Inbox-Entscheidungen) und die **Digest-Zustellung per Mail**. Sicherheitsaspekt von Anfang an: Digest-Inhalte sind Datenklasse `sensibel` — Remote-**Lese**zugriff auf Team-Inhalte verlangt darum die PIN (localhost bleibt frei); Guardrail 2 (nie zu GitHub) bleibt unberührt, da Mission Control lokal liest.

**Zielprodukt-Typ:** Plattform Frontend+Backend (SW, F6) · **Nutzerkreis:** Auftraggeber + Registry-Nutzer, Heim-LAN · **Vertraulichkeit:** privat; Team-Inhalte sensibel (PIN auch für Remote-Lesen) · **Budget:** 0 € API.

## Epics

| Epic | Inhalt | Wunsch-Bezug |
|---|---|---|
| P7-E1 | **Team-Ansicht:** Tab „Team" (Digest-Verlauf + Detail, Charter/team.yaml/konfiguration/SLA read-only), Cockpit-Kachel „letzter Digest", mobiletauglich | „das muss dargestellt werden" |
| P7-E2 | **Konfigurator:** Formular im Team-Tab (Zeitraum 1/7/30, Rechnungs-Abschnitt, Mail-Zustellung), PIN-geschützt, schreibt `konfiguration.yaml` mit sofortigem Commit; Konten nur anzeigen (Erweiterung = Klasse A mit Zugangs-Freigabe) | „Teams sollen konfigurierbar sein" |
| P7-E3 | **Digest-Zustellung per Mail:** `zustellung_mail: ja` wird wirksam — neue Digests gehen an die registrierten Team-Adressen (SWR-033-Kanal), idempotent, nie blockierend, integriert in den abschluss-Lauf | „zusammenfassung per mail" |

## Abnahmekriterien

1. **Team-Tab real:** Digest-Verlauf und -Detail im Browser und am Handy lesbar; Charter/Konfiguration/SLA sichtbar (Stichproben-Checkliste).
2. **Cockpit-Kachel:** team-mail erscheint mit Datum des letzten Digests.
3. **Konfigurator wirksam:** Zeitraum im HMI umstellen → Commit in `konfiguration.yaml` nachweisbar; falsche PIN → klare Ablehnung; ungültige Werte → verständlicher Fehler.
4. **Mail-Zustellung real:** ein echter Digest kommt per Mail an (nach `zustellung_mail: ja`), kein Doppelversand bei wiederholtem Lauf.
5. **PIN-Lesegate:** Team-Inhalte (Digest) remote nur mit PIN lesbar, localhost frei (Test + Stichprobe); Requirements-first (SWR-053–058, Matrix 0 Lücken), Gates als Inbox-DRs, Schätzung, 0 €.

## Rahmen

2 Umsetzungssprints nach G1 (S1: E1+E2 · S2: E3 + Abnahme). Kein neues ADR nötig, solange die bestehenden Muster (Hash-Router ADR-005, Schreibschutz ADR-006, Discovery ADR-004) reichen — Erweiterung der PIN-Regel auf Team-Lesezugriffe wird als ADR-006-Delta dokumentiert. Playbook, Team-Node-Gate, Baselines, Sandbox pusht nie.
