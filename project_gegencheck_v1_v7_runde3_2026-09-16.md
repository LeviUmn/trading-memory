---
name: project_gegencheck_v1_v7_runde3_2026-09-16
description: "Opus-Gegencheck Runde 3 (16.09.2026) der Fable-Korrektur zu Auflage A+B aus [[project_gegencheck_v1_v7_runde2_2026-09-16]]. FREIGEGEBEN MIT AUFLAGEN, keine Freigabe-Blockade. A1/A2/B live per CLI bestaetigt (nicht nur Testlauf): Dry-Run erzeugt keine Fiktiv-Zeile mehr (weder FAIL noch Freigabe/ROT/UNBEKANNT), last_gate_fail.json bleibt im Dry-Run byte-identisch (md5 verglichen), A3-Feldliste (42 Felder) steht vollstaendig im Log (822 Zeichen). Kernteil: die 7 umgestellten Alt-Tests (dry-run->live:true, 21 GATE_BASE-Aufrufe) wurden maschinell Zeichen-fuer-Zeichen als reine live:true-Einfuegungen verifiziert (keine Assertion geaendert), UND per HEAD-Gegenprobe (alte Testdatei gegen neue Skripte) als ERZWUNGEN belegt (genau 8 Fehlschlaege = die 7 Tests + der bereits aus V6-Gruenden abweichende V9-Test, kein Kollateralschaden). Die 4 Aufraeumzeilen (last_sl_vorpruefung.json loeschen) sind per Entfernungsprobe als exakt notwendig belegt, reine Sandbox-Hygiene. Kein stillschweigend aufgegebener Pruefzweck. EINZIGE ECHTE AUFLAGE C1: fuer A1 und A2 selbst gibt es KEINEN neuen Regressionstest -- die neue 15.09.-Suite deckt V1-V5 ab, aber nicht 'Dry-Run schreibt keine Fiktiv-Zeile' oder 'Dry-Run laesst last_gate_fail.json unberuehrt'. Wuerde jemand die beiden dry-run-Guards morgen entfernen, bliebe die Suite trotzdem 102/102 gruen -- der Runde-2-Defekt kaeme unbemerkt zurueck. Diverse nicht blockierende Befunde (N1-N6, B-Rest: Dry-Run-Anzeige jetzt kontrafaktisch/unmarkiert, Kopfkommentar veraltet, Nutzungshinweis unvollstaendig, doppelter live:true-Schluessel Copy-Paste-Rest, Intra-Suite-Kopplung in der neuen 15.09.-Suite, Gesamtbudget-Kappung bei Auflage B kann noch front-loaded/unmarkiert mitten im Wort kappen bei sehr langen Werten)."
metadata:
  node_type: memory
  type: project
  status: "FREIGEGEBEN MIT AUFLAGEN 16.09.2026 -- Auflage C1 (Regressionstests fuer A1/A2 nachziehen) offen, sonst keine Blockade. Code-Diff weiterhin unkommittiert (Levi)"
  originSessionId: session_current
  modified: 2026-09-16T10:51:34.833Z
---

# Opus-Gegencheck Runde 3: Auflage A1/A2/B + die 7 umgestellten Alt-Tests (16.09.2026)

**Prüfling:** unkommittierter Diff `scripts/gate_check.cjs`, `scripts/vollcheck.cjs`, `tests/trading_scripts.test.js` (382+/37−, Basis `cc680a4`). **Methode:** eigener Testlauf (zweimal), `npm run test:all`, eigene CLI-Gegenproben in Sandbox-Kopien, maschinelle Hunk-für-Hunk-Diff-Analyse, HEAD-Testdatei gegen neue Skripte laufen lassen, Isolationsprobe je Suite. Bezug: [[project_gegencheck_v1_v7_runde2_2026-09-16]] (Runde 2).

## Gesamturteil: FREIGEGEBEN MIT AUFLAGEN, keine Freigabe-Blockade

**Tests:** 102/102 (zweimal identisch). `npm run test:all`: 131/132, einziger Fehler `tests/e2e.test.js` (braucht laufendes TradingView, umgebungsbedingt, unabhängig vom Diff).

## Auflage A1 (Dry-Run-Schutz Freigabe-Logger) — BESTÄTIGT live per CLI

Guard liegt um den gesamten ROT/UNBEKANNT/Freigabe-Block. Eigene Gegenprobe: Dry-Run mit PASS+Q-GELB, PASS+Q-ROT und UNKNOWN-Geometrie erzeugen alle keine Zeile; derselbe Lauf live erzeugt sie korrekt. Nebenbefund N6: Dry-Run gibt keinen Hinweis, dass nichts geloggt wurde (nicht blockierend).

## Auflage A2 (`last_gate_fail.json` nur lesen im Dry-Run) — BESTÄTIGT, exakter Schadensfall aus Runde 2 nachgestellt

Live aussichtslos → Dry-Run mit anderer Geometrie dazwischen (Datei md5-identisch danach) → dritter Live-Lauf mit derselben toten Geometrie druckt wieder korrekt "AUSSICHTSLOS-SPERRE VERLETZT" statt "ZEITBOX-KOLLISION". Anzeige bleibt im Dry-Run wie behauptet erhalten. Nebenbefund N1: die erhaltene Anzeige ist jetzt teils kontrafaktisch (sagt "wird ERSETZT", obwohl im Dry-Run nichts geschrieben wird) — Empfehlung: `[dry-run: nicht gespeichert]`-Marker analog zur bestehenden V9-Zeile ergänzen (nicht blockierend).

