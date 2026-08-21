---
name: feedback-tagesabschluss
description: "Am Ende jedes Trading-Tages \"Tag Zusammenfassung speichern\" — strukturiertes Speichern aller Trade-Details, Fehler und Learnings, danach Git-Backup nach GitHub"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-08-21T15:18:21.751Z
---

Wenn der User "Tag Zusammenfassung speichern" sagt, immer eine vollständige Tages-Zusammenfassung in einer neuen Memory-Datei speichern.

**Why:** User möchte aus jedem Trading-Tag lernen — Fehler und Erfolge dokumentieren damit das Trading mit der Zeit besser wird.

**How to apply:** Neue Datei erstellen z.B. `trading_YYYY-MM-DD.md` mit folgender Struktur:

```
## Trades
- Entry: Preis, Stückzahl, Kosten
- Nachkauf: (falls vorhanden)
- TP1: Preis, Stückzahl, Gewinn
- SL/TP2: Preis, Stückzahl, Gewinn
- Gesamtgewinn/-verlust

## Was gut lief ✅
- ...

## Fehler / Was besser sein könnte ❌
- ...

## Key Levels für nächsten Tag 📍
- Support: ...
- Resistance: ...

## Learnings 🧠
- ...

## Marktbedingungen
- VIX, Makro-Regime, besondere News
```

Danach MEMORY.md Index aktualisieren.

## Trade-DB-Eintrag (ergänzt 27.07.2026, seit Fable-Build der SQLite-Trade-DB)

Wenn der Tagesabschluss einen oder mehrere abgeschlossene Trades enthält (nicht bei reinen "kein Trade"-Tagen ohne neue Trade-Nummer): zusätzlich zur `trading_YYYY-MM-DD.md`-Datei für JEDEN neuen Trade `node scripts/add_trade.cjs` im Projektordner (`C:\Users\umnus\tradingview-mcp`) ausführen — mit allen bekannten Feldern (Setup, Entry, SL, TP, Hebel, Position, Ergebnis, Outcome, Regelbruch-Flag). Kein automatischer Import aus der Markdown-Datei — das ist ein expliziter, manueller Schritt, den ich (Claude) beim Tagesabschluss selbst ausführe, nicht der User.

**Why:** Die DB (`scripts/trade_db.cjs`+`trade_stats.cjs`) existiert seit 27.07.2026 für abfragbare Statistik (Win-Rate, RR, Erwartungswert, Monte-Carlo-Vorbereitung), bleibt aber leer/veraltet, wenn dieser Schritt vergessen wird — genau das gleiche Muster wie beim Git-Backup unten, das erst nachträglich als fester Bestandteil ergänzt wurde.

**How to apply:** Nach dem Speichern der Tages-Datei, vor dem Git-Backup: `node scripts/add_trade.cjs --date ... --phase ... --dir ... --result-eur ... --outcome ...` (weitere Felder siehe Kopfkommentar der Datei) für jeden neuen Trade des Tages aufrufen, danach zur Kontrolle `node scripts/trade_stats.cjs` laufen lassen und gegen die Markdown-Zahlen abgleichen.

## Reklassifizierung bestehender Trades (ergänzt 12.08.2026)

Reklassifizierung bestehender Trades: Sobald eine Review (Tagesabschluss oder Fable) die Einstufung eines bereits geloggten Trades ändert (Regelbruch neu/entfällt, Outcome-Wechsel), ist die DB-Aktualisierung (`rule_violation`/`outcome`/`notes` für die betroffene Trade-ID) Teil derselben Review-Session, kein Folgeschritt. Pflicht-Abschlusszeile: "DB-Sync: JA (Trade #XX aktualisiert) / NEIN (kein Fall heute)."

