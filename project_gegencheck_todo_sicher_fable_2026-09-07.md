---
name: project-gegencheck-todo-sicher-fable-2026-09-07
description: "Fable-Gegencheck (07.09.2026) von Opus' 'sicherer' To-Do-Liste (S1-S12/Z1-Z12). Substanzielle Zustimmung, aber 3 Punkte nicht so risikofrei wie behauptet (S4 falsche Präzedenz, S5 dritte Aufruf-Fassung, S7 Frische-Anker+Session-Definition), 4 S-Punkte teilen eine ungenannte Abhängigkeit (Bar-Snapshot), Z1 (Vier-Pane-Layout) sollte laut Fable NICHT als Regelwerksfrage zurückgestellt werden, sondern als kontrollierter Einmalschritt sofort. Reine Prüfung, nichts umgesetzt."
metadata: 
  node_type: memory
  type: project
  status: draft
  modified: 2026-09-07T21:24:04.889Z
  originSessionId: b594a3be-34a1-46da-8616-0f50ceb56daa
---

# Fable-Gegencheck: Opus' sichere To-Do-Liste (07.09.2026)

**Reine Prüfung der Filterlogik, kein Code existiert, nichts umgesetzt.** Maßstab: Opus' eigene Filterregel (automatisiert bestehenden Schritt / entfernt keine Prüfung, bewegt keine Schwelle, erweitert keine Gate-Eingabe / setzt keine Regelwerksentscheidung voraus).

## Zustimmung mit Präzisierung/Einwand pro S-Punkt

- **S1** (`vollcheck.cjs`) — Zustimmung, zwei Präzisierungen: Ein Skript kann keine MCP-Tools aufrufen, nur skriptseitig herleitbare Zeilen erzeugen + übergebene Werte formatieren — Zeitgewinn kleiner als "ein Aufruf statt drei". Format-Zeile darf ✓ nur für tatsächlich Geprüftes setzen, sonst "Häkchen ohne Prüfung"-Fehlerklasse. **Reihenfolge: S1 ist Komposition über S6/S7/S10 — zuletzt bauen.**
- **S2** (`position_tick.cjs`) — Zustimmung, verdeckte Abhängigkeit: Zertifikatspreis-Rechnung braucht Knock-out/Bezugsverhältnis/Spread — genau da saß der `size.cjs`-Short-Bug (28.07.). Auflage: als "berechnet" labeln, `feedback_broker_wert_prioritaet` bleibt Vorrang, Formel einmal gegen echte Quote verifizieren.
- **S3** (`exit_plan.cjs`) — Zustimmung. Hinweis: kodiert Regeltext, muss bei Regeländerung mitziehen.
- **S4** (Trend-Effizienz selbst berechnen) — **Einwand: falsche Präzedenz.** Keine Rechenvorschrift im Regelwerk vorhanden (geprüft, kein Treffer), keine Bars in `gate_check.cjs`. Braucht eine kleine Vorab-Festlegung (welche N Kerzen, Formel) VOR dem Code, plus Bar-Zugriff (Abhängigkeit).
- **S5** (`gate_check_QUICKREF.md`) — **Einwand in der Form.** Statische Datei = dritte Aufruf-Fassung neben Kopfkommentar + 7b1-Template — genau die "stale CLI"-Fehlerklasse vom 31.08. Sicher nur, wenn aus dem Code generiert (`gate_check.cjs --hilfe`).
- **S6** (X-API-Fehlerzähler) — Zustimmung. Kleine Abhängigkeit: Schema von `x_last_fetch.json` erweitern.
- **S7** (Register-Autopflege) — **Einwand: zwei versteckte Fragen.** Session-Extrema-Definition (mit/ohne Vorbörse) ist nicht regelwerksfestgelegt, wurde am 01./02.09. unterschiedlich erhoben. Zweitens: automatischer `updated`-Stempel würde eine Prüfung behaupten, die für 6 von 8 Levelarten nicht stattfand — Verwässerung der 31.08.-Entscheidung. Auflage: Autopflege schreibt Level, `updated` nur mit "auto"-Kennzeichnung oder gar nicht.
- **S8** (Setup-Karte/`--vorschau`) — Zustimmung, mit Hinweis: normalisiert faktisch den Fenster-Entry (legal über 8b Schritt 4 Option 2), Levi sollte das wissen. Abhängigkeiten: Wick-Zonen-Pflege, offenes A3-Feld für Schritt 0.
- **S9** (Trigger-Kandidaten-Zähler) — Zustimmung, braucht Vorab-Festlegung: was genau zählt als Kandidat (Voll-Check mit einem Bein? 2/2-Zustand? 2/2-Trigger?) — sonst lässt sich die Quote nachträglich in jedes Band rechnen.
- **S10** (Levelvollständigkeits-Zeile) — Zustimmung mit Formulierungsauflage: Pflicht- und optionale Levelarten trennen, sonst Druck zur Level-Inflation (Z7 durch die Hintertür).
- **S11** (Swings/OR als Messkategorie) — Zustimmung, technische Abhängigkeit: `eligibleLevels()` filtert heute nur nach `symbol`+`price` — braucht `tp_faehig:false`-Flag, Codeänderung an der Kernfunktion, nicht trivial.
- **S12** (Hypothetisches Ergebnis nachtragen) — Zustimmung. Braucht Bars nach Setup-Zeitpunkt (Replay/Snapshot) + Festlegung Horizont/Reihenfolge.

