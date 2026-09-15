---
name: project-gegencheck-praezedenz-13-1-vs-qrot-fable-umsetzung-2026-09-15
description: "Opus-Gegencheck 15.09.2026 zur Fable-Umsetzung von Option D + b1 (Praezedenz 13.1 vs. 7b1a Q-Score-ROT). FREIGEGEBEN MIT AUFLAGEN, 0 Blocker, 94/94 Tests selbst verifiziert gruen. Kernlogik (nurQ2/qrotVor), b1-Zaehler-Reset, UNBEKANNT-Behandlung, K1-Zirkelbruch alle korrekt. Hauptauflage N1: gate_check.cjs markiert JEDE --chasing yes+PASS+ROT-Kombination als 13.1-Kollision, unabhaengig vom tatsaechlichen k-Wert (Skript kennt k nicht) -- verunreinigt die Schattenmessung und macht Q-ROT im ganzen Chasing-Teilraum faktisch zum Einstiegs-Veto, was Levis Entscheidung ausdruecklich ausschliesst. Braucht eine kurze Levi-Entscheidung (--k-ohne-signal Pflichtfeld machen vs. abgeschwaechte Zeile ohne JSONL-Marker). NOCH NICHT UMGESETZT, NOCH NICHT KOMMITTET."
metadata:
  node_type: memory
  type: project
  status: "FREIGEGEBEN MIT AUFLAGEN 15.09.2026 -- N1 braucht Levi-Entscheidung, N2-N7 sind reine Fable-Nacharbeit; unkommittet"
  originSessionId: dbfa1999-360e-473a-b80c-5e3236d306fd
  modified: 2026-09-15T10:13:25.029Z
---

# Opus-Gegencheck: Fable-Umsetzung Option D + b1 (15.09.2026)

**Bezug:** [[project_praezedenz_13_1_vs_qrot_entscheidungsvorlage_2026-09-15]] (Soll-Vorgabe). Fable-Umsetzung: `scripts/gate_check.cjs` (+76), `scripts/vollcheck.cjs` (+62/−12), `scripts/skipped_fiktiv.cjs`, `tests/trading_scripts.test.js` (+4 Tests), plus Regelwerkstext in `feedback_live_trading.md`/`feedback_chartanalyse.md`/`feedback_vollcheck_format.md`. Alles unkommittet.

**Testlauf vom Gegenchecker selbst ausgeführt** (nicht nur laut Fable-Bericht übernommen): `node --test tests/trading_scripts.test.js` → 94/94 grün.

## Gesamturteil: FREIGEGEBEN MIT AUFLAGEN, 0 Blocker

Kernlogik korrekt: `nurQ2 = erf('q1') && erf('q4')` bildet "ROT nur aus Q2" exakt ab; `qrotVor = rotWirksam && !nurQ2` konsistent zwischen beiden Skripten; b1 setzt den k-Zähler bei Dual-Gate-Bruch hart auf 0 (kein Pause-Bug, über 5-VC-State-Test belegt); UNBEKANNT überall wie ROT behandelt wo gefordert; K1-Zirkelschluss an beiden Enden (Deckel + Verfallszweig) gebrochen; Einmal-Halbierung/Ampel/Gates nachweislich unberührt; Regelwerkstext deckt sich mit Code (bis auf N1); beide offenen Folgefragen (15-Trade-Nenner, 09.09.-Zählerdifferenz) korrekt NICHT entschieden, nur vermerkt.

## Auflagen (nach Priorität)

