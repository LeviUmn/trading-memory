---
name: project_gegencheck_fable_umsetzung_2026-09-09_todos_nach_testtag
description: "Opus-Gegencheck der 11 Fable-TODOs aus der Testtag-09.09.-Analyse: 9/11 sauber live bestätigt (TODO 6 Format-Zeilen-Verifikation und TODO 9 Retest-Zeitbox-Erweiterung stärkste Arbeiten), 1 blockierender Defekt (F1: --impuls-pkt-Hard-Exit killt den 2. Voll-Check im dokumentierten Ablauf), Fables TODO-3-Zahl (11 statt korrekt 10+1 Formverstoß) war falsch. Opus hat 2 triviale Defekte selbst gefixt (F3 MIT-LÜCKEN-Erkennung, F4 R-Zahlen-Guards). Teil B: save_path an data_get_ohlcv MIT AUFLAGEN freigeben, Retest-Zeitbox-Regeltext in feedback_chartanalyse.md 8b nachziehen (Levi-Bestätigung nötig). Status EINGESCHRÄNKT bis F1 behoben."
metadata:
  node_type: memory
  type: project
  status: eingeschraenkt
  originSessionId: f86d6469-f4c5-46be-9c92-a1637d9a3ef6
  modified: 2026-09-09T18:36:16.400Z
---

## Kurzfazit

Opus hat die 11 Fable-TODOs aus [[project_testtag_analyse_2026-09-09]] unabhängig live geprüft (CLI-Läufe, Missbrauchstests, synthetische Gegenprotokolle, nicht nur Codelesen). 9 von 11 Punkte sind sauber und live bestätigt. Ein blockierender Defekt (F1) muss vor dem nächsten Testtag behoben werden. Zwei triviale Defekte (F3, F4) hat Opus selbst gefixt. Fables eigene Erfolgsmeldung zu TODO 3 war an einer Stelle falsch (Kopfzahl 11 statt real 10+1).

## Punkt-für-Punkt (Kurzform, Details im Opus-Bericht dieser Session)

