---
name: project-fable-finalentscheidung-2026-09-03
description: "Fable-Finalentscheidung zu den 5 Regelwerk-Fragen (#3, #11, #12, #21, #22) nach dem Opus-Gegencheck: #3 auf Opus-Option D gewechselt, #11 gekoppelt entschieden (Richtungsfilter als Zwischenlösung + K3-neu als Schattenmessung), #12 = B mit allen drei Opus-Bestandteilen + Neufassung Abbruch-Kriterien (c)/(d), #21 = A mit Doppelrollen-Formulierung und optionalen --tag-hoch/--tag-tief, #22 = A mit korrigierter Toleranztabelle (12/32/122, Bezugspunkt Kerzen-Schluss) + Registeralter hart 90 Min. Bei 4 von 5 Fragen von der ursprünglichen Fable-Empfehlung abgewichen. Umgesetzt, nichts committet."
metadata:
  node_type: memory
  type: project
  status: draft
  originSessionId: e1a9edc1-4a4d-4c86-b03e-98d36d93627e
  modified: 2026-09-03T11:26:28.565Z
---

# Fable-Finalentscheidung zu den 5 Regelwerk-Fragen (03.09.2026)

**Auftrag (Levi):** Beide Sichtweisen ([[project_fable_umsetzung_2026-09-03]] Abschnitt "REGELWERK-FRAGE" und [[project_opus_gegencheck_fable_empfehlungen_2026-09-03]]) gegeneinander abwägen, final ENTSCHEIDEN und umsetzen. Entscheidungsbefugnis für diese 5 Punkte war übertragen ("mit dem Wissen von Opus und Fable umsetzen lassen").

**Vorab selbst nachgeprüft (nicht nur übernommen):**
- **Opus' Ratschen-Rechnung (Abschnitt 0): bestätigt.** K1 (nicht-fallende Tagesrange ÷ konstanter Session-ATR-D) und die alte K3-Formel ((Hoch−Tief)/Tief: Zähler nicht-fallend, Nenner nicht-steigend) sind beide intraday monoton nicht-fallend. Mein früheres Gegenargument "K1 bildet nachlassende Wirkung ab" war rechnerisch falsch — mit K2-Latch wären alle drei Kriterien Einbahnstraßen.
- **Opus' K3-Diagnose "kein absoluter Anker": bestätigt.** 5 % Range bei VIX 15 = 0,75 Punkte = Normalverhalten; an 2/2 Testtagen erfüllt, beide ohne Vola-Schock.
- **Opus' #22-Korrektur: bestätigt.** Meine 62-Min-Grenze für 1H legte den Bezugspunkt (Open vs. Schluss) nicht fest; der eigene VC#1-Beleg (15:00:20, Referenz = 13:00-Kerze) reißt sie in der Open-Lesart (120 Min) sofort. Mit Bezugspunkt Schlusszeit ist das `count:2`-Normalfall-Maximum knapp unter 1 Kerzenlänge, mit Rollover-Verzögerung bis knapp unter 2 — die 2L+2-Grenzen (12/32/122) sind im Normalbetrieb konstruktiv unerreichbar und tragen deshalb als Defekt-Indikator.