**Why:** Genau diese Lücke ließ Trades #9/#12/#31/#34 wochenlang mit einer veralteten Klassifizierung in der DB stehen — die jeweilige Review (u.a. [[feedback_regime_wechsel]], Tagesabschluss 05.08./07.08.) hatte den Regelbruch inhaltlich längst erkannt, aber nie in `rule_violation`/`notes` nachgetragen. Aufgefallen erst beim Phase-1-3-Regelwerk-Audit am 12.08.2026, nicht am Tag der jeweiligen Review selbst. Ein Vorschlag für ein automatisches Backstop-Script (`scripts/check_violation_sync.cjs`, würde Regelbruch-Schlüsselwörter aus den Tages-Dateien automatisch gegen die DB gegenchecken) steht im Raum, ist aber bewusst nicht Teil dieser Änderung — Levi entscheidet separat, ob/wann das gebaut wird.

**Abgrenzung:** Dieser Abschnitt betrifft die Korrektur bereits bestehender, schon geloggter Trade-Einträge. Für die Pflichtprüfung bei JEDEM NEUEN Trade ab jetzt siehe den folgenden Abschnitt "Regelkonformitäts-Pflichtzeile".

## Regelkonformitäts-Pflichtzeile (ergänzt 12.08.2026, Phase-1-3-Regelwerk-Audit)

Für JEDEN Trade des Tages (Win, Loss oder BE) gehört ab sofort eine Pflichtzeile zum Tagesabschluss, parallel zur "DB-Sync: JA/NEIN"-Zeile aus dem Abschnitt "Reklassifizierung bestehender Trades" oben:

`Regelkonformität geprüft: JA — Verstoß: JA/NEIN`

Wird `Verstoß: JA` gesetzt (egal ob der Trade gewonnen oder verloren hat), ist eine Fable-Review noch am selben Tag Pflicht (Sofort-Review), nicht erst bei der nächsten zufälligen Gelegenheit.

**Abgrenzung:** Dieser Abschnitt betrifft die Prüfung bei der Ersterfassung eines neuen Trades — im Unterschied zum Abschnitt "Reklassifizierung bestehender Trades" oben, der die nachträgliche Korrektur bereits geloggter, veralteter Einträge regelt. Beide Pflichtzeilen ("DB-Sync" und "Regelkonformität geprüft") gehören in jeden Tagesabschluss mit Trades, unabhängig voneinander.

**Why:** Volle Herleitung und Zahlen (6 von 6 nachträglich korrigierten Regelverstößen waren ausschließlich Verlust-Trades, kein einziger Gewinn-Trade) siehe [[feedback_regeldisziplin]], Abschnitt "Erkennungs-Zwang + Sofort-Review". Ohne diese Pflichtzeile wird die Regelkonformitätsprüfung bei Gewinn-Trades erfahrungsgemäß stillschweigend übersprungen.

**Ausdrücklich nicht Teil dieser Änderung:** keine automatische Positionsgrößen-Reduktion nach einem Verstoß, keine neue Cooldown-Regel-3 — Levi hat beides am 12.08.2026 explizit abgelehnt, diese Ergänzung ist rein Erkennung + Review, keine automatische Konsequenz.

## SL-Hit-Klassifizierung: Wick/Spike vs. bestätigter Reversal (ergänzt 21.08.2026, Fable-Review nach Trade #42/#43, umgesetzt nach Levi-Entscheidung)

Bei JEDEM SL-Hit gehört ab sofort eine zusätzliche Pflichtzeile zum Tagesabschluss/Sofort-Review, parallel zu "Regelkonformität geprüft" oben:

`SL-Hit-Typ: Wick/Spike (kein Kerzenschluss) ODER bestätigter Kerzenschluss — Punkt-11-Kriterien zum SL-Zeitpunkt: X/4 erfüllt`

War der SL primär durch einen Wick/Spike ausgelöst (kein 5-Min-Kerzenschluss jenseits des SL-Levels) UND waren die Punkt-11-Reversal-Kriterien ([[feedback_live_trading]]) zum Auslöse-Zeitpunkt NICHT vollständig (2-3/4) erfüllt, gilt das explizit als "SL-Hit ≠ bestätigtes Reversal" — der Trade-Ausgang (Verlust) bleibt unverändert bestehen, aber die Review-Sprache darf daraus NICHT automatisch ableiten, dass die zugrunde liegende Markteinschätzung/These falsch war. Beides wird getrennt bewertet: das Ergebnis (Geld verloren, zählt) und die Frage, ob die These inhaltlich widerlegt wurde (nur bei erfüllten Punkt-11-Kriterien der Fall).

