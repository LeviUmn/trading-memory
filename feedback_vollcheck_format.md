---
name: feedback-vollcheck-format
description: "Voll-Check-Ausgabeformat: Fließtext mit ✓/✗ hinter jedem geprüften Punkt, keine Tabelle"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 7128a95a-a9a6-42e6-8a59-8f22dec5c654
  modified: 2026-08-26T09:10:06.404Z
---

Im Live-Loop-Voll-Check (siehe [[feedback_loop_ablauf_uebersicht]] Szenario 1, jede 5. Minute) das Ergebnis als **Fließtext mit ✓/✗ direkt hinter jedem geprüften Punkt** ausgeben — keine Markdown-Tabelle.

**Why:** Am 27.07.2026 (Live-Trading-Tag, Phase-3-Start) auf Tabellen-Format umgestiegen (eine Zeile pro Ebene: 1H/15min/5min/QQQ/Regime). User hat das als Abweichung vom bewährten Design erkannt: die Tabelle zeigt zwar Werte, aber nicht explizit, OB und WAS an Regeln/Kriterien tatsächlich durchgecheckt wurde — genau das ist der Zweck der bestehenden Offenlegungspflicht ([[feedback_loop_ablauf_uebersicht]] Szenario 1, Punkt 9.7: "im Output kurz nennen, was geprüft wurde und was ausgelassen wurde"). Das ursprüngliche Format nutzte dafür ✓/✗ direkt hinter jeder Teilprüfung (Muster aus [[feedback_live_trading]] Punkt 7a: "Indikatoren jetzt erfüllt ✓" / "nicht mehr erfüllt ✗"), damit User und Claude selbst auf einen Blick sehen, welche Kriterien tatsächlich bestätigt/nicht bestätigt/ausgelassen wurden — eine Tabelle mit reinen Zahlenwerten verwischt das.

**How to apply:** Jeder Voll-Check-Output folgt diesem Muster — jede Ebene startet in eigener Zeile mit **fettem Label**, Inhalt bleibt Fließtext-Satz mit ✓/✗ (kein Tabellen-Raster, aber klar getrennt statt einem dichten Absatzblock, siehe Korrektur 28.07.2026):

```
Voll-Check <Uhrzeit> — <1-Satz-Einordnung>

**1H NAS100:** RSI <x> ✓/✗ <bearish/bullish bestätigt>, MACD-H <x> ✓/✗, <EMA50/BB-Status> ✓/✗. Struktur: <HH-HL/LH-LL-Kurzfazit>.

**15min NAS100:** RSI <x> ✓/✗, MACD-H <x> ✓/✗.

**5min NAS100:** RSI <x>, MACD-H <x> (Trigger-Ebene, meist noch → warten).

**QQQ 15min:** <Kurs> unter/über EMA50 ✓/✗, unter/über VWAP ✓/✗, Volumen <erhöht/normal> ✓/✗.

**Regime-Gate (8d):** Schock-Tag ✓/✗ (<Kriterien-Zahl>/3).

**ADX(14, NAS100):** <Wert> (GEMESSEN, KEIN GATE).

**Chartmuster/Fibonacci:** <geprüft, Ergebnis> ODER <ausgelassen, Grund>.

**Tweet-Check:** <fällig+durchgeführt ✓, zuletzt vor Xmin (nichts Neues/kurzer News-Hinweis)> ODER <noch nicht fällig, zuletzt vor Xmin> ODER <ÜBERFÄLLIG ✗ → war Xmin überfällig, jetzt nachgeholt>. (Xmin = reines Minuten-Delta seit dem `x_last_fetch.json`-Zeitstempel, umgerechnet in Ortszeit — der Zeitstempel selbst wird NIE roh/UTC im Output gezeigt, siehe Korrektur unten.)

**Format:** Fließtext ✓

🟢/🔴/→warten <Fazit>
```

Jedes ✓/✗ steht für eine ECHTE Einzelprüfung, nicht dekorativ — bei "ausgelassen" (z.B. kein Impuls für Fibonacci) explizit als "ausgelassen" kennzeichnen statt ✗ zu setzen (✗ = geprüft und nicht erfüllt, "ausgelassen" = gar nicht geprüft, das ist ein Unterschied).

**Korrigiert 28.07.2026 (User-Wunsch nach mehr Struktur, live während Trading-Session):** Der ursprüngliche dichte Absatzblock (alle Ebenen in einem durchgehenden Fließtext ohne Zeilenumbruch) wurde als schwer lesbar empfunden. Lösung: Zeilenumbruch pro Ebene mit fettem Label, Satzform pro Ebene bleibt erhalten — explizit KEINE Rückkehr zur Tabelle (die 27.07.-Korrektur bleibt gültig), sondern eine Zwischenstufe mit mehr visueller Trennung. Zwei Alternativen (Stichpunkte, unveränderter Block) wurden dem User zur Wahl vorgelegt, diese Variante wurde ausdrücklich gewählt.