## Auflage B (Zeilenkappung gestrichen) — BESTÄTIGT für den gemeldeten Defekt, TEILWEISE in der Substanz

A3-Feldliste (42 Felder, 822 Zeichen) steht jetzt vollständig im Log. Realistischer Worst Case (1488 Zeichen) bleibt unter dem 2000er-Budget. Bei künstlich sehr langen Werten (>2000 Zeichen gesamt) kappt das Gesamtbudget aber weiterhin unmarkiert mitten im Wort und "front-loaded" — die zuerst geschriebene Zeile frisst das Budget, die eigentlich wichtige Pflichtfeld-Zeile kann dabei komplett wegfallen. Dieselbe Fehlerklasse wie Auflage B, an einer anderen Schwelle. Nicht blockierend (praktisch kaum erreichbar), aber notiert für eine spätere Nachbesserung (an Zeilengrenzen kürzen, letzte Zeile bevorzugen, `[gekappt]`-Marker).

## Die 7 umgestellten Alt-Tests — Kernteil dieser Runde

- **Mechanisch:** 20 Zeilen sind Zeichen-für-Zeichen reine `live: true`-Einfügungen, keine Assertion geändert. Die restlichen Hunks sind V6-Änderungen im V9-Test (separat, bereits geprüft), 4 Aufräumzeilen und die neue 15.09.-Suite.
- **Erzwungen, nicht gratis:** die alte Testdatei gegen die neuen Skripte laufen lassen ergibt genau 8 Fehlschläge = die 7 gemeldeten Tests + der V9-Test (aus V6-Gründen). Kein Kollateralschaden — die A1/A2-Guards haben keine sonstigen Nebenwirkungen auf die Suite.
- **4 Aufräumzeilen:** Entfernungsprobe zeigt exakt 4 Fehlschläge beim Weglassen — nicht eine zu viel, nicht zu wenig. Reine Sandbox-Hygiene (löschen einer V9-Zustandsdatei aus einer fremden Suite), keine verdeckte Erwartungsänderung.
- **War Dry-Run je Prüfabsicht?** Nein — die Testnamen selbst benennen das Logging als Prüfgegenstand, der Kopfkommentar der Testdatei nannte das alte "schreibt auch im Dry-Run"-Verhalten ausdrücklich als Eigenheit (genau der A2-Defekt), und die Stellen, wo "Dry-Run schreibt nichts" echte Prüfabsicht ist (`gate_check_log.jsonl`, `loop_stopp.cjs`, `position_tick.cjs`, V9), sind unangetastet und unabhängig verifiziert.
- Alle 7 Tests einzeln gegen die alte Prüfabsicht abgeglichen (Tabelle im Volltext) — durchgehend BESTÄTIGT, dieselbe Sache wird geprüft wie vorher, nur der Auslösemodus hat sich geändert. Isolationsprobe (4 betroffene Suiten einzeln): alle grün, keine Suite hängt an einer anderen.

## Einzige echte Auflage: C1 — Regressionstests für A1/A2 fehlen

Für A1 und A2 selbst gibt es keinen einzigen neuen Test. Die neue 15.09.-Suite deckt V1-V5 und die Runde-1-Auflagen ab, aber nicht "Dry-Run schreibt keine Fiktiv-Zeile" oder "Dry-Run lässt `last_gate_fail.json` unberührt". Würde jemand die beiden `dry-run`-Guards (Zeile ~3723, ~3925 in `gate_check.cjs`) künftig entfernen, bliebe die Suite weiterhin 102/102 grün — der Runde-2-Defekt käme unbemerkt zurück. Zwei kleine Tests genügen (Vorschlag im Volltext des Berichts).

## Weitere nicht blockierende Befunde

N1 (Dry-Run-Anzeige kontrafaktisch), N2 (doppelter `live: true`-Schlüssel, Copy-Paste-Rest, Zeile ~2184), N3 (Kopfkommentar der Testdatei veraltet — behauptet noch das alte Dry-Run-Schreibverhalten), N4 (Nutzungshinweis in `gate_check.cjs` nennt die neuen Dry-Run-Ausnahmen nicht), N5 (bewusste Intra-Suite-Kopplung zwischen zwei Tests der neuen 15.09.-Suite, isoliert lauffähig machen wäre sauberer), N6 (Dry-Run gibt keinen Hinweis auf übersprungene Erfassung), B-Rest (Gesamtbudget-Kappung s.o.).

**Stand:** Code-Diff weiterhin unkommittiert. Opus: keine Freigabe-Blockade — C1 kann vor oder nach dem Commit nachgezogen werden, ist aber der einzige Punkt, der aus Sicht des Prüfers vor dem nächsten fiktiven Testtag sinnvoll noch fehlt.

Vorgänger: [[project_gegencheck_v1_v7_runde2_2026-09-16]], [[project_gegencheck_v1_v7_fable_umsetzung_2026-09-16]], [[project_testtag_analyse_2026-09-15]] (V1-V7-Ursprung).
