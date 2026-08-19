# Methodology

## Goal

LLM Economics soll reale Arbeitslasten wirtschaftlich vergleichbar machen, ohne historische Telemetrie vorzutäuschen, die nicht vorhanden ist.

Die Methodik trennt deshalb **Measurement**, **Calibration** und **Estimation**.

## 1. Measurement

Aus vorhandenen lokalen Arbeitsartefakten werden nur direkt beobachtbare Größen erhoben.

Bei einem Chat-Export sind das insbesondere:

- Reihenfolge und Rolle der Nachrichten;
- sichtbarer Nutzertext;
- sichtbarer Assistant-Text;
- Zeit-/Konversationsmetadaten, soweit im Export vorhanden;
- aus dem Text reproduzierbar berechnete Tokenmengen für einen explizit benannten Tokenizer beziehungsweise eine definierte Approximation.

Nicht direkt beobachtbar und deshalb nicht als Messwert zulässig:

- historisch tatsächlich verwendete interne Reasoning-Tokens;
- historischer Prompt-Cache-Hit-Anteil;
- versteckter System-/Tool-Kontext, sofern er im Export nicht enthalten ist;
- exakte historische Modellkonfiguration, falls sie nicht nachvollziehbar gespeichert wurde.

Damit entsteht zunächst eine **observable lower bound** und keine behauptete exakte historische API-Rechnung.

## 2. Workload classification

Workloads werden in sinnvolle Klassen gruppiert, zum Beispiel:

- kurze Wissens-/Alltagsfrage;
- lange Architektur-/Strategiearbeit;
- Coding/Repository-Arbeit;
- Recherche mit externen Quellen;
- Datei-/Dokumentanalyse;
- Extraktion/Klassifikation;
- Zusammenfassung;
- agentische Tool-Nutzung.

Die Klassifikation muss versionierbar sein. Ein späteres automatisches Klassifikationsmodell darf die ursprünglichen Messdaten nicht verändern.

## 3. Representative sampling

Aus jeder relevanten Workload-Klasse wird eine Stichprobe gewählt. Sie soll nicht nur kurze Durchschnitts-Turns enthalten, sondern die tatsächliche Spannweite abdecken, zum Beispiel:

- klein / typisch / groß;
- niedrige / mittlere / hohe Reasoning-Anforderung;
- geringer / hoher Kontextanteil;
- mit / ohne Tool-Nutzung.

Die Stichprobenauswahl wird als eigenes Artefakt gespeichert, damit spätere Kalibrierungen reproduzierbar bleiben.

## 4. Calibration runs

Repräsentative Aufgaben werden über die jeweilige API erneut ausgeführt.

Ein Calibration Run speichert mindestens:

- Input-Referenz oder reproduzierbaren Input-Fingerprint;
- Task-Klasse;
- Provider;
- Modell;
- relevante Modellparameter;
- Datum/Uhrzeit;
- Pricing-Snapshot-Referenz;
- tatsächliches Usage-Objekt der API;
- Laufzeit;
- Erfolg/Fehler;
- optional eine getrennte Qualitätsbewertung.

Wenn die API Reasoning-Tokens separat ausweist, werden diese als gemessene Unterkategorie des Outputs übernommen. Wenn nicht, werden keine Reasoning-Tokens erfunden.

## 5. Pricing snapshots

Preise sind zeitabhängige Eingangsdaten.

Jeder Pricing Snapshot trägt mindestens:

- Provider;
- Modellbezeichnung;
- Gültigkeits-/Abrufdatum;
- Preis für uncached input;
- Preis für cached input, falls vorhanden;
- Outputpreis;
- gegebenenfalls Long-Context-, Batch- oder sonstige relevante Preisregeln;
- Primärquelle oder nachvollziehbare Provenienz.

Historische Simulationen rechnen mit einem explizit gewählten Snapshot. Ein später geänderter Listenpreis darf alte Resultate nicht stillschweigend verändern.

## 6. Estimation

Historische Schätzungen werden aus beobachtbaren Workloads und Calibration Runs abgeleitet.

Ausgabe ist bevorzugt eine Verteilung oder Bandbreite, zum Beispiel:

```text
observed text floor:        21.40 USD
calibrated expected cost:   34.80 USD
calibrated range:           28.10–47.60 USD
```

Die genaue statistische Methode wird erst nach Sichtung realer Daten festgelegt. Einfache Multiplikatoren sind als erste Baseline zulässig, müssen aber als solche gekennzeichnet werden.

## 7. Quality / cost comparison

Kosten allein entscheiden nicht über ein geeignetes Modell.

Später können Calibration Runs deshalb zusätzlich bewerten:

- Task-Erfolg;
- fachliche Qualität;
- Korrekturbedarf;
- Tool-/Workflow-Zuverlässigkeit;
- Laufzeit;
- Kosten.

Damit lässt sich pro Workload-Klasse eine Pareto-Sicht auf Qualität, Kosten und Geschwindigkeit erzeugen.

## 8. Future routing use

Ein späterer Router könnte aus diesen Ergebnissen Regeln ableiten, etwa:

```text
wenn task_class = extraction
und quality_delta(model_small, model_large) <= threshold
→ günstigeres Modell bevorzugen

wenn task_class = architecture_reasoning
und Fehlerkosten hoch
→ stärkeres Modell bevorzugen
```

Diese Routing-Logik ist nicht Teil der ersten Experimentphase. LLM Economics liefert zunächst belastbare Mess- und Vergleichsdaten.

## Non-negotiable distinction

Jedes Ergebnis muss erkennen lassen, ob eine Zahl:

- **measured**,
- **API-measured calibration** oder
- **estimated**

ist.
