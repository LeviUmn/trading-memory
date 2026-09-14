---
name: project-studie-geschwindigkeit-effektivitaet-fable-2026-09-07
description: "Fable-Studie 07.09.2026 (unabhängig von Opus' Version zur selben Frage): Geschwindigkeit/Effektivität des Live-Loops. Kernbefund: Loop verliert Zeit an 6 Chart-Zustandswechseln/VC + 11 redundanten 1H-Lesungen/Stunde + Trigger-Nachschlagearbeit; verliert Trades nicht an Gates, sondern an Ausbruchs- statt Retest-Entries und daran, dass Struktur nur für SL, nicht für TP zählt. 12 priorisierte Vorschläge."
metadata: 
  node_type: memory
  type: project
  status: draft
  modified: 2026-09-07T17:08:20.767Z
  originSessionId: b594a3be-34a1-46da-8616-0f50ceb56daa
---

# Fable-Studie: Geschwindigkeit & Effektivität (07.09.2026, unabhängig von Opus)

**Kernaussage:** Der Loop verliert Zeit an sechs Chart-Zustandswechseln pro Voll-Check, an elf redundanten 1H-Lesungen pro Stunde und an einem Trigger-Moment, der Dinge nachschlägt, die fünf Minuten vorher schon bekannt waren. Er verliert Trades nicht an zu strengen Gates, sondern daran, dass er am Ausbruch statt im berechenbaren Entry-Fenster einsteigt und dass Struktur nur für den Stop, nicht für das Ziel zählt.

## Gemessene Dauer (Screenshot-mtimes als objektive Quelle)
VC#2-6/#11-17 je 41-57s. VC#1 110s (Erstaufbau). **VC#7 (Trigger) 6:40 min**, VC#10 2:46 min, 18:49-Trigger ~4 min. Fires verloren u.a. 15:36-15:42 komplett, plus einzelne in trigger-freien Phasen. Ursache strukturell: ~150s Auslastung von 300s pro 5-Min-Fenster — jeder Aussetzer kippt einen Fire.

## Geschwindigkeit — wo die Zeit hängt
1. 6-7 zwingend sequenzielle Chart-Zustandswechsel pro VC (Timeframe-Wechsel NAS100/QQQ, pane_focus) — größter rein technischer Block, plus die bekannte "QQQ bleibt auf 60"-Fehlerklasse.
2. ~35 Zeilen Fließtext/VC; 1H wird alle 5 Min neu gelesen, obwohl der Override nur am letzten *geschlossenen* 1H-Bar hängt — 11 von 12 Lesungen/Stunde sind informationslos.
3. 45 fehlgeschlagene X-API-Aufrufe an einem Tag ohne Backoff (04.09.) vs. 61 Abrufe auf 84 VCs durch Über-Polling (02.09.) — Fälligkeits-/Fehlerbehandlungsproblem.
4. A3-Ausnahmen als Dauerzustand (`--trend-effizienz`, `--dual-gate-qqq-abstand-atr`) — 5/5 Standardbegründung, Erkenntnisgewinn null.
5. Trigger-Moment: Skript-Header-Recherche (400-Zeilen-Kopfkommentar im Trigger gelesen!), Levelsuche, cooldown_check, gate_check — fast alles vermeidbar, da die A3-Werte längst bekannt sind.
6. Screenshots: 39/55 ausgelassen — faktisch Zeitsparmaßnahme gegen die Regel; Aufruf selbst billig, teuer ist das Betrachten (Vision-Tokens).

**Offene Position:** andere Zeitanforderung — Scalable führt SL/TP exklusiv, jede Minute nach TP1-Fill ist Fill-Risiko. Aktuelles Regelwerk kennt keinen eigenen Positions-Modus für den Voll-Check.

## Effektivität — wo Trades verloren gehen
1. **Entry-Zeitpunkt, nicht TP-Wahl:** Reifegrad 1,7-2,41×ATR bei allen 04.09.-Triggern, Entry = Live-Kurs nach 2-5 gerichteten Kerzen → Struktur-Anker 40-72 Pkt entfernt → TP1-Fenster schrumpft auf 4-29 Pkt. Um 18:49 lag der Kurs *innerhalb* eines 9-Pkt-PASS-Fensters [29547;29556] — ohne dass es als Zahl vorlag.
2. **Leveldichte:** fehlend sind Struktur-Swings (15min/1H-Zwischenhochs/-tiefs), Opening Range, Hochs/Tiefs T-2/T-3, Wochen-Pivots. Asymmetrisch: Struktur zählt für SL (P4), aber ein Zwischenhoch zählt nicht als TP, weil keine kanonische Levelart.
3. **Kein Nenner:** unbekannt, wie viele Trigger-Kandidaten es gab, nur wie viele Gate-Aufrufe — #8 ist Voraussetzung, nicht Kür.
4. Register-Pflege bleibt manuell getaktet trotz identischer Datenbasis (`data_get_ohlcv` ohnehin jede Minute geholt).

