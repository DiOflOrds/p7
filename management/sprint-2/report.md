# Sprint-2-Report P7 — Mail-Zustellung + Lesbarkeit + Abnahmebilanz

*2026-08-15, PL. Tickets T-0010–T-0012 done, Abnahme-DR T-0013 in der Inbox.*

## Ergebnis

**SWR-058:** `digest_zustellung.py` sendet Digests von Teams mit aktivierter Mail-Zustellung über den bestehenden SWR-033-Kanal — jeder Digest genau einmal (Zustellvermerk am Dateiende + Commit als Idempotenz-Anker), Fehler blockieren nie und werden beim nächsten Lauf nachgeholt; integriert als abschluss-Schritt [2c/5], per Konfigurator ein-/ausschaltbar. **SWR-059 (dein Befund aus der Sprint-1-Stichprobe):** Digest und Charter werden jetzt formatiert dargestellt — eigener Markdown-Renderer (DOM-basiert, ES5, keine Bibliothek nach ADR-002): Überschriften mit Trennlinien, Listen, Fett/Kursiv/Code-Chips, Tabellen im bestehenden Stil, saubere Zeilenhöhen, mobiletauglich.

## Verifikation

166 Tests grün (+4 in `test_digest_zustellung.py`: Einmaligkeit, Idempotenz, Deaktiviert-übersprungen, Fehlertoleranz mit Nachholung) · Matrix 59 SWRs / 0 Lücken · node --check grün · SWR-059-UI: Abnahme-Checkliste = Stichprobe 1 in T-0013.

## Abnahmebilanz K1–K5 (Projektauftrag)

| K | Kriterium | Bewertung | Evidenz |
|---|---|---|---|
| 1 | Team-Tab real (Browser + Handy) | **erfüllt** | Sprint-1-Stichproben 1–2 (D002), jetzt formatiert (SWR-059) |
| 2 | Cockpit-Kachel letzter Digest | **erfüllt** | Sprint-1-Stichprobe 4 (D002) + API-Test |
| 3 | Konfigurator wirksam (Commit, Fehlerfälle, Konten Klasse A) | **erfüllt** | Realer Konfigurator-Roundtrip (D002-Stichprobe 2, Commit „Mensch via HMI" in team-mail) + 2 API-Testfälle |
| 4 | Mail-Zustellung real, kein Doppelversand | **zur Abnahme** | 4 Tests grün; Realnachweis = T-0013-Stichproben 2–3 (erster Auto-Abschluss-Lauf nach Aktivierung) |
| 5 | PIN-Lesegate + requirements-first + 0 € | **erfüllt** | D002-Stichprobe 3 real; SWR-053–059, Matrix 59/0; 0,00 € API |

## Aufwand (E5)

Geschätzt 110 min · Ist ≈ 95 min (−14 %). P7 gesamt: 345 min geschätzt, ≈ 300 min Ist.

## Retro (Kurzform)

**Gut:** Der G4-Befund des Auftraggebers wurde noch am selben Tag requirements-first (SWR-059) behoben — der Gate-Stichproben-Mechanismus liefert genau die Rückmeldung, für die er gebaut wurde. Zustellvermerk-in-Datei (statt Statusdatei) hält den Versandzustand git-sichtbar am Artefakt. **Beobachtung:** mdRender deckt bewusst nur das Team-Markdown-Vokabular ab (kein verschachteltes Markdown) — reicht für Digest/Charter; bei Bedarf CR. **Kandidat fürs LeLe:** Renderer auch für Briefe/Reports einsetzen (kleiner CR, gleiche Funktion).