**N1 (zwingend vor nächstem unbeaufsichtigten Solo-Abschnitt, braucht Levi-Entscheidung):** `gate_check.cjs` definiert eine "13.1-Kollision" heute als `rotWirksam && PASS && --chasing yes` — ohne den tatsächlichen k-Wert zu kennen (`--k-ohne-signal` ist nur Wortlaut, kein Pflichtfeld). Ein ganz gewöhnlicher 7b1-Trigger während einer Chasing-Phase hat typischerweise k=0 oder 1, wird aber trotzdem als Kollision markiert. Zwei Folgen: (1) Schattenmessung (`kollision_13_1_qrot`-Marker in skipped_setups_fiktiv.jsonl) wird verunreinigt — genau das Datum, das die spätere Auswertung nach ~10 Kollisionsmomenten tragen soll; (2) Solo-Default "KEIN Einstieg" greift dann auch ohne echten 13.1-Zustand — Q-ROT wird faktisch zum allgemeinen Einstiegs-Veto im ganzen Chasing-Teilraum, was Levis Entscheidung ausdrücklich ausschließt (Q-ROT bleibt außerhalb der 13.1-Kollision kein Veto). Fable hat das erkannt, aber falsch aufgelöst: statt die Erkennung zu verengen, wurde der Regelwerkstext (7b1a Item 4, `feedback_vollcheck_format.md`) an die Implementierungsabkürzung angepasst — ein Skript sollte die Regel nicht umdefinieren.
**Zwei Lösungsoptionen (Levi muss wählen):**
  a) `--k-ohne-signal` bei `--chasing yes` zum Pflichtfeld machen, `kollision13_1` an `k >= 2` binden.
  b) Marker + "KEIN Einstieg"-Zeile nur wenn `k >= 2` übergeben; ohne das Feld nur eine abgeschwächte Zeile ("Kollision möglich — k nicht übergeben, nicht gezählt"), kein JSONL-Marker.

**N2-N5 (mit N1 miterledigen, je 1-2 Zeilen):**
- N2: `vollcheck.cjs` verlangt k≥2 für Kollision, `gate_check.cjs` nicht → zwei widersprüchliche Pflichtzeilen im selben Moment möglich. Fällt mit N1 weg.
- N4: `gfSequenzBruch` liest `Number(null) === 0` als "Zähler auf 0 gefallen" → hebt K1-Deckel fälschlich auf bei nicht bestimmbarem `k_ohne_signal`. Fix: `Number.isFinite(...) && ... === 0`.
- N5: `kollision_13_1_regel` ist im Dedupe-Zweig nicht sticky (nur `kollision_13_1_qrot` ist es) → in sich widersprüchlicher Schattendatensatz möglich.

**N3 + N7 (vor nächstem Testtag):**
- N3: `--k-ohne-signal` wird nicht validiert — ungültige/fehlende Werte fallen still auf generischen Wortlaut zurück, statt Hard-Exit (verletzt bestehendes "kein stilles Durchwinken"-Prinzip, vgl. B4/C1-Fix 09.09.).
- N7: `gfSequenzBruch` (einziger Mechanismus, der den K1-Deckel wieder aufhebt) hat keinen Test; ebenso ungetestet: k≥2 + letzter Gate-Lauf PASS+GELB (nur GRÜN getestet).

**N6 (redaktionell, kann mitlaufen):** Deckel-Text bei PASS+UNBEKANNT verweist auf die Retest-Zeitbox-Bedingung, die dort gar nicht existiert (Zeitbox startet nur bei ROT, nicht UNBEKANNT — das ist die unveränderte Zeitbox-Mechanik, kein Bug, nur ein irreführender Verweistext).

## Commit-Einschätzung des Gegencheckers
"Commit des aktuellen Stands ist vertretbar (nichts ist kaputt, alles ist additiv), sofern die Auflagen als eigener Folgeauftrag festgehalten werden und N1 vor dem nächsten Solo-Loop abgearbeitet ist." Gegenchecker hat selbst nichts geändert (kein Edit/Commit/Push, nur Tests ausgeführt).

## N1-Entscheidung (15.09.2026, nach Rückfrage an denselben Gegenchecker)

**Levi hat Option (b) gewählt, verschärfte Form (Opus-Empfehlung, klar):** Fehlt `--k-ohne-signal` oder ist k<2, druckt `gate_check.cjs` statt der Pflicht-Kombizeile eine explizite Zeile "13.1 × Q-Score-Kollision: NICHT bewertet — k nicht übergeben bzw. k<2/2; keine 13.1-Präzedenz angewandt, kein Schattenmessungs-Eintrag". Kein `kollision_13_1_qrot`-Marker, kein Solo-Default "KEIN Einstieg" in diesem Fall. Zusätzlich eine Plausibilitätswarnung bei `--chasing yes` ohne `--k-ohne-signal` (analog zur bestehenden P6-Warnung).

