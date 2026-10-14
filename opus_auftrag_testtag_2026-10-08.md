---
name: opus_auftrag_testtag_2026-10-08
description: "Opus-Analyse-Auftrag (kopierbarer Prompt) fuer den fiktiven Testtag Do 08.10.2026: Start sobald Testtag + Tagesabschluss fertig (Cron 20:30), Bericht -> opus_bericht_testtag_2026-10-08.md; Endauswertung ist Mo 12.10., Opus bereitet sie nur vor"
metadata:
  node_type: memory
  type: project
  originSessionId: 3a4acdfa-7e3f-4ee0-b05a-9e82cf94fc45
  modified: 2026-10-08T09:37:06.297Z
---

# Opus-Auftrag: Analyse Testtag Do 08.10.2026 (angelegt 08.10. vormittags auf Levis Anweisung)

**Why:** Wie an den Vortagen ([[opus_bericht_testtag_2026-10-07]]) bekommt jeder Testtag eine unabhaengige Opus-Pruefung aus Rohlogs, nicht aus dem Sonnet-Eigenbericht ([[feedback_sonnet_eigenbericht_unzuverlaessig]]). Heute ist der letzte Testtag vor der Endauswertung am **Mo 12.10.2026** (Fr 09.10. kein Testtag).
**How to apply:** Start durch Cron 20:30 (nach Loop-Stopp 20:02, Tagesabschluss, H1-end-Lauf). Prompt unten unveraendert an einen frischen Opus-Agent geben. Ergebnis landet in `opus_bericht_testtag_2026-10-08.md`; Levi bespricht den Bericht (wie 07.10.). Keine Commits ([[feedback_commit_push_nur_levi]]).

## Prompt fuer Opus (kopierbar)

Du bist frischer Opus-Pruefer. Analysiere den fiktiven Testtag **Do 08.10.2026** (NAS100, Repo `C:\Users\umnus\tradingview-mcp`). Format und Tiefe wie dein Bericht zum 07.10.: Memory `opus_bericht_testtag_2026-10-07.md` (lesen, Struktur uebernehmen). Quellen sind Rohlogs/Bars, NICHT das Chat-Transkript und nicht die Selbstaussagen im Abschluss-Memo `project_testtag_2026-10-08_abschluss.md` und Tagesmemo `project_testtag_2026-10-08_ablauf_crons.md` (diese zuerst lesen, jede Aussage dagegen pruefen). Kontext: `levi_entscheide_opus_bericht_2026-10-07.md` (Levi-Entscheide, gelten heute), `project_endauswertung_08_10_vorgezogen_2026-10-02.md`, `project_h1_q2_trendkontext_vorabkriterium_2026-10-01.md`.

**Harte Grenzen:** nur lesen. KEIN git add/commit/push, KEIN `npm run test:e2e`, kein `node --test tests/` ohne Pfadliste, kein CDP, keine Skript-/Regel-Aenderung, kein Memory ausser dem Bericht. Alles, was Logs schreiben koennte, nur auf einer Kopie des ganzen `scripts`-Ordners in `%TEMP%` (sha1 der Originale vorher/nachher notieren und vergleichen). **Kein `h1_auswertung --auswertung end`** (der end-Lauf wurde bereits einmal vom Abschluss gefahren; Wiederholung ist nicht entscheidungsrelevant) — nur `--echt --zaehlstand` ist fuer dich erlaubt.

