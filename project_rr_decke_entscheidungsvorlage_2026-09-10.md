---
name: project-rr-decke-entscheidungsvorlage-2026-09-10
description: "ENTSCHIEDEN 10.09.2026 (war faktisch schon am 04.09. beantwortet): RR = TP1-Zahl (Option A) als Hard Gate ≥1:1, KEINE Zielgröße; Ziel ist die realisierte Kapitalrendite im Monatsdurchschnitt; kein Blend-RR. Ursprünglich Entscheidungsvorlage (Option A vs. B) aus dem Opus-Meilensteincheck 24.08.-10.09.2026, Punkt 8."
metadata:
  type: project
  status: entschieden 10.09.2026 — Option A, verankert in feedback_live_trading 7b1 P7
  originSessionId: session_01MXmo6bNgLxmDdAMgnAUFJn
  modified: 2026-09-10T11:00:19.527Z
---

# RR-Decke: Entscheidungsvorlage (offen seit 03.09.2026) — ENTSCHIEDEN 10.09.2026

> **Ergebnis (Levi, 10.09.2026, in der Klärungskette mit Opus):** Die Frage war bereits am 04.09.2026 beantwortet (Zielbild-Klarstellung + P7-Entfernung), nur nie als erledigt markiert. **Option A gilt:** RR heißt am Entry immer die TP1-Zahl (Hard Gate ≥1:1). RR ist keine Zielgröße; das Ziel ist die realisierte Kapitalrendite im Monatsdurchschnitt. Ein Blend-RR wird nicht eingeführt. Die unter Option A genannte Konsequenz "Zielbild auf ≈1,0 % anpassen" wurde NICHT übernommen — das Renditeziel bleibt, wird nachgelagert gemessen und hängt an der TP2-Realisierung (P9), nicht an einer RR-Schwelle. Verankert: [[feedback_live_trading]] 7b1 P7; F4 in [[project_testtag_analyse_2026-09-03]] als erledigt markiert. Begleitende Fixes: Payoff-Ratio statt "Blend" + Kapitalbasis-Vereinheitlichung + R-Multiple in `stats_common.cjs` ([[feedback_realisiertes_rr]]). Die Vorlage unten bleibt als Zeitdokument stehen.

*(Hinweis zu den Zahlen unten: "1,16:1 über 27 Trades" ist veraltet und war kein Blend, sondern das Payoff-Ratio über alle Exit-Arten — aktueller Stand 0,94:1 auf Kapitalbasis, siehe [[feedback_realisiertes_rr]]. Die Kapitalbasis ist seit 10.09.2026 phasenabhängig, siehe [[project_risikomanagement]].)*

**Die Frage in einem Satz:** Meint „RR" in unserem System die **TP1-Zahl allein** (Option A) oder den **Blend aus TP1 und TP2** (Option B)?

**Warum das entschieden werden muss:** SL-Floor 8c (min. 1,5×ATR) und TP-Cap 8b1 Zone 1 (max. 2×ATR) ergeben rechnerisch **RR(TP1) ≤ 2/1,5 = 1,333:1 bei voller Position**; Zone 2 (2-3×ATR, halbe Position) hebt das auf max. 2,0:1. Das ist keine Beobachtung, sondern Arithmetik (nachgerechnet in [[project_opus_vollpruefung_2026-08-24]] C-2b, [[project_testtag_analyse_2026-08-25]], [[project_testtag_analyse_2026-09-03]] F4; als Selbsttest GC-5 in `gate_check.cjs`). Solange offen ist, was „RR" bedeutet, lässt sich weder sagen, ob das Zielbild „RR 1,5-2:1" verfehlt wird, noch ob überhaupt eine Schwelle geändert werden müsste.

## Option A — RR = TP1-Zahl

