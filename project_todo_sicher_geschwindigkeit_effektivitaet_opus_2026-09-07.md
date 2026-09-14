---
name: project-todo-sicher-geschwindigkeit-effektivitaet-opus-2026-09-07
description: "Opus' gefilterte To-Do-Liste (07.09.2026) aus beiden unabhängigen Studien (Opus+Fable): 12 Punkte S1-S12, die garantiert verbessern OHNE Risiko zu verschieben, plus 12 zurückgestellte Punkte Z1-Z12 (Risikoverschiebung/Regelwerksfrage). NICHTS umgesetzt — reine Leseliste für Levi, danach unabhängiger Fable-Gegencheck geplant."
metadata: 
  node_type: memory
  type: project
  status: draft
  modified: 2026-09-07T17:16:10.389Z
  originSessionId: b594a3be-34a1-46da-8616-0f50ceb56daa
---

# Opus' sichere To-Do-Liste (07.09.2026) — aus beiden Studien gefiltert

**Nichts davon ist umgesetzt oder beauftragt.** Filterregel: nur Punkte, die (a) einen bestehenden Arbeitsschritt automatisieren oder eine bereits vorgeschriebene Rechnung maschinell machen, (b) keine Prüfung entfernen, keine Schwelle bewegen, keine Gate-Eingabe erweitern, (c) keine Regelwerksentscheidung voraussetzen.

## Sicher — Geschwindigkeit
- **S1** `vollcheck.cjs` — ein Aufruf erzeugt den kompletten Pflichtzeilen-Block statt 3 Einzelaufrufe + Handkomposition. Auflage: kein Parameter darf einen Default haben, fehlender Wert = sichtbare Lücke.
- **S2** `position_tick.cjs` — Positionszeile maschinell (SL/TP-Abstände, Zertifikatspreise, R-Multiple, MFE, Stall-Zähler). Genau hier passierte der dokumentierte Zertifikatspreis-Fehler.
- **S3** `exit_plan.cjs` — nur die deterministischen Teile (BE-Trigger, TP1-Teilverkauf, €/Punkt, Zertifikatspreise, Terminalzeit) vorberechnen. Trailing-Stufen bleiben bewusst draußen (strukturgebunden).
- **S4** `--trend-effizienz` in `gate_check.cjs` selbst berechnen statt A3-Standardausnahme (Präzedenz: Q2-Budget).
- **S5** `gate_check_QUICKREF.md` — einseitige Aufrufreferenz, damit im Trigger-Moment nicht der 400-Zeilen-Kopfkommentar gelesen wird (Fables stärkster Einzelfund).
- **S6** X-API-Fehlerzähler + Pflicht-Statuszeile, OHNE Kadenzänderung (nur Diagnose, kein Backoff — der wäre Z4).
- **S7** Register-Autopflege (Session-Extrema, Rundzahl-Band) aus der ohnehin geholten `data_get_ohlcv`-Ausgabe.

## Sicher — Effektivität
- **S8** Setup-Karte / `gate_check --vorschau` — SL-Anker/TP1/Entry-Fenster/TP2 berechnen und anzeigen sobald ein Dual-Gate-Bein steht, in `setup_karte.json`. Beide Beine bleiben Pflicht, keine Order früher — nur Zahlen werden sichtbar, die heute erst nach dem Trigger entstehen. (Das tatsächliche Einsteigen im Fenster statt am Ausbruch ist Z8, nicht hier.)
- **S9** Trigger-Kandidaten-Zähler — der fehlende Nenner für jede Effektivitätsaussage (auch für Opus' eigene ~25-30%-Schätzung).
- **S10** Levelvollständigkeits-Zeile in `register_check.cjs` (4 von 8 kanonischen Levelarten besetzt am 04.09.).
- **S11** Struktur-Swings/Opening Range/T-2/T-3-Extrema als Messkategorie erfassen, aber NICHT TP-fähig machen (bleibt von `eligibleLevels()` ausgeschlossen) — schafft die Datengrundlage für Z7, ändert heute keinen Gate-Ausgang.
- **S12** Hypothetisches Ergebnis zu jedem `skipped_setup` beim Tagesabschluss nachtragen (Spalte existiert, Gegenrechnung fehlt).

---

## Zurückgestellt — Risikoverschiebung oder Regelwerksfrage, Levi entscheidet später

- **Z1** Vier-Pane-Layout mit festen Zeitrahmen — größter gemeinsamer Zeithebel, aber Layout-Werkzeuge sind historisch unzuverlässig (`layout_switch` fast NAS100 überschrieben). Vorschlag für später: separates Testlayout, Nicht-Handelstag.
- **Z2** Screenshot-Regel lockern (Opus A3/Fable A7) — weniger visuelle Kontrolle = mehr Blindflug. 39×-Selbstsuspendierung macht's zur Regelfrage.
- **Z3** Positions-Modus: Setup-Suche-Zeilen bei offener Position aussetzen (Opus A6/Fable A5) — Opus hält es für richtig, aber es ist Auslassung von Prüfschritten. Großteil des Zeitgewinns kommt ohnehin über S2.
- **Z4** X-API-Backoff (Diagnose ist S6, Kadenzänderung selbst bleibt hier).
- **Z5** 1H nur bei neuem 1H-Schluss lesen — **Opus widerspricht Fable hier explizit**: dieselbe Lesung liefert den EMA50(1H)-Wert für die gerade erst (07.09.) beschlossene 1H-Schatten-Pflichtzeile. Mit Z1 kostet die Lesung ohnehin nichts mehr.
- **Z6** `vollcheck_data.mjs` direkt auf CDP (Fable A8) — zweiter Client, umgangene MCP-Guards, Systemrisiko. Nicht mal der Spike vor Z1.
- **Z7** Neue TP-fähige Levelarten scharfschalten (Struktur-Swings/OR/T-2/T-3, NAS100-AVWAP, Fib-Retracement, Bollinger-Kanten) — heben PASS-Quote, können Trefferquote senken. Erst messen (S11), dann entscheiden.
- **Z8** Retest-Entry statt Ausbruchs-Entry + Alert auf Fensterkante — echte Regelentscheidung ("die Wette, die 8b immer schon eingeht, nur jetzt mit Zahl").
- **Z9** QQQ-ATR-Indikator ergänzen oder Feld streichen — beides nicht risikofrei (Layout-Änderung bzw. Pflichtfeld-Entfernung).
- **Z10** Kompakt-Tokens für latchende Pflichtzeilen (Fable A9) — genau die Fehlerklasse vom 27.08. (zählerlose 8d-Zeile), danach verboten. Erledigt sich mit S1.
- **Z11** Bereits von den Autoren selbst abgelehnt: 25er-Rundzahlraster, Mittel-Pivots (M(PP,R1) lag 1,2 Pkt neben ohnehin registrierter Rundzahl).
- **Z12** Einigkeit ohne Diskussionsbedarf: 8c2-Sockel und P4-Anker NICHT anfassen (gerade bestätigtes Design).

**Offen aus dem letzten Gegencheck, nicht Teil dieser Studie, aber gleiches Arbeitspaket:** A3-Pflichtfeld für 7b1-Schritt-0, Varianten-Erzeugungsregel vor #8-Datenlauf, `!==` statt `>` in den Bilanz-Prüfungen.

**Nächster Schritt (noch nicht ausgelöst):** Levi liest diese Liste zuerst selbst. Erst danach lässt er Fable die Liste unabhängig gegenprüfen.
