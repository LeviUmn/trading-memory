---
name: opus_nachtrag_testtag_2026-10-01_1900-2000
description: "Opus-Nachtrag 02.10.2026 zum Testtag 01.10. fuer 19:00-20:00 DE (Bars nachgeholt): KEIN Gate-PASS/Live-Trade moeglich. Short: kein Trigger, 13.1 k=0. Long ab 19:30 2/2, aber 1H-Override-Veto bis 20:00; selbst ohne Veto TP1-Fenster ohne Registerlevel bzw. leer. Q-ROT ohne Einfluss. Neuberechnung tagesmomente/skipped/kombi bytegenau reproduziert und plausibel; der Tag zaehlt fuer Freeze (5/5, AUTO 10/20) und H1. Fuer 19:00-20:00 lief kein Voll-Check, alle Aussagen sind hypothetisch"
metadata:
  type: project
  originSessionId: opus-nachtrag-testtag-2026-10-01-1900-2000
  modified: 2026-10-02T08:14:40.923Z
---

# Opus-Nachtrag Testtag Do 01.10.2026: 19:00-20:00 DE (erstellt Fr 02.10.2026, ca. 10:13 DE)

Bezug: [[opus_bericht_testtag_2026-10-01]] (Vorbericht, Horizont 19:00) und [[project_testtag_2026-10-01_abschluss]]. Levi hat entschieden, dass der Tag fuer den Freeze-Zyklus zaehlt. Die Bars bis 20:00 DE wurden am 02.10. nachgeholt.

**Grundvorbehalt (d), vorab:** Zwischen 19:00 und 20:00 DE lief **kein Voll-Check**. Abdeckung: 43/54 Slots = 80 %, TEILTAG. Es fehlen die Slots 19:05 bis 19:55 (11 Slots). Alles in diesem Nachtrag ist eine **hypothetische Rekonstruktion aus Bars**. Folgendes ist nicht rekonstruierbar:
- Handwerte des Loops: Q1-Selbstauskunft, Punkt-11-Belegpflicht aus Check-ins/Quick-Ticks, Ankerwahl mit `--grund-sl-anker`, Register-Touches und Session-Extrema, Tweet-Check, VIX/RVOL
- Live-Kurse innerhalb der Kerzen
- Shadow-Log-Momente, also auch die tagesmomente-Momente dieser Stunde
- Die Entscheidungen des Operators

## Datenbasis und Methode

**Dateien (nur gelesen):**
- `scripts/nas100_5m_2026-10-01.json`: 311 Bars, letzte Bar 17:55Z.
- `nas100_15m_/nas100_60m_/qqq_15m_/qqq_5m_2026-10-02_fuer_2026-10-01.json`, abgeschnitten bei Bar-Beginn < 01.10. 18:00Z.

**Integritaet:**
- Abgleich mit `nas100_5m_2026-10-01.bis1900.bak.json` (300 Bars): Genau 1 Bar weicht ab, die 17:00Z-Bar. Im Backup war sie noch laufend (H 30339,75 / C 30332,15), jetzt ist sie final (H 30381,65 / C 30378,55). Alle anderen 299 Bars sind identisch. Das ist erwartbar und korrekt.
- Die 15m-Datei stimmt fuer 13:30Z-17:45Z mit der Aggregation aus der 5m-Datei ueberein (H/L/C, je 3 Bars), ohne Abweichung.
- Einzige Luecke in der 5m-Datei: 30.09. 21:00Z-22:00Z (Nachtpause, ausserhalb des Pflichtfensters).

**Skriptlaeufe, ausschliesslich auf Kopien:**
- Kopie: `%TEMP%\opus_nachtrag_1001\scripts`, dazu `%TEMP%\momente_now.jsonl` / `momente_rerun.jsonl`.
- Gelaufen: `tagesmomente.cjs --datum 2026-10-01` und `--auswertung`, `kombi_fiktiv` (gelesen), `gate_check.cjs --sl-vorpruefung --sl-auto` (4 hypothetische Long-Laeufe), `anker_auto.autoAnker()`, `analyse/h1_auswertung.cjs --echt --zaehlstand`.
- Repo unveraendert: `momente_log.jsonl` sha1 f88c4caf… vorher = nachher; `gate_check_log.jsonl` / `sl_anker_wechsel_log.jsonl` / `last_sl_vorpruefung.json` mtime unveraendert (01.10.). Die /tmp-Dateien vc_2026-10-01_* habe ich nicht angefasst.

