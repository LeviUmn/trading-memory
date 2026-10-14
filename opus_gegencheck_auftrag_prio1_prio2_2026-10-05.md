---
name: opus_gegencheck_auftrag_prio1_prio2_2026-10-05
description: "Opus-Gegencheck-Auftrag (Prompt fuer frischen Opus) zur UMSETZUNG Prio 1 + Prio 2, Commit 20d21aa; Frist Mo 05.10.2026 14:30 DE; Status: Auftrag bereit, Gegencheck noch nicht gelaufen"
metadata:
  node_type: memory
  type: project
  originSessionId: 627984e6-a710-48e6-9244-0efcc80854ad
  modified: 2026-10-05T08:28:35.900Z
---

# Opus-Gegencheck der Umsetzung Prio 1 + 2 (Commit 20d21aa) — Frist Mo 05.10.2026 14:30 DE

**Stand 05.10. 10:30 (Sonnet):** Umsetzung ist committet und gepusht (20d21aa, 03.10. 17:27; 4 Dateien: `loop_prompt.cjs`, `vc_nr.cjs`, `vollcheck.cjs`, `tests/trading_scripts.test.js`). Der Gegencheck der UMSETZUNG ist im Memory nicht dokumentiert (abgenommen wurde bisher nur der Auftragsentwurf, [[fable_auftrag_2026-10-03_prio1_prio2]]). Sonnet-Abnahmelauf `loop_prompt.cjs --testtag fiktiv --terminal-zeit 20:02` mit echten Memory-Dateien: Exit 0, Selbsttest gruen, 7 Pflichtfragmente vorhanden.

**Why:** Auftrag R-1: Prio 1 + Prompt-Paket = EINE Betriebsaenderung am 05.10., nur mit Opus-Gegencheck bis 14:30. Prio 1 steckt bereits im committeten `vollcheck.cjs` und ist damit heute aktiv, auch ohne Gegencheck.
**How to apply:** Prompt unten an frischen Opus geben. Bei Abnahme: Memory-Index-Zeile ersetzen. Bei Beanstandung an Prio 1: Levi entscheidet ueber Rueckbau von `vollcheck.cjs` auf f92a4e0 vor 15:15 (an Testtagen nicht zwischen 15:15 und Loop-Stopp aendern).

## Prompt fuer Opus (kopierbar)

Du bist frischer Opus-Gegenchecker. Pruefe die UMSETZUNG (nicht den Entwurf) von Prio 1 + Prio 2 im Commit 20d21aa (Repo C:\Users\umnus\tradingview-mcp, Vorgaenger f92a4e0). Auftrag und Auflagen [O1]-[O14]: Memory `fable_auftrag_2026-10-03_prio1_prio2.md`, Abschnitt "Gegencheck-Punkte (Opus nimmt ab)" 1-5. Frist: heute 14:30 DE (Start-Update 15:00). Nur lesen/pruefen: KEIN git add/commit/push, KEIN Edit an Live-Skripten, KEIN `npm run test:e2e`, kein `node --test tests/` ohne Pfadliste. Alles, was Logs schreibt, nur aus einer Kopie des ganzen `scripts`-Ordners in `$TEMP`.

Pruefe der Reihe nach:
1. `git show 20d21aa -- scripts/vollcheck.cjs`: Warnzeile nur im Vorschlags-Zweig (Flag `vorschlagAktiv13_1`), nur bei bias5 und dgRichtung in {long, short} und ungleich; Text ohne Handlungsvorgabe; Konsequenz-Zeile byte-identisch; Log-Feld `richtungskonflikt_13_1` bedingt (Spread wie `nr_luecke`), nur im Exit-0-Record, `v4.*` zusammen mit `v4.konsequenz`. Golden-Tests (`tests/trading_scripts.test.js` ab ~Z. 4561) unveraendert.
2. Byte-Identitaet (O5): HEAD~1-Kopie gegen HEAD im Konflikt-Szenario in der Sandbox fahren; stdout-Diff = genau 1 Zeile, Log-Diff = genau 1 Feld.
3. Rueckrechnung Prio 1 ueber `vollcheck_log.jsonl` x `oneh_shadow_log.jsonl` (lesend, Kopie). Erwartung: 02.10. 1 VC (#27); 01.10./30.09. 0; 16.09. #26/#27; 28.09. #52. Abweichung melden, nicht passend machen.
4. `scripts/vc_nr.cjs`: stdout nur die Zahl, Fehler stderr + Exit 1, Vortags-State (`datum_de` != Datum von `--jetzt`) -> 1, `--erster-vollcheck` hat Vorrang, kein Schreibzugriff, Paritaet zu `vcNummer` (vollcheck.cjs ~Z. 496). Fixture `erster = 2026-10-02T13:27:09Z`: 13:27:09Z -> 1; 17:26:16Z -> 49; 17:31:11Z -> 50; Slotgrenzen :x4:59/:x5:00.
5. `scripts/loop_prompt.cjs`-Diff: C4/C5 wie `fable_vorschlag_c4_c5_loop_prompt_2026-10-02.md`; [O7]-Klammer durch Slot-Regel ersetzt; P2.1 Aufrufmuster (`N=$(node scripts/vc_nr.cjs --jetzt <J>) || N=x; ...`), P2.2 Slotberechnung (T 20:02 -> {20:00}; T 20:00 -> {19:55, 20:00}; 'keine' -> keine Zeile), P2.3 `--grund-chasing` als eigener `block.push` ohne den Satz "no nur wenn ...", P2.4 "Fire im laufenden Slot" (Levi-Ja 03.10.); Diff sonst additiv, keine Gate-/Regeltexte geaendert. `loop_stopp.cjs` und `gate_check.cjs` unveraendert.
6. Betrieb: `npm test` (nur Offline-Suiten) gruen; sha1 vor/nach von `momente_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `kombi_fiktiv_log.jsonl`, `gate_check_log.jsonl`, `vollcheck_log.jsonl`, `vollcheck_state.json`, `loop_archiv/*.txt` identisch; `loop_archiv.cjs --datum 2026-10-02 --out-dir $TEMP/la_test` Exit 0.
7. Offene Frage zur Einordnung: Ist die Warnzeile (O4) wirklich "reine Messung/Schatten-Zeile" im Sinne von R-1 (hoechstens eine Live-Aenderung pro Testtag)?

Ausgabe: Urteil je Punkt 1-7 (OK / Auflage / Blocker), Gesamturteil "freigegeben" / "freigegeben mit Auflagen" / "nicht freigegeben bis 14:30 -> Prio 1 erst 06.10." mit Rueckbauanweisung, Zeitstempel Start/Ende. Ergebnis als Memory `opus_gegencheck_umsetzung_prio1_prio2_2026-10-05.md` ablegen.
