---
name: feedback-vollcheck-format
description: "Voll-Check-Ausgabeformat: Fließtext mit ✓/✗ hinter jedem geprüften Punkt, keine Tabelle"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 7128a95a-a9a6-42e6-8a59-8f22dec5c654
  modified: 2026-07-28T14:12:22.086Z
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

**Chartmuster/Fibonacci:** <geprüft, Ergebnis> ODER <ausgelassen, Grund>.

**Tweet-Check:** <fällig+durchgeführt ✓ (nichts Neues/kurzer News-Hinweis)> ODER <noch nicht fällig, zuletzt vor Xmin> ODER <ÜBERFÄLLIG ✗ → jetzt nachgeholt>.

**Format:** Fließtext ✓

🟢/🔴/→warten <Fazit>
```

Jedes ✓/✗ steht für eine ECHTE Einzelprüfung, nicht dekorativ — bei "ausgelassen" (z.B. kein Impuls für Fibonacci) explizit als "ausgelassen" kennzeichnen statt ✗ zu setzen (✗ = geprüft und nicht erfüllt, "ausgelassen" = gar nicht geprüft, das ist ein Unterschied).

**Korrigiert 28.07.2026 (User-Wunsch nach mehr Struktur, live während Trading-Session):** Der ursprüngliche dichte Absatzblock (alle Ebenen in einem durchgehenden Fließtext ohne Zeilenumbruch) wurde als schwer lesbar empfunden. Lösung: Zeilenumbruch pro Ebene mit fettem Label, Satzform pro Ebene bleibt erhalten — explizit KEINE Rückkehr zur Tabelle (die 27.07.-Korrektur bleibt gültig), sondern eine Zwischenstufe mit mehr visueller Trennung. Zwei Alternativen (Stichpunkte, unveränderter Block) wurden dem User zur Wahl vorgelegt, diese Variante wurde ausdrücklich gewählt.

**Tweet-Check- und Format-Zeile ergänzt 27.07.2026 (Fable-Tagesabschluss-Review, nach zwei Prozessfehlern desselben Tages — siehe [[feedback_live_trading]] Punkt 9 "Struktureller Anti-Drift-Fix" und [[feedback_prozessfehler_27_07_fuer_fable]]):** Beide Zeilen sind ab jetzt fester, nicht optionaler Bestandteil jedes Voll-Check-Outputs — unabhängig davon, ob der Tweet-Fetch diese Runde überhaupt fällig ist oder ob "alles wie gewohnt" aussieht. Root-Cause war, dass ein "stiller" Check (kein sichtbarer Output bei "nichts zu tun") nicht von einem schlicht vergessenen Check zu unterscheiden war — die beiden Zeilen hier machen den Zustand jeder Runde sichtbar und für den User gegenprüfbar, statt sich auf reines Erinnern über eine lange Tick-Folge zu verlassen. Fehlen sie im Output, gilt der Voll-Check als nicht durchgeführt.