**Indikatoren (Handrechnung per Skript in %TEMP%, `opus_nachtrag_calc.cjs`):**
- EMA50 mit SMA-Seed wie `atr_qqq.computeEma`, ATR(14, Wilder) mit `computeAtr`, MACD-H(12,26,9), QQQ-Session-VWAP ab 13:30Z aus hlc3×Volumen der 5m-Bars.
- **Kalibrierung gegen VC#44 (19:01, Werte der um 19:00 geschlossenen Bars):**

| Groesse | Meine Rechnung | VC#44 | Differenz |
|---|---|---|---|
| 5m-EMA50 | 30408,83 | 30405,8 | +3,0 |
| 15m-EMA50 | 30501,81 | 30495,2 | +6,6 |
| 1H-EMA50 | 30484,86 | 30478,9 | +6,0 |
| QQQ-15m-EMA50 | 741,09 | 740,96 | +0,13 |
| QQQ-VWAP | 739,36 | 739,81 | −0,45 |
| 5m-MACD-H | +4,17 | +2,60 | |
| ATR | 51,0 | 48,2 | |

Alle Urteile unten haben Abstaende, die deutlich groesser sind als diese Abweichungen. Ausnahme ist die 19:10-Kerze am 5m-EMA50; sie ist entsprechend markiert.

## (1) Kerze fuer Kerze, 19:00-20:00 DE

Ausgangslage aus VC#44 (19:01):
- Alle Ebenen bearisch, Dual-Gate 2/2 SHORT, 1H-Override bearisch (1H-Bar 18:00-19:00 DE: Close 30332,55 < EMA50).
- SL-Anker 30414,05 (= AUTO), k=0/2 (Punkt-11 ja), "warten: kein Trigger, Q-ROT-Auslassen".

Die Bar-Zeiten sind DE-Ortszeit, Beginn-Ende.

