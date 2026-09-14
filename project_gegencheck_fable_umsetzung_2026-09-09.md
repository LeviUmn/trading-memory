---
name: project_gegencheck_fable_umsetzung_2026-09-09
description: "Opus-Gegencheck von Fables Umsetzung der 08.09.-TODO-Liste (Punkte 2-12: vollcheck.cjs, position_tick.cjs, gate_check.cjs-Erweiterungen etc.). Urteil EINGESCHRÄNKT: 10/11 Punkte inhaltlich korrekt und live getestet, aber 11 Defekte gefunden (B1-B11), davon 5 blockierend/wichtig — u.a. lässt sich die SL-Anker-Vorprüfung mit --richtung none umgehen, --grund-register-touch ist toter Code (blockiert vollcheck.cjs bei jedem stale Register), Screenshot-A3-Loch, --position ohne Validierung killt den Volumen-Check lautlos. UNGEFIXT."
metadata: 
  node_type: memory
  type: project
  status: open
  modified: 2026-09-09T10:33:28.480Z
  originSessionId: f86d6469-f4c5-46be-9c92-a1637d9a3ef6
---

## Kurzfazit

Opus hat Fables Umsetzung der [[project_testtag_analyse_2026-09-08]]-TODO-Liste (Punkte 2-12) live gegengeprüft — nicht nur Code gelesen, sondern die neuen Skripte (`vollcheck.cjs`, `position_tick.cjs`, `atr_qqq.cjs`, erweiterte `gate_check.cjs`/`add_trade.cjs`/`protokoll_bilanz.cjs`) mit echten 08.09.-Werten testweise ausgeführt. Ergebnis: 10 von 11 Punkten laufen korrekt, keine Leitplanke verletzt (kein neues Gate, keine Schwellenänderung). Aber der Generator, der das Ausführungsdefizit vom 08.09. beheben soll, hat selbst mehrere Umgehungswege, die genau die Pflichtzeilen verschwinden lassen können, um die es am 08.09. ging.

**Urteil: EINGESCHRÄNKT.** Fünf kleine, lokal begrenzte Korrekturen (B1-B5, B7) vor dem nächsten Testtag nötig — Opus schätzt das als "heute noch erledigt"-Aufwand ein (wenige Zeilen pro Fix).

## Gefundene Defekte

**B1 [blockierend]:** SL-Anker-Vorprüfungszeile in `vollcheck.cjs` ist an `richtung === 'long'/'short'` geknüpft statt an "≥1 Dual-Gate-Bein" (wie in `feedback_vollcheck_format.md` E3 spezifiziert). Mit `--richtung none` bei 2/2 bestätigten Beinen entfällt die Zeile lautlos — exakt der P4(a)-Fehler vom 08.09., jetzt mit Skript-Segen.

**B2 [blockierend]:** `--grund-register-touch` ist toter Code — `g['register-touch']` ist immer `undefined`, weil der Schlüssel nicht in der A3-Spec steht. Bei WARNUNG/HART-Register (Normalfall, da Register vom Vortag) bricht `vollcheck.cjs` immer mit Exit 1 ab, auch mit angegebenem Grund. Praktisch: Der Generator läuft in der häufigsten Ausgangslage gar nicht.

**B3 [wichtig]:** Screenshot-Feld hat A3-Loch in beide Richtungen — `--screenshot ausgelassen` ohne Grund erzeugt keine sichtbare Lücke, UND ein mitgelieferter `--grund-screenshot` wird verworfen (falsche Fehlermeldung "kein Grund angegeben").

**B4 [wichtig]:** `--position` ist ein ungeprüfter Magic-String — nur exakt `offen` aktiviert Volumen-Check/Positions-Block. `--position ja` läuft durch als "vollständig", ohne Volumen-Check — die eine Zeile, die am 08.09. bei ~190 Ticks 0-mal erschien.

**B5 [wichtig]:** Format-Zeile meldet "vollständig" auch wenn Bausteine ✗ sind (Tweet-Check ✗, Register HART, Screenshot ausgelassen gleichzeitig getestet) — Vollständigkeits-Flag reagiert nur auf A3-Gründe, nicht auf ✗-Zustände.

**B6 [mittel]:** `atr_qqq` behauptet "letzte abgeschlossene Kerze", nutzt aber alle Bars inkl. evtl. laufender Kerze (kein `--qqq-at`-Cutoff durchgereicht).

**B7 [mittel]:** Neue Statusdateien (`trigger_kandidaten_log.jsonl`, `last_sl_vorpruefung.json`, `setup_karte.json`) fehlen in `.gitignore`.

**B8 [mittel]:** `--vorschau` (Setup-Karte) zeigt Registeralter nicht an — TP1-Kandidat kann aus veraltetem Register stammen, unsichtbar.

**B9-B11 [klein]:** gemischte Zeitquellen (`new Date()` vs. `--jetzt` an 3 Stellen), unklare Pfeil-Notation in `--rechnung`-Ausgabe, `--dry-run` in `add_trade.cjs` nicht vollständig dicht (Unterprozess schreibt trotzdem Statusdateien).