**Bilanz: Bei 4 von 5 Fragen von meiner ursprünglichen Empfehlung abgewichen** (#3 ganz, #11 teilweise Richtung Opus, #12 erheblich ergänzt, #22 Zahlen korrigiert); bei #21 blieb der Kern (Option A), aber Formulierung und Datenquelle wurden nach Opus' Präzisierungen geändert.

---

## #3 — 8d-Kriterium 2 (Makro-Häufung): **Option D (Opus), nicht mein ursprüngliches C**

**Entscheidung:** K2 erfüllt bei ≥2 Ereignissen am selben Handelstag, die auf der Standard-Blackout-Liste stehen UND messbar vom Konsens abgewichen sind (Ist/Konsens im Voll-Check dokumentiert, Marktreaktion benannt, bewusst keine Prozentschwelle). K2 ist ein Zustand (Latch bis Handelsschluss). Der 1–2-Tage-Rückblick des alten Wortlauts entfällt ausdrücklich. Konsenswert nicht verfügbar → "nicht gemessen", nie ✓.

**Warum Abweichung von C:** Opus' Einwand trägt: Mein C hätte "Datenüberraschung (Konsens-Abweichung)" stillschweigend durch "Termin auf der Blackout-Liste" ersetzt — eine unausgewiesene Semantik-Änderung der geltenden Regel, und zugleich der Haupttreiber des Kalenderfilter-Effekts (zwei Termine sind häufig, zwei echte Abweichungen selten). D behält die an Tag 2 bewiesene Auditierbarkeit (geschlossene Blackout-Liste) UND den Kern der geschriebenen Regel (Überraschung). Die Latch-Entscheidung selbst bleibt — aber mit Opus' Begründung (kein vierter unkalibrierter Parameter durch ein Verfallsfenster), nicht meiner widerlegten.

**Änderung:** [[feedback_chartanalyse]] 8d, Punkt 2 neu gefasst (alter Wortlaut als ~~durchgestrichen~~ zitiert); Event-Ausweis in der 8d-Pflichtzeile ([[feedback_vollcheck_format]]).

## #11 — 8d-Kriterium 3 (VIX): **gekoppelt mit #3 — Richtungsfilter (nur 1. Klausel) als Zwischenlösung + Option C als Schattenmessung**

**Entscheidung (zweistufig):**
1. **Gate-wirksam ab sofort:** Bestehende Formel bleibt, zählt aber NUR bei aktuellem VIX ≥ Vortagesschluss. Damit hat 8d unter dem K2-Latch genau einen rückstellbaren Freiheitsgrad (der strukturelle Mindestbedarf aus der Ratschen-Rechnung) und der belegte 02.09.-Fehlmodus (Feuern auf fallenden VIX) ist behoben. Die zweite Klausel meines ursprünglichen A ("näher am Hoch als am Tief") entfällt — Opus' Flacker-Nachweis an der bedeutungslosen Mittelwert-Kante trägt. Ausdrücklich Zwischenlösung mit Verfallsdatum, nicht Endzustand.
2. **Schattenmessung (GEMESSEN, KEIN GATE), Hausmuster Paket 6:** K3-neu = VIX ≥ 20 ODER Tagesveränderung ggü. Vortagesschluss ≥ +10 % läuft parallel in der 8d-Pflichtzeile mit. **Entscheidungsregel: nach ~15 Handelstagen Datenlage (oder beim nächsten Opus-Meilenstein-Review, was zuerst eintritt) entscheidet Levi über den Wechsel.**

**Warum nicht sofort hart auf C (Opus' Präferenz):** Opus benennt selbst, dass C die Fehlerrichtung von "zu oft gesperrt" auf "seltener gesperrt" dreht — bei Echtgeld-Ambition die riskantere Richtung — und schlägt die Schattenmessung als Einführungsweg ausdrücklich vor. Bei n=2 Datentagen ist der risikofreie Weg (alte Fehlerrichtung = konservativ, kostet nur potenzielle Trades) der richtige; die Zwischenlösung nimmt derweil den nachgewiesenen Defekt raus. Das ist exakt Opus' eigener Fallback ("A als Zwischenlösung, nur erste Klausel") — kein Kompromiss gegen Opus, sondern dessen zweiter Zweig.

**Änderung:** [[feedback_chartanalyse]] 8d Punkt 3 + neuer Block "8d-Ratschen-Fix" (Kopplungsbegründung, Schattenformel, Entscheidungsregel, erweiterte Pflichtzeile); [[feedback_vollcheck_format]] 8d-Template-Zeile. Reine Regelwerk-/Protokolländerung — `gate_check.cjs` prüft 8d konstruktionsbedingt nicht (N-15), kein Code nötig.

## #12 — Abbruch-Gate: **Option B + alle drei Opus-Bestandteile + Neufassung (c)/(d) — der dringliche Punkt**

**Entscheidung:** Prüfer-Feld `Abbruch-Empfehlung: KEIN EINWAND / EINGESCHRÄNKT (<Mechanismus>) / ABBRUCH (<Begründung>)` als Pflichtfeld jeder Vortagesanalyse, vom Gate 1:1 übernommen. Ergänzt um Opus' drei fehlende Bestandteile: **(1) Fail-Closed** (Feld fehlt/unklar/Analyse fehlt → wie ABBRUCH), **(2) Prüfer-Auftrag im selben Zug geändert** (sonst produziert der nächste Prüfer das Feld auftragswidrig nicht), **(3) dreiwertig statt binär** (EINGESCHRÄNKT = Tag normal, aber der defekte Mechanismus muss vor dem ersten Order-Ablauf nachweislich funktionieren — gegen den Alarmanlagen-Effekt). Zusätzlich Opus-Option D übernommen: (e) gilt nur für Testtage OHNE frisches Levi-Go (der Mensch ist das Gate für Tag 1, die Maschine für Cron-Folgetage). Meine ursprüngliche B-Begründung ("Loop kann Analyse nicht interpretieren") war falsch verortet — es ist ein Ermessens-, kein Fähigkeitsproblem; im Dokument korrigiert.

**Neufassung (c)/(d) — Wortlaut siehe [[project_testtag_plan_01_02_09_2026]] Abschnitt "ABBRUCH-GATE v2", Kern:**
- **(c):** greift nur bei UNKNOWN aus **strukturellem Verifikationsfehler** (korrekt registriertes Level vom Abgleich nicht getroffen). Klarstellung (i): UNKNOWN aus fehlender/mit `--grund-*` ersetzter Messfeld-Angabe = Dokumentationsmangel, zählt NICHT (sonst würde ein vergessener `--cluster-level` an einem 2/2-Moment den Folgetag in den Beobachtungsmodus schicken).
- **(d):** von "Ursache 3" verallgemeinert auf **registerbezogene Exit-1-Ursachen** (fehlt/unlesbar, Zukunfts-Zeitstempel, >90-Min-Obergrenze). Klarstellung (ii): ein nach der EXIT-CODE-1-KLAUSEL sofort korrigierter und im selben Trigger-Moment erfolgreich wiederholter Exit 1 zählt NICHT (nur Bilanzzeile). Klarstellung (iii): nicht-registerbezogene Exit-1-Fälle (A3-Messfeld, Preisformat) zählen generell nicht.

**Änderung:** [[project_testtag_plan_01_02_09_2026]], neuer Abschnitt "ABBRUCH-GATE v2" (alte Tag-2-Fassung als historisch markiert), inkl. Prüfer-Auftrags-Satz und Register-UTC-Konvention für künftige Testtage.

## #21 — Register Herkunft vs. Tragfähigkeit: **Option A, mit Opus' beiden Präzisierungen**

**Entscheidung:** Kennzeichnen, kein Gate (wie ursprünglich) — aber (1) **Doppelrollen-Formulierung** statt Warnton: durchhandelt = als Zielmarke erreichbarkeitsbelegt / als Haltezone geschwächt (verhindert Vermeidungshaltung gegenüber legitimen Retest-/Magnet-Setups; zusätzliches Argument übernommen: am Anlassfall 02.09. hätte auch ein Gate nichts geändert, der Trade scheiterte ohnehin an 8c2/8b); (2) **Datenquelle nicht die Registerfelder** (nachweislich inkonsistent erhoben: Hoch ohne, Tief mit Vorbörse; Tag-1-Wert inhaltlich falsch bei frischem Stempel), sondern neue Parameter `--tag-hoch`/`--tag-tief` aus derselben `quote_get`-Ablesung wie 8d-K1. Von Opus' zwei Varianten die **optionale** gewählt (nicht A3-pflichtig): bewusst gegen weiteres Wachstum der Exit-1-Pfade direkt vor dem nächsten Testtag — passt zum Anzeige-Charakter, und Opus benennt genau diesen Weg als den risikoärmeren; fehlen die Werte, sagt die Ausgabe sichtbar "nicht geprüft".

**Änderung:** `scripts/gate_check.cjs` (neue optionale Parameter, `TP-Tragfaehigkeit`-Zeilen für TP1 und TP2, nur live, getestet: alle 4 Pfade — innerhalb/außerhalb Range, fehlend, vertauscht, Batch unberührt); [[feedback_chartanalyse]] neuer Absatz "TP-Tragfähigkeits-Anzeige" vor 8d; [[feedback_live_trading]] 7b1: beide Kommandotemplates + Parameter-Bullet.

## #22 — Frischefenster + Registeralter: **Option A mit Opus' korrigierten Zahlen + 90 Min hart**

**Entscheidung Teil 1 (Kerzenfrische):** Ausweisen, kein Gate (wie ursprünglich; Opus' schärferes Argument übernommen: ein Gate ohne mögliche Abhilfe ist ein Zufallsgenerator). Aber mit korrigierter Tabelle — **mein 1L+2-Vorschlag (7/17/62) war falsch**, weil der Bezugspunkt fehlte und die `count:2`-Rollover-Mechanik das Alter bis knapp unter 2 Kerzenlängen treibt: **Bezugspunkt = Kerzen-SCHLUSS, Schwellen 2L+2 = 5min ≤12 / 15min ≤32 / 1H ≤122 Min** (im Normalbetrieb konstruktiv unerreichbar → Überschreitung = echter Feed-Defekt, die einzige Aussage, die so eine Schwelle tragen kann). Plus Opus' wichtigster Zusatz: **Spreizungs-Ausweis** (max. Altersdifferenz der Ebenen — der VC#13-Fall, den keine Einzelschwelle erfasst). Neue Pflichtzeile `MTF-Frische:` im Voll-Check-Template inkl. Format-Zeilen-✓.

**Entscheidung Teil 2 (Registeralter):** **Hart 90 Minuten** (wie ursprünglich vorgeschlagen, von Opus gestützt; ersetzt das 1440-Provisorium). Mit übernommenen Zusätzen: `updated`-Konvention explizit (echte UTC des ABGESCHLOSSENEN Voll-Checks; fiktive Testtage zwingend echte UTC — sonst Hard-Exit bei jedem Aufruf, jetzt angekündigt statt teuer überraschend). Opus' pro-Level-`updated`-Idee **bewusst NICHT übernommen**: Die dokumentierte D-Entscheidung vom 31.08. dagegen bleibt bestehen (für die statischen Pflicht-Levelarten Dauer-Rauschen; ein Zeitstempel ist nur so ehrlich wie sein Schreiber — der Tag-1-Fehler war ein nicht nachgeführter WERT, den auch ein pflichtgemäß mitgeschriebener Stempel nur fängt, wenn der Schreiber ehrlich stempelt, also dieselbe Fehlerquelle); die `--tag-hoch`/`--tag-tief`-Anzeige aus #21 liefert für die kritischste driftende Kategorie (Tages-Extrema) bereits eine unabhängige Frischquelle. Revisit-Trigger (erneuter realer Stale-Level-Treffer) bleibt dokumentiert.

**Änderung:** `scripts/gate_check.cjs` (Konstante 1440→90 + Begründungskommentar + Fehlermeldung; getestet: 890-Min-Register → Exit 1); [[feedback_vollcheck_format]] (MTF-Frische-Zeile + Abschnitt); [[feedback_loop_ablauf_uebersicht]] (Anschlussregel an die #15-Timestamps); [[feedback_session_update]] Schritt 6 (`updated`-Konvention).

---

## Geänderte Dateien (für Levis Sichtung — NICHTS committet, kein MEMORY.md-Eintrag)

Repo: `scripts/gate_check.cjs` (#21 Anzeige, #22 90-Min-Grenze; 5 Testpfade grün, Batch-Semantik unverändert).
Regelwerk (`memory/`): `feedback_chartanalyse.md` (8d K2/K3 + Ratschen-Fix-Block + TP-Tragfähigkeits-Absatz), `project_testtag_plan_01_02_09_2026.md` (ABBRUCH-GATE v2), `feedback_vollcheck_format.md` (MTF-Frische, 8d-Template, Format-Zeile), `feedback_session_update.md` (updated-Konvention), `feedback_live_trading.md` (7b1-Templates + --tag-hoch/--tag-tief-Bullet), `feedback_loop_ablauf_uebersicht.md` (Frische-Anschlussregel).

**Offen für Levi (keine Blocker):** (1) Opus' empfohlene 20-Tage-Auszählung (Anteil struktureller No-Trade-Tage unter der neuen 8d-Fassung) — halbe Stunde, härtet die #3/#11-Kalibrierung; (2) Wahl VIX vs. VXN bzw. Schwelle 20 vs. 18 für K3-neu ist Feinjustierung während der Schattenphase; (3) die zwei kosmetischen Codepunkte aus Opus Abschnitt 6 (Q2-Toleranz 1pp→0,1pp, irreführende Meldung bei nicht-numerischem `--cluster-level`) — bewusst nicht angefasst (kein Handlungsdruck laut Opus, Minimal-Invasivität vor Testtag). *→ Alle drei auf Levis Anweisung am 03.09.2026 nachmittags umgesetzt, siehe Nachtrag unten.*

---

## Nachtrag (03.09.2026, Levi-Go): die drei zurückgestellten Punkte, jetzt umgesetzt

### 1. Die 20-Tage-Auszählung (empirische Kalibrierung #3/#11)

**Datenbasis:** VIX-Tagesbars real über das TradingView-MCP-Tool geholt (`data_get_ohlcv`, CBOE:VIX bzw. aufgelöst CBOE_DLY:VIX, Timeframe D; der formende 03.09.-Bar ausgeschlossen). Fenster: die letzten 20 abgeschlossenen Handelstage **06.08.–02.09.2026**. Blackout-Termine rekonstruiert aus den projektinternen Protokollen (Standard-Blackout-Liste = [[feedback_session_update]] "Market-Moving Events": PCE/Core PCE, CPI/Core CPI, PPI, NFP, Jobless Claims, ADP, FOMC-Protokolle/Fed-Entscheidungen/-Reden, GDP — ISM/JOLTs/CB Confidence/Michigan/Philly Fed/S&P-PMIs zählen per 28.07.-Korrektur ausdrücklich NICHT).

| Tag | VIX H / L | Range (H−L)/L | ≥5 %? | Blackout-Termine (Liste) | ≥2? | Beide? |
|---|---|---|---|---|---|---|
| Do 06.08. | 16,03 / 15,11 | 6,09 % | ✓ | Jobless Claims (1) | ✗ | ✗ |
| Fr 07.08. | 15,36 / 14,77 | 3,99 % | ✗ | NFP (1) | ✗ | ✗ |
| Mo 10.08. | 15,72 / 15,10 | 4,11 % | ✗ | — | ✗ | ✗ |
| Di 11.08. | 15,61 / 15,23 | 2,50 % | ✗ | — | ✗ | ✗ |
| Mi 12.08. | 15,42 / 14,39 | 7,16 % | ✓ | CPI vermutet, UNBELEGT (0–1) | ✗ | ✗ |
| Do 13.08. | 14,80 / 14,39 | 2,85 % | ✗ | **PPI + Jobless Claims (2)** ✦ | **✓** | ✗ |
| Fr 14.08. | 14,72 / 14,18 | 3,81 % | ✗ | — (Michigan zählt nicht) ✦ | ✗ | ✗ |
| Mo 17.08. | 15,47 / 14,89 | 3,90 % | ✗ | — ✦ | ✗ | ✗ |
| Di 18.08. | 16,09 / 15,60 | 3,14 % | ✗ | — (Housing etc.) ✦ | ✗ | ✗ |
| Mi 19.08. | 15,95 / 14,77 | 7,99 % | ✓ | FOMC-Minutes (1) ✦ | ✗ | ✗ |
| Do 20.08. | 16,14 / 14,91 | 8,25 % | ✓ | Jobless Claims (1; Philly Fed zählt nicht) ✦ | ✗ | ✗ |
| Fr 21.08. | 15,88 / 15,08 | 5,31 % | ✓ | — (nur S&P-PMIs) ✦ | ✗ | ✗ |
| Mo 24.08. | 16,06 / 15,61 | 2,88 % | ✗ | — ✦ | ✗ | ✗ |
| Di 25.08. | 16,30 / 15,13 | 7,73 % | ✓ | — ✦ | ✗ | ✗ |
| Mi 26.08. | 15,74 / 15,21 | 3,48 % | ✗ | **GDP 2nd Est + PCE Price Index (2)** ✦ | **✓** | ✗ |
| Do 27.08. | 15,13 / 14,42 | 4,92 % | ✗ (knapp) | Jobless Claims (1) ✦ | ✗ | ✗ |
| Fr 28.08. | 14,84 / 14,13 | 5,02 % | ✓ (knapp) | Fed-Chair-Rede Warsh (1; Chicago PMI zählt nicht) ✦ | ✗ | ✗ |
| Mo 31.08. | 15,48 / 14,86 | 4,17 % | ✗ | — | ✗ | ✗ |
| Di 01.09. | 16,80 / 14,95 | 12,37 % | ✓ | — (ISM Mfg üblich, zählt nicht; rekonstruiert) | ✗ | ✗ |
| Mi 02.09. | 16,82 / 15,12 | 11,24 % | ✓ | ADP (1) ✦ | ✗ | ✗ |

✦ = Termin(e) direkt Memory-verifiziert ([[project_makrodaten_2026-08-13_bis_14]], testtag-Dateien 24.–28.08., [[project_testtag_analyse_2026-09-02]]). Validierung der VIX-Daten: der 02.09.-Wert 11,24 % deckt sich exakt mit dem Testtag-Protokoll (VC-Endstand), der 01.09./02.09.-Verlauf mit der Opus-Analyse.

**Ergebnis:**
- (a) Tage mit ≥2 Standard-Blackout-Terminen: **2/20 = 10 %** (13.08., 26.08.)
- (b) Tage mit VIX-Intraday-Range ≥5 %: **9/20 = 45 %**
- **Produkt (Opus-Formel, Unabhängigkeitsannahme): ≈ 4,5 %**
- **Direkte Koinzidenz (bessere Schätzung, da beide Reihen vorliegen): 0/20 = 0 %** — an beiden ≥2-Termine-Tagen lag die VIX-Range UNTER 5 % (2,85 % / 3,48 %)

**Einordnung:** Deutlich unter Opus' Tragbarkeitsgrenze von ~30 %. Die alte (jetzt korrigierte) 8d-Fassung hätte im Fenster also KEINEN Tag komplett gesperrt — der Kalenderfilter-Effekt ist real (10 % ≥2-Termine-Tage), aber die Koinzidenz mit K3-alt war in diesem ruhigen Fenster null. Zwei Detailbefunde: (i) Opus' Prämisse "K3-alt ist praktisch dauerhaft erfüllt" relativiert sich auf 45 % — hoch für ein Schock-Kriterium ohne einen einzigen echten Vola-Schock im Fenster, aber nicht "immer"; (ii) die zwei mit Abstand größten Range-Tage waren die beiden Testtage 01./02.09. (12,4 %/11,2 %), beide ohne Vola-Schock — bestätigt die Dimensionsfehler-Diagnose unabhängig. Zusatz: K3-neu (VIX ≥20 ODER Δ ≥+10 %) hätte an **1/20 Tagen gefeuert (01.09.2026, +12,60 % intraday: Tageshoch 16,80 vs. Vortagesschluss 14,92)** — korrigiert 03.09.2026 nach Opus-Vollaudit B-3: ~~0/20, größtes Δ +9,5 % am 01.09., knapp unter der Schwelle~~ war Schluss-zu-Schluss gerechnet (15,20→16,34 = +9,52 %), K3 ist aber ein INTRADAY-Kriterium (bei jedem Voll-Check gegen den aktuellen VIX-Stand geprüft), maßgeblich ist also Tageshoch vs. Vortagesschluss. Die VIX-≥-20-Klausel bleibt 0/20 (Tageshoch-Max 16,82). Der einzige Treffer ist genau der nervöseste Tag des Fensters und kein anderer — ein gutes Zeichen für die Formel, sogar stärker als die alte "knapp verfehlt"-Lesart.

**Unsicherheiten (das ist eine Schätzung mit Kalenderrekonstruktion, kein Backtest):** (1) Der August-CPI-Termin (~11./12.08.) ist in keinem Protokoll belegt — als Mittwochs-Solotermin änderte er nichts an (a); selbst im unwahrscheinlichsten Fall (Zusammenfallen mit einem Claims-Donnerstag) stiege (a) auf 3/20 = 15 %, Produkt ≈ 6,75 %. (2) Die VIX-Tagesbars enthalten erweiterte Handelszeiten (Bar-Beginn 09:15 DE) — die Session-Messung im Loop (ab 15:00) sieht tendenziell eine kleinere Range, 45 % ist also eine Obergrenze für (b). (3) Die Produktformel unterstellt Unabhängigkeit von Kalender und Vola — die direkte Auszählung (0/20) ist die ehrlichere Zahl, bei n=20 aber statistisch dünn. (4) "≥2 Termine" zählt Release-Bündel als EINEN Termin (NFP+Unemployment = 1, konsistent mit der Tag-2-Praxis "ADP = genau ein Blackout-Termin"). (5) Unter der NEUEN K2-Fassung (D: Termin UND Konsens-Abweichung) wäre (a) höchstens gleich, eher kleiner — der 13.08. qualifizierte auch unter D (PPI-Miss + Claims 209K vs. 202K beide dokumentiert abgewichen), für den 26.08. sind die Ist/Konsens-Werte nicht protokolliert.

### 2. VIX vs. VXN und Schwelle 20 vs. 18 für die K3-Schattenmessung: **Es bleibt bei VIX ≥ 20**

VXN ist über das MCP-Tool problemlos abrufbar (CBOE:VXN, aufgelöst CBOE_DLY:VXN — steht ohnehin im Intermarket-Dashboard 5a) und wurde über dasselbe 20-Tage-Fenster gegen VIX gestellt. **Entscheidung: Referenzgröße bleibt VIX, Schwelle bleibt 20.** Begründung (Zahlen aus dem Fenster): (1) VXN-Tageshoch lag an 20/20 Tagen ≥20 (Schluss **19/20** — nur der 28.08. schloss mit 19,92 knapp darunter; korrigiert 03.09.2026 nach Opus-Vollaudit B-6, ~~18/20~~ war um eins zu niedrig, die Korrektur stützt das Argument zusätzlich) — die konventionelle 20er-Marke ist für VXN quasi-dauerhaft gerissen, eine "VXN ≥ 20"-Klausel hätte exakt den Degenerationsfehler der alten K3-Formel; (2) eine VXN-eigene Schwelle (~28 = 1,4×20) hätte keinen externen konventionellen Anker und wäre spread-abhängig (VXN/VIX-Schlussverhältnis 1,34–1,58, Mittel 1,44 — nicht konstant) — genau die vierte unkalibrierte Zahl, deren Vermeidung Opus' Hauptargument für Option C war; (3) die anker-freie Δ-Klausel auf VXN brächte keinen Sensitivitätsgewinn: VXN-Prozentbewegungen sind wegen des höheren Basisniveaus eher GEDÄMPFT (01.09.: VXN +8,8 % vs. VIX +9,5 %); (4) 20 vs. 18: im Fenster diskriminiert keine der beiden Zahlen (VIX-Max 16,82 — beide nie erreicht), 18 wäre unverankert, und die Schattenphase soll erst die konventionell verankerte 20 gegen Daten testen statt die Messreihe mit einer zweiten ungeprüften Zahl zu starten. **Revisit-Trigger dokumentiert** (in [[feedback_chartanalyse]], 8d-Ratschen-Fix-Block): NAS100-Vola-Ereignis, das VXN zeigt, während VIX <20 bleibt und die Δ-Klausel schweigt → Referenzfrage neu bewerten. Die Schattenmessung bleibt unverändert GEMESSEN, KEIN GATE; an der Formel wurde nichts geändert — die Entscheidung schließt die Feinjustierungsfrage, statt eine neue Variante zu öffnen.

### 3. Die zwei kosmetischen Codepunkte (Opus Abschnitt 6, Anmerkungen 1+2) — umgesetzt in `scripts/gate_check.cjs`

**(1) Q2-Budget-Konsistenz-Toleranz 1 pp → 0,1 pp:** Vergleichsschwelle im Punkt-9-Block geändert, Warnung nennt jetzt die Toleranz explizit. **Getestet:** Aufruf mit `--impuls-reifegrad-atr 0.5` (Formelwert 66,667 %) und `--dual-gate-q2-budget-pct 66.2` (0,47 pp Abweichung — vorher stumm) → WARNUNG erscheint; mit `66.65` (0,017 pp) → keine Warnung. Weiterhin WARNUNG, kein Gate.

**(2) Nicht-numerisches `--cluster-level` von "fehlt" unterschieden:** Neuer Erkennungsfall `clusterLevelInvalid` (übergebener Wert, der weder Zahl noch `none`/`keine` ist) mit eigener Meldung im 8c2-UNKNOWN-Zweig. **Getestet:** `--cluster-level abc` → `SL-Cluster-Zuschlag (8c2): --cluster-level "abc" ist UNGUELTIG — weder Zahl noch "none"/"keine" (Tippfehler?) — 8c2 NICHT GEPRUEFT (UNKNOWN statt stillem PASS…)` statt der irreführenden "fehlt"-Meldung. Verhalten unverändert sicher (pass:null → UNKNOWN, kein stiller PASS); Batch-Semantik unberührt; Fall "fehlt mit `--grund-cluster-level`" liefert weiterhin die Fehlt-Meldung mit Grund.

*(Beides uncommitted, wie alles aus dieser Runde — kein git commit, kein MEMORY.md-Eintrag, gemäß Leitplanken.)*

---

## Zweiter Nachtrag (03.09.2026, Levi-Go): Audit-Nachzügler K-1/K-2/K-5/K-7/B-3/B-6

Umsetzung der sechs Nachzügler aus dem Opus-Vollaudit ([[project_opus_vollaudit_2026-09-03]], Ebene 2 + 2b). Alle minimal-invasiv, nichts committet, kein MEMORY.md-Eintrag; finaler Opus-Gegencheck vor dem Commit steht aus.

1. **K-1 (Tweet-Fälligkeits-Widerspruch):** [[feedback_vollcheck_format]] — Template-Zeile `Tweet-Check:` auf die Doppelbedingung umgestellt (Raster-Slot Minute % 10 == 0 UND Delta ≥10 Min per `x_fetch_stamp --check`; der Klammertext sagte wörtlich "NICHT Delta seit dem letzten Ist-Abruf"); im mittleren Zweig Slot `(x_fetch_stamp --check: JA/NEIN)` ergänzt; der 28.08.-Absatz "Kerzenraster-Fassung" als Historie markiert (durchgestrichen, mit Stand-03.09.-Vorsatz — identische Behandlung wie in [[feedback_loop_ablauf_uebersicht]] Schritt 6).
2. **K-2 (Terminalbedingung fehlte in der Format-Zeile):** [[feedback_vollcheck_format]] — Format-Zeilen-Template um `[| Terminalbedingung 21:45 geprüft ✓]` ergänzt (analog Order-Sperre/Halbierungsfenster) plus eigener Erklärabsatz; erfüllt jetzt die "im Fließtext UND in der Format-Zeile"-Pflicht aus [[feedback_live_trading]] Punkt 15d.
3. **K-5 (Register gitignoren):** `.gitignore` im Code-Repo — `scripts/level_register.json` bei den anderen Statusdateien (`x_last_fetch.json`, `last_gate_check.txt`, `last_cooldown_check.txt`, `trades.db`) eingetragen; Opus-Empfehlung "ignorieren" übernommen (Statusdatei, wird bei jedem Voll-Check neu geschrieben, Schema im Kopfkommentar von `gate_check.cjs` dokumentiert).
4. **K-7 (konsenslose Blackout-Termine):** [[feedback_chartanalyse]] 8d K2 — Opus' Ergänzungssatz geprüft und inhaltlich übernommen: Ereignisse ohne veröffentlichten Konsenswert (Fed-Reden, FOMC-Protokolle) qualifizieren für K2 nur bei einer im Voll-Check ausdrücklich benannten, dem Ereignis zugeordneten Marktreaktion; die dokumentierte Reaktion ersetzt dort die Konsens-Abweichung als Bedingung (b). Mit Klarstellung, dass Bedingung (a) Blackout-Liste Pflicht bleibt und die "bloße Marktreaktionen zählen nicht"-Klausel unberührt ist (Beleg-Fall: Warsh-Rede 28.08.).
5. **B-3 (K3-neu-Baseline):** Nachtrag Abschnitt 1 oben korrigiert von ~~0/20, +9,5 % knapp verfehlt~~ (Schluss-zu-Schluss) auf **1/20 (01.09.2026, +12,60 % intraday: Tageshoch 16,80 vs. Vortagesschluss 14,92)** — K3 ist ein Intraday-Kriterium. Baseline-Referenz zusätzlich im "8d-Ratschen-Fix"-Block in [[feedback_chartanalyse]] verankert, damit die 15-Tage-Auswertung gegen die richtige Ausgangszahl vergleicht.
6. **B-6 (VXN-Schluss-Quote):** Nachtrag Abschnitt 2 oben korrigiert von ~~18/20~~ auf **19/20** (nur der 28.08. schloss mit 19,92 unter 20; Opus-Gegenmessung). Entscheidung VIX statt VXN unverändert — die Korrektur stützt sie sogar stärker. Dieselbe Zahl im Ratschen-Fix-Block in [[feedback_chartanalyse]] mitkorrigiert.