Begründung gegen Option (a, Pflichtfeld): Batch-/Backtest-Läufe über historische JSONs kennen k grundsätzlich nicht — ein Pflichtfeld würde Nachrechnungen abwürgen (Bruch von "A-Pflichten gelten nur live, nicht rückwirkend"). Das "kein stilles Durchwinken"-Prinzip greift hier nicht als Gegenargument, weil `--k-ohne-signal` laut Regelwerk ausdrücklich optional ist (nur Wortlaut, kein Gate) — das eigentliche Problem war nie der Fallback-Text, sondern dass trotz unbekanntem k eine Kollision behauptet wurde. Ein Hard-Exit an einem reinen Doku-Feld wäre zudem riskant, weil er ausgerechnet im 17:55-Moment-Typ (für den Option D geschrieben wurde) einen Gate-Lauf blockieren könnte.

Auch die 7b1a-Regeltextstelle und `feedback_vollcheck_format.md` (die Fable an die zu breite Implementierungsabkürzung angepasst hatte) müssen entsprechend auf die (b)-Definition zurückgesetzt werden.

**Status: N1 entschieden (Option b, verschärft), an Fable gegeben zusammen mit N2-N7.**

## Finale Nachprüfung (15.09.2026, derselbe Gegenchecker, nach Fable-Nacharbeit)

**Gesamturteil: FREIGEGEBEN MIT AUFLAGEN — Code ist commit-fähig.** Alle 7 Punkte (N1-N7) real umgesetzt und je durch einen Test abgesichert, der vor dem Fix rot gewesen wäre (selbst verifiziert: `node --test tests/trading_scripts.test.js` → 95/95 grün, exakt statt nur behauptet). N1 exakt wie empfohlen (Kollision nur bei bekanntem k≥2, kein Pflichtfeld, "NICHT bewertet"-Zeile, kein Marker/Solo-Default, Plausibilitätswarnung) — Beweis: Test zählt jetzt exakt `2` statt `>=2`, der fremde V3-Lauf ohne k zählt korrekt nicht mehr mit. Nebeneffekt: weil b1 k nur bei vollem Dual-Gate hochzählt, sind die Kollisionsdefinitionen beider Skripte jetzt semantisch deckungsgleich (nicht nur ähnlich). Fables zwei Eigenentscheidungen (Tri-State `null` im JSONL-Schema, Warnzeile bei `--chasing no`+k≥1) als vertretbar bewertet.

**Einzige verbleibende Auflage (redaktionell, KEIN Commit-Blocker):** die neue Plausibilitätswarnung (`--chasing no` + `--k-ohne-signal >= 1`) ist im Regelwerk (`feedback_live_trading.md` Z.732, `--chasing`-Bullet) nicht dokumentiert — nur die alte P6-Warnung steht dort. Ein Halbsatz nachtragen oder die Warnzeile streichen.

Verbotszone weiterhin eingehalten (Q1-Q4, Ampel, Kerzenschwelle, Zeitbox-Mechanik, Scharfschaltungsstufe unverändert); beide offenen Folgefragen (15-Trade-Nenner, 09.09.-Zählerdifferenz) weiterhin offen markiert. Wartungshinweis: `CHASING_K_MAX = 2` ist jetzt in beiden Skripten dupliziert (kein gemeinsames Modul) — bei künftiger Schwellenänderung an beiden Stellen anpassen.

**Status: Redaktionelle Auflage erledigt (15.09.2026)** — Fable hat in `feedback_live_trading.md` Z.732 (--chasing-Bullet) und Z.1221 (13.1 Item 3) je einen Satz zur neuen Plausibilitätswarnung (`--chasing no` + `--k-ohne-signal >= 1`, keine Gate-Wirkung) nachgetragen, keine Code-Änderung, Tests unverändert 95/95. **Committet und gepusht 15.09.2026 (cc680a4, main → origin/main). Auftrag vollständig abgeschlossen.**
