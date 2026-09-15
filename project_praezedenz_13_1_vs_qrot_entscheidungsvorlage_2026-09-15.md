---
name: project-praezedenz-13-1-vs-qrot-entscheidungsvorlage-2026-09-15
description: "Opus-Entscheidungsvorlage 15.09.2026 zur Praezedenz zwischen Punkt 13.1 (Chasing-Pflichtvorschlag) und 7b1a Q-Score-ROT (Auslassen empfohlen, kein Veto). 4 Optionen (A/B/C/D), Opus empfiehlt Option D (begruendete Praezedenz: Q-ROT geht vor, wenn Q1 oder Q4 das ROT tragen -- was strukturell fast immer der Fall ist, weil Halbieren Q1/Q4-Einwaende nicht repariert) mit Option A als Kurzfassung. Teilfrage b (k/2-Zaehler-Start): Empfehlung b1 (Zaehler startet erst mit vollstaendigem Dual-Gate). NOCH KEINE Levi-Entscheidung getroffen."
metadata:
  node_type: memory
  type: project
  status: "Vorlage fertig, Levi-Entscheidung noch ausstehend (Stand 15.09.2026)"
  originSessionId: dbfa1999-360e-473a-b80c-5e3236d306fd
  modified: 2026-09-15T08:24:57.591Z
---

# Präzedenz 13.1 (Chasing-Pflichtvorschlag) vs. 7b1a Q-Score-ROT — Entscheidungsvorlage (Opus, 15.09.2026)

**Herkunft:** [[project_praezedenz_13_1_vs_qrot_todo_2026-09-15]] (TO-DO), Anlass [[project_rueckblick_handelstag_2026-09-14_1530-2200]] (17:55-Moment 14.09.2026). Diese Vorlage ändert **nichts** am Code/Regelwerk — reine Entscheidungsgrundlage, keine Umsetzung vor Levis Entscheidung.

## Die Frage
Wenn 13.1 zwingend "50%-Einstieg AKTIV vorschlagen" verlangt und der Q-Score gleichzeitig ROT ("Auslassen empfohlen") meldet — wer antwortet zuerst, und was gilt im unbeaufsichtigten Solo-Loop?

## Kernbefund (strukturell, nicht nur der Anlassfall)
Chasing und Q-Score überschneiden sich nur in Q2. Q1 (Ablehnungsmuster) und Q4 (Runway) misst etwas anderes, das die Halbierung nicht repariert. Da 13.1 ein volles Dual-Gate voraussetzt, ist Q3 (Kohärenz) bei jedem 13.1-Moment praktisch immer ✓ — also müssen bei ROT (≤2/4) die Ausfälle aus {Q1, Q2, Q4} kommen, **nie können beide Ausfälle Q2 sein**. Folge: bei jeder Kollision fällt zwingend mind. einer von Q1/Q4 aus — ein Einwand, den Halbieren nicht beantwortet. Am 14.09. bestätigt (1/4 ROT: Q1✗, Q2✗, Q4✗, nur Q3✓).

Bereits vorhandene Teilregelung (nicht vollständig): [[feedback_chartanalyse]] 8b Retest-Zeitbox — nach PASS+Q-ROT läuft die Zeitbox (2 VC), bei Verfall geht es weiter zu "13.1-Prüfung oder Auslassen". Das erzeugt einen Zirkelschluss (K1): der 13.1-Vorschlag löst erneut `gate_check.cjs --chasing yes` aus, der Q-Score bleibt unverändert ROT.

