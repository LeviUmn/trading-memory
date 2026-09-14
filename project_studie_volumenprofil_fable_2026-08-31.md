---
name: project-studie-volumenprofil-fable-2026-08-31
description: "Unabhängige Fable-Studie 31.08.2026 zu drei Strategieerweiterungs-Fragen: Volumenprofil (NEIN für NAS100 jetzt — kein Volumen auf FOREXCOM:NAS100, QQQ-Level nicht 1:1 übertragbar, echter Wiedervorlage-Trigger: IBKR/MNQ-Umstieg), Indikatoren-auf-Indikatoren (NEIN — geglättete Kopien, verletzt Rohwert-Pflicht-Architektur), Renko/Kagi/Line-Break/P&F intraday (NEIN — preisgetrieben, inkompatibel mit CronCreate-Zeitraster und der gesamten ATR/Kerzenschluss-Kalibrierung). Parallel lief eine unabhängige Opus-Studie zur selben Frage."
metadata: 
  node_type: memory
  type: project
  originSessionId: e40d2c87-d383-4377-a725-334d9fa865f3
  modified: 2026-08-31T12:52:24.775Z
---

# Studie: Volumenprofil, Meta-Indikatoren und preisgetriebene Charttypen — Integration in die bestehende NAS100-Strategie? (Fable, 31.08.2026)

Unabhängige Studie auf Levis Anfrage nach Lektüre des TradingView-Hilfeartikels "Volumenprofil-Indikatoren: Grundkonzepte". Reine Entscheidungsvorlage — nichts wurde umgesetzt, kein Chart angefasst. Eine parallele Opus-Studie zur selben Frage lief unabhängig und wurde von mir bewusst nicht eingesehen.

## Kurzfazit

Alle drei Erweiterungen bekommen von mir ein **Nein für den jetzigen Zeitpunkt**, aus jeweils unterschiedlichen, konkreten Gründen. (1) Das Volumenprofil scheitert nicht erst an der Tick-Volumen-Qualitätsfrage, sondern eine Ebene früher: `FOREXCOM:NAS100` liefert auf TradingView **gar kein Volumen** (empirisch dokumentiert — der Volume-Indikator zeigt dort einen Fehler, VWAP keine Linie), ein Profil auf dem Trigger-Chart hätte also nichts zu aggregieren; auf QQQ wäre es möglich, aber QQQ-Preislevel dürfen laut eigener, bewährter Regel **nie 1:1 auf NAS100 übertragen** werden — genau das bräuchten aber 8b1/8c2. Der ehrliche Wiedervorlage-Trigger ist der ohnehin geplante IBKR/MNQ-Umstieg (Schweiz-Umzug): Futures haben echtes, vollständiges Volumen, dort wird die Frage neu und ernsthaft gestellt. (2) "Indikatoren auf Indikatoren" liefern methodisch fast ausschließlich geglättete Kopien bereits vorhandener Information und würden die mühsam erkämpfte Rohwert-Pflicht-Architektur (jede Pflichtzeile = ein tatsächlich abgelesener, benannter Einzelwert) direkt unterlaufen — Nein, mit einem sauberen Plan B, falls Levi es trotzdem will. (3) Renko/Kagi/Line-Break/P&F sind preisgetrieben und damit strukturell inkompatibel mit dem gesamten zeitgetakteten Fundament (CronCreate `Minute % 5`, bestätigter 5-Min-Kerzenschluss, ATR(14, 5min) als Einheit aller SL/TP-Gates) — als Trigger-Ersatz ausgeschlossen, und auch als Zusatzansicht rechtfertigt der Nutzen die realen Kosten (Pane-Knappheit, `tab_new` defekt, Umschalt-Störung des Live-Loops) nicht.

---

## 1. Volumenprofil: NEIN für jetzt — mit klar benanntem Wiedervorlage-Trigger

### 1a. Die Datenfrage entscheidet, bevor die Methodenfrage überhaupt drankommt

