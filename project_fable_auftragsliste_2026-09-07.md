---
name: project-fable-auftragsliste-2026-09-07
description: "ECHTE Opus-Verbesserungsliste (nicht Sonnet-Konsolidierung) aus der 04.09.-Testtag-Analyse, mit Codeverifikation: 10 Punkte (4 SKRIPT-FIX, 2 REGELWERK-FRAGE, 2 DATENTASK, 2 PROMPT/PROZESS) + Anhang A (Register-Frische, bindende Auflage). Zwei Korrekturen an Opus' eigener 04.09.-Analyse enthalten. Entwurf zur Weitergabe an Fable."
metadata: 
  node_type: memory
  type: project
  status: draft
  modified: 2026-09-07T16:40:57.637Z
  originSessionId: b594a3be-34a1-46da-8616-0f50ceb56daa
---

# Auftragsliste für Fable — Opus-Analyse Testtag 04.09.2026

**Diese Liste stammt von Opus selbst (nicht von Sonnet konsolidiert), inklusive Codeverifikation gegen `gate_check.cjs`.** Status: Entwurf zur Weitergabe an Fable. Nichts davon ist umgesetzt oder beschlossen. Punkte der Kategorie REGELWERK-FRAGE brauchen vor der Umsetzung eine ausdrückliche Entscheidung von Levi; Fable liefert dort die Entscheidungsvorlage, nicht die Entscheidung.

**Quelle:** [[project_testtag_analyse_2026-09-04]], Abschnitt „Für das Gespräch am 07.09. offen", Punkte **1, 2, 3, 4, 5, 6, 8**. **Punkt 7 (Degraded-Mode X-API) ist ausgeschlossen** — Levi hat den toten Kanal am 07.09. als eigenen vergessenen Reconnect eingeordnet, nicht als Regelwerkslücke. Kein Auftrag an Fable.

**Zwei Befunde, die Opus beim Gegencheck am Code neu ermittelt hat und die die Prioritäten gegenüber der Analyse verschieben:**

