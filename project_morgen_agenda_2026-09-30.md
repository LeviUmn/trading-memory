---
name: project_morgen_agenda_2026-09-30
description: "Agenda fuer die gemeinsame Durchsprache mit Levi am 30.09.2026 frueh: Opus-Analyse Testtag 29.09., 4 Entscheidungen, 3 Fable-Auftraege (noch nicht beauftragt), offene Aufraeumpunkte"
metadata:
  node_type: memory
  type: project
  originSessionId: 06006c65-4ed0-4041-9028-70709ef145bd
  modified: 2026-09-30T10:02:51.751Z
---

# Morgen frueh (Mi 30.09.2026): gemeinsame Durchsprache mit Levi

**STATUS 30.09.2026: ERLEDIGT.** Durchsprache hat stattgefunden; Levis Entscheidungen a-d + P3-Verlaengerung + Faktenprotokoll stehen in [[project_testtag_analyse_2026-09-29]] (Abschnitt "Levi-Entscheidungen 30.09.2026"), die Opus-Pruefung/Spezifikation in [[opus_vorschlag_2026-09-30]]. Auftrag 0 + A (Tagesende-Stichtag, Abdeckungsanzeige, IBF-Schatten) von Fable am 30.09. umgesetzt; Auftrag B (Q3-auto, Drift-Hinweis, Kleinigkeiten) erst nach dem Loop-Stopp 30.09. Korrektur: der 30.09.2026 ist ein **Mittwoch** (29.09. = Dienstag), nicht "Di". Nicht gepusht waren 2 Commits (57c711a + ee27c70), nicht nur ee27c70 — am 30.09. von Sonnet ohne Auftrag gepusht (Incident); seither gilt: Commit UND Push beauftragt nur Levi ([[feedback_commit_push_nur_levi]]).

Levi will am 29.09. abends "alles speichern" und morgen frueh gemeinsam durchgehen. **(Stand 29.09. abends: noch nichts beauftragt, nichts committet, keine Regel geaendert.)**

**Lesestoff (in dieser Reihenfolge):**
1. [[project_testtag_analyse_2026-09-29]] — Kurzfassung mit Urteil, 3 Gate-Laeufen, Freeze, P3, 1H-Override, Q3-auto/Tagesende
2. Voller Opus-Bericht: `memory/opus_bericht_testtag_2026-09-29.md` (Kopie aus dem Scratchpad, 21 KB)
3. [[project_p1p3_anker_reset_pflicht_2026-09-29]] (P3-Rueckbau-Kriterien), [[project_opus_analyseliste_2026-09-29_1h_override]]

**4 Entscheidungen fuer Levi (Opus-Empfehlung in Klammern):**
a) Zaehlt der 29.09. als Freeze-Tag? (nein, Teiltag -> 2/5 Tage, AUTO 5/20)
b) Was ist der "19:55-Schluss": Schluss der 19:50- oder der 19:55-Kerze? (Setup 3: +0,69R vs +0,84R)
c) 1H-Override bis Freeze-Review unveraendert? (ja; neu pruefen ab >=20 Episoden, heute 9)
d) Usage-Limit-Vorsorge vor Testtag-Start (Budget vor 15:10 pruefen)

**3 Fable-Auftraege (erst nach Levi-Freigabe, je Opus-Gegencheck, freeze-konform):**
1. Q3-auto als Schattenzeile in gate_check.cjs (Kriterium 0 Widersprueche + 0 UNBEKANNT in 3 Tagen; Stand 1/3)
2. Tagesende-Auswertungsregel (Stichtag 19:55 als Default im Nachtrag + Meldung in protokoll_bilanz; Rueckbau Stichtag 20:00; Stand 1/3, haengt an Entscheidung b)
3. Abdeckungsanzeige in tagesmomente --auswertung (Anteil Fenster mit VCs, Soll >=80 %, reine Anzeige)
Kleinigkeiten ohne Auftrag: A2-Zaehler (P2 zaehlt nicht-faellige VCs mit), kerzen-qqq-Heuristik als Hinweis statt Warnung, Session-Extrema ins Register, Sonnet-Prozessregel "TP1/SL aus dem Live-Lauf, nicht aus der Vorpruefung".

**Zustand des Repos am Abend 29.09.:** HEAD ee27c70 (nicht gepusht). Ungetrackt (bewusst nicht committet, kein `git add -A`): scripts/loop_archiv/2026-09-29.txt (Tagesprotokoll, aus VC-Ausgaben + Live-Gate-Laeufen zusammengesetzt), scripts/kombi_fiktiv_log.jsonl, scripts/last_*_out.txt, scripts/analyse/..., *.json.bak. Levi entscheidet morgen, ob/was als Backup committet wird.

**Sonstiges offen:** Remote-Control fuer Testtage vorher an ([[feedback_testtag_start_verlaesslichkeit]]); Mac-Umzug-Cutover erst nach Testtag-Block ([[project_mac_umzug]]); trades.db nur lokal -> vor Umzug sichern.

**Why:** Abend-Abschluss ohne Levi-Entscheidungen; damit morgen nichts aus dem Chat rekonstruiert werden muss. **How to apply:** Morgen erst diese Datei lesen, dann mit Levi a)-d) klaeren, erst danach Fable beauftragen.
