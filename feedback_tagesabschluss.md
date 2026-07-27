---
name: feedback-tagesabschluss
description: "Am Ende jedes Trading-Tages \"Tag Zusammenfassung speichern\" — strukturiertes Speichern aller Trade-Details, Fehler und Learnings, danach Git-Backup nach GitHub"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-07-27T11:58:28.917Z
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
