---
name: feedback-size-script-short-bug
description: "scripts/size.cjs hatte einen Vorzeichenfehler bei Short-SL-Preisen (rechnete SL zu hoch statt zu niedrig) — gefixt 28.07.2026, vor jeder Nutzung Datum der letzten Änderung prüfen"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-28
  modified: 2026-07-28T16:40:19.745Z
---

**Bug (bis 28.07.2026):** `scripts/size.cjs` berechnete den Zertifikat-SL-Preis für Short-Positionen als `zert * (1 + slAbstandPctZert)` — also einen HÖHEREN Preis als der Entry. Das ist falsch: ein Leveraged-Zertifikat verliert an Wert, wenn die Position gegen einen läuft — bei Short genauso wie bei Long. Alle echten Broker-Fills im Trade-Log (z.B. Trade #9: Entry 3,47€ → SL 3,44€; Trade #3: Entry 3,80€ → SL 3,73€) zeigen das eindeutig: SL-Preis liegt bei Short IMMER unter dem Entry-Preis, nie darüber.

**Konkreter Vorfall:** Trade #26 (28.07.2026), Short 488 Stk à 5,10€, NAS100-Entry 27.482,6, SL-Level 27.600. Das Script gab fälschlich "SL beim Broker eintragen: 5,21€" aus — korrekt war 4,99€. Der Fehler wurde vor Weitergabe an den User erkannt (Gegencheck mit den echten Trade-Log-Beispielen), nicht erst danach.

**Fix:** `slPreisZert = zert * (1 - slAbstandPctZert)` jetzt für BEIDE Richtungen gleich (kein Ternary mehr zwischen long/short) — der SL-Preis fällt immer, unabhängig von der Richtung. Verifiziert: Script gibt jetzt 4,991€ für obiges Beispiel aus.

**Why:** Ein falscher SL-Preis beim Broker ist sicherheitskritisch — entweder wird die Position gar nicht abgesichert (SL zu weit weg/falsche Richtung) oder sofort fälschlich ausgelöst. Das Script wurde bereits mehrfach für echte Trades genutzt, seit wann der Bug bestand ist unklar (frühere Short-Trades vor der Script-Einführung liefen über Handrechnung, siehe Trade-Log).

**How to apply:** Bei JEDEM Script-Output für einen SL-Preis (egal ob long oder short) kurz gegen die Grundregel checken: SL-Zertifikatspreis muss IMMER unter dem Entry-Preis liegen — nie darüber. Weicht ein Script-Output davon ab, nicht blind übernehmen, sondern nachrechnen (siehe [[feedback_zertifikat_pnl]] für die generelle Formel-Gegencheck-Pflicht bei P&L-Zahlen — gilt hier analog für SL-Preise).

**Nachtrag 28.07.2026 (Fable-Tagesabschluss-Review, Trade #26):** Code-Review von `scripts/size.cjs` nach dem Fix bestätigt: Das Script berechnet ausschließlich den SL-Preis, es gibt keine eigene TP-Preis-Berechnung im Script (TP wird weiterhin separat aus der Chartstruktur bestimmt) — damit existiert kein analoger zweiter Bug-Vektor an anderer Stelle desselben Scripts. Der eigentliche strukturelle Schwachpunkt war aber prozessual: Das Script lief bereits mehrfach live, bevor der Fehler durch einen zufälligen Gegencheck auffiel — es gab keinen vorherigen Testlauf gegen bekannte Long- UND Short-Beispiele aus `trade_log.md`. **Neue Regel:** Vor jeder erstmaligen Live-Nutzung eines neuen oder geänderten Trading-Scripts (size.cjs, cooldown_check.cjs, trade_db.cjs etc.) mindestens einen Testlauf mit je einem bekannten Long- und Short-Beispiel durchführen und den Output explizit gegen die echten historischen Werte gegenchecken, bevor es im Live-Loop verwendet wird.
