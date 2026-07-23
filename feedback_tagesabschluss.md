---
name: feedback-tagesabschluss
description: "Am Ende jedes Trading-Tages \"Tag Zusammenfassung speichern\" — strukturiertes Speichern aller Trade-Details, Fehler und Learnings, danach Git-Backup nach GitHub"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-07-23T11:30:32.333Z
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
