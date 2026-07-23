---
name: feedback-positions-status-pflicht
description: "SL + TP1 + TP2 müssen in jedem Positions-Status-Update während einer offenen Position mitgeschrieben werden, nicht nur beim Entry"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-07
---

Während Trade #13 (07.07.2026) bat der User explizit: "Bitte mir immer beim Positionsstatus dazuschreiben [TP1] und SL im Kurs, das ist wichtig für mich."

**Why:** Der Positions-Kasten aus [[feedback_live_trading]] Punkt 8 wird bisher nur beim Entry-Moment gezeigt, danach laufen die schnellen Status-Updates nur mit Kurs/RSI/MACD-H (Punkt 2/3). Der User will die SL/TP-Level aber bei jedem Update im Blick behalten, ohne selbst nachrechnen oder zurückscrollen zu müssen.

**How to apply:** Ab sofort bei JEDEM Status-Update während einer offenen Position (auch beim kurzen "kein Kipp-Punkt"-Format) SL-Kurslevel und TP1 mitschreiben, z.B.: "Kurs 29.121 (Entry 29.127 | 🔴 SL 29.010 | 🟢 TP1 29.214 | TP2 29.316) | RSI ... | MACD-H ...". Gilt zusätzlich zum vollen Positions-Kasten bei echten Kipp-Punkten (TP1 erreicht, SL-Nachzug etc.), nicht als Ersatz dafür.

**Farbcodierung (ergänzt 07.07.2026):** SL immer mit 🔴, TP1 immer mit 🟢 markieren (TP2 ohne Farbmarker, da nachrangig) — User will die Level auf einen Blick unterscheiden können, ohne den Text lesen zu müssen.

**ÜBERHOLT in einem Detail (markiert 16.07.2026, Fable-Review):** Der Teilsatz "TP2 ohne Farbmarker" gilt nicht mehr — seit 14.07.2026 bekommt auch TP2 immer 🟢, siehe [[feedback_positions_farbcode]] (neuere Regel, gilt vorrangig; dort steht auch die Zertifikatspreis-Ergänzung). Der Rest dieser Datei (SL+TP1+TP2 in JEDEM Status-Update mitschreiben) bleibt unverändert gültig.
