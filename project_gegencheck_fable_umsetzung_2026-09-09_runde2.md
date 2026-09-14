---
name: project_gegencheck_fable_umsetzung_2026-09-09_runde2
description: "Opus-Gegencheck Runde 2 (09.09.2026): Fables B1-B8-Fixes + vier Nachbesserungen (Fragen 1-4) alle live verifiziert und korrekt. Trotzdem EINGESCHRÄNKT — drei neue Defekte C1-C3 betreffen die Kernfunktion (Vollständigkeitsnachweis): C1 = --qqq-gate-Tippfehler löscht Dual-Gate-Bein UND meldet trotzdem 'vollständig' (dieselbe Klasse wie B4, nicht gefixt); C2 = Format-Zeile kann 'vollständig' zeigen, während Bausteine LUECKE tragen; C3 = --register-touch-Nachweis ist semantisch invertiert (echter Touch verschwindet, unbelegte Behauptung wird angezeigt). Plus: Opus hat vollständige, geprüft lauffähige Kommandotemplates für vollcheck.cjs UND position_tick.cjs geliefert (fehlten bisher komplett in der Regelwerksdatei — dasselbe Risiko wie die 5x nachgebesserte gate_check.cjs-Vorlage, nur mit 52 statt ~25 Pflichtfeldern)."
metadata: 
  node_type: memory
  type: project
  status: open
  modified: 2026-09-09T11:08:33.703Z
  originSessionId: f86d6469-f4c5-46be-9c92-a1637d9a3ef6
---

## Kurzfazit

Zweite Gegencheck-Runde: Opus hat Fables B1-B8-Fixes und die vier Nachbesserungen (Fragen 1-4 aus [[project_gegencheck_fable_umsetzung_2026-09-09]]) mit 31 Live-Testläufen geprüft — **alle acht Fixes plus alle vier Nachbesserungen bestätigt funktionierend**, keine der Fable-Behauptungen widerlegt (Unterschied zur ersten Runde, wo die Punkt-10-Beispielzahl falsch war). Leitplanken weiter eingehalten: kein neues Gate, keine Schwellenänderung, `--batch`-Crash nachweislich vorbestehend.

**Trotzdem erneut EINGESCHRÄNKT** — drei neue Defekte (C1-C3) betreffen genau die Kernfunktion, um die es am 08.09. ging: den belastbaren Vollständigkeitsnachweis.

## Neue Defekte

**C1 [wichtig, B4-Klasse, nicht mitgefixt]:** `--qqq-gate` (und neun weitere Felder: `--qqq-kerze-bestaetigt`, `--tweet-fetch`, `--fib-im-register`, `--folgekerze`, `--muster-9d1`, `--teilverkauf`, `--terminal-geprueft`, `--retest-bedingung`, `--spike-b1/b2/b3`) sind weiterhin ungeprüfte Magic Strings über `toBool()`. Ein Tippfehler wie `--qqq-gate open` (statt `offen`) wird zu `undefined` und löst DREI Folgen gleichzeitig aus: (a) das QQQ-Bein fällt aus dem Dual-Gate → aus 2/2 wird 1/2 → **unterdrückter Trigger**; (b) acht A3-Pflichtfelder werden lautlos nicht mehr verlangt; (c) die Format-Zeile meldet trotzdem "vollständig". Das ist derselbe Fehler wie B4, nur auf dem Feld mit der größten Hebelwirkung — am 08.09. war das einzige Zone-1-Fenster des Nachmittags 60-90 Sekunden breit, ein unterdrückter 2/2-Trigger ist die teuerste Fehlerart im Projekt.

**C2 [wichtig]:** Die Vollständigkeits-Logik in `vollcheck.cjs` führt zwei getrennte Listen (`bausteine`-Anzeige vs. `luecken`-Kipplogik) statt eine aus der anderen abzuleiten. Nicht-parsebare Zahlenwerte (z.B. `--adx nichtgemessen`) erzeugen einen LUECKE-Baustein in der Klammer, kippen aber die Format-Zeile NICHT, weil sie keine A3-Ausnahme sind — Ergebnis: "Voll-Check #12 vollständig (… | ADX LUECKE | …)", ein Selbstwiderspruch in derselben Zeile. Noch schwerer: bei kaputten MTF-Werten (z.B. `--ema50-15m keinwert`) zeigt der Baustein sogar `MTF ✓`, während das NAS100-Bein intern auf "n/a" fällt. Fix-Richtung von Opus: `luecken` aus `bausteine` ableiten (jeder Nicht-✓/n.a.-Baustein kippt), plus Typ-Guard in `resolveA3` für numerische Felder.

