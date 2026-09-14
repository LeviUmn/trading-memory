---
name: project_studie_volumenprofil_sonnet_2026-08-31
description: "Sonnets eigene Studie (parallel zu unabhängigen Opus- und Fable-Studien) zu drei Fragen: Volumenprofil-Integration, Indikatoren auf Indikatoren, Renko/Kagi/Line-Break/Point&Figure für Intraday. Fazit: Volumenprofil bedingt ja (nur als Level-Quelle für 8b1/8c2, nicht als neuer Trigger, Tick-Volumen-Kaveat zuerst prüfen), Indikatoren-auf-Indikatoren nein als Gate-Input, alternative Charttypen nein wegen Inkompatibilität mit dem zeitbasierten Dual-Gate-Trigger."
metadata:
  type: project
  originSessionId: session-2026-08-31
  modified: 2026-08-31T12:50:48.716Z
---

# Studie: Volumenprofil, Meta-Indikatoren, alternative Charttypen (Sonnet, 31.08.2026)

Quelle der Frage: Levi hat den TradingView-Hilfeartikel "Volumenprofil-Indikatoren: Grundkonzepte" (`C:\Users\umnus\Downloads\Volumenprofil.pdf`) vorgelegt und um drei unabhängige Studien gebeten (Opus, Fable, Sonnet — jeweils ohne Einsicht in die anderen). Das hier ist meine eigene Einschätzung, gegen den bestehenden Regelwerk-Kontext (`feedback_chart_layout.md`, `feedback_chartanalyse.md`, `feedback_live_trading.md`, `project_risikomanagement.md`) gehalten, die ich in dieser Session ohnehin schon vollständig geladen hatte.

## Kurzfazit

**Volumenprofil: bedingtes Ja**, aber nur als zusätzliche Level-Quelle für TP-Realismus (8b1) und SL-Cluster-Erkennung (8c2) — nicht als neuer eigenständiger Trigger, und nur nachdem der Tick-Volumen-Kaveat für NAS100 als Index-CFD empirisch geprüft wurde. **Indikatoren auf Indikatoren: Nein als Gate-Input**, allenfalls als rein visuelles Glättungsmittel, weil sie keine unabhängige Bestätigung liefern, sondern nur eine Transformation derselben zugrundeliegenden Daten. **Renko/Kagi/Line-Break/Point & Figure für Intraday: Nein.** Diese Charttypen sind preisgetrieben, nicht zeitgetrieben — das kollidiert strukturell mit dem gesamten Dual-Gate-Mechanismus, der auf bestätigten Kerzenschlüssen zu festen Uhrzeiten beruht, und die Volumendaten darauf sind laut Artikel selbst synthetisch verzerrt.

---

## 1. Volumenprofil

**Der Tick-Volumen-Kaveat ist der zentrale Punkt, und er wird leicht übersehen.** Der Artikel selbst listet die Volumentypen: "Handelsvolumen" für Aktien, aber **"Tick-Volumen" für Indizes/Forex/Krypto-CFDs** — explizit definiert als "Anzahl der Kursaktualisierungen", nicht echtes gehandeltes Volumen. NAS100 wird hier als Index-Zertifikat/CFD gehandelt, fällt also exakt in diese Kategorie. Das bedeutet: der POC wäre nicht "das Preisniveau, an dem am meisten gehandelt wurde", sondern "das Preisniveau, an dem der Kursfeed am häufigsten aktualisiert wurde". Das korreliert mit Aktivität/Volatilität, ist aber kein echtes Order-Flow-Signal — man bekommt der Sache nach eher ein "Time-at-Price"-Profil als ein echtes Volumenprofil, nur unter falschem Namen. Das macht das Konzept nicht wertlos (Zonen, in denen der Kurs viel "Zeit"/Aktualisierungen verbraucht hat, sind trotzdem oft Konsolidierungszonen), aber es ist etwas anderes als das, was der Artikel verspricht, und sollte vor jeder Entscheidung kurz empirisch gegengecheckt werden: Volume-Profile-POC auf NAS100 (Tick-Volumen) gegen dieselbe Zeitspanne auf QQQ (echtes Handelsvolumen) vergleichen — wenn die Zonen grob übereinstimmen, ist der Tick-Proxy brauchbar, wenn nicht, ist er es nicht. Das ist ein Nachmittag Arbeit, kein Grundsatzprojekt, und sollte vor einer Ja/Nein-Entscheidung stehen, nicht danach.

