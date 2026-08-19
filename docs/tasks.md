# Tasks

Stand: 19. August 2026

## Current

### Phase 1 — minimale Datenverträge

Als nächster Implementierungsschritt sollen die Kernobjekte definiert werden:

- `workload`
- `pricing_snapshot`
- `calibration_run`
- `cost_estimate`

Vor Implementierung festlegen:

- welche Felder gemessen, importiert oder geschätzt sind;
- wie lokale/private Rohdaten nur referenziert statt kopiert werden;
- wie Provider-spezifische Usage erhalten bleibt;
- wie Pricing-Snapshots datiert und mit Quellen versehen werden;
- wie Unsicherheit in Estimates dargestellt wird.

## Next

1. Datenverträge/Schemas entwerfen und mit kleinen Beispielen testen.
2. ChatGPT-Exportstruktur an einem lokalen, nicht committed Sample untersuchen.
3. deterministischen Workload-Importer bauen.
4. Pricing-Snapshot-Format und Provider-Adapter definieren.
5. repräsentative Workload-Klassen und Sampling-Regeln festlegen.
6. erst dann echten API-Calibration-Runner bauen.

## Deferred

- automatisches Routing;
- Project-42-Integration;
- UI/Dashboard;
- produktive Kostenlimits/FinOps-Steuerung.
