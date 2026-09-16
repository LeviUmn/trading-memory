---
name: project_regelwerk_entscheidung_2026-08-31
description: "Levis Entscheidung zu den 7 Vorschlägen aus dem Opus-Review des Testtags 28.08.2026 ([[project_testtag_analyse_2026-08-28]] Abschnitt 18) + Fable-Umsetzung am 31.08.2026 — alle 7 umgesetzt. Kern: gate_check/cooldown_check-Output in Datei + Datei-Zitierpflicht, Gate-Eingaben-Herkunftszeile, 8d-Einzelkriterien, Voll-Check-Nr. mit sichtbarer Rechnung, 6. Faktenprotokoll-Unterpunkt (Option i), GC-5-Öffnungsklausel, add_trade.cjs --dry-run (einzige Code-Änderung, unkommittet). NEU: Abschnitt 'Nachbesserungen nach Opus-Gegencheck' — tee durch >-Redirect + Exit-Code-Zeile ersetzt, stale CLI-Fassungen angeglichen, 'gilt rückwirkend' bei Kriterium 2 zurückgenommen."
metadata:
  node_type: memory
  type: project
  status: done_all
  originSessionId: session-2026-08-31
  modified: 2026-08-31T12:33:12.274Z
---

# Levi-Entscheidung zu den Opus-Vorschlägen (28.08.2026-Review), umgesetzt 31.08.2026

Quelle der 7 Vorschläge: [[project_testtag_analyse_2026-08-28]] Abschnitt 18 (priorisierte Liste, bewusst kurz gehalten — Konsequenz-Regel des Validierungstesttags: "nur das konkret gescheiterte Element fixen, kein Rundumschlag"). Levi hat alle 7 freigegeben, bei Vorschlag 5 explizit **Option (i)** gewählt. Umsetzung durch Fable (Regelwerk-Autorschaft, [[feedback_modellwahl_trading]]) am 31.08.2026. Vorbild-Muster: [[project_regelwerk_entscheidung_2026-08-28]].

## Punkt 1 — gate_check/cooldown_check-Output in Datei schreiben, daraus zitieren: ERLEDIGT

**⚠️ Aufrufweg ÜBERHOLT noch am 31.08.2026:** Die unten dokumentierte `| tee`-Fassung maskierte in einer Bash-Pipeline den Exit-Code des Node-Skripts — nach Opus-Gegencheck ersetzt durch `>`-Redirect + Exit-Code-Zeile im selben Befehl, siehe Abschnitt "Nachbesserungen nach Opus-Gegencheck" unten. Die beiden folgenden Zeilen bleiben nur als historischer Stand stehen, NICHT anwenden.

Umgesetzt in `feedback_live_trading.md`, an beiden Stellen der wörtlichen Ausgabepflicht:
- **CronCreate-Prompt Item (7):** Aufrufweg jetzt `node scripts/cooldown_check.cjs | tee scripts/last_cooldown_check.txt`, ins Protokoll kommt der per Read/cat gelesene Dateiinhalt — Mechanismus gilt skriptunabhängig, obwohl cooldown_check am 28.08. auch ohne Datei zeichengenau war.
- **CronCreate-Prompt Item (8) + Punkt 7b1 (Verschärfung 26.08., zweite Hälfte):** `node scripts/gate_check.cjs <Parameter> | tee scripts/last_gate_check.txt`, zitiert wird der vollständige Dateiinhalt. "Wörtlich" ist damit ein Kopiervorgang statt einer Disziplinfrage; der Prüfer kann die Datei gegenlesen. Kein Code-Change an den Skripten (wie vorgesehen).

## Punkt 2 — Rohwert-Pflicht auf die gate_check-EINGABEN ausgedehnt: ERLEDIGT