- **TODO 1** (Kommandotemplate im Cron-Prompt): BESTÄTIGT. Kleiner Befund: `MEMORY_DIR_CANDIDATES`-Reihenfolge könnte eine veraltete Kopie im Repo-Ordner still bevorzugen — nicht dringend.
- **TODO 2** (`--state`): BESTÄTIGT, aber **F1 [BLOCKIEREND]**: `--impuls-pkt` wird laut Regelwerk ab VC#2 aus dem State erwartet, kommt aber nur aus dem State, wenn jemals `--impuls-ursprung` gesetzt wurde — im dokumentierten Standardablauf (VC#1 mit `--impuls-pkt`, nicht `--impuls-ursprung`) killt das VC#2 mit Hard-Exit 1. Zweimal reproduziert. Fix: entweder `impuls_pkt` als reinen Fortschreibewert in den State legen, oder `--impuls-ursprung` bei VC#1 zum Pflichtfeld machen und `--impuls-pkt` aus der State-Liste streichen. **F2 [klein]:** Tageswechsel-Hinweis "State verworfen" wird beim fehlschlagenden Erstlauf unterschlagen (gleiche Ursache wie F1: Hinweise werden erst im Erfolgsfall gedruckt).
- **TODO 3** (protokoll_bilanz-Regexe): TEILWEISE BESTÄTIGT. Kopfzeilen 18/18 und `is22` korrekt. **Fables Zahl "11 2/2-ohne-7b1" ist falsch** — Skript liefert real 10 + 1 separat ausgewiesenen Formverstoß (VC#14 "zurück UNTAUGLICH" ohne wörtliche URTEIL-Zeile), macht in Summe zwar 11, aber die Kopfzahl selbst zeigt 10. Nichts versteckt, aber die N2-Erfassungspflicht wird um eins untertrieben, wenn man nur die Kopfzahl liest. Nicht selbst gefixt (Entscheidung, kein Trivialfix) — Empfehlung: Formverstoß-Fälle in die Blockstatistik mit aufnehmen.
- **TODO 4** (Tweet-Fälligkeit maschinell): BESTÄTIGT, alle 4 Zweige live getestet (nicht fällig / überfällig / über-polling / Rohwerte).
- **TODO 5** (Skipped-Setup-Erfassung): BESTÄTIGT, MFE/MAE/R-Vorzeichen für alle 6 Long/Short×Ausgang-Fälle korrekt geprüft, VC#12-Reproduktion exakt (−0,63 R). **F4 [von Opus gefixt]:** ohne `--hoch`/`--tief` konnte das Skript strukturell unmögliche R-Zahlen liefern und einen Terminalkurs außerhalb der Bewegungsspanne akzeptieren — 2 Guards ergänzt. **F5 [offen, P2]:** keine Deduplizierung — VC#9/10/12 (eine Beobachtung laut Opus' eigener Analyse) würden als 3 Einträge gezählt. **F6 [trivial, offen]:** `skipped_setups_fiktiv.jsonl` fehlt in `.gitignore`.
- **TODO 6** (Format-Zeilen-Verifikation): BESTÄTIGT, stärkster Punkt der Runde. Reale Zahlen (18/18 MTF-Frische, 18/18 ADX ohne Zeile) bestätigt, Missbrauchstest mit synthetischem Skript-Protokoll: 0 Falschmeldungen. **F3 [von Opus gefixt]:** `formatCount`/`formatNrs` erkannten nur "vollständig", nicht "MIT LÜCKEN" — hätte sobald `vollcheck.cjs` real läuft bei jedem Lücken-Fall eine Falschmeldung "Kopfzeile ohne Format-Zeile" erzeugt. Gefixt und gegen 5 reale Testtage regressionsgeprüft.
- **TODO 7** (QQQ-ATR/EMA50 selbst rechnen): Skriptseite BESTÄTIGT (`computeEma` gegen Referenzimplementierung geprüft, Guards für zu wenige Bars/falschen Timeframe). MCP-Seite (`save_path`) siehe Teil B.
- **TODO 8** (1H-Schatten-Logging): BESTÄTIGT, `--dry-run` weist jetzt "AUSGESETZT" aus statt zu schweigen.
- **TODO 9** (Retest-Zeitbox-Erweiterung): BESTÄTIGT, sauber, komplette End-to-End-Kette (PASS+Q-ROT → Zeitbox → VC+1 wartet → VC+2 VERFALLEN → verschwindet danach) live durchgespielt. Regeltext-Frage siehe Teil B.
- **TODO 10** (Terminalbedingung): BESTÄTIGT.
- **TODO 11** (Append-Log): BESTÄTIGT, reale Datei zeigt genau die 4 Live-Aufrufe ohne wörtliches Zitat, die Opus in der Testtag-Analyse fand. **F7 [P3, offen]:** keine Rotation, ~5 MB/Monat Wachstum geschätzt.

## Teil B — Entscheidungen

**B1 — `save_path` an `data_get_ohlcv` (src/tools/data.js): MIT AUFLAGE FREIGEBEN.** Kompatibilität unbedenklich (optional, additiv). Drei reale Risiken nachgestellt: (a) unbeschränkter Schreibpfad — Tippfehler/Pfadfehler könnte `level_register.json`, `trades.db`, `x_last_fetch.json` etc. überschreiben; (b) Lesewerkzeug wird ohne Permission-Prompt zum Schreibwerkzeug; (c) relatives cwd könnte bei falschem Startverzeichnis zu stiller Falschzahl führen; (d) `save_path` mit `summary=true` wird still ignoriert. Auflagen: Pfad gegen festes Projekt-Basisverzeichnis auflösen und auf `scripts/` + festes Namensmuster beschränken, `.json`-Endung erzwingen, bekannte Zustandsdateien namentlich sperren, Fehler statt Stille bei `summary=true`, `.gitignore`-Eintrag. Zusätzlich: **Trockenlauf vor dem nächsten Testtag zwingend** (nach MCP-Neustart: `save_path` → `atr_qqq --ema-bars` → `gate_check --qqq-bars-5m`), sonst hängt TODO 7 zum vierten Mal in Folge am `--grund`-Ausfall.

**B2 — Retest-Zeitbox-Regeltext (feedback_chartanalyse.md 8b): NACHZIEHEN, Code NICHT zurücknehmen.** Begründung: Zeitbox erlaubt keinen Trade, erzwingt nur Dokumentation/VERFALLEN — Erweiterung kann nur mehr Disziplin erzeugen, nie Risiko. Schließt exakt das am 09.09. gemessene Loch (VC#1, VC#12 ohne VERFALLEN-Erklärung). Alter Wortlaut ("nach Gate-FAIL") ist Artefakt aus einer Zeit vor PASS+Q-ROT als regelmäßigem Ausgang, keine bewusste Beschränkung. `feedback_vollcheck_format.md` nennt PASS+Q-ROT bereits als Geltungsfall — Zurücknehmen würde zwei Regelwerksdateien in Widerspruch bringen. Konkreter Textvorschlag: 8b Schritt 4 auf "nach Gate-FAIL/UNKNOWN sowie nach Gate-PASS mit Q-Score ROT, wenn ausgelassen wurde" erweitern. **Braucht Levis Bestätigung**, da formale Prozessregeländerung.

## Von Opus angewendete Fixes
- `scripts/protokoll_bilanz.cjs` (F3): `formatCount`/`formatNrs` erkennen jetzt auch "MIT LÜCKEN"
- `scripts/skipped_fiktiv.cjs` (F4): 2 Plausibilitäts-Guards gegen unmögliche R-Zahlen im `--nachtrag`

Kein Commit, kein Push. Alle von Opus' Tests berührten Laufzeitdateien wiederhergestellt.

## Status
**EINGESCHRÄNKT — F1 muss vor dem nächsten Testtag behoben werden** (killt sonst den 2. Voll-Check im Standardablauf, genau der Rückfall, den TODO 1+2 verhindern sollten). F2/F5/F6/F7/TODO-3-Kopfzahl sind P2/P3, kein Blocker. B1/B2 warten auf Levis Entscheidung.
