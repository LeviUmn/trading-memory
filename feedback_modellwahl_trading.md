---
name: feedback-modellwahl-trading
description: "Rollenteilung seit 24.08.2026: Sonnet 5 NUR Live-Trading-Ausführung, Fable 5 Regelwerk/Tagesabschluss/Analyse UND Skripte/Code, Opus 5 nur punktueller Meilenstein-Check. Seit 27.08.2026: Fable nicht mehr im Tarif enthalten, läuft aber über aufgeladenes Nutzungsguthaben ('Draws from usage credits')"
metadata:
  type: feedback
  originSessionId: session-2026-07-27
  modified: 2026-08-27T08:53:23.591Z
---

## STAND 27.08.2026: Fable-Verfügbarkeit — Deprecation-Risiko eingetreten, aber per Nutzungsguthaben gelöst

Levi meldete am 27.08.2026 zunächst, dass Fable 5 für seinen Tarif nicht mehr regulär erreichbar ist. Genau das war seit 27.07.2026 als Risiko vermerkt (siehe "Offener Punkt" unten). Levi hat daraufhin Nutzungsguthaben aufgeladen — `/model` zeigt seitdem "Set model to Fable 5 ... · Draws from usage credits", d.h. Fable läuft weiter, zieht aber pro Nutzung von bezahltem Guthaben statt vom regulären Tarifkontingent. Fables Rolle (Regelwerk-Autorschaft, Tagesabschluss, laufende Analyse, Skripte/Code) bleibt damit vorerst unverändert bestehen — die Rollenteilung unten gilt weiter.

**Zu beachten künftig:** Da Fable jetzt Nutzungsguthaben verbraucht (Kosten), bei größeren/laufenden Fable-Aufträgen ggf. kurz gegenchecken, ob Levi das im Blick behalten will (z.B. bei sehr umfangreichen Paketen). Kein Hard-Gate, nur Kostenbewusstsein.

---

## Modellwahl-Regel: Rollenteilung Sonnet/Fable/Opus (aktualisiert 24.08.2026, seit 27.08.2026 überholt s.o.)

**Regel (Stand 24.08.2026, ersetzt die Fassung vom 27.07.2026 unten):**
- **Sonnet 5:** NUR noch Live-Trading-Ausführung — der 1-Minuten-Loop (Quick-Tick UND 5-Minuten-Voll-Check), Order-Entscheidungen in Echtzeit. Kein Regelwerk, keine Tagesabschlüsse, keine Skripte mehr.
- **Fable 5 (ich):** Regelwerk-Autorschaft und -Änderungen, Tagesabschluss (Narrativ, Einordnung, Regime-Diagnose), laufende Fehler-/Tradinganalyse, UND jetzt explizit auch Skripte/Code (`scripts/*.cjs` etc.) — bisher lag Skript-Arbeit bei niemandem fest zugeordnet, das ist jetzt geschlossen.
- **Opus 5:** kein fester, laufender Prozessbestandteil (nicht täglich/wöchentlich) — nur punktueller Cross-Check bei Phasenübergängen (z.B. #50-Review), größeren Regelreformen, oder als Quartals-Audit.

**Why:** Autor und Prüfer dürfen nicht dasselbe Modell sein, sonst entsteht ein Bestätigungsfehler-Risiko — demonstriert am 24.08.2026 an Fables eigenem Prüffehler bei Trade #38/#43 (SL/ATR-Mindestdistanz nicht nachgerechnet, RR-Gate fälschlich als ausreichend gewertet, obwohl SL nur ~1,18x ATR statt des Schock-Tier-Minimums 2,5-3x ATR betrug). Der frühere Zustand (Fable prüfte eigene/Sonnet-Trades nachträglich, ohne unabhängige Gegeninstanz) ließ genau diesen Fehlertyp zweimal unentdeckt durchrutschen. Zusätzlich: zahlenbasierte Pflichtzeilen (SL/ATR-Ratio, RR-Gate, TP-Realismus) sollen künftig nicht mehr von einem Modell "im Kopf" geprüft werden, sondern automatisiert über `scripts/gate_check.cjs` (siehe [[project_opus_vollpruefung_2026-08-24]] Phase 2). Auslöser der gesamten Neuordnung: der gemeinsame Opus+Fable-Vollcheck vom 24.08.2026, siehe [[project_opus_vollpruefung_2026-08-24]].

**How to apply:** Bei jeder Anfrage zu Regelwerk-Änderungen, Tagesabschluss, Skript-Arbeit (Bugfixes, neue Tools wie `gate_check.cjs`) oder größeren Trading-Analysen künftig Fable einsetzen, nicht Sonnet. Sonnet bleibt ausschließlich für den operativen Live-Loop (Quick-Tick + 5-Min-Voll-Check). Opus wird nur gezielt angefragt (Meilenstein/Reform/Quartal), nicht routinemäßig.

**Offener Punkt — Fable-Verfügbarkeit (unverändert seit 27.07.2026):** Fable 5 hat weiterhin kein angekündigtes Deprecation-Datum. Sollte Anthropic eine Fable-5-Deprecation ankündigen, muss diese Regel neu bewertet werden — bei jedem größeren Modell-Review (z.B. nächster Phasenübergang) kurz gegenchecken, ob eine Deprecation-Ankündigung für Fable oder Sonnet 5 vorliegt.

---

## Vorherige Fassung (27.07.2026 — 23.08.2026, durch obige Regel ersetzt)

**Regel (alt):** Sonnet 5 blieb Engine für alles zeitkritische im aktiven Trading (1-Minuten-Loop: sowohl Quick-Tick als auch der 5-Minuten-Voll-Check, siehe [[feedback_loop_ablauf_uebersicht]]). Fable wurde **immer** für Analyse, Reviews und Tagesabschlüsse hinzugezogen — unabhängig davon, ob es zeitkritisch wäre oder nicht. Skripte/Code hatten keine feste Modell-Zuordnung.

**Why (alt):** Am 27.07.2026 kam die Frage auf, ob Opus 5 (neu released 24.07.2026, laut Anthropic nahe an Fable-Performance bei halbem Preis) oder Fable 5 besser als Trading-Engine geeignet wären. Ein per Agent-Tool hinzugezogenes Fable-Review empfahl ursprünglich eine Aufteilung nach Zeitkritikalität (Sonnet/Opus für den Loop inkl. Voll-Check, Fable nur für nicht-zeitkritische Reviews) — u.a. weil Fable als Flaggschiff-Modell vermutlich mehr interne Denkzeit pro Tool-Call-Kette braucht (unbestätigt, keine echten Benchmarks verfügbar) und das bei zeitkritischen Entry-/Exit-Entscheidungen zum Verpassen von Gelegenheiten führen könnte. Levi hat sich bewusst für eine einfachere, klare Trennung entschieden: Sonnet für ALLES aktive Trading (auch den 5-Min-Voll-Check), Fable ausschließlich für nachträgliche Analyse/Reviews/Tagesabschluss.

**Warum ersetzt:** Diese Fassung ließ Fable sowohl Regelwerk-Autor als auch einzige Prüfinstanz der eigenen/Sonnet-Trades sein — genau das Bestätigungsfehler-Risiko, das die neue Fassung oben adressiert (siehe Why oben, [[project_opus_vollpruefung_2026-08-24]]).
