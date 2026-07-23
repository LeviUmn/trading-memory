---
name: feedback-broker-blockade-playbook
description: "Konkretes Eskalations-Playbook wenn eine SL-Order bei Scalable Capital nicht stornierbar ist und dadurch Position/Verkauf blockiert — Vorlage aus dem Trade-#14-Vorfall (08.07.2026)"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: e3468883-3dd5-4cd2-aa4f-c6979adc898f
---

Wenn eine aktive SL-Order bei Scalable Capital nicht storniert werden kann UND dadurch der manuelle Verkauf der Position blockiert ist (siehe [[feedback_broker_sl_tp_exklusiv]] für den Normalfall SL/TP-Exklusivität — dieses Playbook ist der Eskalationsfall, wenn selbst die Stornierung selbst fehlschlägt), gilt folgender Ablauf:

### Erkennung & Eskalations-Timer
1. Stornierungsversuch schlägt fehl (Fehlermeldung oder keine Reaktion) → **max. 2 Wiederholungsversuche**, dann sofort als Blockade werten. Nicht wiederholt dieselbe Aktion probieren, während der Kurs weiterläuft — das kostet nur Zeit (bei Trade #14 liefen mehrere Stornierungsversuche über ~40 Min ins Leere, bevor reagiert wurde).
2. Sofort prüfen: Ist wenigstens der manuelle Verkauf OHNE Stornierung möglich? Falls ja → direkt verkaufen, fertig, kein Hedge nötig.
3. Falls auch der manuelle Verkauf blockiert ist (wie bei Trade #14) → Schritt 4.

### Hedge-Workaround (wenn Position weder änderbar noch schließbar ist)
4. **Neue Gegenposition öffnen** — exakt gleiche Positionsgröße in €, entgegengesetzte Richtung, gleiches Instrument (NAS100-Faktor-Zertifikat), möglichst gleicher Hebel. Ziel: Netto-Exposure auf ~0 neutralisieren, während die Blockade besteht.
   - Stückzahl über die Positionsgrößen-Formel berechnen (Positions-€ ÷ aktueller Zertifikatspreis des Hedge-Produkts), nicht einfach dieselbe Stückzahl wie die Originalposition übernehmen (unterschiedliche Zertifikate haben unterschiedliche Preise/Hebel-Umrechnung).
   - Der Hedge ist ab dem Moment seiner Eröffnung ein eigenständiger Trade mit eigenem SL/TP-Gedanken — nicht nur ein Blindschutz. Bei Trade #14 wurde der Hedge (Short) später eigenständig weiter analysiert und lief bis TP1 + Exit-Signal.
5. Parallel Support-Kontakt aufnehmen (E-Mail/Chat bei Scalable) — dokumentiert den Vorfall und beschleunigt ggf. eine serverseitige Lösung.
6. Blockade in regelmäßigen Abständen erneut testen (Stornierung/Verkauf versuchen), nicht nur einmalig aufgeben.

### Auflösung
7. Sobald die Blockade sich löst: Original-Position sofort schließen (Market-Order, keine Limit-Feinjustierung — Sicherheit vor Preis in dieser Situation).
8. Hedge-Position separat nach normaler Chart-Analyse weiterführen oder schließen — sie ist ab jetzt ein regulärer, unabhängiger Trade.
9. Vorfall als **eigenständigen operationellen Incident** dokumentieren, getrennt von der Trade-Qualität bewerten (siehe [[feedback_regeldisziplin]] dritte Kategorie: Verlust durch externe Störung ≠ Verlust durch Regelbruch).

### Präventiv für Phase 2/3 (größere Positionsgrößen)
Das operationelle Risiko dieses Szenarios skaliert linear mit der Positionsgröße — bei doppelter Position kostet ein identischer Vorfall etwa doppelt so viel. Deshalb zusätzlich:
- Bei SL-Nachzieh-Entscheidungen in der Nähe von TP-Leveln nicht unnötig oft nachziehen (jede Änderung ist ein Stornieren+Neu-Setzen, mehr Angriffsfläche für denselben Bug)
- Hedge-Produkt (Ticker/Zertifikat-ISIN für die Gegenrichtung) idealerweise schon vor der Session bekannt/vorbereitet haben, um in der Blockade keine Zeit mit der Produktsuche zu verlieren

**Why:** Fable-5-Review (09.07.2026) stufte das fehlende schriftliche Playbook als einzige harte Blockade vor dem Phase-1→2-Übergang ein — der Trade-#14-Vorfall wurde live improvisiert, nicht nach einem festgelegten Ablauf. Mit diesem Playbook ist der Ablauf jetzt vorab festgelegt statt unter Zeitdruck neu erfunden zu werden.

**How to apply:** Bei jeder SL-Stornierung, die fehlschlägt, dieses Playbook Schritt für Schritt abarbeiten statt frei zu improvisieren. Nach Anwendung in einem echten Vorfall dieses Playbook auf Basis der neuen Erfahrung nachschärfen.