Der Artikel ordnet Index-CFDs korrekt der Kategorie "Tick-Volumen" zu (Anzahl Kursaktualisierungen, kein echtes Handelsvolumen). Für die generelle Belastbarkeit von Tick-Volumen als Proxy gibt es tatsächlich brauchbare Evidenz: Die bekannteste Untersuchung (Casper Marney, 2011, auf Basis großer Spot-FX-ECNs) fand ~90% Korrelation zwischen Tick-Aktivität und echtem aggregiertem Volumen; spätere Praxis-Analysen bestätigen 0,85–0,90 auf großen Instrumenten und Stunden-Timeframes, mit **deutlicher Degradierung auf niedrigen Timeframes und kleineren Feeds** ([MQL5-Blog](https://www.mql5.com/en/blogs/post/741311), [Global Prime](https://globalprime.medium.com/why-is-tick-volume-important-to-monitor-56a936eea70d), [Trading Wyckoff](https://tradingwyckoff.com/en/tick-volume-vs-real-volume/), [Tradeciety](https://tradeciety.com/why-fake-volumes-in-forex-can-help-you-win-using-volume-in-forex)). Für eine 5-Min-Intraday-Strategie auf einem einzelnen Broker-CFD-Feed ist das genau der schwache Bereich des Proxys: POC/HVN/LVN aus Tick-Volumen auf 5min wären eine Aktivitäts-Landkarte, keine Liquiditäts-Landkarte — brauchbar als grobe Orientierung, nicht als Level-Quelle für harte Gates.

Diese Diskussion ist für uns aber weitgehend akademisch, denn der konkrete Feed ist noch schlechter als die Artikel-Kategorie: **`FOREXCOM:NAS100` liefert auf TradingView überhaupt keine Volumendaten** — seit Juli 2026 mehrfach dokumentiert ([[feedback_chart_layout]]: Volume-Indikator zeigt roten Fehler, VWAP zeigt keine Linie, "kein temporärer Bug, tritt nach jedem Neustart wieder auf"). Genau deshalb existiert die ganze Zwei-Pane-Architektur: QQQ trägt die Volumen-Rolle. Ein Volume Profile auf der NAS100-Pane hätte schlicht keine Eingangsdaten — es würde mit hoher Wahrscheinlichkeit gar nicht rendern, so wie Volume/VWAP dort auch nicht rendern. Das ist der harte, vorgelagerte Ausschlussgrund.

Bliebe QQQ (Pane 1): Dort gibt es echtes Handelsvolumen — aber nur den Cboe-One-Teilfeed (~10–20% des konsolidierten Volumens, dokumentiert: "absolute Zahlen nicht als Marktgröße interpretieren"). Ein Session-Profil auf dieser Basis ist eine Stichprobe einer Stichprobe; POC/VAH/VAL daraus sind ungefähre Zonen. Und selbst wenn sie präzise wären, greift die zweite bestehende Regel: **QQQ-Preise gelten nur als Richtungs-Referenz, nie als 1:1-Level für den NAS100-Chart** ([[feedback_chart_layout]]). Ein QQQ-POC bei 712,40 lässt sich nicht regelkonform in ein NAS100-TP1 bei 29.480 übersetzen — genau diese Übersetzung bräuchten aber die beiden Andockstellen, die HVN/LVN attraktiv machen würden (8b1 TP-Realismus: "TP2-Kandidat muss ein echtes Chart-Level sein"; 8c2 SL-Cluster: NAS100-Preiszonen).

### 1b. Technische Auslesbarkeit über die MCP-Tools

Die Pine-Graphics-Tools (`data_get_pine_lines/labels/boxes/tables`) lesen den Primitives-Pfad von **Pine-Skripten** (`line.new()`/`box.new()` etc.). Das eingebaute TradingView-Volume-Profile ist eine native Studie mit eigenem Histogramm-Renderpfad — es gibt einen dokumentierten Präzedenzfall für genau diese Sorte Sonderfall: "Relative Volume at Time" taucht **nie** in `chart_get_state`/`data_get_indicator` auf, obwohl es sichtbar am Chart hängt ("vermutlich ein anderer interner Studientyp", [[feedback_chart_layout]]). Es ist realistisch zu erwarten, dass das eingebaute Volume Profile ähnlich unsichtbar für die Tools wäre — dann bliebe es **rein visuell**, ohne Weg in `gate_check.cjs`. Auslesbar wäre nur ein Community-/Eigenbau-Pine-Skript, das POC/VAH/VAL selbst berechnet (per `request.security_lower_tf`) und als `line.new`/`box.new` zeichnet — technisch machbar (das System hat mit `anchored_vwap_remote.pine` genau diesen Weg schon einmal erfolgreich beschritten), aber es würde auf QQQ laufen müssen und damit wieder am 1:1-Übersetzungsverbot hängen. Ein Andocken an `gate_check.cjs` wäre mechanisch trivial (das Skript nimmt beliebige benannte CLI-Parameter, das Schattenmess-Muster aus Paket 6 existiert), scheitert aber inhaltlich an der Einheiten-Frage: Entry/SL/TP stehen in NAS100-Punkten, das Profil stünde in QQQ-Dollar.

### 1c. Redundanz gegen die bestehenden Level-Quellen

Ehrlich bewertet: Volume Profile misst eine **echte andere Dimension** (Volumen-nach-Preis statt Preis-nach-Zeit) — HVN/LVN sind konzeptionell kein Duplikat von Pivots/Fibonacci/PDH-PDL. Die Redundanz entsteht praktisch: An Intraday-Sessions fallen POC/VA-Kanten sehr häufig mit dem zusammen, was das System schon hat (VWAP ± Sigma-Bänder sind volumengewichtete Preiszonen — die Remote-AVWAP liefert genau das bereits maschinenlesbar; Konsolidierungsboxen aus der 8b1-Box-Logik SIND die HVN-Zonen des Tages in Preisform; PDH/PDL/Pivots decken die Struktur-Kanten). Der Artikel selbst nennt VP "reaktiv, nicht prädiktiv" — dieselbe Rolle, die die vorhandenen Level-Quellen schon besetzen. Der eigenständige Mehrwert (LVN als "Beschleunigungszonen" für TP-Runway-Schätzung) ist real, aber klein, und er hängt zu 100% an Datenqualität, die dieser Feed nicht hergibt.

### 1d. Chart-Realestate

NAS100 6/10, QQQ 4/10 — ein Slot wäre frei, das ist nicht der Engpass. Der Engpass ist der dokumentierte Punkt-14-Befund ([[feedback_chart_layout]], Plan-Upgrade-Diskussion): "ein weiterer Indikator ist auch eine weitere Zahl, die unter Zeitdruck gelesen werden muss." Ein Histogramm, das über dem Preis liegt (VP rendert in die Preisskala hinein, nicht in eine Sub-Pane), verändert zusätzlich die visuelle Lesbarkeit des Kerzencharts selbst — bei einem Setup, in dem Screenshots Teil des Voll-Checks sind, kein kostenloser Zusatz.

### Fazit Frage 1

**NEIN für jetzt.** Nicht wegen des Artikels oder der Methode — Volume Profile ist auf Instrumenten mit echtem Volumen ein legitimes, gut verstandenes Werkzeug —, sondern weil die konkrete Infrastruktur es dreifach blockiert: kein Volumen auf dem Trigger-Chart, nur Teil-Volumen auf dem Bestätigungs-Chart, und das bestehende 1:1-Übersetzungsverbot QQQ→NAS100 versperrt den einzigen Weg in die Gates. **Der richtige Wiedervorlage-Trigger ist konkret benennbar:** Sobald der geplante IBKR-Umstieg kommt (Schweiz-Umzug, [[project_risikomanagement]] nennt Micro Nasdaq Futures/MNQ als Alternative), handelt Levi ein Instrument mit echtem, vollständigem Börsenvolumen — dort ist ein Session-/Fixed-Range-Profil direkt auf dem Trigger-Instrument sinnvoll und diese Studie sollte dann neu aufgerollt werden. Bis dahin: nicht einbauen, auch nicht "nur visuell" (visuell-ohne-Regelanbindung ist im System-Jargon eine Karteileiche mit Ablesekosten).

---

## 2. "Indikatoren auf Indikatoren": NEIN — geglättete Kopien, die die Rohwert-Architektur unterlaufen

### 2a. Was damit gemeint ist

TradingView erlaubt, einen Indikator auf die Ausgabe eines anderen zu legen ("Apply indicator on..."): z.B. ein gleitender Durchschnitt auf der ADX-Linie, Bollinger Bands auf dem RSI, ein RSI auf dem MACD-Histogramm. Formal entsteht eine zweite Ableitungsschicht: Preis → Indikator 1 → Indikator 2.

### 2b. Methodische Bewertung: fast immer nur Glättung + Lag, keine neue Information

Entscheidend ist, dass die meisten Basis-Indikatoren **selbst schon geglättete Ableitungen** sind. Der ADX ist bereits ein doppelt Wilder-geglätteter DX; ein zusätzlicher MA auf dem ADX ist eine dritte Glättungsstufe — er enthält keine Information, die nicht schon in der ADX-Linie steckt, nur später. Ein RSI auf dem MACD-Histogramm normalisiert eine bereits normalisierte Differenz zweier EMAs — mathematisch definiert, informationstheoretisch leer. Für ein System, dessen dokumentierte Schwäche nie "zu wenig Signale" war, sondern **Timing** (Fable-Vollanalyse #36–43: "Ursachen Timing/Sizing, kein falscher Dual-Gate-Entscheid in 8 Trades"), ist zusätzlicher Lag exakt die falsche Richtung.

Die eine Variante mit echtem methodischem Kern: **Bollinger Bands auf dem RSI** (adaptive Überkauft/Überverkauft-Schwellen statt statischer 70/30 — die Grundidee hinter Indikatoren wie dem "RSI mit adaptiven Bändern"). Das ist keine reine Kopie, sondern eine Kontextualisierung. Aber: Das System nutzt RSI primär über Divergenzen und Flip-Verhalten (Chop-Erkennung Punkt 7), nicht über statische Schwellen — der Anwendungsfall, den adaptive Bänder verbessern würden, ist im Regelwerk gar nicht die tragende RSI-Rolle. Marginaler theoretischer Gewinn, kein adressiertes reales Problem.

### 2c. Technische Bewertung: Kollision mit der Rohwert-Pflicht

Die jüngste Regelwerks-Geschichte (28.08.-Testtag, Opus-Review, 31.08.-Fixes) ist eine einzige Bewegung **hin zu** nachprüfbaren Rohwerten: "ADX(14, NAS100): X (GEMESSEN, KEIN GATE)" muss ein tatsächlich abgelesener Zahlenwert sein, nie "~X"/"implizit"; die neue Gate-Eingaben-Pflichtzeile verlangt für jeden CLI-Parameter von `gate_check.cjs` eine Herkunftsangabe ("abgelesen wo, wann"). Ein Meta-Indikator bricht diese Architektur an zwei Stellen: **(1)** Sein Wert ist per Konstruktion kein Rohwert, sondern eine Ableitung — die Herkunftszeile würde zirkulär ("MA-auf-ADX 22,1, abgelesen aus einem Indikator, der aus dem ADX berechnet ist, der..."). **(2)** Jede neue Zahl ist eine neue Pflichtzeile im ohnehin als Wachstumsproblem dokumentierten Tick-Prompt (Punkt 14, "Hinweis zu spät gelesen" bei Trade #42) — und zwar eine, deren Informationsgehalt gegen null geht. `gate_check.cjs` könnte einen `--adx-ma`-Parameter trivial aufnehmen; genau deshalb ist die technische Machbarkeit hier das falsche Kriterium. Die Frage ist nicht "geht das", sondern "trägt die Zahl ihr Ablesegewicht" — und das tut sie nicht.

### 2d. Falls Levi es trotzdem will: der saubere Weg

Das System hat für genau diesen Fall ein bewährtes Muster, das ich empfehlen würde statt jeder Sofort-Scharfschaltung: **eine** (nicht mehrere) Meta-Größe als Schattenmessung im GEMESSEN-KEIN-GATE-Status, mit eigenem optionalem CLI-Parameter in `gate_check.cjs`/`add_trade.cjs` (analog `--adx`/`--trend-effizienz`), fester Review-Pflicht nach 10–15 Fällen und Streichung bei ausbleibendem Befund (das 7a1-Karteileichen-Prinzip). Wobei die günstigste "Meta-Information" gar keinen neuen Indikator braucht: **Die Steigungsrichtung des ADX über die letzten 2–3 abgeschlossenen 5-Min-Kerzen** (steigend/fallend, aus zwei ohnehin abgelesenen Rohwerten differenzierbar) liefert das, was ein MA-auf-ADX verspricht — Trendstärke-Dynamik statt Momentaufnahme — ohne neuen Slot, ohne neue Ableitungsschicht, ohne Rohwert-Bruch.

### Fazit Frage 2

**NEIN.** Methodisch überwiegend geglättete Kopien mit zusätzlichem Lag, die kein dokumentiertes Problem des Systems adressieren; architektonisch ein direkter Rückschritt hinter die Rohwert-Pflicht, die sich das System in den letzten Wochen teuer erarbeitet hat. Wenn überhaupt: ADX-Steigung aus vorhandenen Rohwerten als Schattenmessung — kein neuer Indikator.

---

## 3. Renko / Kagi / Line-Break / Point & Figure intraday: NEIN — strukturell inkompatibel mit dem zeitgetakteten Fundament

### 3a. Der Kern-Konflikt: Preisereignis vs. Zeitraster

Die gesamte Trigger-Mechanik ist zeitbasiert und darauf kalibriert: CronCreate feuert jede Wanduhr-Minute, `Minute % 5 == 0` definiert den Voll-Check, der Entry hängt am **bestätigten 5-Min-Kerzenschluss** NAS100 + 15-Min-Kerzenschluss QQQ (Dual-Gate, Punkt 7b), und Regeln wie 8a2 (Zwei-Kerzen-Reclaim), die Chop-Auflösung ("3+ abgeschlossene Kerzenschlüsse"), die Schattenmess-Parameter `--kerzen-nas100`/`--kerzen-qqq` und der verbindliche ATR(14, **5min**) als Einheit aller SL-Floors (8c) setzen sämtlich voraus, dass eine "Kerze" ein festes Zeitintervall ist. Renko/Kagi/Line-Break/P&F drehen das um: Ein Ziegel/eine Linie schließt, wenn der **Preis** eine Schwelle überschreitet — das kann dreimal pro Minute passieren oder eine Stunde lang gar nicht. Für den `Minute % 5`-Mechanismus gibt es dann kein definiertes "die Kerze ist jetzt zu"-Ereignis mehr: Der Loop könnte zwar jede Minute per `data_get_ohlcv` pollen und zählen, ob neue Bricks entstanden sind (maschinell ableitbar wäre ein Brick-Schluss also im Prinzip), aber ein Brick-Schluss zu einem zufälligen Zeitpunkt zwischen zwei Ticks würde bis zu ~60–85 Sekunden verspätet erkannt (Cron-Anlauf ~10–25s kommt obendrauf) — und vor allem: **jede einzelne Schwelle im Regelwerk wäre neu zu kalibrieren.** Ein ATR(14) auf Renko-Bricks ist eine völlig andere Zahl als ATR(14) auf 5-Min-Kerzen (Bricks haben per Definition nahezu konstante Range); die 1,5×/2,5×-SL-Floors, die Drei-Zonen-TP-Logik, die Kerzen-Zähler — alles verlöre seine empirische Basis aus 43 Trades. Das ist kein Anbau, das wäre ein Systemneubau.

### 3b. Tool-Support und die "Zusatzansicht"-Frage

`chart_set_type` unterstützt diese Typen laut Tool-Doku technisch (Renko explizit gelistet, "etc." für die übrigen). Aber der Tool-Support ist die kleinste Hürde. Entscheidend: `chart_set_type` schaltet den Typ **derselben Pane** um — eine echte Parallel-Ansicht "Renko neben Kerzen" bräuchte eine dritte Pane oder einen zweiten Tab, und `tab_new` funktioniert in der Desktop-App dokumentiert nicht zuverlässig ([[feedback_chart_layout]]: "meldet Erfolg, aber tab_count bleibt bei 1"). Ein drittes Pane würde NAS100+QQQ verkleinern — exakt das Lesbarkeitsproblem, wegen dem das VIX-Pane 2026 abgeschafft wurde. Und ein temporäres Umschalten der NAS100-Pane im laufenden Loop wäre aktiv gefährlich: Alle Indikatoren (EMA50! ATR!) rechnen nach dem Umschalten auf synthetischen Bricks — ein `data_get_study_values` im falschen Moment liefert dann EMA50/ATR-Werte, die mit den Gate-Eingaben nichts mehr zu tun haben, und der Rückschalt-Schritt wäre eine neue Variante der bekannten "QQQ bleibt auf 60min stehen"-Fallenklasse. Der theoretische Nutzen einer Renko-Zusatzansicht (visuelles Rausch-Filtern, klarere Struktur-Brüche) ist real, aber genau diese Funktion erfüllen im System bereits die Chop-Erkennung (RSI/MACD-Flip + VIX), die Trend-Effizienz-Messung und die Box-Logik — quantitativ statt visuell, und damit protokollierbar.

### 3c. Die Volumenverzerrungs-Warnung des Artikels

Die Artikel-Warnung (Tagesvolumen wird durch N synthetische Bricks geteilt → verzerrte Profile) ist für NAS100 gegenstandslos — wo kein Volumen ist, kann keines verzerrt werden. Relevant wird sie als **Kombinationsverbot**: Falls je (Stufe IBKR/MNQ, siehe Frage 1) ein Volume Profile eingeführt wird, darf es ausschließlich auf zeitbasierten Kerzen laufen, nie auf einem dieser Charttypen. Das bestätigt indirekt das Gesamturteil: Die beiden Erweiterungen aus Frage 1 und Frage 3 schließen sich sogar gegenseitig aus.

### Fazit Frage 3

**NEIN, vollständig** — weder als Trigger-Ersatz (strukturell inkompatibel mit CronCreate/Kerzenschluss/ATR-Kalibrierung) noch als Zusatzansicht (kein zuverlässiger technischer Platz dafür, Umschalt-Risiko im Live-Loop, Nutzen bereits durch quantitative Bestandsregeln abgedeckt). Das ist der klarste der drei Befunde. Wer preisgetriebene Rauschfilterung will, hat sie im System schon — als Zahl statt als Ziegel.

---

## Gesamturteil

Drei Mal Nein, aber nicht gleichwertig: Frage 3 ist ein grundsätzliches Nein (Architektur-Widerspruch), Frage 2 ein methodisches Nein mit billigem Ersatz (ADX-Steigung aus Rohwerten), Frage 1 ein **Noch-nicht** mit konkretem, bereits im Fahrplan liegendem Wiedervorlage-Trigger: dem Wechsel auf ein Instrument mit echtem Volumen (IBKR/MNQ). Kein Punkt rechtfertigt eine Änderung am laufenden System — konsistent mit [[feedback_dont_change_running_system]] und der Regeländerungs-Tempo-Bremse (Punkt 14). Diese Studie ist als Verbesserungsfund vermerkt, nicht als Umsetzungsauftrag.

**Quellen (Tick-Volumen-Recherche):** [MQL5: Real volume vs tick volume](https://www.mql5.com/en/blogs/post/741311) · [Global Prime: Why Is Tick Volume Important](https://globalprime.medium.com/why-is-tick-volume-important-to-monitor-56a936eea70d) · [Trading Wyckoff: Tick Volume vs Real Volume](https://tradingwyckoff.com/en/tick-volume-vs-real-volume/) · [Tradeciety: Using Volume In Forex](https://tradeciety.com/why-fake-volumes-in-forex-can-help-you-win-using-volume-in-forex)