## Die 4 Optionen
- **A — Q-ROT hat Vorrang:** 13.1-Konsequenz max. "beobachten" bei Q-ROT. Solo-Default: kein Einstieg. Pro: konservativ, deckt sich mit 14.09.-Praxis. Contra: macht ROT faktisch zum Veto im Chasing-Teilraum, widerspricht Scharfschaltungsstufe, kein Datengewinn für die 15-Trade-Auswertung.
- **B — 13.1 hat Vorrang:** Vorschlag wird auch bei Q-ROT aktiv formuliert. Solo-Default: 50%-Einstieg. Pro: respektiert Wortlaut ("zwingend" schlägt "empfohlen"). Contra: riskantester Solo-Default, n=1-Regel bekäme Vorrang vor anderer n=1-Regel, Halbierung beantwortet Q1/Q4 strukturell nicht.
- **C — Gleichrangig mit Pflicht-Kombizeile:** Beide Aussagen textlich ausgegeben. Ohne expliziten Solo-Default unvollständig (wird sonst implizit zu A, da Pflicht-Rückfrage im Solo-Loop = faktisches Veto). Opus-Empfehlung: als Ausgabeformat in A oder D integrieren, nicht als eigenständige Option.
- **D — begründete Präzedenz (Opus-Vorschlag):** "Ein Sizing-Mechanismus kann keinen Einstiegs-Einwand beantworten, der nicht aus dem Sizing stammt." Trägt Q1 oder Q4 das ROT (praktisch immer) → Q-ROT geht vor, 13.1 max. "beobachten". Trägt nur Q2 (+Q3) das ROT (heute strukturell fast unerreichbar) → 13.1 greift regulär. Plus Pflicht-Kombizeile + Schattenmessung in `skipped_setups_fiktiv.jsonl` (Marker `kollision_13_1_qrot`, Auswertung nach ~10 Kollisionsmomenten). Solo-Default: kein Einstieg, aber vollständig dokumentiert.

## Teilfrage b (k/2-Zähler-Start)
Drei Lesarten b1 (Zähler startet erst mit vollständigem Dual-Gate) / b2 (Zähler startet mit reinen Chasing-Kriterien, Code rechnet heute so) / b3 (Mischform). **Opus-Empfehlung: b1** — stärkere Textstütze (8b Schritt 4 Nr. 4 verlangt Dual-Gate "weiterhin sauber" während der 2 Checks), stellt die Sequenz wieder her (Kollision K2 entfällt im Normalfall), verhindert Nullzeit-Beobachtung. Gegeneinwand: b1 macht 13.1 seltener (lief 03.09. deshalb 84 VC lang nie), löst K2 aber nicht vollständig (bei spät eintreffendem PASS+Q-ROT kann k längst ≥2 sein, z.B. wie am 10.09. mit k bis 9/2).

## Opus-Gesamtempfehlung
**Option D** (mit A als legitime Kurzfassung ohne Schattenmessung) + **b1**. Kein Blocker für beaufsichtigten Testtag (Levi entscheidet ohnehin im Einzelfall). **Wohl aber vor dem nächsten unbeaufsichtigten Solo-Abschnitt zu entscheiden.** Bei Wahl von Option B: nicht als reine Rangordnung, sondern mit Solo-Loop-Einschränkung — von einem unbeschränkten B rät Opus aktiv ab.

## Historische Nachrechnung (09.09./10.09.)
Keine weiteren Kollisionsfälle gefunden (09.09.: PASS+Q-ROT immer ohne Chasing; 10.09.: einziger `--chasing yes`-Live-Aufruf hatte GESAMTSTATUS FAIL, Q-Score "nicht relevant"). Die Regel wird also aus den Regeltexten + dem einzigen 14.09.-Fall abgeleitet, nicht aus einer Serie — entsprechend als Default-Festlegung mit Revisionsklausel zu verstehen, nicht als kalibrierte Regel.

## Offene Folgefragen (falls entschieden wird, für Fable-Auftrag)
1. Gilt der Vorrang auch bei Q-Score UNBEKANNT (N-4-Zustand)? Opus-Vorschlag: ja, wie ROT behandeln.
2. Zählt eine unter B/D2 genommene Q-ROT-Position als Freigabe im 15-Trade-Prüfkriterium (7b1a Option A)? Nenner-Definition offen.
3. Hängt zusammen mit der noch offenen n=2-Frage in [[project_rueckblick_handelstag_2026-09-14_1530-2200]] (Option-A-Datenpunkte bewusst nicht eingetragen, Levi entscheidet).
4. Zählerdifferenz 09.09. (3 gefundene Trigger vs. "4 Gate-PASS" im Regeltext) beim nächsten Durchgang abgleichen.

## Was Opus bewusst NICHT vorschlägt
Q-Score zum harten Gate machen (Scharfschaltungsstufe vorwegnehmen), 13.1-Schwelle ohne echte Fälle ändern, Retest-Zeitbox-Mechanik selbst ändern, 14.09.-Ausgang (+1,4R) als Entscheidungsargument werten (Hindsight-Warnung), DAX mitentscheiden.

**Status: Vorlage steht, Levi-Entscheidung ausstehend.**