| 5m-Bar | O / H / L / C | 5m vs EMA50 | MACD-H | Dual-Gate (15m NAS / QQQ, letzter geschl. 15m-Bar) | 1H-Override (letzter geschl. 1H) | Ereignis |
|---|---|---|---|---|---|---|
| 19:00-05 | 30332,55 / 30381,65 / 30327,05 / 30378,55 | unter (30407,65) | +5,6 | SHORT / SHORT | bearisch (C 30332,55 < 30484,86) | – |
| 19:05-10 | 30378,55 / 30394,55 / 30374,15 / 30379,75 | unter | +6,4 | SHORT / SHORT | bearisch | – |
| 19:10-15 | 30379,75 / **30419,25** / 30377,35 / 30400,25 | unter (30406,30; knapp, mit VC-Offset etwa 3 Pkt unter EMA) | +8,0 | 15m-Bar 19:00 C 30400,25 < 30497,82 / QQQ 739,42 < 741,03 → SHORT / SHORT | bearisch | **Docht ueber dem SL-Anker 30414,05** → V9/3a-Wechselpflicht nach oben, kein Trigger |
| 19:15-20 | 30400,25 / 30447,75 / 30397,55 / **30440,25** | **UEBER** (30407,64) | +11,3 | SHORT / SHORT | bearisch | **Trigger-Moment (a), frischer 5m-Schluss ueber EMA50 (Long-Richtung)**: Long-Dual-Gate 0/2. Schluss ueber dem alten Anker 30414,05 |
| 19:20-25 | 30440,25 / 30520,75 / 30440,05 / **30520,75** | ueber | +17,9 | SHORT / SHORT | bearisch | **Trigger-Moment (b): Pivot PP 30444,18 per Schluss gebrochen (long)**, Long-Dual-Gate 0/2. Hoch 30520,75 **trifft alle offenen Short-SLs** (30438-30513,7) |
| 19:25-30 | 30520,75 / 30542,75 / 30509,25 / 30535,75 | ueber | +22,2 | **15m-Bar 19:15: C 30535,75 > 30499,31 / QQQ 742,64 > 741,09 (VWAP 739,54) → LONG / LONG = 2/2 LONG** (ab 19:30) | **bearisch** (C 30332,55 < 30484,86) → Veto | **Trigger-Moment (a) QQQ-15m frischer Schluss ueber EMA50.** 2/2 LONG technisch erfuellt, **1H-Override dagegen → kein gueltiges 2/2, kein 7b1** |
| 19:30-35 | 30535,75 / 30602,55 / 30535,75 / 30580,65 | ueber | +26,5 | LONG / LONG | bearisch → Veto | – |
| 19:35-40 | 30580,65 / 30582,55 / 30551,85 / 30557,25 | ueber | +26,3 | LONG / LONG | Veto | – |
| 19:40-45 | 30557,25 / 30587,25 / 30548,75 / 30587,05 | ueber | +26,6 | 15m 19:30 C 30587,05 > 30502,75 / QQQ 743,88 > 741,20 → LONG / LONG | Veto | Schluss 30587,05 knapp **unter** dem US-Session-Hoch 30588,05 (das ohnehin nicht im Register steht, siehe Vorbericht A) → kein Level-Bruch |
| 19:45-50 | 30587,05 / **30620,85** / 30580,55 / 30580,55 | ueber | +24,7 | LONG / LONG | Veto | Hoch 30620,85: Docht ueber 30588,05 / Rundzahl 30600, Schluss darunter. PDH 30635,25 und R1 30649,57 nicht erreicht |
| 19:50-55 | 30580,55 / 30590,25 / 30543,15 / 30546,65 | ueber | +19,9 | LONG / LONG | Veto | – |
| 19:55-20:00 | 30546,65 / 30561,55 / 30513,35 / 30531,85 | ueber | +14,5 | 15m 19:45 C 30531,85 > 30503,89 / QQQ 742,57 > 741,25 → LONG / LONG | erst **mit Schluss 20:00:00** wird der 1H-Bar 19:00-20:00 (C 30531,85 > EMA50 30486,71) bullisch | Veto faellt exakt zum Stichtag 20:00 (Terminalzeit) |