- **Maximal erreichbar:** 1,333:1 (Zone 1, volle Position), 2,0:1 (Zone 2, halbe Position). Historisch gemessen Phase 3: 1,03:1 = exakt am unteren Rand des Fensters (C-2b).
- **Konsequenz für die Rendite (1,5 % Risiko je Trade, Exit „TP1-Teilverkauf + Rest auf BE"):** höchstens **1,0 % je Trade** (volle Position) bzw. **0,75 %** (halbe Position) — siehe Strukturbefund in [[feedback_live_trading]] 7b1 P7. Levis Zielbild (Ø 1,5 % je Gewinn-Trade, Monatsdurchschnitt, [[project_testtag_analyse_2026-09-03]] „Zielbild-Klarstellung") ist unter Option A **bei voller Position arithmetisch nicht erreichbar**.
- **Was Option A logisch nach sich zöge:** entweder eine der Sicherheitsschwellen anfassen (Zone-1-Cap anheben ODER SL-Floor senken — beide stammen aus echten Verlust-Vorfällen, Fable/Opus am 04.09. einstimmig: NICHT anfassen) ODER das Zielbild nach unten korrigieren (≈1,0 % statt 1,5 %).
- **Vorteil:** eindeutig messbar an jedem einzelnen Trade, kein Modellanteil. **Nachteil:** ignoriert den TP2-Runner, der laut 27.07.-Reform bewusst ohne RR-Deckel läuft.

## Option B — RR = TP1/TP2-Blend

- **Theoretisch (50/50-Teilverkauf, TP1 ≈1,2-1,33, TP2 ≥2 nach 8b2, real eher 2,5-3):** Blend ≈ **1,85:1** bei voller Position ([[project_testtag_analyse_2026-09-03]], Fable-Gegencheck F4; Opus-Adjudikation: „Rendite = Risiko% × RR_blended" → 1,5 % × 1,85 = **2,78 %**, über dem Ziel). Bei halber Position: 1,39 % (knapp unter Ziel).
- **Historisch real:** Blend über 27 Trades **1,16:1** ([[feedback_realisiertes_rr]]) → 1,5 % × 1,16 = **1,74 %** (volle Position, über Ziel; halbe Position 0,87 %, unter Ziel).
- **Der Haken:** Der Blend hängt an genau einer Größe, der **TP2-Realisierungsquote** — und die ist empirisch niedrig: TP2-Quote 11,1 % (3/27) lifetime, 7,7 % (1/13) im August (`abschluss.cjs --monat 2026-08`, 10.09.2026). Wird TP2 selten erreicht, kollabiert der Blend Richtung Option A. Genau deshalb steht P9 (TP2-Realisierungsquote messen, 15-Trade-Review) noch offen.
- **Was Option B logisch nach sich zöge:** keine Schwellenänderung; stattdessen wird die TP2-Quote zur entscheidenden Messgröße (P9), und jede Positions-Halbierung (Zone 2, Chasing, Zeitfenster, Q-Score GELB) drückt den Trade unter das Ziel — der Hebel liegt bei Entry-Timing/Leveldichte, nicht bei RR-Schwellen ([[project_studie_geschwindigkeit_effektivitaet_opus_2026-09-07]] Konvergenz).
- **Vorteil:** entspricht dem gelebten Exit-Modell (TP1 hart 1:1, TP2 offen). **Nachteil:** Modellannahme (Aufteilung, TP2-Trefferquote), erst nach ~15 Trades mit gemessener TP2-Quote wirklich belastbar.

## Was sich NICHT ändert, egal wie entschieden wird

- Das RR-Gate 8b (TP1 ≥ 1:1) bleibt Hard-Gate; 8c-Floor, 8c2-Cluster, Zone-3-Cap bleiben (04.09.-Konsens, keine davon war je die bindende Restriktion).
- `gate_check.cjs` rechnet weiterhin keine Renditezeile (P7-Entscheidung Levi 04.09.); gemessen wird nachgelagert (`trade_stats.cjs`, `abschluss.cjs`).
- Diese Vorlage fügt **keine** Pflichtzeile und **keine** Regel hinzu.

## Vorlage für die Entscheidung (ein Satz)

> „RR gilt ab jetzt als **[A: TP1-Zahl | B: TP1/TP2-Blend]**; Konsequenz: **[A: Zielbild auf ≈1,0 %/Trade bei voller Größe anpassen — Schwellen bleiben | B: Schwellen bleiben, TP2-Realisierungsquote (P9) wird zur Pflicht-Messgröße im nächsten 15-Trade-Review]**."

Erst nach dieser Entscheidung ist sinnvoll zu beurteilen, ob das Renditeziel mit dem aktuellen Regelwerk erreichbar ist. Bis dahin: Zahlen weiter messen (`trade_stats.cjs`, `abschluss.cjs --monat`), nichts an Schwellen ändern.
