---
name: feedback-tagesabschluss
description: "Am Ende jedes Trading-Tages \"Tag Zusammenfassung speichern\" — strukturiertes Speichern aller Trade-Details, Fehler und Learnings"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
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
