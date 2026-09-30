---
name: project_opus_analyseliste_2026-09-29_1h_override
description: "Analyse-Punkt fuer Opus (Levi 29.09.2026): 1H-Override pruefen — behindert er zu viele Entries? Nur Analyse/Vorschlag, Regelaenderung erst nach Freeze-Ende + Levi-Entscheidung"
metadata:
  node_type: memory
  type: project
  originSessionId: 06006c65-4ed0-4041-9028-70709ef145bd
  modified: 2026-09-29T15:35:30.421Z
---

# Opus-Analyseliste: 1H-Override (Auftrag Levi 29.09.2026, 17:35 DE)

**Frage von Levi:** "Ist 1H zwingend notwendig um short zu gehen? Ist das eine Pflichtregel?" — danach: Opus soll sich das nochmal naeher ansehen, ob wir da nicht etwas aendern muessen zwecks Entries fuer Trades.

**Stand der Regel (belegt):** Verbindlich seit 04.09.2026 ([[project_1h_kriterium_offene_frage_2026-09-03]]): 1H-Override = Bias des zuletzt GESCHLOSSENEN 1H-Bars (EMA50-Seite + Strukturrichtung), laufender Bar zaehlt nicht. Wirkt als Veto (darf nicht dagegenstehen), nicht als Zusatzbestaetigung. Ueberschuss-Klausel fuer den laufenden Bar (>=1,0xATR ueber >=2 5m-Schluesse) wurde am 04.09. bewusst NICHT eingefuehrt (P8/Stufe 3, erst nach Gegenrechnung gegen 01./02.09.). Im Freeze steht "1H-Override + Totzone" auf der Liste "nicht anfassen" ([[project_testtag_analyse_2026-09-28]]).

**Anlass 29.09.2026 (Fakten aus den Logs, fuer Opus nachzurechnen):**
- 17:15-Schluss: NAS100 15m (30278,95 < EMA50 30321,2) UND QQQ 15m (736,53 < 738,11) beide unter ihrer EMA50 = 2/2 Short-Beine, 5m ab 17:00 unter EMA50 (RSI 5m ~37, MACD-H ~-12).
- 1H-Override stand dagegen: letzter geschlossener 1H-Bar (16:00) Schluss 30387,55 ueber 1H-EMA50 ~30347; der 17:00-Bar lief bei ~30286 darunter, zaehlt aber erst nach Schluss um 18:00. Kurs fiel 17:00-17:25 von ~30362 auf 30256,55 (~106 Pkt).
- Short-Anker-Vorpruefung 17:31 (LH 30372,55): TAUGLICH, aber SL-Distanz 2,72xATR, TP1-Fenster nur 11,6 Pkt breit (30158,65-30170,25), max. RR 1,10:1, kein Registerlevel im Fenster — d.h. die Geometrie haette den Entry ohnehin fast unmoeglich gemacht (unabhaengig vom 1H-Override).
- Messung laeuft im 1H-Schatten (`oneh_shadow_log.jsonl`, "3/4-Blockade"); E7-Kriterium: Dauer >=15 Min UND Bewegung >=1,5xATR.

**Zu pruefen (Analysefragen an Opus):**
1. Wie oft und wie lange hat der 1H-Override ueber alle Testtage (17./21./23./25./28./29.09.) Entries blockiert (3/4-Blockaden aus `oneh_shadow_log.jsonl`), und was haetten die blockierten Momente in R gebracht (Richtung mit/gegen 1H, TP1/SL erreicht)? Auch die Faelle, in denen der Override Verlust vermieden hat (28.09.: FLOOR-Simulation ~-2R gespart).
2. Ist "letzter geschlossener 1H-Bar" zu traege (bis zu 60 Min Verzug)? Wie haetten die zurueckgestellte Ueberschuss-Klausel (P8) bzw. eine 1H-Struktur-/Slope-Variante die Blockaden veraendert — mit Zahlen, nicht Gefuehl.
3. Ist der Override das eigentliche Nadelöhr fuer Entries, oder sind es (heute belegt) die Geometrie-Grenzen (SL-Distanz/TP1-Fenster, Q2/Q4, Anker)? Blockaden nach Ursache aufschluesseln, damit keine Regel gelockert wird, die gar nicht der Engpass ist.
4. Falls Aenderung empfohlen: nur als Vorschlag mit Vorab-Kriterium (n, erwartete Wirkung, Rueckbau-Bedingung), freeze-konform (Schattenzeile/Messung zuerst), Overfitting-Warnung bei kleinem n. Realisiertes Payoff-Bild beachten (Ø -0,51R, Payoff 0,94:1, [[feedback_realisiertes_rr]]): "mehr Entries" ist kein Ziel an sich.

**Nicht tun:** keine Regelaenderung am Override ohne Levi-Freigabe und ohne Freeze-Ende; am 29.09. wurde er unveraendert angewendet (keine Abweichung im Protokoll).

**Why:** Levi vermutet, dass der 1H-Override Entries kostet und will eine belegbare Entscheidungsgrundlage. **How to apply:** In die Opus-Tagesanalyse 29.09. (20:01-Job) als eigener Abschnitt aufnehmen und beim naechsten Freeze-Review einplanen.
