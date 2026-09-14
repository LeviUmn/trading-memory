---
name: project-gegencheck-fable-umsetzung-2026-09-07
description: "Opus-Gegencheck der Fable-Umsetzung (Runde 1) der Auftragsliste vom 04.09. — 12/13 Punkte am Code verifiziert OK, 1 Korrektur (O1). 5/6 Fable-Gegenbefunde bestätigt, 1 widerlegt (Datenzugriff NICHT blockiert). #9 (ATR 21.08.) jetzt real gemessen: 38,4/42,3 Pkt, widerlegt Opus' eigene Kalibrierungshypothese. E1-E7: konkrete Empfehlungen für Fable Runde 2, u.a. Kurswechsel bei E2 (30-Pkt-Sockel bleibt, Vorschlag zurückgezogen)."
metadata: 
  node_type: memory
  type: project
  status: draft
  modified: 2026-09-07T16:48:12.805Z
  originSessionId: b594a3be-34a1-46da-8616-0f50ceb56daa
---

# Opus-Gegencheck der Fable-Umsetzung (07.09., Runde 1 → Vorgaben für Runde 2)

## Teil 1 — Gegenprüfung, am Code ausgeführt (nicht nur gelesen)

**Alle 12 Umsetzungspunkte bestätigt OK** (#1a/#1b, #2a/#2b, #5a, #6a/b/c, #7, #8-Werkzeug, Anhang A, `.gitignore`+BOM-Fix), mit Zeilenangaben live gegengetestet — u.a. TP2-Kandidatenzeile, A3-Pflicht+`none`, Zonenband-Geometrie inkl. Short-Vorzeichen, 8c2-Gegenrechnung, Wiederholungssperre, Register-Frische. Alle Skripte `node --check` sauber, keine Batch-Regression.

**Eine Korrektur nötig — O1:** `protokoll_bilanz.cjs` erzeugt einen strukturellen Dauer-Fehlalarm bei Register-Updates für jedes Protokoll, das älter ist als der Touch-Log ("Dokument behauptet 4, gezählt 0"). Fix: Wenn der Touch-Log für das Protokolldatum keine Einträge hat und sein ältester Eintrag jünger ist als das Protokolldatum → Ausgabe "nicht messbar (Log beginnt am …)" statt Abweichung/Exit 1.

### Fables sechs Gegenbefunde — Bewertung
| # | Befund | Urteil |
|---|---|---|
| 1 | Bandlogik behandelt #42 milder als 04.09. | **Bestätigt.** Opus' Option C ist tot. |
| 2 | ATR-Hypothese aus F4(3) durch `trading_2026-08-21.md` widerlegt | **Bestätigt und jetzt real gemessen** (siehe O2). |
| 3 | Zwei "wörtliche" Zitate im 04.09.-Protokoll abgekürzt | **Bestätigt** (Aufruf-Zeilen 1743/1805, Ausgabeblöcke selbst vollständig). |
| 4 | Stiller Formatwechsel ab VC#49 | **Bestätigt**, 11× ohne Sekunden/Formel, nicht offengelegt. |
| 5 | `data_get_ohlcv` blockiert #8/#9 strukturell | **Widerlegt.** Zwei Workarounds funktionieren, siehe O3. |
| 6 | UTF-8-BOM-Fix | **Bestätigt**, korrekt gelöst. |

### Opus' eigene neue Befunde
**O2 — #9 ist jetzt gemessen** (via `replay_start 2026-08-22` + `atr_from_ohlcv.cjs` auf echte 5min-Bars 21.08.): ATR(14,5min) bei #42 (16:07) = **38,4 Pkt**, bei #43 (16:57) = **42,3 Pkt**. SL-Distanz beider Trades = 53 Pkt → nur **1,25-1,38× ATR**, unter dem normalen 1,5×-Floor, nicht nur unter dem Schock-Floor 2,5× — beide Trades waren schon deshalb regelwidrig platziert. Der 30-Pkt-Sockel dominierte am Ursprungstag genauso wie am 04.09. (0,5×ATR dort = 19-21 Pkt). Originalregel vom 21.08. wörtlich: "SL in einer bereits per Wick getesteten Zone (±20-30 Pkt) braucht zusätzlich min. 0,5× ATR Puffer" — der Sockel ist die Regel, kein verrutschter Parameter. Nachtrag für `trades.db`: `atr_at_entry` #42=38,4, #43=42,3.

**O3 — Datenzugang NICHT blockiert, zwei Workarounds:** (1) `ui_scroll left` lädt Historie nach (301→499 Bars nach einem Scroll), `chart_scroll_to_date`/`set_visible_range` tun das nicht. (2) `replay_start <Datum>` ist der saubere Weg — Serie endet am Replay-Datum, `data_get_ohlcv count:300` liefert die 300 Bars davor. Für #8: ~1,7 Handelstage pro Zyklus bei count 500 → ~12 Zyklen für 20 Tage, als eigene Session machbar. Chart nach O2-Messung mit `replay_stop` zurückgestellt, läuft wieder auf 5min/Realtime.

---

## Teil 2 — E1-E7: konkrete Empfehlungen für Runde 2 (Levi entscheidet am Ende)

### E1 (#1c) — TP2-`none`-Rechtsfolge → **Option C**
`none` + kein Register-Kandidat ≥RR2 → PASS + halbe Position. `none` + Kandidat vorhanden (Kandidatenzeile beweist es bereits) → UNKNOWN, weil der Beleg widerlegt ist. Am 04.09. hätte C korrekt UNKNOWN ergeben (6 Kandidaten vorhanden, nicht übergeben).

### E2 (#3) — 30-Punkte-Sockel → **Kurswechsel: Option D, Frage schließen**
Opus zieht den eigenen Lockerungsvorschlag zurück. Drei unabhängige Gründe: (1) O2 — Sockel dominierte auch am Ursprungstag, ist Regelkern, nicht Fehlkalibrierung. (2) Der hypothetische 04.09.-Trade wäre nach echten Chartdaten voll ausgestoppt worden (−35,44€). (3) Der SL lag auf der Entry-Seite der Zone — geometrisch die schlechteste Lage, 8c2 hatte inhaltlich recht. `--experiment ohne-sockel` bleibt als Messinstrument; Wiederaufnahme nur falls #8 zeigt, dass 8c2 in >50% der FAILs alleiniger Blocker ist UND die Quote unter das E6-Zielband fällt.

### E3 (#4) — P4-vs-8c2-Kollision → **neue Lösung: "SL-Anker-Vorprüfung"**
Bandlogik tot, Alters-/Impulskriterium (Option B) auch verworfen (die 04.09.-Zone war alt, "alt" war kein Sicherheitsindiz — Kurs fiel glatt durch). Stattdessen die Kollision vor den Trigger-Moment verlegen: (a) Wick-Zonen als eigene Registerkategorie (`levelart: "Wick-Zone (8c2)"`, `tief`/`hoch`). (b) Neue Pflichtzeile in der Levelsuche jedes Voll-Checks, die den SL-Kandidaten gegen 8c2 vorprüft und "SL derzeit untauglich" ausweist, bevor ein Trigger verbrannt wird. (c) Ein Setup ohne gültigen SL-Anker löst keinen 7b1-Ablauf aus. Technisch: neuer Aufruf-Modus `--sl-vorpruefung` (reine Anzeige, kein A3-Zwang, Exit immer 0), nutzt die bereits vorhandene `gegenrechnung8c2`-Logik.

### E4 (#6a) — Zeitbox-Deadline → **12 Minuten, bestätigt**

### E5 (#7) — Fib-Faktor 2,0 → **Nein**
Liegt fast immer jenseits der 3×ATR-TP1-Kappung, verwässert die Registerprüfung. Bei 1,272/1,618 + Measured Move bleiben. Aufnahme später nur, falls #8 zeigt, dass TP1-Fenster systematisch im 2,0-Bereich liegen.

### E6 (#8) — Quotenbänder → **neu und fest** (ersetzt Opus' ersten, unbrauchbaren Vorschlag)
Nenner = Trigger-Kandidaten (nicht alle Bars). Primärmaß PASS-fähige Trigger/Trigger gesamt: **<10%** → Gate-System zu eng, Kalibrierung eröffnen · **10-35%** → keine Schwellenänderung, Hebel = Leveldichte+Timing · **>35%** → Problem liegt beim Trigger/Timing, nicht bei den Gates. Sekundärmaß: erwartete PASS/Handelstag, Zielkorridor **0,3-1,0** (Phase-3-Historie ~1,5 Trades/Tag). Vorab-Bindung: Kalibrierung nur für das Gate, das in >50% der FAILs alleiniger Blocker ist, und nur wenn beide Maße unter Band liegen.

### E7 (#10) — 1H-Entscheidungsschwelle → **bestätigt, zwei Verschärfungen**
≥5 Testtage ODER ≥10 Blockaden bleibt. Zusätzlich: Zähldefinition schärfen (Blockade zählt nur ab ≥15 Min Dauer UND ≥1,5×ATR Kursbewegung in der blockierten Richtung). Beidseitig auswerten — zu jeder Blockade das kontrafaktische Ergebnis nachtragen (hätte der blockierte Trade TP1/SL erreicht?), sonst misst man nur die Kosten der Regel, nie den Nutzen.

---

## Für Fable Runde 2 direkt umsetzbar
O1 (Bilanz-Fehlalarm-Fix) · E1 Option C · E3 (`--sl-vorpruefung` + Wick-Zonen-Registerkategorie) · E4/E5 als Konstanten-Bestätigung · #9-Nachtrag in `trades.db` (ATR-Werte oben). E2 ist eine Regelwerksentscheidung (Empfehlung: schließen, nicht ändern) — kein Code nötig, nur Dokumentation der Entscheidung. #8-Datenlauf braucht eine eigene Session mit ~12 `replay_start`-Zyklen.

---

## Runde 2 — Opus-Gegencheck (07.09., zweiter Durchlauf)

**Alle 6 Runde-2-Punkte am Code live verifiziert: OK.** O1 (Log-jünger-als-Protokoll → "nicht messbar" statt Exit 1, echte Abweichungen feuern weiter korrekt), E1 (beide Zweige getestet: Kandidat vorhanden → UNKLAR, keiner vorhanden → PASS+halbe Position), E3 (`--sl-vorpruefung` liefert korrektes Urteil, dockt sauber an `gegenrechnung8c2` an; Fable hat mit `--cluster-level register` mehr gebaut als beauftragt, sinnvoll), E4/E5 (12 Min + Fib-Ablehnung korrekt dokumentiert), #9-Nachtrag (nur 2 Werte inhaltlich geändert, Rest des 46-Zeilen-Diffs ist Spalten-Nachpflege seit Paket 6 — Fables Erklärung bestätigt), E2 (Kurswechsel sauber und ungeschönt dokumentiert, Code unverändert).

**Fables zwei markierte Punkte:** (a) `trades.sql`-Diff bestätigt harmlos (nur `atr_at_entry` #42/#43 geändert). (b) Batch-Summary nutzt noch alte Quotenbänder (`<2/2-10/>10%`) — bestätigt, **muss vor dem #8-Lauf weg**, nicht nur "irgendwann": E6 hat auch den Nenner geändert (PASS je Trigger-Kandidat statt je Datensatz).

### Neue Befunde — Runde 3 nötig (klein, blockiert Levis Entscheidungen nicht)

**N1 — SL-Vorprüfung wird zum Gummistempel ohne Wick-Zone im Register.** Getestet: leeres Register → `URTEIL: SL-ANKER TAUGLICH`. Wiedergänger der alten "nicht geprüft = geprüft und unbedenklich"-Fehlerklasse (03.09.-Liste Punkt 1), diesmal über die neue Register-Quelle. Fix: `--cluster-level register` ohne Wick-Zonen → `pass: null`, Urteil "NICHT ABSCHLIESSEND PRÜFBAR"; Abwesenheit muss über `--cluster-level none` belegt werden, nicht durch ein leeres Register.

**N2 — Statistik-Loch durch die neue Rechtsfolge.** "Untauglicher SL-Anker → kein `gate_check`-Aufruf" heißt: diese Fälle verschwinden aus jeder FAIL-Statistik und aus #8. Fix: `--sl-vorpruefung` gibt bei UNTAUGLICH den fertigen `add_skipped_setup.cjs`-Aufruf aus (wie der FAIL-Pfad es schon tut), Pflichterfassung; `protokoll_bilanz.cjs` zählt "2/2-Zustand ohne 7b1 (SL-Anker untauglich)" als eigene Kategorie.

**N3 (Nit, keine Pflicht):** Ausgabetext zu E1 sagt "entschieden 07.09.", sollte wie bei E3 "nach Opus-Empfehlung, Levi-Bestätigung ausstehend" heißen. Auftragsliste Punkt 3 Option C fehlt der Inline-Vermerk "zurückgezogen" (bei Punkt 4 korrekt vorhanden).

**Batch-Quotenbänder (b, aus Runde 2):** `:2179` auf E6-Fassung umstellen — Nenner Trigger-Kandidaten, Bänder <10/10-35/>35%, plus `--handelstage <n>` für das Sekundärmaß.

### Opus' Fazit: entscheidungsreif für Levi
#1c (Code läuft als Option C), #3 (Sockel, Empfehlung D — Datenlage vollständig), #4 (Vorprüfung ist die einzige echte neue Regelentscheidung, die Levi treffen muss — Bandlogik und Alterskriterium beide verworfen), E4/E5 (12 Min, kein Fib 2,0). Offen: die beiden Datenläufe #8/#9-Vertiefung und (6f)-Bestätigung.

---

## Runde 3 — Fable-Umsetzung (07.09., vor letztem Opus-Gegencheck)

**N1 UMGESETZT:** `--cluster-level register` ohne Wick-Zone → `pass:null` statt `true`, Semantikzeile "NICHT ABSCHLIESSEND PRÜFBAR". `--sl-vorpruefung` jetzt mit drei Urteilen (TAUGLICH/UNTAUGLICH/NICHT ABSCHLIESSEND PRÜFBAR). Getestet mit leerem Wick-Zonen-Register und ohne `--atr`.

**N2 UMGESETZT:** UNTAUGLICH druckt fertigen `add_skipped_setup.cjs`-Aufruf, Erfassung Pflicht. `protokoll_bilanz.cjs` zählt "2/2-Zustand ohne 7b1 (SL-Anker untauglich)" als eigene Kategorie (Urteile/protokollierte Zeilen/Skipped-Aufrufe), meldet Abweichung.

**Batch-Quotenbänder UMGESETZT:** Nenner jetzt Trigger-Kandidat (`triggerId`-Feld), Bänder <10/10-35/>35%, `--handelstage <n>` fürs Sekundärmaß. Getestet.

**N3 UMGESETZT:** "#1c entschieden" → "Option C nach Opus-Empfehlung, Levi-Bestätigung ausstehend" (0 Grep-Treffer der alten Formulierung). Auftragsliste Punkt 3 Option C trägt jetzt "ZURÜCKGEZOGEN 07.09.2026".

**Nebenbefund beim Testen (kein Auftrag, nur Beobachtung):** Im E6-Batch bestand der 18:49-Datensatz mit `clusterLevel:"none"` + übergebenem TP2 strikt mit PASS — konsistent, bestätigt nochmal, dass TP2-Übergabe + 8c2 die beiden alleinigen Stellschrauben am 04.09. waren.

**Fable-Einschätzung:** Damit ist die Liste aus eigener Sicht entscheidungsreif; wartet auf finalen Opus-Gegencheck.

---

## Runde 3 — finaler Opus-Gegencheck (07.09.): ALLES BESTÄTIGT, keine offenen Defekte

N1 OK (auch im normalen Live-Gate-Pfad verifiziert, nicht nur `--sl-vorpruefung`). N2 OK (drei getrennte Signale — Urteil/Protokollzeile/Erfassung — schließen jeden stillen Umgehungsweg). Batch/E6 OK (Nebenprobe: der 04.09.-Kontrafaktum-Datensatz läuft korrekt auf PASS). N3 OK (ZURÜCKGEZOGEN-Vermerk jetzt bei Punkt 3 UND 4).

**Wichtige Einschränkung zur Batch-Semantik für den späteren #8-Lauf** (kein Code-Defekt, aber vor dem Datenlauf festzulegen): "Trigger PASS-fähig sobald ein Datensatz PASS liefert" ist nur korrekt, wenn jeder eingespeiste Datensatz für sich regelkonform ist (SL=P4-Anker, TP1=Registerlevel). Ohne eine festgeschriebene Erzeugungsregel für Varianten (welche SL-Anker, welche TP1-Kandidaten, wie viele je Trigger) misst der Batch Variantenzahl statt Machbarkeit.

### Opus' Endurteil nach 3 Runden
**Entscheidungsreif für Levi.** Keine offenen Defekte. Zu entscheiden: #1c (Option C aktiv, umschaltbar), #3 (Sockel, Empfehlung D), #4/(6f) (SL-Vorprüfungs-Rechtsfolge — einziger echter neuer Regelmechanismus), E4/E5 (bestätigen).

**Commit durchgeführt: `eb7caeb` (07.09.2026 18:30), alle drei Auflagen eingehalten** — verifiziert (`git log -1`, `git status`): 8 Dateien (+1621/−101), `trade_stats.cjs` und `memory/` bleiben draußen, Commit-Text mit ausdrücklichem Absatz "Keine Handelsregel wird live aktiviert". Nicht gepusht (lokal, 1 Commit vor `origin/main`).

**Commit-Empfehlung (Nachtrag, bereits umgesetzt): Ja, mit drei Auflagen.**
1. Selektiv `git add` (nicht `-a`) — `trade_stats.cjs` trägt 82 uncommittete Zeilen aus einem früheren, ungeprüften Zyklus; bewusst ein-/ausschließen. Zum Commit gehören: `gate_check.cjs`, `.gitignore`, `trades.sql`, die 5 neuen `scripts/*.cjs`.
2. `memory/` bleibt draußen (untracked, gehört nicht ins Repo).
3. Commit-Text soll klarstellen: keine Handelsregel wird aktiviert — die einzige neue Rechtsfolge (6f) steht nur im Regelwerk, markiert "Levi-Bestätigung ausstehend"; die zwei Code-Semantikänderungen (TP2-`none`→Option C, leeres Wick-Register→UNKNOWN) sind beide strenger als vorher, also risikoarm; `--sl-vorpruefung` ist reine Anzeige.

---

## Runde 4 — Finalisierung nach Levis Bestätigung + letzter Opus-Gegencheck (07.09.)

Levi hat alle vier offenen Punkte bestätigt (#1c Option C, #3 Sockel bleibt, #4 SL-Anker-Vorprüfung wird bindend, E4/E5 12 Min + kein Fib 2,0). Fable hat finalisiert: Commit `c72a836` (3 Dateien, +43/−27) — Platzhalter ersetzt, #4 als **Schritt 0** im 7b1-Ablauf (`feedback_live_trading.md`) und Schritt 8b im Voll-Check-Ablauf dokumentiert (reine Prozessvorgabe, kein neues Code-Gate), Bilanz-Kategorie "2/2 ohne 7b1" als erwarteter Normalfall.

**Opus-Gegencheck: alles bestätigt, per Verhaltensregression gemessen (nicht nur gelesen).** `git diff eb7caeb..c72a836` über 7 Testszenarien mit `--json` verglichen — `status`, `overallPass`, Sizing-Flag und jedes einzelne Gate-`pass` identisch zwischen beiden Commits. Reine Text-/Kommentaränderung + eine neue Bilanz-Meldung, keine Gate-Logik verändert. Platzhalter: 0 Grep-Treffer in den Regelwerksdateien (nur die Zeitdokument-Berichte selbst erwähnen den alten Stand, korrekt). Commit-Message "vorbildlich" — stellt den Unterschied zu `eb7caeb` klar heraus.

**Design-Frage zu #4 beantwortet:** Fables Wahl (Prozessvorgabe statt Code-Gate) ist richtig, weil `gate_check.cjs` einen 2/2-Trigger nicht selbst erkennen kann — ein technischer Zwang wäre nur Selbstzertifizierung. Aber die Lücke ist real: Ein übersprungener Schritt 0 bleibt unsichtbar, weil kein UNTAUGLICH-Urteil entsteht und `protokoll_bilanz.cjs` nur Urteile gegen Erfassungen zählt. **Kein Risiko** (ein übersprungener Schritt 0 gibt keinen falschen Trade frei, fällt nur auf den Vor-07.09.-Zustand zurück), aber Prozessdisziplin-Lücke, angesichts der Loop-Historie (17× falsche Register-Frische, 39× ausgelassener Screenshot) real.

**Für ein künftiges Arbeitspaket vorgemerkt (keine Runde 5, kein Zeitdruck):**
1. `--sl-anker-vorpruefung <tauglich|nicht-abschliessend>` als neues A3-Pflichtfeld in `gate_check.cjs` (analog `--cluster-level`/`--tp2-level-price`) — macht eine Auslassung maschinell sichtbar.
2. Erzeugungsregel für Trigger-Varianten vor dem #8-Datenlauf festlegen (sonst misst die Quote Variantenzahl statt Machbarkeit).
3. Optional: `!==` statt `>` in den beiden Bilanz-Prüfungen (Restdetail, kein Muss).

### Opus' Endurteil: KEINE RUNDE 5 NÖTIG
"Nach vier Runden ist alles sauber und konsistent... Das System ist aus meiner Sicht bereit für den nächsten Testtag." Auftragsliste vollständig abgearbeitet, alle vier Levi-Entscheidungen widerspruchsfrei in Code und Regelwerk festgeschrieben.