## Z5-Frage (1H nur bei neuem Schluss) — Fable stimmt Opus zu
Mit Begründung: die 1H-Schatten-Messung braucht den aktuellen EMA50(1H)-Wert; Fortschreiben statt Neulesen wäre eine Änderung der Messdefinition = Regelwerksfrage. Z5 bleibt zurückgestellt. Der eigentliche Hebel bleibt Z1 — mit Vier-Pane kostet die Lesung nichts mehr.

## Wo Fable Opus widerspricht: zu streng gefiltert
- **Z1 (Vier-Pane-Layout) — Fable widerspricht der Einordnung als Regelwerksfrage.** Das ist keine Handelsregel-Frage, sondern ein einmaliges operatives Risiko (Layout-Tools), vollständig entschärfbar mit Testlayout an einem Nicht-Handelstag. **Größter Zeithebel beider Studien — Fables Empfehlung: nicht "später", sondern als kontrollierter Einmalschritt mit eigener Freigabe, getrennt von der Regelwerk-Liste.**
- **Z9 (QQQ-ATR)** — Indikator-Teil ist Chartkonfiguration wie ADX/AVWAP zuvor, kein Pflichtfeld-Eingriff → gehört zu S, nicht zu Z. Nur das Feld-Streichen gehört zu Z. Opus hat beides zusammengelegt.
- **Z3 (Positions-Modus)** — Regelfrage ist laut Fable weitgehend vorentschieden (`feedback_positionsfokus_bei_offener_position` sagt bereits: keine Musterjagd bei offener Position) — sollte schnell entschieden werden, nicht "später".
- Z2, Z4, Z6, Z7, Z8, Z11, Z12: Zustimmung ohne Einwand.

## Ungenannte Abhängigkeiten
1. **Bar-Snapshot als gemeinsamer Baustein:** S4, S7, S12, S2 (MFE) brauchen alle dieselbe gespeicherte `data_get_ohlcv`-Ausgabe — ein eigener Schritt, den Opus nirgends nennt, ohne den vier S-Punkte nicht baubar sind.
2. **Reihenfolge:** S6/S7/S10 → S1 (Komposition) · S11 → `eligibleLevels`-Flag → erst dann S9-Messung sinnvoll · S8 → Wick-Zonen-Pflege + A3-Feld Schritt 0 · S2/S3 → Zertifikatsformel verifizieren.
3. **Vorab-Festlegungen, kein Regelcharakter, aber schriftlich nötig:** Trend-Effizienz-Formel (S4), Session-Extrema-Definition (S7), Kandidaten-Definition (S9), Horizont/Reihenfolge Gegenrechnung (S12), Pflicht/optional-Trennung (S10).

## Fables Endeinschätzung
Liste in der Substanz vertrauenswürdig — kein S-Punkt lockert ein Gate, Trennungen S3/Trailing, S8/Z8, S11/Z7 sauber. Aber: **S7 und S5 nicht so risikofrei wie behauptet, S4 mit falscher Präzedenz begründet.** Vier Punkte teilen die Bar-Snapshot-Abhängigkeit. Vor dem Bauen zu klären: (1) Z1 aus der Regelwerks-Schublade raus, als operativen Einmalschritt entscheiden. (2) Die fünf Vorab-Festlegungen absegnen. (3) S7 nur mit "kein automatischer updated-Stempel ohne Kennzeichnung" freigeben. Danach empfohlene Baureihenfolge: **Bar-Snapshot → S6/S7/S10/S11 → S1/S9 → S2/S3/S8/S12**, S5 als generierte Hilfe statt statische Datei. Kein weiterer Gegencheck nötig danach.
