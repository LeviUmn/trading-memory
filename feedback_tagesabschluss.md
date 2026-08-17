---
name: feedback-tagesabschluss
description: "Am Ende jedes Trading-Tages \"Tag Zusammenfassung speichern\" — strukturiertes Speichern aller Trade-Details, Fehler und Learnings, danach Git-Backup nach GitHub"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-08-12T10:56:44.168Z
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