## Priorisierte Vorschläge

| # | Vorschlag | Kategorie | Priorität | Empfehlung |
|---|---|---|---|---|
| 1 | **A1 Vier-Pane-Layout** (NAS100 5/15/60min + QQQ 15min fest, kein Timeframe-Wechsel) | PROMPT/PROZESS + Layout | hoch | ja, zuerst |
| 2 | **A2/B1 `gate_check --vorschau`** — Vollberechnung ohne A3-Zwang, Exit 0: "PASS-fähig ab Entry ≤X mit TP1 Y/TP2 Z" + vorbefülltes Kommando; plus `alert_create` auf der Fensterkante | SKRIPT-FIX | hoch | ja — wirkt auf Trigger-Dauer UND Entry-Qualität |
| 3 | **B3 Trigger-Kandidaten-Zähler** live protokolliert + #8-Replay | SKRIPT-FIX/DATENTASK | hoch | ja — ohne Nenner keine Effektivitätsaussage |
| 4 | **B2 `--auto-levels`** (Struktur-Swings 15min/1H, T-2/T-3-Extrema, Opening Range als neue Levelarten 9/10) | SKRIPT-FIX + REGELWERKSFRAGE | hoch | ja für Swings/OR, NEIN für 25er-Rundzahlen |
| 5 | **A5 Positions-Modus-Voll-Check** (MTF/Stall/Positionskasten bleiben, Levelsuche/Fib/SL-Vorprüfung/1H-Schatten entfallen), Ziel <30s | REGELWERKSFRAGE | mittel-hoch | ja |
| 6 | **A4 1H nur bei neuem 1H-Schluss lesen** | REGELWERKSFRAGE | mittel | ja |
| 7 | **A3 X-API-Backoff** (Fehlerzähler, ab 3 Fehlern nur alle 30 Min) mit Pflicht-Statuszeile | SKRIPT-FIX | mittel | ja |
| 8 | **B4 Register-Autopflege aus Bars** (Session-Extrema/Rundzahl-Band automatisch aus derselben `data_get_ohlcv`-Ausgabe) | SKRIPT-FIX | mittel | ja |
| 9 | **A6** QQQ-ATR als Indikator ergänzen; `--trend-effizienz` rechnen oder von A3 zurückstufen | PROMPT + REGELWERKSFRAGE | mittel | ja |
| 10 | **A9 Kompakt-Tokens** für latchende Blöcke (8d K2/K3, Tweet-Check bei bekanntem Ausfall) | PROMPT | niedrig | ja, sofort |
| 11 | **A7 Screenshot:** Aufnahme Pflicht, Betrachtung nur bei Trigger/Strukturbruch | REGELWERKSFRAGE | niedrig | ja |
| 12 | **A8 `vollcheck_data.mjs` direkt auf CDP** (zweiter Client, umgeht MCP-Guards) | SKRIPT-FIX (Spike) | mittel, hohes Potenzial | erst Spike, nicht blind bauen |
| — | `trade_stats.cjs`-Bugs | SKRIPT-FIX | niedrig | erledigt, Commit `6a42fa5` |

## Bewusst NICHT empfohlen
**B5 — P4-Anker-Definition und 8c2-Sockel anfassen.** Hier entstehen die meisten FAILs, aber das ist gerade bestätigtes Design. Der legitime Hebel ist der Entry-Zeitpunkt (B1), nicht der Anker.

## Was Geschwindigkeit/Effektivität nur vortäuscht (ehrliche Risikoverschiebungen)
- Fires nachholen/VC auf 10 Min dehnen — verschiebt Trigger-Erkennung, nicht empfohlen.
- X-Backoff — echte Ersparnis, Preis ist Nachrichtenblindheit, braucht Pflichtzeile.
- Screenshot-Verzicht — verlagert Chartmuster-Erkennung an den Trigger.
- Direkt-CDP (A8) — Systemrisiko (zweiter Client, umgangene Guards), nur nach Spike.
- Mehr Level (B2) — TP-Qualität muss über Levelart-Kennzeichnung ehrlich bleiben, sonst wandert das Rundzahl-Tautologieproblem in die Swings.
- Retest-Entry (B1) — weniger Chasing, aber auch weniger Läufer-Trades — das ist die Wette, die 8b schon immer eingeht, nur jetzt mit Zahl statt Bauchgefühl.
