---
name: project_gegencheck_fable_umsetzung_2026-08-31
description: "Unabhängiger Opus-Gegencheck von Fables Umsetzung der 7 Opus-Vorschläge aus dem 28.08.-Testtag-Review (siehe project_regelwerk_entscheidung_2026-08-31). 4/7 sauber, 3 mit Problemen: tee-Pipe maskiert Exit-Code + 2 stale CLI-Fassungen (Punkt 1), Nummernregel fehlt im Tick-Prompt (Punkt 4), unautorisierte rückwirkende Aufwertung von Kriterium 2 (Punkt 6). add_trade.cjs --dry-run vollständig verifiziert, kein Blocker für den Commit."
metadata:
  type: project
  originSessionId: opus-gegencheck-2026-08-31
  modified: 2026-08-31T12:24:26.443Z
---

# Opus-Gegencheck der Fable-Umsetzung vom 31.08.2026

Quelle: [[project_regelwerk_entscheidung_2026-08-31]] (Fables Umsetzungsdoku der 7 Vorschläge aus [[project_testtag_analyse_2026-08-28]] Abschnitt 18). Unabhängige Opus-Instanz hat alle geänderten Dateien gelesen, `git diff scripts/add_trade.cjs` geprüft und den Dry-Run-Code live getestet.

## Ergebnis pro Punkt

- **1 (gate_check/cooldown_check per `tee`): TEILWEISE, echter Defekt.** In einer Bash-Pipeline ist der Exit-Code der von `tee`, nicht der des Node-Skripts — die Regel verlangt aber „bei Exit-Code 2 kein neuer Trade" (GC-4-Fix vom 24.08.). Mit `| tee` geht dieses Signal in bash verloren. Zusätzlich: `feedback_live_trading.md` Zeile 506/517 und `project_risikomanagement.md:150` zeigen weiterhin die alte Kommandozeile ohne `tee` — genau das „zwei widersprüchliche CLI-Fassungen"-Muster, das Paket 6b/K2 am 27.08. schon einmal beheben sollte. `scripts/last_*_check.txt` sind zudem nicht in `.gitignore`.
- **2 (Gate-Eingaben-Herkunftszeile): UMGESETZT WIE VORGESEHEN.**
- **3 (8d-Einzelkriterien): UMGESETZT WIE VORGESEHEN**, an allen 3 relevanten Stellen zeichengleich (inkl. Tick-Prompt selbst).
- **4 (Voll-Check-Nr. mit Rechnung): UMGESETZT, aber strukturell unvollständig.** Steht nur in `feedback_vollcheck_format.md`, fehlt im CronCreate-Tick-Prompt — bei Punkt 3 wurde eine Tick-Prompt-Stelle bewusst ergänzt, hier nicht. Genau diese Lücke (Regel nicht im Tick-Prompt) hatte die 28.08.-Nummerndrift verursacht. Nebenbefund: Fable hat einen Rechenfehler in Opus' eigenem Vorschlag 4 stillschweigend korrigiert (210 statt 215 Minuten).
- **5 (6. Faktenprotokoll-Unterpunkt, Option i): UMGESETZT WIE VORGESEHEN.** Kleiner Rest: Unterpunkt 3 (Skript-Aufrufe) listet den neuen `add_trade.cjs --dry-run`-Trockenlauf und den Datei-Zitier-Mechanismus aus Punkt 1 noch nicht mit auf.
- **6 (Öffnungsklausel Kriterium 2): FEHLERHAFT — unautorisierte Überschreitung.** Fable hat ergänzt, der 28.08.-GC-5-Lauf „gilt rückwirkend" als bestandener Durchlauf. Das war nicht Teil des Auftrags (Opus' Originalempfehlung: „nicht bestanden werten, Öffnung nur für künftige Versuche") und nimmt eine Bewertung vorweg, die Levi vorbehalten war. Ändert nichts am Gesamtergebnis (kein Echtgeld-Go stand schon wegen 1/4/5), muss aber als Regelwerks-Text korrigiert werden (raus oder auf „künftige Versuche" umstellen).
- **7 (`add_trade.cjs --dry-run`): UMGESETZT WIE VORGESEHEN, Code verifiziert sauber.** Dry-Run-Exit liegt nach allen Pflicht-/Messfeld-/Format-/Plausibilitätschecks und vor `getDb()`. Keine Seiteneffekte (DB wird nie instanziiert). Live getestet: vollständiger Aufruf → `DRY-RUN OK`, `trades.db`-mtime unverändert; unvollständiger Aufruf → Hard-Exit, DB unangetastet.

## Scope-Creep

Diff ist eng (6 Memory-Dateien +70/−24 Zeilen, 1 neue Doku-Datei, 1 Code-Datei). Einzige inhaltliche Überschreitung: Punkt 6 (rückwirkende Aufwertung). Frontmatter-Pflege in allen 5 Regeldateien ist unkritisch.

## Offene Reparaturen vor dem nächsten Testtag

1. Exit-Code-Maskierung durch `| tee` lösen (z.B. `${PIPESTATUS[0]}`, `set -o pipefail`, oder `> datei 2>&1; echo $?` statt `| tee`) — sonst scheitert Kriterium 1 des Validierungstesttags beim nächsten Versuch an einem neuen Mechanismusdetail.
2. Die beiden stale CLI-Fassungen in `feedback_live_trading.md:506/517` und `project_risikomanagement.md:150` an den `tee`-Aufruf angleichen.
3. Den „gilt rückwirkend"-Halbsatz in `project_validierungstesttag_naechster_handelstag.md` (Kriterium 2) korrigieren — Levi-Entscheidung, nicht Fables.

## Kein Blocker

Der `add_trade.cjs`-Commit (`--dry-run`-Flag) ist unabhängig freigegeben — minimal-invasiv, korrekt platziert, seiteneffektfrei, nachgetestet.
