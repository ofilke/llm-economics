# Agent Instructions for llm-economics

Dieses Repository ist ein eigenständiges Experiment zur Analyse der Wirtschaftlichkeit realer LLM-Workloads.

Projektregeln:

- Arbeite innerhalb dieses Repositories, sofern keine breitere Änderung ausdrücklich beauftragt ist.
- Lege Projektdokumentation unter `docs/` ab.
- Nutze `docs/INDEX.md` als Dokumentations-Einstiegspunkt.
- Nutze `docs/AI_CONTEXT.md` für den kurzen aktuellen Arbeitsstand.
- Architekturentscheidungen gehören später nach `docs/decisions/`, sobald echte irreversible oder weitreichende Entscheidungen anstehen.
- Sichtbare Workflow-Einstiegspunkte gehören nach `tools/`; interne Tool-Helfer nach `tools/helpers/`.
- Persistente lokale Daten gehören nach `var/`, konkrete Run-Zustände und temporäre Ausführungsdaten nach `run/`.
- Persönliche Chat-Exporte, Arbeitsdateien, API-Schlüssel und andere private Rohdaten dürfen nicht committed werden.
- Preise und Modellfähigkeiten sind zeitabhängige externe Daten. Keine Preisannahme als dauerhaft gültige Wahrheit in Code oder Dokumentation behandeln; Pricing-Snapshots müssen Datum und Quelle tragen.
- Messdaten und Schätzwerte müssen getrennt bleiben. Insbesondere dürfen historische Reasoning-Tokens nicht aus sichtbarem Output als gemessene Werte dargestellt werden.
- Das Projekt bleibt anbieterneutral im Kern. Anbieter- oder API-spezifische Logik gehört hinter klar benannte Adapter/Importpfade.
- Eine spätere Project-42-/Inference-Integration darf den Standalone-Betrieb nicht voraussetzen oder ersetzen.

## Current classification

**Experiment**. Zuerst Methodik und reproduzierbare Messung; kein vorschnelles produktives Routing-System bauen.