Umgesetzt in `feedback_live_trading.md`, CronCreate-Prompt Item (8): neue **Gate-Eingaben-Pflichtzeile** VOR jedem `gate_check.cjs`-Aufruf, Muster `Gate-Eingaben: ATR(5min) 45,4 (abgelesen <Quelle/Zeit>) | Cluster-Level 29.563,95 (<welcher Wick, wann>) | TP2-Level 29.213,90 (<Herkunft, z.B. Session-Pivot S2>)`. Jeder CLI-Zahlenwert muss vorher hergeleitet/abgelesen im Protokoll stehen; fehlt die Zeile, gilt 7b1-Schritt 5 als nicht abgeschlossen.

## Punkt 3 — Regime-Gate 8d: alle drei Kriterien einzeln ausweisen: ERLEDIGT

Umgesetzt in drei Dateien (dasselbe Format, an jeder Stelle, an der die 8d-Zeile wörtlich vorgegeben ist — die dritte Stelle war nötig, weil der CronCreate-Prompt sonst das alte Format weiter diktiert hätte):
- `feedback_chartanalyse.md` Punkt 8d: neuer Absatz "Alle drei Kriterien einzeln ausweisen" mit Pflichtformat `Regime-Gate (8d): Schock-Tag ✗ (0/3) [Range <X> Pkt / ATR-D <Y> = <Z>× ✗/✓ | Makro-Häufung ✗/✓ | VIX-Range <W>% ✗/✓ oder "nicht gemessen"] | Regime: Trend/Chop` — "nicht gemessen" als zulässige ehrliche Alternative (v.a. VIX), keine neue Schwelle, kein neues Gate.
- `feedback_vollcheck_format.md`: Template-Zeile 8d entsprechend erweitert + Begründungsabsatz "8d-Einzelkriterien-Klammer".
- `feedback_live_trading.md` CronCreate-Prompt Item (5): Format-String angepasst.

## Punkt 4 — Voll-Check-Nummer mit sichtbarer Rechnung in der Kopfzeile: ERLEDIGT

Umgesetzt in `feedback_vollcheck_format.md`: Kopfzeilen-Template jetzt `Voll-Check <Uhrzeit> (Nr. <N> = (<Minuten seit erstem Voll-Check>)/5+1) — <1-Satz-Einordnung>`; neuer Absatz "Verschärfung 31.08.2026" mit der korrigierten Diagnose (28.08.: Nummer nicht mitgezählt, sondern mit dem Fire-Zähler verschmolzen, +2 bis +5 pro Slot bis Nr. 98 statt 52) — die Rechnung wird jedes Mal ausgeschrieben, nicht nur ihr Ergebnis.

## Punkt 5 — Ort der 6 Tagesabschluss-Pflichtzeilen (LEVI: Option i): ERLEDIGT

Umgesetzt in `feedback_tagesabschluss.md`, Abschnitt "Faktenprotokoll-Abschluss": aus 5 werden **6 feste Unterpunkte** — neuer Unterpunkt 6 "Pflicht-Abschlusszeilen", rein faktisch befüllt (SL-Hit-Typ mit Punkt-11-Zähler X/4 [nur bei SL-Hit], DB-Sync, Regelkonformität geprüft, Skipped-Setups aufgelöst, SQL-Dump aktualisiert, B1-B3-Schattenmessung geprüft). An fiktiven Testtagen faktische Fassung (z.B. `DB-Sync: NEIN (fiktiver Testtag, kein trades.db-Schreibzugriff)`). Die "verbotenen Satztypen"-Liste ist unverändert und gilt jetzt ausdrücklich für alle 6 Unterpunkte gleichermaßen.

## Punkt 6 — Öffnungsklausel für Kriterium 2 des Validierungstesttags: ERLEDIGT

Umgesetzt in `project_validierungstesttag_naechster_handelstag.md`, Kriterium 2: "Auch die korrekte Ausgabe 'nicht einschlägig' mit Begründung (z.B. Vorbedingung SL-Floor+Zone-1 nicht erfüllt) zählt als bestandener Durchlauf" — dieselbe Öffnung wie in Kriterium 3, weil GC-5 konstruktionsbedingt nur bei "SL-Floor bestanden UND Zone 1" greift.