- **Ein fehlendes `--tp2-level-price` macht den GESAMTSTATUS strukturell UNKNOWN** (`gate_check.cjs:1061-1067` setzt `pass: null`, `:1237` macht daraus UNKNOWN). Alle vier Läufe vom 04.09. hätten also **selbst bei sonst perfekten Gates nie PASS** ergeben können. Die P7-Klarstellung („TP2 ist kein Hard-Gate, nur Sizing-Signal") ist im Code **nicht** umgesetzt.
- **Opus' eigene 04.09.-Aussage „eine Lockerung von 8c2 allein hätte null Trades erzeugt" ist in dieser Allgemeinheit falsch.** Sie gilt für eine Lockerung des *Puffers* (dann braucht es einen konformen, weiter entfernten SL → 4-5 Pkt breites TP1-Fenster → tot). Sie gilt **nicht** für eine Verengung des *Trigger-Radius*: Ohne den 30-Punkte-Sockel wäre 8c2 am 18:49 gar nicht ausgelöst worden (Abstand SL↔Zone 12,6 Pkt > 0,5×ATR = 11,7 Pkt), der **ursprüngliche** SL wäre gültig geblieben — und der Lauf hätte alle übrigen Gates bestanden.

**Zusammengenommen:** Punkt 1 und Punkt 3 dieser Liste sind die beiden einzigen Stellschrauben, die zwischen der Serie „0 PASS in 11 Läufen" und einem ersten PASS am 04.09. 18:49 standen. Das macht sie zu den wichtigsten Punkten der Liste — und zugleich zu solchen, die Levi bewusst entscheiden muss, weil eine davon (8c2) direkt aus zwei Echtgeldverlusten stammt.

**Kategorien:** **SKRIPT-FIX** (Code/Datenstruktur, keine Handelsregel betroffen — Fable kann direkt umsetzen) · **REGELWERK-FRAGE** (Entscheidung am Handelsregelwerk — nicht ohne Levi umsetzen) · **DATENTASK** (Messung/Auswertung, die eine Entscheidung erst entscheidbar macht) · **PROMPT/PROZESS-ERGÄNZUNG** (Pflichtzeile/Ablauf im Loop-Prompt).

---

## Hoch

### 1. SKRIPT-FIX — `--tp2-level-price`: das N/A ist ein stiller PASS-Blocker, nicht nur ein fehlendes Sizing-Signal
**Datei:** `scripts/gate_check.cjs:1060-1108` (Gate 3b), Statusaggregation `:1230-1238`

Fehlt `--tp2-level-price`, wird `gates.tp2RealismGate.pass = null` gesetzt. `anyUnknown` (Zeile 1232) macht daraus `status = 'UNKNOWN'`, `overallPass = false`. Fables 03.09.-Aussage stimmt, die P7-Formulierung vom 04.09. ("kein Hard-Gate, nur Sizing-Signal") ist im Code nicht abgebildet. Am 04.09. war der Parameter 4/4 nicht gesetzt — ein PASS war also unabhängig von 8c2 von vornherein ausgeschlossen.

**(a) Diagnosezeile — sofort, kein Regelbezug:** Skript berechnet ab welchem Preis RR≥2 gilt und listet Registerlevel darüber/darunter (analog TP1-Fenster-Logik `:1310-1324`):
`TP2-Kandidaten (Anzeige): RR>=2 verlangt TP2 >= 29637,65 — im Register verfügbar: 29650, 29654,6, 29660,8. Nicht übergeben.`

**(b) `--tp2-level-price` auf die A3-Messfeld-Pflichtliste** (`:1745-1785`, analog `--cluster-level`-Fix von 03.09.): `none` als Beleg zulässig, Fehlen ohne `--grund-tp2-level-price` → Hard-Exit 1.

**(c) REGELWERK-ENTSCHEIDUNG (Levi) — was soll belegtes `none` bewirken?** → **VON LEVI BESTÄTIGT 07.09.2026 abends: Option C** (im Code seit Runde 2 aktiv).
- **A (Status quo im Code):** `pass: null` → UNKNOWN → kein Trade. Widerspricht aber Levis Kopfkommentar (`:1054-1056`, "kein Ausschlussgrund").
- **B (P7-Lesart, Opus-Empfehlung):** belegtes `none` → `pass: true` + `sizingFlag: 'halbe Position'`. Selbstdurchsetzend, kein UNKNOWN-Blocker mehr. Contra: Halbierung drückt Rendite unter Ziel (03.09.: 0,87% real bei halber Position vs. Ziel 1,5%).
- **C:** wie B, aber nur wenn zusätzlich belegt ist, dass kein Registerlevel ≥RR2 existiert; existiert eines und wurde nicht übergeben → UNKNOWN. Trifft genau den 04.09.-Fall (3/4 verfügbar, nicht übergeben).

**Quelle:** Analyse 04.09., F5; Codeverifikation 07.09.

---

### 2. SKRIPT-FIX — 8c2: Zonenband statt Einzelpreis + automatische Konformitäts-Gegenrechnung
**Datei:** `scripts/gate_check.cjs:1156-1222`

**(a) `--cluster-level` ist ein Skalar**, eine Dochtzone ist aber ein Band (#42/#43: 29.240-29.254 laut Trade-DB-Notes). Aus einem Skalar lässt sich nicht unterscheiden: SL *in* der Zone (der Fall, der #42/#43 kostete) vs. SL 12,6 Pkt jenseits der Oberkante (04.09.-Fall). **Zu tun:** optionale Parameter `--cluster-zone-hoch`/`--cluster-zone-tief`; Abstand ab relevanter Kante messen, im Detailtext ausweisen. Ohne Parameter unverändertes Skalar-Verhalten. Zusätzlich Mehrfachzonen-Prüfung zulassen (heute nur ein Level, an Mehr-Zyklen-Tagen unprotokollierte Auswahl).

**(b) 8c2-Konformitäts-Gegenrechnung als Pflicht-Ausgabezeile bei FAIL** (kein Gate, alle Eingangsgrößen liegen schon vor):
```
8c2-Gegenrechnung (Anzeige): konformer SL <= 29487,65 -> TP1-Fenster [29620,05; 29624,05] = 4,0 Pkt
  -> Register-Level im Fenster: KEINES -> Trade mit konformem SL arithmetisch unmoeglich;
     8c2 ist hier der protokollierte, nicht der eigentliche Blocker.
```
Hätte am 18:49 sofort gezeigt, dass die Diskussion nicht bei 8c2 zu führen ist.

**Quelle:** Analyse 04.09., F4(1)/(2); Trade-DB #42/#43.

---

### 3. REGELWERK-FRAGE — Der 30-Punkte-Sockel im 8c2-Trigger-Radius: gate-entscheidend in beide Richtungen
**Datei:** `scripts/gate_check.cjs:1186` (`triggerRadius = Math.max(30, requiredBuffer)`)

**Korrektur an Opus' eigener Analyse:** "Lockerung hätte null Trades erzeugt" gilt nur für den *Puffer*. Am *Trigger-Radius* zeigt sich das Gegenteil — 18:49-Trigger: Abstand SL↔Zone 12,6 Pkt, Trigger-Radius heute max(30; 0,5×ATR=11,7) = 30 → ausgelöst → FAIL. **Ohne Sockel** (Radius = 11,7): 12,6 > 11,7 → nicht ausgelöst → **PASS mit dem ursprünglichen SL**, und alle übrigen Gates hätten gehalten (RR 1,101, SL-Distanz 1,79×ATR, TP1 1,97×ATR = Zone 1 volle Position, TP1 registerverifiziert; mit übergebenem TP2 29.650 wäre RR2=2,29 ≥2).

**→ Punkt 1(b/c) + eine Entscheidung gegen den Sockel hätten am 04.09. 18:49 den ersten GESAMTSTATUS PASS der ganzen Serie ergeben.** Einschränkung: ein SL 12,6 Pkt über einer frisch getesteten Zone ist genau die Konstellation, die 8c2 verhindern soll — ein PASS dort ist kein Freispruch, sondern eine Risikoentscheidung.

**Optionen:**
- **A — Sockel streichen** (Radius = 0,5×ATR). Pro: regimeneutral. Contra: bei ATR 10-15 wird 8c2 praktisch wirkungslos.
- **B — Sockel absenken** (15-20 Pkt). Contra: Zahl frei gegriffen ohne Punkt 9.
- **C — Sockel behalten, Rechtsfolge staffeln (Opus-Empfehlung):** SL *innerhalb* Zonenband → FAIL unverändert. SL jenseits der Kante aber innerhalb Trigger-Radius → kein FAIL, sondern "halbe Position". Trifft #42 (SL 1 Pkt über Kante) und #43 (SL im Band) weiterhin hart, macht 04.09. handelbar. Contra: weitere Halbierungsquelle. Braucht Zonenband aus Punkt 2(a). **ZURÜCKGEZOGEN 07.09.2026 (Opus-Gegencheck, Fable-Gegenbefund bestätigt):** geometrisch nicht kohärent — #42 (SL auf der sicheren Seite) würde halbiert, 04.09. (SL auf der Entry-Seite) bliebe FAIL; siehe Kurswechsel unten und Punkt 4.
- **D — nichts ändern.** Pro: kein Overfitting nach vier fiktiven Tagen. Contra: Serie bleibt vermutlich bei 0 PASS ohne Leveldichte-Maßnahmen.

**Reihenfolge-Hinweis:** Diese Entscheidung nicht vor Punkt 9 (ATR-Messung 21.08.) und nicht vor Punkt 8 (Machbarkeitsmessung) treffen.

**Nachträgliche Gegenprobe (07.09., echte Chartdaten):** Der hypothetische Trade wäre am realen Kursverlauf **voll ausgestoppt worden** — Entry 29.553,85, Höchstkurs danach in der ganzen Session nur 29.558,85 (41 Pkt vor TP1), SL-Bruch im 19:00-19:05-Fenster, Kurs danach glatt bis 29.431,95 durchgefallen. −41,9 Pkt / −1,0 R / ca. −35,44 € bei 5.000€-Position. Das ist exakt das Muster, gegen das 8c2 gebaut wurde (SL 12,6 Pkt über einer frisch getesteten Zone, die kurz danach durchbrochen wurde) — **dieser eine Datenpunkt stützt eine Sockel-Lockerung nicht, er spricht dagegen.** Verschiebt die Beweislast: 1 von 1 durch den Sockel verhinderten Trades wäre ein Verlust gewesen. Ändert nichts an Punkt 1/2 (TP2-Bug bleibt real, Sockel bleibt gate-entscheidend), härtet aber die Reihenfolgen-Empfehlung: nicht ohne Punkt 8/9 entscheiden.

**KURSWECHSEL 07.09.2026 (Opus-Gegencheck nach Fable-Runde 1, [[project_gegencheck_fable_umsetzung_2026-09-07]] E2/O2):** Opus zieht den Lockerungsvorschlag zurück und empfiehlt jetzt **Option D — nichts ändern, Frage schließen.** Drei Gründe: (1) Punkt 9 ist gemessen — ATR(14, 5min) am 21.08. bei #42 = 38,4 Pkt, bei #43 = 42,3 Pkt (via `replay_start 2026-08-22` + `atr_from_ohlcv.cjs`); 0,5×ATR = 19-21 Pkt < 30, der Sockel dominierte also schon am Ursprungstag und IST die Regel ("±20-30 Pkt"), kein verrutschter Parameter. (2) Die Gegenprobe oben: voller Stop-out (−35,44 €). (3) Der SL lag auf der Entry-Seite der Zone — geometrisch die schlechteste Lage. `--experiment ohne-sockel` bleibt als Messinstrument für Punkt 8; Wiederaufnahme nur, falls Punkt 8 zeigt, dass 8c2 in >50 % der FAILs alleiniger Blocker ist UND die Quote unter das E6-Zielband fällt. **VON LEVI BESTÄTIGT 07.09.2026 abends: Option D — Sockel bleibt, Frage geschlossen.** Nebenbefund O2 zu Punkt 9: SL-Distanz 53 Pkt = 1,25-1,38×ATR, also unter dem NORMALEN 1,5×-Floor — #42/#43 waren schon deshalb regelwidrig platziert; die "ATR ≤ 21"-Ableitung aus der DB und F4(3) sind damit widerlegt. `atr_at_entry` für #42/#43 ist seit 07.09.2026 in `trades.db`/`trades.sql` nachgetragen.

**Quelle:** Analyse 04.09., F4(3), hier korrigiert; Codeverifikation `:1183-1196`.

---

### 4. REGELWERK-FRAGE — Kollision P4 (SL am Docht) vs. 8c2 (SL jenseits jeder Dochtzone)
Am 04.09. wurden beide Regeln 4/4 korrekt befolgt und kollidierten 4/4 — kein Pfad erfüllt P4 + 8c2 + RR≥1 + Zone-3-Kappung gleichzeitig.

**Optionen:**
- **A — Selbstreferenz-Ausnahme:** SL-Anker-Zone zählt nicht gegen sich selbst. Contra: greift am falschen Ort, wenn genau dieser Docht mehrfach getestet wurde.
- **B — Alters-/Impuls-Kriterium:** 8c2 prüft nur gegen Zonen, die nicht vom laufenden Impuls stammen. Neues protokollpflichtiges Ermessensfeld.
- **C (Opus-Empfehlung, deckungsgleich mit Punkt 3 Option C):** Bandlogik — FAIL nur bei SL *im* Band, sonst Sizing-Signal. Löst Punkt 3+4 in einer Änderung, an Ursprungsfällen validierbar. **ZURÜCKGEZOGEN 07.09.2026 (Opus-Gegencheck, Fable-Gegenbefund bestätigt):** geometrisch nicht kohärent — #42 (SL 1 Pkt auf der sicheren Seite) würde halbiert, 04.09. (SL auf der Entry-Seite) bliebe FAIL; Option B (Alters-/Impulskriterium) ebenfalls verworfen (die 04.09.-Zone war alt, Kurs fiel glatt durch).
- **D — P4 statt 8c2 anpassen:** ist faktisch der heutige Zustand — nachweislich unbrauchbar (TP1-Fenster 4,0/5,25 Pkt, kein Level darin).
- **NEU E (Opus-Empfehlung seit 07.09.2026, Fable-Umsetzung Runde 2): "SL-Anker-Vorprüfung"** — Kollision vor den Trigger-Moment verlegen: (a) Wick-Zonen als Registerkategorie `Wick-Zone (8c2)` (`register_touch.cjs --wick-zone`), (b) `gate_check.cjs --sl-vorpruefung` (Anzeige, kein A3, Exit 0) prüft den SL-Kandidaten in jedem Voll-Check mit Setup gegen 8c2/8c, (c) Setup mit UNTAUGLICHEM Anker löst keinen 7b1-Ablauf aus. Beide Regeln bleiben unverändert. Regeltext: [[feedback_live_trading]] Item (6f) + 7b1 Schritt 0, [[feedback_vollcheck_format]], [[feedback_loop_ablauf_uebersicht]]. **VON LEVI BESTÄTIGT 07.09.2026 abends — bindend als 7b1 Schritt 0.**

**Quelle:** Analyse 04.09., F4(2).

---

### 5. SKRIPT-FIX + PROMPT/PROZESS — Bilanzzahlen maschinell auszählen; drei Korrekturen im 04.09.-Protokoll
**Dateien:** `memory/testtag/testtag_2026-09-04.md`, neu: `scripts/protokoll_bilanz.cjs`

**(a) Korrekturen (als Nachtragszeile, nicht überschreiben):** `--chasing`: 1/4 `yes` (VC#7), nicht "nie". Voll-Checks: 55, nicht 59. Register-Updates: 4, nicht 3.

**(b) Ursachenbehebung maschinell:** Die 03.09.-Liste hat dieselbe Fehlerklasse bereits als Prompt-Vorgabe adressiert (dort #13) — am 04.09. dreimal wieder aufgetreten. Prompt-Weg ist empirisch gescheitert. Vorschlag `scripts/protokoll_bilanz.cjs` (Muster `quote_check.cjs`): zählt Voll-Check-Überschriften, `gate_check.cjs`-Aufrufe + `--chasing`-Werte, Screenshot-Referenzen gegen `screenshots/`; gibt fertigen Bilanz-Block aus; Exit-Code 1 bei Abweichung Dokument↔Zählung. Register-Updates: `scripts/register_touch.cjs` schreibt Zeitstempel + Log-Zeile nach `register_touch_log.jsonl` statt Erinnerung.

**Quelle:** Analyse 04.09., F1, F9; 03.09.-Liste #13 (als nicht wirksam einzustufen).

---

## Mittel

### 6. SKRIPT-FIX — P1-Retest-Zeitbox: Wanduhr-Deadline statt Voll-Check-Zählung, plus Wiederholungssperre
**Datei:** `scripts/gate_check.cjs:1380-1392`; neu: `scripts/last_gate_fail.json`

**(a)** Zeitbox dehnt sich bei Protokolllücken still (04.09.: zweimal 5 Min). Skript soll absolute Deadline ausgeben: `Retest-Zeitbox läuft ab um 19:01:12 (FAIL + 12 Min) ODER nach 2 Voll-Checks — was zuerst eintritt.` Regelentscheidung: 12 Min (Opus-Empfehlung) oder 15 Min.

**(b)** Neu-Aufruf-Sperre bei unverändertem SL nach nicht-entry-abhängigem FAIL: 8c2 ist SL-abhängig und kann im Entry-Fenster nie abgebildet sein (04.09. 18:55: identischer SL, identisches Ergebnis, aussichtslos). Sofort: Warnzeile bei FAIL an nicht-entry-abhängigem Gate. Sauber: Fingerabdruck (`sl`, `tp1`, `clusterLevel`, FAIL-Gates) nach `last_gate_fail.json`, Warnung bei identischem Wiederholungsaufruf. `gate_check.cjs` hat heute kein Gedächtnis über Aufrufe hinweg — neu zu bauen.

**(c)** Kollisionsregel für laufende Zeitbox + neuer 2/2-Trigger im selben Voll-Check (04.09. 15:50, bisher folgenlos): Vorschlag — neuer Trigger ersetzt laufende Zeitbox, als eigene Zeile ausweisen.

**Quelle:** Analyse 04.09., F2(a)/(b).

---

### 7. SKRIPT-FIX — Fib-Extension / Measured Move in die Register-Pipeline, nicht in den Loop-Prompt
**Datei:** Register-Pflege (P5), `scripts/level_register.json`

Rundzahl-Band-Hälfte von P5 funktionierte am 04.09. vorbildlich; Fib-Extension-Hälfte fand nicht statt — Fib wurde nur als Retracement gerechnet, nie als Extension/TP-Kandidat. Identisch zum 03.09.-Befund. "Manuell von Sonnet" heißt in der Praxis: gar nicht. **Zu tun:** Extension-Berechnung in denselben Pfad wie die Rundzahlen-Erzeugung, `levelart: "Fib-Extension"` mit Impulsursprung/-hoch + Faktor (1,272/1,618 Standard, 2,0 zur Entscheidung). Zusätzlich prüfen: Measured Move als zweiter vorwärtsgerichteter Kandidat. Wirkt genau dort, wo TP1-Fenster (17,8-42,1 Pkt) auf reine 50er-Rundzahlen trafen und leer blieben — ändert kein Risikoprofil, anders als eine Schwellenlockerung.

**Quelle:** Analyse 04.09., F7, Gesamtbild-Abschnitt.

---

### 8. DATENTASK — Machbarkeitsmessung der fünf PASS-Bedingungen (Bauplan)
11 Gate-Läufe über 4 Tage, 0 PASS. Auszuzählen: wie oft sind (a) SL≥1,5×ATR, (b) SL 8c2-konform, (c) TP1≤3×ATR, (d) TP1≥SL-Distanz, (e) TP1 auf Registerlevel gleichzeitig erfüllbar.

**Bauplan:** `gate_check.cjs --batch` wiederverwenden (nicht neu bauen) — misst garantiert dieselbe Live-Logik. **Bekannte Batch-Abweichungen zuerst beheben:** Register wird im Batch nie gelesen (`:790`), A2-TP1-Pflicht neutralisiert (`:932`), fehlendes `--cluster-level` läuft auf PASS statt UNKNOWN (`:1212`) — Flag `--batch-strikt` vorschlagen. Datenbeschaffung: `data_get_ohlcv` 5min über ~20 Tage + Tages-Bars für Pivots. Ausgabe: nicht nur Gesamtquote, sondern welche Bedingung wie oft bindend ist. Kontrafaktische Läufe im selben Durchgang: mit Fib-Extensions (Punkt 7), 25er-Rundzahlraster, ohne 30-Pkt-Sockel (Punkt 3), mit 8c2-Bandlogik (Punkt 3/4-C).

**REGELWERK-ENTSCHEIDUNG vorab:** Levi legt vor dem Lauf fest, welche Quote welche Konsequenz hat (Opus-Diskussionsvorschlag: <2% → Gate-System zu eng; 2-10% → Leveldichte reicht; >10% → Problem liegt beim Trigger-Timing, nicht bei den Gates).

**Flankierend:** `scripts/add_skipped_setup.cjs` existiert bereits (`ablehnungsgrund`, `hypothetisches_ergebnis`) — jeder Gate-Lauf ohne PASS dort verpflichtend erfassen, damit eine Datenbasis wächst, auch ohne Trades.

**Quelle:** Analyse 04.09., Gesamtbild-Abschnitt.

---

### 9. DATENTASK — ATR(5min) am 21.08.2026 (#42/#43) messen; Trade-DB liefert sie nicht
`atr_at_entry` ist für #42/#43 in `trades.db` NULL — muss aus historischen 5min-Bars (21.08., ca. 15-19 Uhr DE) via `data_get_ohlcv` berechnet werden.

**Indiz aus der DB (schwächt Opus' eigene F4(3)-Hypothese):** Beide Trades `tier='schock'`, `sl_distance=53` Pkt. Schock-Floor SL≥2,5×ATR → ATR höchstens ~21 Pkt — im selben Bereich wie 04.09. (22,3-33,1), nicht bei den vermuteten 60+. Falls die Messung das bestätigt, ist die Ursache nicht "Fehlkalibrierung", sondern eine bewusste Zielkonflikt-Entscheidung — ändert Punkt 3 nicht, aber die Begründung.

**Bereits belegt (`notes`-Feld):** Zone 29.240-29.254, SL #43=29.248 (innerhalb), SL #42=29.255 (1 Pkt über Oberkante) — beide bleiben unter jeder Bandregel-Option FAIL, stärkstes Argument für Bandlogik.

**Quelle:** Analyse 04.09., F4(3); `scripts/trades.sql` Z.85-86; `gate_check.cjs:879`.

---

## Niedrig

### 10. PROMPT/PROZESS-ERGÄNZUNG — 1H-Asymmetrie: messen statt drehen, mit vorab definierter Entscheidungsregel
Keine Regeländerung nach einem Tag (Overfitting-Gefahr). Drei risikofreie Schritte: (1) "3/4-Blockade" als Ereignis definieren und in der Tagesbilanz zählen (04.09.: 1 Blockade, ~100 Min, 222 Pkt). (2) Schattenmessung — Abstand Kurs↔1H-EMA50 absolut und in ATR, je Voll-Check nach `oneh_shadow_log.jsonl`. (3) Entscheidungsschwelle vorab festlegen (Vorschlag: ≥5 weitere Testtage ODER ≥10 Blockaden).

**P8-Kandidaten zur Vorbereitung, nicht zur Entscheidung:** (i) Überschuss-Klausel bei X×ATR-Abstand + Einigkeit der anderen 3 Ebenen; (ii) räumliches Kriterium — 1H-EMA50 blockiert nur, wenn sie zwischen Entry und TP1 liegt (Opus hält das für konzeptionell sauberste); (iii) Strukturanker statt EMA (letztes 1H-HH/LL).

**Quelle:** Analyse 04.09., F8; [[project_1h_kriterium_offene_frage_2026-09-03]].

---

## Anhang A — Bindend, nicht Teil der acht Gesprächspunkte: Register-Frischeprüfung

**Auflage aus Opus' Abbruch-Empfehlung "EINGESCHRÄNKT"**, vor dem ersten 7b1-Order-Ablauf zu erledigen. Am 04.09. haben 17 aufeinanderfolgende Voll-Checks (VC#29-#45) ein 95-175 Min altes Register als "weiter gültig" bezeichnet. Ursache (codeverifiziert): `gate_check.cjs` kennt 60-Min-Warnung und 90-Min-Hard-Exit (`:446`, `:1908`) korrekt, aber nur beim Gate-Aufruf — zwischen Triggern prüft niemand, die Voll-Check-Pflichtzeile kennt offenbar nur die 60-Min-Schwelle.

**Vorschlag:** `scripts/register_check.cjs` (Muster `x_fetch_stamp.cjs`), läuft bei jedem Voll-Check, Ausgabe wörtlich in die Pflichtzeile, Exit-Code 0/1/2 für ok/Warnung/hart. Plus `register_touch.cjs` als einziger Schreibweg für `updated` (echte UTC, lehnt Zukunftsstempel ab). Schwellen 60/90 aus einer gemeinsamen Konstantenquelle.

---

## Anhang B — Beobachtet, nicht beauftragt

- A3-Messfeld-Ausnahmen als Dauerzustand (`--trend-effizienz`, `--dual-gate-qqq-abstand-atr` 4/4 mit Standardbegründung ersetzt) — kein Auftrag heute.
- Plausibilitätswarnung (VC#7 `kerzen-qqq`) ignoriert, folgenlos.
- Screenshot-Selbstsuspendierung (39 VCs ohne Screenshot) — gehört zur Screenshot-Bilanz aus Punkt 5(b).
- Punkt 7 der Ursprungsliste (X-API) — ausgeschlossen, Levis eigener Bedienfehler.

---

## Übersicht

| Priorität | SKRIPT-FIX | REGELWERK-FRAGE | DATENTASK | PROMPT/PROZESS | Summe |
|---|---|---|---|---|---|
| Hoch | 2 (#1,#2) | 2 (#3,#4) | – | 1 (#5) | **5** |
| Mittel | 2 (#6,#7) | – | 2 (#8,#9) | – | **4** |
| Niedrig | – | – | – | 1 (#10) | **1** |
| **Summe** | **4** | **2** | **2** | **2** | **10** |

**Empfohlene Reihenfolge (Opus):** #1 und #2 zuerst (entscheidungsneutral, machen #3/#4 überhaupt erst sauber entscheidbar) → #9 (die eine Zahl) → Levis Entscheidung zu #3/#4 → #7 und #8 parallel, weil unabhängig von der Schwellenentscheidung.

*Diese Liste ist ein Entwurf zur Weitergabe an Fable. Nichts davon ist umgesetzt oder beschlossen.*
