---
name: project-praezedenz-13-1-vs-qrot-todo-2026-09-15
description: "TO-DO fuer 15.09.2026 (Opus): Praezedenzregel zwischen Punkt 13.1 (Chasing, Pflicht-Vorschlag halbe Position) und 7b1a Q-Score-ROT (kein Veto, 'Auslassen empfohlen') klaeren -- es gibt aktuell KEINE Regel, wer gewinnt, wenn beide gleichzeitig aktiv sind. Gefunden bei der Nachrechnung des Handelstags 14.09.2026 (17:55-Moment), dort aber nicht scharf geworden, weil die Chasing-Kriterien parallel zusammenbrachen. Levi: 'reicht erstmal, klaeren wir morgen'."
metadata:
  node_type: memory
  type: project
  status: "ERLEDIGT 15.09.2026 -- Opus-Entscheidungsvorlage fertig, siehe [[project_praezedenz_13_1_vs_qrot_entscheidungsvorlage_2026-09-15]]; Levi-Entscheidung dort noch offen"
  originSessionId: a2b3dbfe-4ee4-466f-855e-65c519af6e1f
  modified: 2026-09-15T08:25:12.628Z
---

# TO-DO 15.09.2026 (Opus): Praezedenz 13.1 (Chasing) vs. 7b1a Q-Score-ROT

**Herkunft:** [[project_rueckblick_handelstag_2026-09-14_1530-2200]], Nachtrag "14.09.2026 abends" (Nachrechnung des 17:55-Moments, Levis Rueckfrage zu Chasing/50%-Position).

## Die Frage in einem Satz

Wer gewinnt, wenn Punkt 13.1 (Chasing bestaetigt: 4-5 klar gerichtete Kerzenschluesse) eine halbe Position **aktiv vorschlagen muss** (Pflicht laut 13.1), und gleichzeitig der Q-Score **ROT** steht (7b1a: "Auslassen empfohlen", ausdruecklich **kein hartes Veto**, Levi/Sonnet behalten bei den ersten 8 Trades des 15-Trade-Fensters die Entscheidung)?

## Warum das geklaert werden muss

- Es gibt **keine Praezedenzregel** im Regelwerk (der bestehende Stacking-Absatz regelt nur 8b1a-Schock vor Q-Score und die Nicht-Kumulation der Halbierung selbst -- nicht diesen Fall).
- Besonders relevant im **Solo-Loop ohne Levi ansprechbar**: Zwei gleichzeitig aktive, gegensaetzliche Handlungsempfehlungen (13.1 "vorschlagen" vs. Q-ROT "auslassen empfohlen") duerfen nicht stillschweigend durch die Reihenfolge im Code oder eine Zufallsentscheidung des Loop-Agenten aufgeloest werden.
- Zusatzfrage (Teilfrage b aus der Analyse): Ab wann laeuft der k/2-Zaehler aus 13.1 -- muss das vollstaendige Dual-Gate waehrend der 2 Checks schon stehen, oder reicht die Chasing-Kriterien-Erfuellung allein? Am 14.09. waeren die Chasing-Kriterien schon ab ca. 17:35 erfuellt gewesen, das Dual-Gate erst ab 17:55 -- je nach Lesart haette 13.1 zeitgleich mit dem Q-ROT-Urteil "50% aktiv vorschlagen" verlangt.

## Bezug zum 14.09.-Fall (nicht scharf geworden, aber Anlass)

Am 17:55-Moment war Chasing = JA (halbe Position als Sizing-Hinweis gedruckt) UND Q-Score 1/4 ROT gleichzeitig aktiv. Die Kollision wurde in diesem Fall nicht entscheidungsrelevant, weil die Chasing-Kriterien in der nachfolgenden Retest-Zeitbox (8b Schritt 4, VC+1/VC+2) selbst zusammenbrachen -- das Setup verfiel damit ohnehin, unabhaengig von der Praezedenzfrage. Das naechste Mal koennte die Kollision aber real werden.

## Levi-Entscheidung

"Reicht erstmal, klaeren wir morgen. Bitte als To-do fuer morgen fuer Opus notieren." (14.09.2026)

**Naechster Schritt (15.09.2026):** Opus soll Optionen fuer eine Praezedenzregel ausarbeiten (analog zum Stil der Q2/Q4- bzw. Punkt-11-Entscheidungsvorlagen), inkl. Nachrechnung an den bereits vorhandenen historischen Chasing-Faellen (siehe die 09.09./10.09.-Testtag-Protokolle, die "--chasing yes/no"-Werte fuehren), und Levi zur Entscheidung vorlegen. Keine Umsetzung vor Levis Entscheidung.
