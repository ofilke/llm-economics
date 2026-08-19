# Roadmap

## Target

LLM Economics macht reale LLM-Arbeitslasten reproduzierbar messbar und wirtschaftlich vergleichbar. Das Projekt soll historische Kostenfragen beantworten und später belastbare Daten für Model-/Inference-Routing liefern, ohne persönliche Rohdaten in das Repository zu verschieben.

| Phase | Outcome | Status |
|---|---|---|
| 0 | Projektgrundlage, Scope, Privacy- und Messregeln | complete |
| 1 | minimale Datenverträge für Workload, Pricing, Calibration und Estimate | planned |
| 2 | lokaler Importer für Chat-/Arbeitsdaten + observable workload metrics | planned |
| 3 | Pricing-Snapshot-Verwaltung mit Primärquellen/Datum | planned |
| 4 | API Calibration Runner mit echter Usage-Telemetrie | planned |
| 5 | historische Kostensimulation mit Bandbreiten | planned |
| 6 | Modellvergleich pro Workload-Klasse inkl. Qualität/Kosten/Laufzeit | planned |
| 7 | exportierbare Routing-Empfehlungen / Project-42-Handoff | deferred |

## Phase 1 — Data contracts

Zuerst werden kleine, anbieterneutrale Verträge definiert für:

- `workload`;
- `pricing_snapshot`;
- `calibration_run`;
- `cost_estimate`.

Acceptance:

- gemessene und geschätzte Größen sind strukturell unterscheidbar;
- Pricing ist versioniert/datierbar;
- Provider-spezifische Usage kann erhalten bleiben, ohne den Kernvertrag an einen Anbieter zu koppeln;
- Rohdaten müssen nicht in den Vertrag kopiert werden, sondern können lokal referenziert/gehasht werden.

## Phase 2 — Local workload import

Erster Importpfad: vorhandene Chat-/Arbeitsdaten lokal analysieren.

Ziel ist nicht, ChatGPT-internen Verbrauch zu rekonstruieren, sondern direkt beobachtbare Workload-Größen reproduzierbar zu erfassen.

Acceptance:

- keine persönlichen Rohdaten werden committed;
- Import ist deterministisch;
- Tokenizer/Approximation ist im Ergebnis ausgewiesen;
- fehlende historische Telemetrie wird explizit als unbekannt markiert.

## Phase 3 — Pricing snapshots

Provider-/Modellpreise werden als datierte Snapshots importiert beziehungsweise gepflegt.

Acceptance:

- Primärquelle und Abrufdatum vorhanden;
- keine stillen Preisänderungen alter Simulationen;
- Sonderregeln wie Cached Input oder Long Context sind modellierbar.

## Phase 4 — Calibration runner

Repräsentative Workloads werden kontrolliert über APIs ausgeführt und die echte Usage gespeichert.

Acceptance:

- API-Key ausschließlich lokal;
- Usage-Rohobjekt bleibt nachvollziehbar;
- Output/Reasoning werden nur so getrennt, wie die API sie tatsächlich ausweist;
- Run-Provenienz und Modellparameter sind reproduzierbar.

## Phase 5 — Historical simulation

Aus realen beobachtbaren Workloads plus Calibration-Daten entstehen Kostenschätzungen.

Acceptance:

- mindestens observable floor + calibrated estimate;
- Unsicherheit/Bandbreite sichtbar;
- Kosten nach Workload-Klasse und Modell aufschlüsselbar;
- keine falsche Exaktheit bei Reasoning oder Cache-Annahmen.

## Phase 6 — Model economics

Mehrere Modelle werden für dieselben Workload-Klassen verglichen.

Neben Kosten können Qualität, Korrekturbedarf und Laufzeit einfließen.

Ziel ist eine Entscheidungssicht, nicht ein pauschales Ranking aller Modelle.

## Phase 7 — Optional Project 42 integration

Erst wenn die Methodik belastbar ist, können Ergebnisse maschinenlesbar an einen Inference-/Routing-Layer übergeben werden.

LLM Economics entscheidet dann nicht selbst über Plattform-Orchestrierung. Es liefert Economics-/Benchmark-Daten; der jeweilige Router behält die Ausführungsentscheidung.

## Explicitly deferred

- automatisches Model Routing im ersten Schritt;
- produktive Billing-/Kostenkontrolle;
- Cloud-Ablage persönlicher Chat-Rohdaten;
- Kopplung an ChatGPT-spezifische interne Telemetrie, die nicht offiziell verfügbar ist;
- ein einzelner statischer Preisrechner ohne Workload-/Calibration-Kontext.