**Redundanz gegen Bestehendes — hier sehe ich tatsächlich echten Zusatznutzen, nicht nur Doppelung.** Die Strategie hat bereits mehrere "echte Chart-Level"-Quellen: Pivots (7a1a, formelbasiert), Fibonacci (geometrisch, aus einem gewählten Impuls abgeleitet), Session-Extreme. Das sind alles *abgeleitete* Level — berechnet aus einer Formel oder einem gewählten Referenzpunkt. HVN/LVN wären die einzige *empirisch beobachtete* Level-Quelle: "hier hat der Markt tatsächlich Zeit verbracht" statt "hier müsste laut Formel ein Level liegen". Das ist konzeptionell näher an dem, was die SL-Cluster-Regel (8c2, "SL in bereits per Wick getesteter Zone braucht zusätzlichen Puffer") eigentlich sucht, aber heute nur manuell/visuell über Wick-Beobachtung approximiert. Ein HVN direkt an der geplanten SL-Zone wäre ein objektiverer Beleg dafür als die aktuelle Heuristik.

**Der technische Haken, den ich für den wichtigsten halte: maschinelle Lesbarkeit.** Das eingebaute TradingView-Volumenprofil ist ein natives Studio (kein gewöhnliches, offenes Pine-Skript) — laut den Tool-Konventionen in `CLAUDE.md` gilt für "protected/encrypted"-Indikatoren, dass `data_get_indicator` nicht zuverlässig funktioniert und man auf `data_get_study_values` ausweichen soll, was hier aber nicht reicht, weil POC/VAH/VAL keine "Standardwerte" wie RSI/MACD sind. Ob `data_get_pine_lines`/`data_get_pine_boxes` die vom nativen Volumenprofil gezeichneten Linien/Boxen überhaupt sauber ausliest, ist ungeklärt und sollte vor jeder Integrationsentscheidung an einem einzelnen Testtag verifiziert werden — sonst bleibt POC/VAH/VAL rein visuell und kann nicht in `gate_check.cjs`/die Rohwert-Pflicht einfließen, sondern nur qualitativ in die Chartmuster-Beschreibung.

**Chart-Realestate:** 6 NAS100 + 4 QQQ Indikatoren sind schon ordentlich voll (das System hat selbst schon einmal aufgeräumt, siehe VWAP-Dedup). Ein Histogramm-Overlay ist aber typischerweise am Preisachsenrand platziert, nicht in einem eigenen Pane — das dürfte weniger Konflikt erzeugen als ein weiterer Oszillator-Pane.

**Verhältnis zur Trigger-Logik:** Die Beispielstrategie im Artikel (Rückkehr zum Vortages-POC als Einstiegssignal) ist im Kern mean-reversion — das würde neben der trendfolgenden Dual-Gate-Logik ein zweites, konkurrierendes Setup aufmachen. Davon rate ich ab, nicht weil Mean-Reversion per se falsch wäre, sondern weil das System explizit gegen unkontrolliertes Hinzufügen paralleler Trigger-Pfade diszipliniert ist (jede neue Gate-Ebene hat bisher Monate gebraucht, um sauber zu kalibrieren — siehe die Paket-6-Historie).

**Mein Fazit:** Bedingtes Ja, aber eng geschnitten — als zusätzliche, empirisch beobachtete Level-Quelle für 8b1/8c2, nicht als neuer Trigger. Voraussetzung vor der Entscheidung: (a) Tick-Volumen-Proxy-Check NAS100 vs. QQQ, (b) Lesbarkeitscheck über `data_get_pine_lines`/`data_get_pine_boxes`. Ohne beides ist es eine Wette auf ein Konzept, dessen Datenbasis für dieses Instrument nicht die ist, für die es entworfen wurde.

---

## 2. Indikatoren auf Indikatoren

Das ist eine etablierte Technik, kein Exotenkonzept — StochRSI (Stochastik auf RSI) ist das bekannteste Beispiel und existiert genau deshalb, weil RSI in starken Trends lange in Extrembereichen "hängen" kann und eine zusätzliche Schicht das wieder sensibilisiert. Insofern: methodisch nicht per se unseriös.

**Aber der Kernpunkt für diese Strategie ist ein anderer als "ist es seriös".** Das ganze Dual-Gate-Konzept lebt davon, dass NAS100 und QQQ **unabhängige** Datenquellen sind — zwei verschiedene Instrumente müssen unabhängig voneinander bestätigen. Ein Indikator auf einem Indikator ist per Definition **nicht unabhängig** von seiner Basis — er ist eine mathematische Transformation derselben zugrundeliegenden Preisreihe, keine zweite Informationsquelle. Er kann ein Muster deutlicher sichtbar machen (Glättung, Sensibilisierung), aber er liefert keine Bestätigung im Sinne dessen, was die Dual-Gate-Architektur eigentlich will. Genau dieses Muster — etwas, das wie eine zusätzliche Bestätigung aussieht, aber keine ist — hat das Regelwerk an anderer Stelle schon einmal explizit adressiert (`feedback_dual_gate_confirmation.md`: bei Multi-Instrument-Setups denselben Bestätigungsgrad prüfen, nicht nur formal zwei Häkchen setzen).