**C3 [wichtig]:** `--register-touch`-Nachweis ist semantisch invertiert. `register_touch.cjs` setzt `updated` immer auf die echte Aufrufzeit — Folge: Ein ECHTER Touch macht `register_check.cjs` sofort wieder Exit 0 → die übergebene `--register-touch`-Zeile wird verworfen und taucht im Voll-Check NIE auf. Umgekehrt kann die Zeile nur erscheinen, wenn das Register nachweislich NICHT aufgefrischt wurde (Exit 1/2 bei "Touch"-Behauptung) — dieser Zustand ist mit einem echten Touch logisch unerreichbar, die angezeigte Zeile kann also nur eine unbelegte Behauptung sein. Getestet mit frei erfundenem Belegtext `"register_touch: Testlauf"` — wurde nicht geprüft, Format-Zeile zeigte trotzdem "vollständig (Register-Check HART→neu (Exit 2, Touch ✓))".

**C4 [mittel]:** `vollcheck.cjs --dry-run` ist nicht dicht — der Unterprozess `gate_check.cjs --sl-vorpruefung` schreibt weiterhin `trigger_kandidaten_log.jsonl` und überschreibt `last_sl_vorpruefung.json` (die Referenzdatei, die der spätere Live-Aufruf per `--sl-vorpruefung-ref last` zitiert). Verfälscht TODO-12-Zähler UND kann die SL-Vorprüfungs-Referenz spurlos ersetzen.

**C5/C6 [klein/kosmetisch]:** `atr_qqq.cjs`-CLI-Text weiterhin falsch ohne `--at` (B6 nur in gate_check.cjs gefixt); `position_tick.cjs` hat B5 nicht übernommen (fester "vollstaendig"-Präfix); doppelter Zeilen-Präfix bei Register-Check; Grammatik.

## Kommandotemplates geliefert (bisher fehlend in der Regelwerksdatei)

Opus stuft das Fehlen eines vollständigen, kopierbaren Kommandotemplates für `vollcheck.cjs`/`position_tick.cjs` als dasselbe Risiko ein wie die schon 5x nachgebesserte `gate_check.cjs`-Vorlage — nur größer (52 A3-Pflichtfelder + 11 situative Blöcke bei `vollcheck.cjs` vs. ~25 bei `gate_check.cjs`, und `vollcheck.cjs` läuft 46-84x/Tag statt nur beim Entry). Der aktuelle Kopfkommentar-Beispielaufruf in `vollcheck.cjs` ist selbst NICHT vollständig lauffähig. Opus hat zwei geprüft lauffähige Maximalfall-Templates geliefert (Basisblock + alle situativen Anhänge) — **noch nicht in `feedback_vollcheck_format.md` eingetragen**, das ist der nächste Schritt mit dem größten Ertrag/Aufwand-Verhältnis (keine Codeänderung nötig). Volltext der Templates steht im Opus-Bericht dieser Session (Runde 2, Abschnitt 4) — bei Bedarf dort nachschlagen, bevor sie ins Regelwerk übernommen werden.

## Gesamturteil

`EINGESCHRÄNKT` — fail-safe (alle drei Kern-Defekte unterdrücken Trigger/Nachweise, keiner erschleicht eine Freigabe), kein ABBRUCH. Aufwand für C1/C2 klein (Enum-Kopie aus B4, Ableitungslogik). C3 etwas mehr Denkarbeit.

**Empfohlene Reihenfolge:**
1. Fable: C1 + C2 fixen
2. Hauptagent: die zwei Kommandotemplates in `feedback_vollcheck_format.md` eintragen (kein Code nötig)
3. Fable: C3 + C4, danach C5/C6 + Restposten B9-B11 + Frage 5 (`register_check.cjs --jetzt`)
4. Trockenlauf: ein Voll-Check + ein Positions-Tick, wörtlich aus dem Template kopiert, Exit 0, "vollständig" ohne A3-Ausnahme
5. Erst danach nächster Testtag — Opus' bindende Auflage aus [[project_testtag_analyse_2026-09-08]] bleibt bis dahin offen

Nichts committet.