**Why:** Trade #42/#43 (21.08.2026) wurden beide durch Wicks in der Zone 29.240-254 ausgestoppt, während zum Zeitpunkt des jeweiligen SL-Hits nur 1-2 von 4 Punkt-11-Kriterien erfüllt waren — ein Fable-Live-Check (21.08., ~35-40 Min NACH beiden SL-Hits) zeigte NAS100 5min und QQQ 15min beide noch unterhalb ihrer jeweiligen EMA50, also weiterhin nicht bestätigt. Der Fable-Review vom 21.08.2026 stellte fest, dass "SL wurde getroffen" in der Live-Kommunikation faktisch mit "der Markt hat die Short-These widerlegt" gleichgesetzt wurde, obwohl das nach den eigenen Kriterien zum Zeitpunkt nicht belegt war. Diese Vermischung erschwert eine ehrliche Nachbesprechung: ein Verlust durch eine unglückliche SL-Platzierung in einer mehrfach getesteten Zone (siehe [[feedback_chartanalyse]] Punkt 8c2) ist ein anderes Problem als ein Verlust durch eine tatsächlich falsche Markteinschätzung, und beide brauchen unterschiedliche Lehren.

**How to apply:** Bei jedem SL-Hit im Tagesabschluss/Sofort-Review zuerst prüfen, ob der letzte relevante 5-Min-Kerzenschluss VOR dem SL-Hit bereits jenseits des SL-Levels lag (dann: bestätigter Schluss, kein reiner Wick) oder ob der SL nur durch das Hoch/Tief innerhalb einer Kerze berührt wurde, ohne dass der Schlusskurs selbst das Level bestätigte (dann: Wick/Spike). Danach den Punkt-11-Stand zum exakten SL-Zeitpunkt dokumentieren (nicht rückwirkend zum späteren, ggf. schon weiter entwickelten Stand). Fehlt diese Zeile bei einem SL-Hit-Trade, gilt der Tagesabschluss als unvollständig — gleiche Behandlung wie die bestehende "Regelkonformität geprüft"-Zeile.

**Abgrenzung zu [[feedback_live_trading]] Punkt 11 (Volumen-Kriterium):** Das dort am 21.08.2026 ergänzte fünfte, dämpfende Volumen-Kriterium wirkt VOR/WÄHREND der Position (soll einen verfrühten Dreh-Vorschlag verhindern). Diese Regel hier wirkt NACH dem SL-Hit, in der Nachbesprechung — beide entstanden aus demselben Fable-Review, adressieren aber unterschiedliche Zeitpunkte im Prozess.

## Git-Backup nach jedem Tagesabschluss (ergänzt 23.07.2026)

Nach dem Speichern der Tages-Zusammenfassung UND dem MEMORY.md-Update: den kompletten `memory/`-Ordner committen und zu `github.com/LeviUmn/trading-memory` (Private Repo) pushen.

**Why:** User-Entscheidung 23.07.2026 nach Einrichtung des Backups — ein Trading-Tag ist der natürliche Rhythmus, weil an diesem Punkt ohnehin schon alle Learnings/Regel-Änderungen des Tages fertig dokumentiert sind, kein separater Trigger nötig.

**How to apply:**
```
cd "<Pfad zu memory/>"
git add -A
git commit -m "<kurze Zusammenfassung des Tages, z.B. 'Session 23.07.2026 — ATR-Umstellung, Regel-Audit'>"
git push
```
Falls an dem Tag keine echten Trades liefen, aber Regel-/Memory-Änderungen gemacht wurden (wie am 23.07.2026), gilt derselbe Rhythmus — der Trigger ist der Tagesabschluss selbst, nicht ob ein Trade stattfand.