## Punkt-10-Diskrepanz aufgeklärt

Fables Abschlussbericht behauptete für den 08.09. einen Buchverlust von "−10,31 €" — das ist reproduzierbar, aber nur mit erfundenen Demo-Werten aus dem Skript-Kopfkommentar, nicht mit den echten 08.09.-Werten. Korrekt (nachgerechnet über die `size.cjs`-Formel): **−6,50 Pkt → −2,98 € Buchverlust** (deckt sich mit Opus' ursprünglicher Größenordnungsschätzung "+15€ bei TP1-Teilverkauf"). Das Skript selbst ist korrekt, nur Fables Beispielrechnung im Bericht war falsch — vierter Vorfall derselben Fehlerklasse "Bilanzzahl ohne Nachrechnen", ausgerechnet an der Stelle, die genau das verhindern soll.

## Antworten auf die sechs offenen Fragen

1. **Reversal-Schwelle bleibt 2/4**, nicht 3/4 — belegt durch zwei Regelwerksstellen (22.07. Musterbeispiel, 21.08. Dämpfer-Formulierung). Nachbesserung: Exit-Preis-Vorschlag fehlt noch in der Skriptzeile (Pflicht laut Punkt 11), nur "Exit-Vorschlag + ggf. Gegenrichtungs-Setup" ohne konkreten Preis.
2. **RVOL-Schwelle 1,0× nicht als ✓/✗ führen** — diese Schwelle wurde am 25.08.2026 explizit von Levi als Q3-Tiebreaker kassiert (zu nah an der eigenen Grenze, nicht belastbar). Empfehlung: als GEMESSEN-KEIN-GATE-Rohwert wie ADX führen, nicht als Urteil. Nur der 50%-Dämpfer aus Punkt 11 ist belegt und korrekt in `position_tick.cjs`.
3. **UNTAUGLICH bleibt Warnung, kein Hard-Abbruch** — Fables konservative Wahl ist regelkonform (7b1 Schritt 0: "Bindung ist Prozess, kein Hard-Gate im Skript"). Ergänzung empfohlen: `protokoll_bilanz.cjs` soll UNTAUGLICH-Urteile bei tatsächlich live erfolgten Aufrufen als `problems`-Eintrag zählen, plus `--grund-sl-vorpruefung`-Nutzung in Tagesstatistik aufnehmen (Gefahr: Dauerzustand wie bei `--dual-gate-qqq-abstand-atr`).
4. **Terminalbedingung: A3-pflichtig machen, aber NICHT auf 21:45 hartcodieren** — das ist eine Sessionvereinbarung, keine feste Regel. Feld soll `HH:MM` oder `keine` verlangen.
5. **`register_check.cjs` sollte optionales `--jetzt` bekommen** — bei echtem Loop irrelevant (Sekunden-Abweichung), relevant bei Replays. Kein Blocker, aber erst nach B1-B5.
6. **Ja, Regelwerk-Kommandotemplate an ZWEI Stellen in `feedback_live_trading.md` nachziehen** (7b1 Schritt 5 UND die "Gate-Check ausgeführt:"-Pflichtzeile) — beide führen aktuell keines der drei neuen A3-Felder, würden also nach Kopieren zu Exit 1 führen. Exakter Zusatz-Block von Opus geliefert (nach `--chasing`, vor `--tp1-level-price`): `--sl-vorpruefung-ref last` (oder `--sl-vorpruefung-urteil "..."`), `--override-1h-close`, `--override-1h-ema50`, optional `--override-1h-bar-zeit`. Zusätzlich muss `feedback_vollcheck_format.md` `vollcheck.cjs`/`position_tick.cjs` als vorgesehenen Erzeugungsweg benennen, sonst erleiden die neuen Skripte dasselbe Schicksal wie die Regeln, die sie ersetzen sollen.

## Offene To-dos (Reihenfolge)

1. Fable: B1, B2, B3, B4, B5, B7 fixen (alle als "wenige Zeilen" eingeschätzt)
2. Sonnet/Hauptagent: `feedback_live_trading.md` an den zwei genannten Stellen um die drei neuen Pflichtfelder ergänzen (Frage 6)
3. `feedback_vollcheck_format.md`: `vollcheck.cjs`/`position_tick.cjs` als offiziellen Erzeugungsweg dokumentieren
4. RVOL-Zeile in `position_tick.cjs`/`vollcheck.cjs` von ✓/✗ auf GEMESSEN-KEIN-GATE umstellen (Frage 2)
5. Danach Trockenlauf: ein vollständiger Voll-Check + ein Positions-Tick, beide Exit 0 ohne A3-Ausnahme
6. Erst danach: nächster Testtag — Opus' bindende Auflage aus [[project_testtag_analyse_2026-09-08]] bleibt bis dahin offen, kann nur an einem echten Testtag eingelöst werden

Nichts committet. B6, B8-B11 sind niedrigere Priorität (mittel/klein), kein Blocker für den nächsten Testtag laut Opus.