**Short-Seite 19:00-19:30 (Dual-Gate 2/2 SHORT, 1H nicht dagegen):**
- Kein Trigger-Moment in Short-Richtung: kein frischer 5m-/QQQ-Schluss unter EMA50 (der Kurs lag schon darunter und stieg) und kein Level-Bruch nach unten (Tiefs 30327,05-30397,55, Session-Tief 30264,05 weit weg).
- 13.1 blieb aus: Punkt-11-Kriterium 1 lag durchgehend vor (5m-MACD-H in jeder Kerze positiv: VC#43 +3,5, VC#44 +2,6, gerechnet +5,6 bis +22), ab 19:20 auch Kriterium 2 (Schluss ueber 5m-EMA50). Damit k=0, keine "50 %-Einstieg AKTIV"-Konsequenz.
- Ab 19:30 ist das Short-Dual-Gate 0/2.

**SL-Anker short (V9/X1):**
- 19:10-Kerze: Docht 30419,25 > 30414,05. Nach 3a ("hoechstes HIGH aller abgeschlossenen 5m-Kerzen seit dem Impulstief", Tief 30264,05) wird der Anker zur **Pflicht** nach oben verschoben: 30419,25.
- 19:15-Kerze: Schluss 30440,25 ueber dem alten Anker, neues Hoch 30447,75.
- 19:20-Kerze: 30520,75.
- AUTO (Skript `autoAnker`): bis 19:15 30414,05; ab 19:20 `null` ("kein bestaetigter Swing nach Extrem"). Der Anker wandert also mit dem Kurs und liegt ab 19:25 unter dem Kurs. Fuer Shorts ist er damit funktionslos. Das ist kein Chasing, sondern dieselbe 3a-Luecke (Backlog ii im Vorbericht).

**Long-Seite 19:30-20:00 (2/2 LONG technisch, 1H-Veto):**
- Nach der verbindlichen Override-Definition vom 04.09. zaehlt nur der zuletzt GESCHLOSSENE 1H-Bar. Das ist bis 19:59 der Bar 18:00-19:00 DE mit Close 30332,55, rund 152 Pkt unter der EMA50.
- Ergebnis: **"2/2-Zustand verworfen (1H-Override)"**, kein 7b1-Lauf, kein Live-gate_check.
- Mindestpause, Stall und Punkt 12 sind nicht einschlaegig, weil keine Position offen war.

## (2) Antwort (a): Haette das unveraenderte Regelwerk 19:00-20:00 einen Gate-PASS oder Live-Trade erzeugt?

**Nein, weder long noch short.** Blockiert haben:
- **19:00-19:30 SHORT:** kein Short-Trigger (Abschnitt 1). 13.1 gesperrt, weil die Punkt-11-Signale k auf 0 hielten.
- **19:20 und 19:25 LONG-Trigger** (5m-EMA50-Reclaim, PP-Bruch): Long-Dual-Gate 0/2, weil NAS-15m und QQQ-15m noch unter EMA50 standen. Kein 7b1.
- **19:30-20:00 LONG:** Dual-Gate 2/2, aber **1H-Override-Veto** in jedem Slot (19:30, 19:35, …, 19:55). Das Veto faellt erst mit dem 1H-Schluss um 20:00:00, also zum Stichtag. Danach ist nichts mehr Teil der Messung.

**Zusatz zur Information: Haette ohne das Veto ein PASS entstehen koennen?** Echte Skripte auf der Kopie, `--sl-vorpruefung --sl-auto --cluster-level none`:

| Moment | Entry | Anker (Herleitung) | SL | SL-Dist. | TP1-Fenster | Ergebnis |
|---|---|---|---|---|---|---|
| 19:30 | 30535,75 | 30440,05 (Tief der Ausbruchskerze 19:20, engste denkbare Struktur) | 30415,2 | 120,5 = 2,43×ATR 49,7 | [30656,3 ; 30684,85], 28,6 Pkt | **kein Registerlevel und keine 50er-Rundzahl im Fenster** (30650 und R1 30649,57 liegen darunter, 30700 darueber) |
| 19:30 | 30535,75 | AUTO 30437,85 (`autoAnker`: erstes Swing-Tief 15:45 nach Extrem 30588,05) | ca. 30413,0 | etwa 2,47×ATR | praktisch dasselbe Fenster | ebenso kein Level |
| 19:30 | 30535,75 | 30293,55 (Tief vor dem Ausbruch) | 30268,7 | 267 = 5,37×ATR | **LEER** | max. RR 0,558 |
| 19:45 | 30587,05 | 30440,05 | 30415,7 | 171,3 = 3,52×ATR 48,7 | **UNLOESBAR/LEER** | max. RR 0,853 |

- Ab 19:45 und 20:00 lieferte `autoAnker` long `null`.
- Ein Live-Lauf haette also mit keinem plausiblen Anker ein PASS-faehiges TP1 aus dem Register gehabt. Der Weg waere ein Retest gewesen, der bis 20:00 nicht stattfand (Tief nach 19:30: 30513,35).
- Q-Score haette ohnehin ROT ergeben:
  - Q2 = |30535,75 − 30416,92| / 49,7 = **2,39× ✗** (19:45: 3,13× ✗)
  - Q3 ✗, weil der 1H-Bar dagegen steht
  - Q1/Q4 nicht belegt
  - Bestenfalls 2/4 = ROT.

**IBF-Schatten (nur Messung):**
- Die Bedingungen 1-3 der IBF-Definition waren 19:30-20:00 erfuellt (Veto aktiv, 3/4 long, Kurs ueber der 1H-EMA50).
- Ohne Voll-Check gibt es aber keinen Shadow-Log-Eintrag. Die Episode existiert fuer `ibf_schatten.cjs` nicht.
- Inhaltlich waere sie nach Kriterium 1 nicht zaehlend gewesen: Q3 ✗ + Q2 ✗ ergibt hoechstens 2/4 = ROT, AUTO-Fenster zu bzw. `null`.

## (3) Antwort (b): Haette ein Trade ohne Q-ROT einen Unterschied gemacht?

**Fuer 19:00-20:00: nein.** Es gab keinen einzigen PASS, an dem Q-ROT haette greifen koennen. Blockiert haben Trigger, Dual-Gate, 1H-Override und die TP1-Geometrie, nicht der Q-Score.

**Neutrale Messdaten fuer den ganzen Tag mit Horizont 20:00.** Sie aendern die Zahlen des Vorberichts, der auf 19:00 stand:

| Position | Horizont 19:00 (Vorbericht) | Horizont 20:00 (jetzt) |
|---|---|---|
| Q-ROT-PASS 16:11:52, volle Groesse | +1,13 R | +1,13 R (TP1 16:25, unveraendert) |
| Q-ROT-PASS 17:06:44, volle Groesse | −0,02 R OFFEN | **−1,00 R** (SL 30489,3 in der 19:20-Kerze, Kerzenschluss 19:25) |
| **Netto beide Q-ROT-PASS, volle Groesse** | +1,11 R | **+0,13 R** |
| 13.1-Haelfte mit TP1+BE, mit Stall 12.1a (17:06 Stall-Exit 18:00) | +0,22 R | +0,22 R (Stall feuerte vor 19:00, unveraendert) |
| 13.1-Haelfte mit TP1+BE, ohne Stall | (−0,01 R) | **−0,22 R** (0,5 × (0,56 − 1,00)) |
| Strenge Punkt-11-Lesart, hypothetisches Gate 17:41 (PASS-ROT, SL 30445,1) | −0,04 R OFFEN | **−1,00 R** (19:15-Kerze, Hoch 30447,75) |

Die Formulierung im Vorbericht "Q-ROT kostete +1,13 R" gilt damit nur fuer den Einzeltrade 16:11. **Saldo aller Q-ROT-Ausschluesse des 01.10. mit 20:00-Horizont: +0,13 R** (volle Groesse). Das ist reine Messung, ohne Bewertung und ohne Vorschlag. Die Q-ROT-Frage bleibt fuer die Endauswertung (H1, 30.10.).

## (4) Antwort (c): Freeze-Zaehler und Plausibilitaet der Neuberechnung

**Reproduktion:**
- `tagesmomente.cjs --datum 2026-10-01` habe ich auf der Kopie von `momente_log.jsonl` mit der neuen Bars-Datei neu laufen lassen. Ergebnis **bytegenau identisch** mit dem Repo-Stand (sha1 f88c4cafdffad0b6… = f88c4cafdffad0b6…). Bars: "ok (66/66 im Pflichtfenster)".
- Tagesbilanz 01.10.:
  - ALT 4 unabhaengig, TP 2 / SL 2 / OFFEN 0, Σ **+0,00**
  - AUTO 1, SL 1, Σ **−1,00**
  - FLOOR 4, TP 3 / SL 1, Σ **+2,00**
- `--auswertung`:
  - **5 bewertbare Tage** (25., 28., 29., 30.09., 01.10.)
  - ALT n=10, Σ **+2,33** (Ø +0,23; ohne besten Tag −0,11)
  - AUTO n=10, Σ **−1,87** (Ø −0,19; ohne besten Tag −0,38)
  - FLOOR n=21, Σ **−1,14** (Ø −0,05)
  - "FREEZE-ENDE ERREICHT: nein — AUTO 10/20 (fehlen 10; Abbruchregel 5/10 bewertbare Tage)"
  - **Die Zahlen von Levi bzw. Fable stimmen.**
- Gegenprobe der Summen gegen den Vorbericht (Tabelle C):
  - AUTO bis 30.09.: n 9, Σ −0,87 (−0,70 +0,03 −1,00 +0,80); dazu 01.10. n 1, −1,00 → n 10, Σ −1,87 ✓
  - FLOOR bis 30.09.: n 17, Σ −3,14; dazu +2,00 → n 21, Σ −1,14 ✓

**Stichprobe 1, AUTO-Moment 16:56 (VC#19), von Hand:**
- Entry 30376,15, AUTO-Anker 30458,25 → SL 30489,0 (Dist 112,85), RR1-Punkt 30263,3.
- Ab der Bar 17:00 war das tiefste Tief 30264,05 (17:10-Kerze). Das liegt **0,75 Pkt ueber** dem RR1-Punkt, also kein TP.
- Bis 19:15 lag kein Hoch ≥ 30489,0 (Maxima 30414,05 / 30410,85 / 30447,75). Die 19:20-Kerze hat das Hoch 30520,75 ≥ 30489,0 → **SL-HIT, exit_ts 17:25Z = Kerzenschluss 19:25 DE, −1,00 R** ✓.
- Alter Wert: OFFEN +0,39 = (30376,15 − 30332,55) / 112,85 = +0,386 ✓.

**Stichprobe 2, ALT-Moment 17:06 (VC#21) bzw. skipped 17:06:**
- Entry 30330,85 (skipped: 30329,85), SL 30489,3, RR1-Punkt 30172,4.
- Tiefstes Tief danach 30264,05, also kein TP. Erstes Hoch ≥ 30489,3 ist 30520,75 in der 19:20-Kerze → **SL-HIT 19:25, −1,00 R** ✓.
- Kombi: Stall-Exit 12.1a beim Kerzenschluss 18:00, Close 30348,85 → (30329,85 − 30348,85) / 159,45 = −0,119 → **−0,12 R × 0,25 = −0,03 R** ✓. `r_ohne_stall` −1,00 (SL 19:25) ✓.

**Zusaetzlich nachgerechnet:** skipped 16:07 (FAIL rrGate)
- SL 30513,7: Bis zur 19:15-Kerze lag kein Hoch darueber (Max 30458,25 um 16:30); die 19:20-Kerze hat 30520,75. Das sind 39 Kerzen von 16:10 bis 19:20 → **SL-HIT 19:25, −1,00 R** ✓.
- MFE 76,5 (30340,55 − 30264,05) ✓.

**Fachlich: Ist der Wechsel OFFEN → SL-HIT 19:25 nachvollziehbar?** Ja.
- Der 19:00-Wert war ein Artefakt des verkuerzten Horizonts.
- Die 19:20-Kerze (+80,5 Pkt, Schluss am Hoch 30520,75) hat **saemtliche** offenen Short-SLs des Tages auf einmal genommen (Spanne 30438,15-30513,7).
- Das ist genau das *optional-stopping*-Risiko, vor dem der Vorbericht (G2) gewarnt hat. Mit 19:00-Horizont haetten die Zahlen gelautet: ALT +0,99 statt +0,00, AUTO +0,39 statt −1,00, FLOOR +3,21 statt +2,00, skipped-Tagessumme +2,03 statt **+0,00**.
- Der Stichtag 20:00 aus der Bewertbarkeits-Definition hat damit eine Verzerrung von −1,0 bis −1,4 R je Variante verhindert.

**Kleiner Datenbefund (Messhygiene, ohne Wirkung auf R):**
- In `skipped_setups_fiktiv.jsonl` tragen die Eintraege 16:07 und 17:06 nach dem Neu-Nachtrag `exit_art` "SL-HIT", aber weiter `terminalkurs` 30332,55. Das ist der alte 19:00-Schluss.
- Ursache: `skipped_fiktiv.cjs` Z. 330 setzt `terminalkurs` nur bei OFFEN neu und behaelt sonst den Vorwert.
- `ergebnis_r` ist korrekt. Das Feld ist nur ein Restwert aus dem 19:00-Lauf; das kann ins Backlog.

**H1-Zaehlstand** (`h1_auswertung --echt --zaehlstand` auf der Kopie, keine R-Werte):
- Der 01.10. wird **jetzt gezaehlt**: "gezaehlte (bewertbare) Tage: 1 (2026-10-01)". Der Vorbericht hatte bei 19:00-Horizont noch "nicht gezaehlt" ermittelt.
- Zelle A (Pruefzelle, FLOOR): n 0. FLOOR-B n 4, AUTO-B n 1. Zwischenauswertung faellig: nein.
- Hinweis ohne Wertung: Alle 4 FLOOR-Momente landen in "Trend nein", obwohl der 01.10. ein Short-Trendtag war. Die Trendkontext-Definition nach K-Kriterien habe ich hier nicht geprueft (nicht belegt, ob das korrekt ist). Bei der naechsten H1-Sichtung gegenpruefen.

**Kombi gesamt:** `r_primaer` ist unveraendert (16:11 +0,14, 17:06 −0,03), also bleibt Σ gewichtet **+0,85 R** (n 8). Ohne Stall waere 17:06 −0,25 gewichtet.

## (5) Antwort (d): Grenzen dieses Nachtrags

- **Kein Voll-Check 19:00-20:00** (11 Slots fehlen, Abdeckung 80 %, TEILTAG). Jede Aussage zu Gates, Triggern und Ankern in dieser Stunde ist eine Bar-Rekonstruktion, kein Protokoll.
- Der Loop haette weitere Handwerte und Entscheidungen erzeugt, die nicht rekonstruierbar sind:
  - Punkt-11-Signal je Check-in
  - Ankerwechsel mit eigener Begruendung (3a-Luecke)
  - Register-Touches: neues Session-Hoch? Das Register fuehrte 30882,6 als "Session-Hoch", das US-Hoch 30588,05 fehlte, der Docht 30620,85 ueberstieg es
  - Q1-Selbstauskunft, Tweet-Slots 19:10-19:50
  - Ein eventueller Gate-Lauf trotz Veto waere ein Regelbruch gewesen
- **Fehlende tagesmomente-Momente:**
  - Mit Voll-Checks haette es weitere Shadow-Log-Momente gegeben. Long-Momente ab 19:31 waeren wegen 1H dagegen *nicht qualifiziert* gewesen.
  - Short-Momente 19:06/19:11/19:16/19:21 waeren qualifiziert, aber nicht unabhaengig gewesen (vor dem Exit 19:25 aller Varianten).
  - **Einzige zaehlrelevante Luecke ist ein moeglicher Moment um ca. 19:26.** 15m und QQQ sind bis zum 15m-Schluss 19:30 noch short, also qualifiziert und unabhaengig.
  - Hypothetisch fuer Entry 30509-30543 (Kerzenspanne 19:25), Skript `geometrie` auf der Kopie: **FLOOR SL-HIT −1,00 R** (19:30- bzw. 19:45-Kerze); **AUTO `null`**, also fuer den AUTO-Zaehler ohne Wirkung. ALT nicht rekonstruierbar (Live-Anker unbekannt; nach 3a waere der 8c-Floor bindend, also wie FLOOR).
  - **Folge:** Der AUTO-Stand 10/20 ist gegen die fehlenden VCs robust. FLOOR (und eventuell ALT) koennte durch die Luecke um eine −1-R-Bewegung zu guenstig stehen. Das ist nicht belegt, nur eine Groessenordnung.
- Die Indikatorwerte stammen aus eigener Rechnung mit kleinen Kalibrierabweichungen (siehe Methode). Die Urteile haben grosse Abstaende, mit Ausnahme der markierten 19:10-Kerze am 5m-EMA50; diese ist fuer kein Urteil entscheidend.

## Kurzfassung fuer Levi

1. 19:00-20:00: **kein Gate-PASS, kein Trade.** Short ohne Trigger und mit 13.1 k=0. Long-Trigger 19:20/19:25 bei 0/2. Ab 19:30 2/2 LONG, aber **1H-Override-Veto bis 20:00**. Selbst ohne Veto: kein PASS-faehiges TP1 im Register (Fenster ohne Level oder leer), Q-Score ROT.
2. Q-ROT war in dieser Stunde ohne Einfluss. Tagessaldo aller Q-ROT-Ausschluesse bei 20:00-Horizont: **+0,13 R** statt +1,11 R (17:06 jetzt −1,00).
3. Neuberechnung bytegenau reproduziert, Stichproben stimmen, OFFEN → SL-HIT 19:25 ist fachlich korrekt. Die 19:20-Kerze nahm alle Short-SLs auf einmal.
4. Freeze: 5/5 Tage, AUTO 10/20, FREEZE-ENDE nein. H1 zaehlt den 01.10. jetzt (Zelle A n 0).
5. Hypothetisch, ohne Voll-Check. Die einzige zaehlrelevante Luecke (Moment ca. 19:26) haette AUTO nicht veraendert; FLOOR waere eventuell um −1 R schlechter.