## Punkt 7 — add_trade.cjs-Trockenlauf an fiktiven Testtagen: ERLEDIGT (mit Code-Fix)

**Verifikations-Befund (wichtig, weicht von der Annahme im Auftrag ab):** `scripts/add_trade.cjs` hatte KEINEN Schreibschutz für vollständige Aufrufe — der Hard-Exit vom 27.08. greift nur bei UNVOLLSTÄNDIGEN Feldern; bei vollständigen, gültigen Feldern schrieb das Skript IMMER per `upsertTrade()` in `trades.db` (keine `--confirm-write`-artige Option existierte). Ein vollständiger Testaufruf am fiktiven Testtag hätte also tatsächlich geschrieben.

**Deshalb (wie im Auftrag für diesen Fall vorgesehen) minimal-invasiver Code-Fix:** Neues Flag `--dry-run` in `scripts/add_trade.cjs` — führt alle Checks aus (Pflichtfelder, Messfeld-Pflicht, Format-/Enum-/Plausibilitätschecks) und endet VOR dem Öffnen der DB mit `DRY-RUN OK …`/Exit 0. Getestet am 31.08.2026: vollständiger Aufruf → DRY-RUN OK, `trades.db`-mtime unverändert (27.08. 10:12); unvollständiger Aufruf → Hard-Exit 1 wie bisher. **NICHT committet — Repo-Änderung, Freigabe durch Levi steht aus.**

**Regelwerk:** `feedback_live_trading.md`, neuer Absatz "add_trade.cjs-Trockenlauf an fiktiven Testtagen — ausdrücklich ERWÜNSCHT, kein Verbot": Nach jedem abgeschlossenen fiktiven Trade Aufruf mit allen 7 Pflicht-Messfeldern **plus `--dry-run`** (Pflicht-Bestandteil, da das Skript ohne Flag schreibt); Skript-Ausgabe wörtlich ins Protokoll. Querverweis auch in `feedback_tagesabschluss.md` Unterpunkt 6/3.

---

## Offene Punkte (notiert, NICHT umgesetzt — Leitplanke "kein Rundumschlag")

- **Git-Commit für `scripts/add_trade.cjs` (`--dry-run`-Flag) steht aus** — Levi muss die Repo-Änderung freigeben und committen (lassen).
- Kriterium 4 des Validierungstesttags formuliert weiterhin "Ein sauberer Abbruch bei einem unvollständigen fiktiven Testaufruf zählt als Beleg" — mit dem neuen `--dry-run` ist jetzt auch der VOLLSTÄNDIGE Aufruf gefahrlos und der stärkere Beleg; die Kriterien-Formulierung selbst wurde bewusst nicht angefasst (nur Kriterium 2 war freigegeben).
- 7b1c-Wortlaut-Unschärfe aus dem Opus-Review (Abschnitt 13(2)): "Zeile entfällt ersatzlos" vs. die in der Praxis bessere explizite "entfällt, weil …"-Ausgabe — von Opus selbst als "sollte einmal angeglichen werden" markiert, war nicht Teil der 7 freigegebenen Punkte.
- Die weiteren 28.08.-Befunde ohne Vorschlagsstatus (Positionsgröße/€/Hebel im Positions-Kasten, 9d1-Vorrangklausel vor Stall-Aktion, 5min-RSI/MACD-H-Ausfälle, ADX-Zeitebenen-Vermischung) bleiben Gegenstand des nächsten fokussierten Testtags bzw. künftiger Opus-Reviews — keine neuen Regeln dazu in dieser Runde.

---

## Nachbesserungen nach Opus-Gegencheck (31.08.2026)

Der unabhängige Opus-Gegencheck ([[project_gegencheck_fable_umsetzung_2026-08-31]]) fand 3 Reparaturbedarfe; Levi hat alle 3 freigegeben, Fable hat sie am 31.08.2026 umgesetzt:

**Nachbesserung 1 — `| tee` maskierte den Exit-Code (Punkt 1 oben korrigiert):** Der am Vormittag eingeführte `| tee`-Aufrufweg gab in einer Bash-Pipeline den Exit-Code von `tee` zurück, nicht den des Node-Skripts — das "Exit-Code 2 = kein Trade"-Signal (cooldown_check: 2 bei Cooldown; gate_check Live-Modus: 2 bei FAIL UND UNKNOWN, verifiziert am Skript-Code, 3 = UNKNOWN nur im Batch-Modus) ging verloren. Neuer, einziger Aufrufweg in `feedback_live_trading.md` (CronCreate-Prompt Item 7 + 8, 7b1 Schritt 5, `Gate-Check ausgeführt:`-Pflichtzeile, Datei-Zitier-Fix-Absatz): `>`-Redirect in die `last_*_check.txt`-Datei plus Exit-Code-Abfrage **im selben Befehl** (`; echo "Exit-Code: $?"` im Bash-Tool bzw. `; Write-Output "Exit-Code: $LASTEXITCODE"` in PowerShell, dort ohne `2>&1` wegen PS-5.1-NativeCommandError-Verhalten); zitiert wird weiterhin der per Read/cat gelesene Dateiinhalt. Nebenbefund mitrepariert: `scripts/last_gate_check.txt` + `scripts/last_cooldown_check.txt` in die Repo-`.gitignore` aufgenommen (Repo-Änderung, NICHT committet — Levi-Freigabe steht aus, wie beim `--dry-run`-Commit).

**Nachbesserung 2 — stale CLI-Fassungen angeglichen:** `feedback_live_trading.md` 7b1 Schritt 5 (Kommandotemplate) und die `Gate-Check ausgeführt:`-Pflichtzeile tragen jetzt denselben Redirect-Aufrufweg; `project_risikomanagement.md` Workflow Schritt 0 (cooldown_check) ebenso; zusätzlich gefundenes Vorkommen `feedback_session_update.md` (informativer Morgen-Check) verweist jetzt auf den einheitlichen Aufrufweg statt auf den nackten Aufruf. Es gibt damit wieder nur EINE Fassung pro Skript (dasselbe Muster wie Paket 6b/K2 vom 27.08.). Bewusst NICHT angefasst: der getrennte DAX-Aufruf mit `TRADE_LOG_PATH` in [[project_dax_erweiterung]] (eigener DAX-Kontext, siehe [[feedback_dax_trennung]]) sowie rein historische Doku-Erwähnungen alter Aufrufe in Analyse-/Prüfdokumenten.

**Nachbesserung 3 — "gilt rückwirkend" bei Kriterium 2 zurückgenommen:** In `project_validierungstesttag_naechster_handelstag.md` wurde der nicht beauftragte Halbsatz gestrichen, der den 28.08.-GC-5-Lauf rückwirkend als bestandenen Durchlauf wertete. Die Öffnungsklausel (Opus-Vorschlag 6) gilt jetzt ausdrücklich NUR für künftige Testtage; der 28.08.-Lauf bleibt bei "unentschieden" ([[project_testtag_analyse_2026-08-28]] Abschnitt 15, Kriterium 2). Am Gesamtergebnis (kein Echtgeld-Go) ändert sich nichts, da Kriterien 1/4/5 ohnehin nicht bestanden waren.

**Nicht Teil dieses Auftrags, weiterhin offen (aus dem Gegencheck):** Punkt 4 (Voll-Check-Nummernregel fehlt im CronCreate-Tick-Prompt) und der Rest zu Punkt 5 (Unterpunkt 3 des Faktenprotokoll-Abschlusses listet `add_trade.cjs --dry-run` und den Datei-Zitier-Mechanismus nicht mit) — beide von Levi in dieser Runde nicht freigegeben, nur notiert.

**Rohprotokoll-Verweis (nachgetragen 16.09.2026, Memory-Aufräumen S2/E4):** Das vollständige Rohprotokoll des fiktiven Testtags vom 31.08.2026, dessen Opus-Zweitreview die Nachbesserungen in dieser Datei auslöste, liegt in [[testtag/testtag_2026-08-31]] — war bisher von keiner Datei aus verlinkt.