**Tweet-Check- und Format-Zeile ergänzt 27.07.2026 (Fable-Tagesabschluss-Review, nach zwei Prozessfehlern desselben Tages — siehe [[feedback_live_trading]] Punkt 9 "Struktureller Anti-Drift-Fix" und [[feedback_prozessfehler_27_07_fuer_fable]]):** Beide Zeilen sind ab jetzt fester, nicht optionaler Bestandteil jedes Voll-Check-Outputs — unabhängig davon, ob der Tweet-Fetch diese Runde überhaupt fällig ist oder ob "alles wie gewohnt" aussieht. Root-Cause war, dass ein "stiller" Check (kein sichtbarer Output bei "nichts zu tun") nicht von einem schlicht vergessenen Check zu unterscheiden war — die beiden Zeilen hier machen den Zustand jeder Runde sichtbar und für den User gegenprüfbar, statt sich auf reines Erinnern über eine lange Tick-Folge zu verlassen. Fehlen sie im Output, gilt der Voll-Check als nicht durchgeführt.

**Zeitzonen-Fix in der Tweet-Check-Zeile (ergänzt 25.08.2026, Opus-Vorschlag 2 aus dem Testtag-Review, siehe [[project_testtag_analyse_2026-08-24]]):** Die Vorlage in Zeile 32 oben ist die korrigierte, ab jetzt geltende Fassung. ~~Die alte Fassung `<fällig+durchgeführt ✓ (nichts Neues/kurzer News-Hinweis)> ODER <noch nicht fällig, zuletzt vor Xmin> ODER <ÜBERFÄLLIG ✗ → jetzt nachgeholt>` nannte das Minuten-Delta nur im mittleren Zweig und sagte nichts zur Zeitzone des zugrundeliegenden Zeitstempels aus~~ — gilt nicht mehr. Ab jetzt zwei feste Pflichten für diese Zeile: (a) jeder aus `x_last_fetch.json` gelesene Fetch-Zeitstempel wird, falls überhaupt als Uhrzeit ausgegeben, explizit in Ortszeit umgerechnet und mit "(Ortszeit)" gekennzeichnet — nie die rohe UTC-Zeitangabe unkommentiert übernehmen; (b) die Zeile nennt in JEDEM der drei Zweige das reine Minuten-Delta seit dem letzten Fetch, nicht nur einen Uhrzeit-Vergleich.

**Why:** Am Testtag 24.08.2026 verglich Sonnet den UTC-Zeitstempel aus `x_last_fetch.json` direkt mit den in Ortszeit geführten Loop-Uhrzeiten und berechnete daraus fälschlich eine "3-Stunden-Tweet-Fetch-Lücke" (15:51-18:52 Uhr) — real waren es 57 Minuten, durchgehend regelkonform unter der geltenden 60-Minuten-Schwelle. Das hätte fast eine Regeldiskussion auf falscher Grundlage ausgelöst. Ein reiner Uhrzeit-Vergleich ist zonenabhängig und im Nachhinein nicht verlässlich prüfbar; ein Minuten-Delta seit dem letzten Fetch ist es nicht — es bleibt unter jeder Zeitzonen-Interpretation gleich. Quelle: [[project_testtag_analyse_2026-08-24]] Abschnitt 2.3 und 9 (Opus-Review, Punkt "Tweet-Fetch-Zähler").

**ADX-Zeile ergänzt (26.08.2026, Fable-Umsetzung nach Opus-Kadenz-Prüfung):** ADX(14, NAS100) wird seit 24.08.2026 als GEMESSEN-KEIN-GATE-Rohwert in `trades.db` gespeichert (`--adx` in `scripts/gate_check.cjs`, siehe dessen Kopfkommentar) und in [[feedback_session_update]] Schritt 6 einmal pro Session gelesen — tauchte bisher aber in keiner laufenden Voll-Check-Ausgabe auf und wurde im Loop faktisch nie gezeigt. Die Zeile oben schließt diese Lücke, exakt nach demselben "GEMESSEN, KEIN GATE"-Prinzip wie die bestehende Trend-Effizienz-Anzeige — keine Schwelle, kein Veto, reine Kontext-Zahl für [[feedback_chartanalyse]] Punkt 8d.

**Prinzip auf zwei weitere Pflichtzeilen ausgedehnt (ergänzt 26.08.2026, siehe [[project_testtag_analyse_2026-08-25]] Abschnitt 6 + 14 Vorschlag 1):** Die Tweet-Check-Zeile war laut Testtag-Analyse 25.08.2026 "die einzige Pflichtzeile in diesem Projekt, die aus einem Urteil einen Rohwert gemacht hat — und die einzige, die an diesem Tag nicht gebrochen wurde". Dasselbe Prinzip (Rohwert + Vergleichsschwelle in derselben Zeile, nicht nur ein Urteil) gilt seit 26.08.2026 auch für die `SL/ATR-Ratio:`-Zeile (siehe [[feedback_chartanalyse]] Punkt 8c, "Format-Verschärfung") und die `Kerze geschlossen:`-Zeile aus Punkt 7d0 (siehe [[feedback_live_trading]], dort "Format-Verschärfung 26.08.2026") — beide gehören nicht zum Voll-Check-Standardformat oben (sie erscheinen nur an Trigger-/Entry-Momenten), folgen aber demselben Formatprinzip wie die Tweet-Check-Zeile hier.