**Technisch käme dazu:** Die Rohwert-Pflicht-Architektur (`gate_check.cjs`, die Pflichtzeilen in `feedback_vollcheck_format.md`) verlangt heute schon klar benannte Einzelwerte pro Basis-Indikator. Jede zusätzliche Meta-Indikator-Schicht multipliziert die Zahl der Werte, die pro Voll-Check abgelesen, benannt und auf Rohwert-Pflicht geprüft werden müssten — bei einer Regelwerks-Historie, die gerade erst mühsam saubere Rohwert-Disziplin für die *bestehenden* vier Werte (ADX/RSI/MACD-H/EMA50) erreicht hat (28.08.-Review), ist das der falsche Zeitpunkt, die Zahl der Pflichtwerte zu erhöhen.

**Mein Fazit:** Nein als neuer Gate-Input oder neue Pflichtzeile. Die einzige vertretbare Anwendung wäre rein visuell/qualitativ — z.B. eine kurze gleitende Glättung auf RVOL, um das Volumen-Soft-Gate (8a3) weniger noisy zu machen, ohne daraus eine neue benannte Pflichtgröße zu machen. Das ist ein kleiner, optionaler Nice-to-have, kein strategischer Baustein.

---

## 3. Renko, Kagi, Line-Break, Point & Figure für Intraday

Hier ist meine Einschätzung eindeutig, weil es kein Abwägungs-, sondern ein Architekturproblem ist: Der gesamte Live-Loop ist **zeitgetaktet** — CronCreate feuert jede Minute, der Voll-Check prüft alle 5 Minuten auf einen **bestätigten Kerzenschluss zu einem festen Uhrzeitpunkt** (die ganze Voll-Check-Nummerierung basiert auf `Minuten seit Start / 5 + 1`, siehe die jüngste Regelwerksrunde vom 31.08.). Renko, Kagi, Line-Break und Point & Figure sind **preisgetrieben, nicht zeitgetrieben**: ein neuer Balken/Ziegel entsteht erst, wenn der Kurs eine bestimmte Distanz zurückgelegt hat — das kann in einer volatilen Minute passieren oder erst nach zwei Stunden Seitwärtsbewegung.

Das lässt sich nicht einfach in den bestehenden Mechanismus einbauen, ohne ihn grundlegend umzubauen: Man müsste bei jedem 1-Minuten-Tick prüfen, ob inzwischen ein neuer Ziegel/Balken entstanden ist (über wiederholtes `data_get_ohlcv` mit Bar-Count-Vergleich), statt wie heute zu einem festen Zeitpunkt einen Kerzenschluss zu erwarten. Das ist technisch machbar, aber es ersetzt ein sauberes, deterministisches Timing-Modell durch ein Polling-Modell mit Unschärfe (der neue Ziegel könnte irgendwann zwischen zwei 1-Minuten-Ticks entstanden sein, nicht exakt zum Tick-Zeitpunkt) — eine Verschlechterung der Präzision, nicht eine Verbesserung.

Dazu kommt der im Artikel selbst benannte Punkt: Volumen auf diesen synthetischen Charttypen ist verzerrt (ein Tagesvolumen wird rechnerisch auf N Ziegel verteilt). Das würde jede Kombination mit Punkt 1 (Volumenprofil) zusätzlich entwerten.

**Als reine Zusatzansicht neben dem bestehenden Kerzenchart** (nicht als Ersatz des Triggers) sehe ich ebenfalls wenig Mehrwert: Der erklärte Zweck solcher Charts — Rauschfilterung, klarere Trendsicht — wird in dieser Strategie bereits durch ADX (Trendstärke) und den MTF-Vierschritt abgedeckt. Ein zusätzlicher Chart-Typ würde Komplexität hinzufügen (weiteres Pane oder weiterer Tab, ständiger Kontextwechsel während des Voll-Checks), ohne eine Lücke zu schließen, die nicht schon durch bestehende Werkzeuge adressiert ist.

**Mein Fazit:** Nein, weder als Trigger-Ersatz noch als Zusatzansicht. Der Aufwand-Nutzen ist hier eindeutig negativ, und "kein Rundumschlag" gilt hier besonders, weil eine Umstellung des Trigger-Timings das Herzstück der gesamten Regelwerks-Architektur berühren würde.
