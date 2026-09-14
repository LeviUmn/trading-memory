---
name: project-1h-kriterium-offene-frage-2026-09-03
description: "ENTSCHIEDEN 04.09.2026 (Opus+Fable, Levi-Go): Die am Testtag 03.09. gesetzte Regel 'Reversal-Hoch muss per bestätigtem 1H-Schluss überboten sein' war eine Sonnet-Eigenauslegung ohne Regelwerksgrundlage. Verbindlich seitdem: 1H-Override = Bias des zuletzt GESCHLOSSENEN 1H-Bars (EMA50-Seite + Strukturrichtung), kein Zusatzkriterium, laufender Bar zählt nicht — verankert in feedback_live_trading 7b1 + 13.1. Überschuss-Klausel (P8) zurückgestellt bis Gegenrechnung gegen 01./02.09."
metadata:
  node_type: memory
  type: project
  status: resolved
  originSessionId: session_01GAqDaLvWGcEg7c44Lreoaj
  modified: 2026-09-04T11:48:18.656Z
---

## ENTSCHIEDEN 04.09.2026 (Opus+Fable konvergiert, Levi-Go, Fable-Umsetzung — siehe [[project_testtag_analyse_2026-09-03]])

**Antwort: Option (2) — reine Sonnet-Eigeninterpretation ohne Grundlage im Regelwerkstext.** [[feedback_chartanalyse]] Punkt 8 Schritt 1 definiert den 1H-Bias als HH/HL bzw. LH/LL + EMA50-Verhältnis des abgeschlossenen Bars; ein Kriterium "Reversal-Hoch muss per bestätigtem 1H-Schluss überboten sein" existiert nirgends. Der 14-15-Uhr-Bar hatte am 03.09. bereits über EMA50 mit höherem Hoch geschlossen — der 1H-Override war ab 15:00 erfüllt.

**Verankert in [[feedback_live_trading]] 7b1 ("1H-Override — verbindliche Definition") und 13.1 ("13.1 verankert", Punkt 2):** 1H-Override = Bias des zuletzt GESCHLOSSENEN 1H-Bars (EMA50-Seite + Strukturrichtung), kein Zusatzkriterium; der laufende Bar zählt weiterhin nicht. Damit ist auch der F3-Interlock (13.1 durch "unvollständiges" Dual-Gate blockiert) strukturell gelöst.

**Nicht eingeführt:** die von Opus vorgeschlagene Überschuss-Klausel für den laufenden 1H-Bar (≥1,0×ATR über ≥2 5min-Schlüsse) — herabgestuft auf P8/Stufe 3, erst nach Gegenrechnung gegen die Testtage 01./02.09. (dort blockierte das 1H-Kriterium #20/#34 korrekt). Mit der Klarstellung oben ist sie für den 03.09.-Fall nicht mehr nötig.

---
*(Ursprüngliche Frage vom 03.09.2026, als Historie belassen:)*

## Kontext (Testtag 03.09.2026, live während des Loops aufgeworfen)

Ab ca. 15:12 Uhr lief auf NAS100 ein 1H-Reversal (4 Bars LH-LL, danach starker Ausbruchs-Bar mit Schluss 29.199,8, Hoch 29.213,8). Der darauffolgende 1H-Bar (15:00-16:00-Fenster) handelte ab ca. 15:30 Uhr deutlich über diesem Hoch (bis auf ~29.310), blieb aber bis zu seinem Schluss gegen 16:00 Uhr technisch "forming".

Sonnet hat sich in dieser Session selbst die Regel gesetzt, für die 1H-Strukturbestätigung im Dual-Gate **nur einen bestätigt GESCHLOSSENEN 1H-Bar** zu werten — nicht den Live-Kurs innerhalb der laufenden Kerze —, in Analogie zur bekannten Regel für 5min/15min-Kerzen ("nur das bestätigte Element zählt, nie der forming bar", siehe [[feedback_loop_ablauf_uebersicht]] Szenario 1 Schritt 2). Das ist ausdrücklich eine **eigene, konservative Auslegung** — der exakte 1H-Kriterium-Wortlaut aus dem Dual-Gate stand nicht vollständig im Kontext dieser Session (Auszug aus `feedback_live_trading.md` mit dem 7b1-Ablauf fehlte).

**Levis Rückfrage (15:46 Uhr):** "Worauf warten wir bei 1H? Die eine Kerze ist doch schon über EMA?" — korrekt: EMA50 war seit 15:12 Uhr nie das offene Kriterium, sondern ausschließlich der Bar-Schluss selbst.

## Offene Frage für Opus/Fable

Ist die "nur bestätigter Schluss zählt"-Regel für die 1H-Ebene im Dual-Gate:
1. bereits so im Regelwerk verankert (dann nur eine Bestätigung/Klarstellung nötig), oder
2. eine reine Sonnet-Eigeninterpretation ohne Grundlage im Dual-Gate-Wortlaut, die zu unnötig späten/verpassten Entries führen kann, wenn ein starker Move die alte Marke bereits deutlich überboten hat, bevor die Kerze formal schließt?

Falls (2): sollte eine Zwischenregel eingeführt werden, z.B. "Live-Kurs reicht, wenn er die Referenzmarke um mindestens X Punkte/Y× ATR überboten hat" — oder bleibt der strikte Bar-Schluss aus Sicherheitsgründen (Vermeidung von False Signals durch Intrabar-Spikes) die bessere Wahl?

**Levi-Anweisung (15:48 Uhr):** Frage zurückgestellt, wird später gemeinsam mit Opus und Fable besprochen — keine Regeländerung heute, aktuelle Testtag-Session behält die konservative Auslegung bei.

## Zusatzkontext für die spätere Diskussion

Am selben Trigger-Moment lag zusätzlich der Chasing-Check (Punkt 13) im Weg — 5 bullische Kerzen in Folge ohne Konsolidierungspause, RSI 70-75. Beide Punkte zusammen bedeuten: selbst bei einer gelockerten 1H-Regel wäre an dieser Stelle kein Vollentry zulässig gewesen, nur eine reduzierte Chasing-Größe. Die 1H-Kriterium-Frage ist also nicht die einzige Bremse in diesem konkreten Fall — relevant bleibt sie trotzdem grundsätzlich für künftige, weniger überhitzte Situationen.
