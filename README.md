# LLM Economics

`llm-economics` ist ein Experiment zur wirtschaftlichen Analyse realer LLM-Workloads.

Das Projekt soll nicht nur Tokenpreise ausrechnen, sondern beantworten:

- Was hätte eine reale Arbeitsweise über eine API gekostet?
- Welche Anteile entfallen auf Input, Cached Input und Output inklusive Reasoning?
- Welche Workload-Klassen verursachen welche Kosten?
- Welche Modelle liefern für eine Aufgabe das sinnvollste Verhältnis aus Qualität, Kosten und Laufzeit?
- Welche Erkenntnisse lassen sich später für Model Routing und Inference-Entscheidungen verwenden?

## Current Status

**Classification: Experiment**

Aktuell wird zuerst die Methodik definiert. Es gibt noch keine produktive Routing- oder Abrechnungsfunktion und keine Kopplung an Project 42.

## Two target use cases

### 1. Historical workload simulation

Aus echten, lokal vorhandenen Arbeits- und Chatdaten wird eine reproduzierbare Workload-Projektion erzeugt. Da ChatGPT-Exporte keine vollständige historische API-artige Usage-Telemetrie mit Reasoning-Tokens liefern, wird zwischen direkt messbaren Daten und kalibrierten Schätzungen unterschieden.

Ziel ist eine belastbare Antwort auf Fragen wie:

> Was hätte diese reale Arbeitsweise mit Modell X über die API ungefähr gekostet?

### 2. Future routing economics

Repräsentative Aufgabenklassen werden mit echten API-Runs kalibriert. Damit kann später untersucht werden, wann ein günstigeres Modell ausreicht und wann ein leistungsfähigeres Modell den Mehrpreis rechtfertigt.

Das Projekt soll daraus zunächst **Entscheidungsdaten** erzeugen. Automatisches Model Routing ist ein möglicher späterer Verbraucher, aber nicht Teil des ersten Scopes.

## Measurement model

Die Kostenanalyse unterscheidet mindestens:

```text
workload
├── input tokens
│   ├── uncached input
│   └── cached input
├── output tokens
│   ├── visible output
│   └── reasoning tokens (wenn vom API-Usage-Objekt ausgewiesen)
├── model / pricing snapshot
├── task class
└── calibration metadata
```

Reasoning-Tokens werden nicht aus sichtbarem Text erfunden. Historische Chatdaten liefern deshalb zunächst eine Untergrenze; realistischere Schätzungen werden über echte, repräsentative API-Calibration-Runs abgeleitet.

## Privacy boundary

Persönliche Chatverläufe, Arbeitsdateien und andere Rohdaten gehören **nicht** in dieses Repository.

Das Repository enthält Code, Tests, Methodik, Schemas und Dokumentation. Reale Workloads sollen standardmäßig lokal verarbeitet werden. Persistente lokale Eingaben und Ergebnisse gehören in `var/`, konkrete temporäre Läufe in `run/`; beide Bereiche werden nicht als persönliche Datensätze versioniert.

## Project 42 relationship

`llm-economics` bleibt zunächst eigenständig. Später kann Project 42 beziehungsweise ein Inference-/Routing-Layer seine Ergebnisse verwenden. Das Projekt soll dafür keine versteckte Abhängigkeit von einem bestimmten Orchestrator oder einem einzelnen Modellanbieter bekommen.

## Documentation

Startpunkt: [`docs/INDEX.md`](docs/INDEX.md)
