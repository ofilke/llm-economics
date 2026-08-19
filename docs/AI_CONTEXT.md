# AI Context for LLM Economics

## Current state

`llm-economics` ist neu angelegt und als **Experiment** klassifiziert. Ziel ist nicht nur ein Tokenpreis-Rechner, sondern die wirtschaftliche Analyse realer LLM-Arbeitslasten.

Der erste konkrete Anwendungsfall ist die Frage:

> Was hätte eine tatsächlich genutzte Chat-/Arbeitsweise ungefähr gekostet, wenn dieselben Aufgaben über APIs statt über ein ChatGPT-Abo gelaufen wären?

Die historischen ChatGPT-Daten liefern sichtbare Eingabe- und Ausgabetexte, aber keine vollständige per-Turn-Usage-Telemetrie mit tatsächlichen internen Reasoning-Tokens. Deshalb wird kein scheinpräziser historischer Verbrauch erfunden.

Stattdessen ist die geplante Methodik zweistufig:

1. vorhandene reale Workloads lokal vermessen und nach Aufgabentypen klassifizieren;
2. repräsentative Stichproben als echte API-Calibration-Runs ausführen und die dort ausgewiesene Usage verwenden, um Kosten- und Reasoning-Verteilungen für vergleichbare historische Workloads abzuleiten.

## Intended outputs

- Kostenuntergrenze aus direkt beobachtbaren Text-/Tokenmengen;
- kalibrierte Kostenspannen statt falscher Einzelwerte;
- Aufschlüsselung nach Modell, Task-Klasse und Usage-Komponenten;
- Vergleich unterschiedlicher Modelle für dieselbe Workload-Klasse;
- später mögliche Entscheidungsdaten für Inference-/Model-Routing.

## Usage dimensions

Mindestens getrennt behandeln:

- uncached input tokens;
- cached input tokens;
- total output tokens;
- reasoning tokens, sofern durch die jeweilige API tatsächlich ausgewiesen;
- sichtbaren Output als abgeleitete Differenz, wenn Usage-Metadaten dies zulassen;
- Modell und Pricing-Snapshot;
- Task-Klasse;
- Run-/Calibration-Provenienz.

Reasoning-Tokens sind abrechnungstechnisch modell-/anbieterabhängig zu interpretieren und dürfen nicht als eigenständiger Preisfaktor angenommen werden, wenn die jeweilige Preisliste sie als Output abrechnet.

## Privacy and data boundary

Persönliche Chats und Arbeitsdateien bleiben lokal. Das Repository enthält keine persönlichen Rohdaten. `var/` ist für lokale persistente Mess-/Importdaten vorgesehen, `run/` für konkrete Ausführungen; beide sind gitignored.

## Project 42 relationship

Das Experiment ist eigenständig. Project 42 kann seine Ergebnisse später als Entscheidungsgrundlage verwenden. Die mögliche spätere Rolle ist besonders im Inference-/Routing-Kontext interessant: günstige Modelle für einfache Workloads, stärkere Modelle nur dort, wo Qualität den Mehrpreis rechtfertigt.

Die Integration ist bewusst Zukunftsmusik. Zuerst muss die Messmethodik belastbar sein.

## Next decision

Als nächstes sollte ein minimaler Datenvertrag für `workload`, `pricing_snapshot`, `calibration_run` und `cost_estimate` definiert werden. Erst danach ist die Implementierung eines Importers für ChatGPT-Exporte sinnvoll.
