---
name: opus_antwort_umbau_oder_freeze_2026-10-14
description: "Opus-Antwort auf Levis Frage 14.10.2026 'alle Regeln umbauen oder den ungebrauchten Teil freezen?': Variante B — neue schlanke kern_*-Skripte DANEBEN, altes Regelwerk (vollcheck/gate_check/tagesmomente/Kombi/Q-/H1-/IBF-Messreihen) unveraendert eingefroren und weiter aufrufbar (Schatten 'alt haette' per gate_check --dry-run), getrennte kern_*-Logs, Rollback = alten Loop-Prompt starten; einzige Aenderung am alten Pfad: DST-Fix vollcheck (4 Zeilen, Pflicht bis 19.10.); ~2.800 neue Zeilen statt Eingriff in 12.650; Echtgeld nur nach Levi-GO"
metadata:
  type: project
  originSessionId: opus-umbau-oder-freeze-2026-10-14
  modified: 2026-10-14T10:40:27.107Z
---

# Antwort an Levi: Alles umbauen oder den Rest einfrieren? — 14.10.2026

**Zeitstempel:** 12:37-12:55 DE. Code nur gelesen (scripts/*.cjs, tests/, package.json); keine Aenderung, kein Testlauf, kein git, MEMORY.md nicht angefasst.

## Antwort in Kurzform

**Einfrieren, nicht umbauen — Variante B.** Die neuen 12 Regeln bekommen eigene, schlanke Skripte (`kern_*`) daneben. Das alte Regelwerk bleibt Byte fuer Byte, wie es ist: eingefroren, nicht geloescht, weiter aufrufbar und mit gruenen Tests. Die einzige Ausnahme ist der DST-Fix (4 Zeilen in `vollcheck.cjs`, Pflicht bis 19.10.). Die alten Messreihen (10 Freeze-Tage) sind damit abgeschlossen und werden nicht mehr fortgeschrieben. Alles Neue landet in eigenen `kern_*`-Logdateien, sodass sich nichts vermischt. Das Rollback ist trivial: Am naechsten Testtag startet der Cron wieder mit dem alten `node scripts/loop_prompt.cjs`. Echtgeld gibt es in keiner Variante automatisch — nur nach deinem ausdruecklichen GO nach der Zwischenanalyse.

## Begruendung mit Zahlen aus dem Code

| | (A) alle alten Skripte umbauen | **(B) Kern daneben, Alt eingefroren** | (C) Hybrid `--regelwerk kern\|alt` |
|---|---|---|---|
| Betroffener Code | gate_check 5.543 Z. + vollcheck 2.438 + position_tick 485 + protokoll_bilanz 740 + loop_prompt 296 + kombi/skipped/tagesmomente ~1.100 ≈ **10.600 Zeilen umschreiben** | **~2.800 neue Zeilen** in ~10 Dateien, alt 4 Zeilen (DST) | dieselben ~10.600 Zeilen bekommen Verzweigungen (gate_check: 61 `process.exit(1)`-Pfade, 94 A3-/Messfeld-Stellen) |
| Tests | `tests/trading_scripts.test.js` (5.995 Z., 217 Tests) + `tagesmomente.test.js` (29 Tests) muessten grossteils neu geschrieben werden | **alle 246 bestehenden Tests bleiben unveraendert gruen**; neu `tests/kern.test.js` (~40 Tests) | bestehende Tests gruen nur, wenn jeder Default-Pfad exakt erhalten bleibt — schwer nachzuweisen |
| Risiko | hoch: Bruch der laufenden Werkzeuge, Verlust der Vergleichbarkeit, kein Rueckweg | **niedrig**: der alte Pfad wird nicht beruehrt (sha1 vorher/nachher pruefbar) | mittel-hoch: ein vergessener Schalter mischt alte und neue Logik in einem Log |
| Messbarkeit | alte Reihe bricht ab, kein "alt haette" mehr moeglich | **saubere Trennung** (kern_*-Logs) + Schatten "alt haette" ueber das unveraenderte `gate_check.cjs --dry-run` (schreibt laut Code Z. 4085-4087 kein gate_check_log, keine Kombi- und keine Q4-Schatten-Zeile) | Trennung nur ueber Feld im Log, Fehlerquelle |
| Rollback | nur ueber git | **ein Befehl**: alter Loop-Prompt | Schalter, aber gemischter Code |
| Aufwand | 2-3 Wochen | **3-4 Fable-Tage** + Opus-Gegencheck | 1,5-2 Wochen |

## Was eingefroren wird, was als Schatten weiterlaeuft

**Eingefroren** (unveraendert, nicht im Kern-Loop aufgerufen, weiter manuell lauffaehig): `vollcheck.cjs` (bis auf den DST-Fix), `gate_check.cjs` (Live-Modus), `loop_prompt.cjs`, `position_tick.cjs`, `kombi_fiktiv.cjs`, `skipped_fiktiv.cjs`/`skipped_nachtrag.cjs`, `tagesmomente.cjs`, `protokoll_bilanz.cjs`, `loop_archiv.cjs`, `anker_auto.cjs`, `register_check`/`vc_nr`, `analyse/h1_auswertung`, `analyse/ibf_schatten`, `analyse/rk13_1_filter`. Logs `gate_check_log`, `vollcheck_log`, `momente_log`, `kombi_fiktiv_log`, `skipped_setups_fiktiv`, `oneh_shadow_log`, `q2/q4_schatten_log`, `anker_auto_log`, `sl_anker_wechsel_log`, `trigger_kandidaten_log`, `register_touch_log` werden **nicht mehr beschrieben** — die Messreihen Q-Score, H1 (abgeschlossen 08.10.), IBF, Kombi, tagesmomente ALT/AUTO/FLOOR, 13.1-Diagnose und Zeitbox/Register sind damit **abgeschlossen** (Stand 13.10.).

**Weiter genutzt (unveraendert, nur mit neuen Pfaden):** `size.cjs` (`--risiko`), `kapital_constants.cjs`, `cooldown_check.cjs`, `notbremse_check.cjs` (nur Echtgeld), `quick_tick.cjs --log scripts/kern_quick_tick_log.jsonl`, `loop_stopp.cjs --state/--tick/--log/--fiktiv-log` mit Kern-Pfaden (Kompatibilitaet in Paket K6 pruefen, sonst kleines `kern_stopp.cjs`), `atr_qqq.cjs` (computeAtr/computeEma), `register_touch.cjs` (nur im Start-Update fuer die Level-Liste), `x_fetch_stamp.cjs` (nur Start-Update + vor Entry).

**Schatten im Kern-Pfad (ohne Wirkung):** je Gate "alt haette PASS/FAIL/Q-Ampel" (ueber `gate_check.cjs --dry-run`, best effort, sonst "nicht bewertbar: Grund"), Q-Score, Chop-Verdacht (ADX/Effizienz), Spread-Gate im Fiktivbetrieb (s. offene Punkte).

**Getrennte Messreihe:** nur Dateien mit Praefix `kern_` (`kern_check_log`, `kern_gate_log`, `kern_trades_fiktiv`, `kern_state.json`, `kern_archiv/<datum>.txt`, `kern_level_<datum>.json`); jeder Eintrag traegt `regelwerk: "kern-1.0"` und `testtag_modus`. Die Kern-Auswertung (`kern_bilanz.cjs`) zaehlt ab dem ersten Kern-Tag, alte Dateien liest sie nie. Bars werden weiter als `nas100_5m_<datum>.json` usw. gespeichert (gemeinsame Rohdaten, keine Messreihe).

**Rollback:** Cron-Prompt wieder aus `node scripts/loop_prompt.cjs` (alter Weg) statt `node scripts/kern_loop_prompt.cjs`; vorher `scripts/_freeze_alt_2026-10-14.sha1` (Hash aller alten Skripte/Logs, Paket K0) gegenpruefen. Keine Datei muss zurueckgespielt werden.

**Dauerregeln:** kein `npm run test:e2e` ohne Levi-Vorwarnung; `node --test` nur mit expliziter Pfadliste; kein Commit/Push durch Subagenten ("bereit zum Commit" melden); DST-Fix bis 19.10. — im Kern-Pfad ueber das gemeinsame Modul `kern_zeit.cjs` (ICU-Offset Europe/Berlin vs. America/New_York) und als 4-Zeilen-Fehlerkorrektur in `vollcheck.cjs`.

Auftrag an Fable: [[fable_auftrag_kern_regelwerk_2026-10-14]].