Prufe der Reihe nach:
1. **Gueltigkeit/Abdeckung** bis Stichtag 20:00: Slots 15:25-20:00, VC-Nummern/Dateinamen/Kopfzeilen (`vc_nr`), Hard-Exits, Quick-Ticks (Doppel-Fires, Luecken), Bars-Dateien lueckenlos, Loop-Start (Cron 15:20), Start-Update 15:00, **Terminal 20:02** (Slot-Regel P2.2, `--terminal-geprueft` im Terminal-Slot), protokoll_bilanz. Freeze-Zaehlung: bewertbare Tage (7/10 -> mit heute 9/10?) und AUTO (16/20 + heute).
2. **Tagesbild** (5m/15m/60m/QQQ): Marktverlauf, Dual-Gate-Zeitstrahl bar-fuer-bar nachgerechnet (EMA50 selbst rechnen), Session-Extrema.
3. **Alle Live-gate_check-Laeufe** (gate_check_log): Ausloeser, Geometrie, RR/Zone, Q1-Q4, Ergebnis; skipped-/Kombi-Nachtraege bar-fuer-bar nachrechnen (Exit, R, MFE), Stall-Exit-Wirkung, Q-ROT-Wirkung, Dubletten (gleicher Anker + gleiches TP1 = 1 Idee).
4. **Umsetzung der Levi-Entscheide vom 08.10.:** (E2) **kein Gate bei Trigger mit 1H-Override dagegen** — jeden Fall mit Override dagegen pruefen; (E3) Zeitbox-Neuaufrufe unveraendert, aber dedupliziert ausweisen; (E4) **B5: US-Session-Hoch/-Tief ab 15:30 sofort ins Register** — Register-Eintraege (`level_register.json`, `register_touch`) gegen die Bars pruefen, verpasste Trigger/Q4-Folgen benennen; (E5) **Punkt-11-Bezug fuer 13.1 = Gate-Richtungs-Lesart**, in `--grund-chasing` benannt? Pruefe zusaetzlich fuer JEDEN VC, in dem die 5m-Serie gegen die Dual-Gate-Richtung lief, ob nach der **Serien-Lesart** (02.10./05.10.) ein 13.1-Konflikt entstanden waere (Beinahe-Konflikte ausweisen, mit hypothetischem Ergebnis); (E9) wurde die Prompt-Datei bei jedem Fire voll gelesen (soweit aus Logs erkennbar)? Bruchliste fuehren.
5. **13.1-Richtungskonflikte:** `analyse/rk13_1_filter.cjs --ab 2026-10-08 --bis 2026-10-08` auf der Kopie; Prio-1-Warnzeile (Richtungskonflikt) im Live-Betrieb erstmals ausgeloest? `richtungskonflikt_13_1` in vollcheck_log.
6. **Gate-Praxis nach Buchstabe** (Levi-Entscheid 2 vom 02.10.): leeres/levelloses TP1-Fenster -> Gate trotzdem fahren; Trigger in Gate-Richtung; Gegenrichtungs-Crosses; nicht eingeloeste Gate-Ankuendigungen (Muster VC#10 am 07.10.); Q3 manuell nach EMA-Seite vs Q3-AUTO (Widerspruchsfaelle zaehlen).
7. **Handwerte:** `--punkt11-signal`, `--stale-n`, `--chasing`, unbestaetigte Anker, Anker-Disziplin (sl_anker_wechsel_log), Retest-Bedingungen. Fehler benennen, nicht passend machen.
8. **H1/IBF:** `analyse/h1_auswertung.cjs --echt --zaehlstand` (Kopie, keine R-Werte), `analyse/ibf_schatten.cjs --tage 2026-10-08`, `tagesmomente --auswertung`, `kombi_fiktiv --auswertung` (Kopie).
9. **Abschluss-Memo korrigieren (nur als Liste im Bericht, nicht ueberschreiben):** Abweichungen Memo vs Logs; Bestaetigtes.
10. **Vorbereitung der Endauswertung Mo 12.10.2026** (Levi, 08.10.; heute 9/10 Tage max.): welche Zahlen liegen bereit (ALT/AUTO/FLOOR kumuliert, skipped roh UND dedupliziert, Q-ROT mit/ohne Artefakt 02.10. 17:36, Kombi, H1 end-Ergebnis laut Abschluss, IBF, O12-Stand (Tag 3/4?), Q3-AUTO-Vorab); **Entscheidungsvorlage fuer Levi** (Urteile je Variante, Freeze-Ende ja/nein, was am Montag zu beschliessen ist); Liste "erst nach der Endauswertung" fortschreiben (Punkt-11-Bezugsrichtung, Zeitbox nach PASS-ROT, Gegenrichtungs-Cross, 1H-Override-Gates, DST-Fix `vollcheck.cjs` bis **19.10.**).

**Ausgabe:** Urteil GUELTIG/EINGESCHRAENKT/UNGUELTIG mit Begruendung, Abschnitte 1-10, nummerierte Entscheide fuer Levi mit je einer Empfehlung, ehrliche Gesamtwertung (Gut / Nicht gut / Disziplin / Inhaltlich). Zeitstempel Start/Ende. Ergebnis als Memory `opus_bericht_testtag_2026-10-08.md` mit Frontmatter (name, description, metadata.type project) ablegen, Index-Zeile in MEMORY.md ergaenzen (eine Zeile, max. 200 Zeichen). Sonst nichts veraendern.
