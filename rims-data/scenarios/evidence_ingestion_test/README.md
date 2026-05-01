# Evidence Ingestion Test Scenario

Synthetic test scenario for validating the evidence record ingestion pipeline.
This scenario is domain-agnostic and exercises the acceptance points from
`add-evidence-record-ingestion`.

## Project: Project Alpha

A 4-task project with 3 dependencies, designed as an acceptance fixture.

### Tasks

| ID    | Title                  | Start      | End        | Program |
|-------|------------------------|------------|------------|---------|
| W-001 | Requirements Analysis  | 2026-04-01 | 2026-04-15 | A-001   |
| W-002 | System Design          | 2026-04-16 | 2026-05-15 | A-001   |
| W-003 | Implementation Phase 1 | 2026-05-16 | 2026-07-15 | A-001   |
| W-004 | Integration Testing    | 2026-07-16 | 2026-08-15 | A-001   |

### Dependencies

| ID    | From  | To    | Type             |
|-------|-------|-------|------------------|
| D-001 | W-001 | W-002 | FS               |
| D-002 | W-002 | W-003 | FS               |
| D-003 | W-003 | W-004 | FS/SS conflicting |

## Scenario Files

- `evidence_records.ndjson`: primary synthetic evidence bundle
- `plants.json`: scenario-local program row so Program Alpha does not fall back to unrelated root plants
- `plantProfiles_matrix.json`: minimal scenario-local chart matrix for Program Alpha
- `conflict_report.json`: machine-readable breach-driver conflict artifact for MVP review flows

## Test Cases Exercised

### 1. Basic synthesis
W-001 has straightforward single-source records for title, dates, program, and type.

### 2. Supersession
W-003 start date has two records:
- `ev-t003-start-old` = 2026-05-01, confidence 0.70
- `ev-t003-start-new` = 2026-05-16, confidence 0.90, supersedes the old one

Expected: canonical value is 2026-05-16 while the audit chain is preserved.

### 3. Non-breach-driver conflict
W-002 title has two candidates:
- `ev-t002-title-a` = "System Architecture", confidence 0.80
- `ev-t002-title-b` = "System Design", confidence 0.85

Expected: resolve silently via defaults. `conflict_report.json` records this under `suppressed_conflicts`.

### 4. Breach-driver conflict: start_date
W-004 start date has two candidates:
- `ev-t004-start-a` = 2026-07-01, confidence 0.80
- `ev-t004-start-b` = 2026-07-16, confidence 0.82

Expected: default policy still selects a canonical value, but the conflict is surfaced in `conflict_report.json`.

### 5. Breach-driver conflict: dependency_type
D-003 dependency type has two candidates:
- `ev-d003-type-a` = `FS`, confidence 0.70
- `ev-d003-type-b` = `SS`, confidence 0.75

Expected: default policy selects `SS`, and the conflict is surfaced in `conflict_report.json`.

### 6. Uncertainty metadata
W-003 end date retains uncertainty and derivation metadata for later risk modeling.

### 7. Confidence spectrum
Records span confidence from 0.70 to 0.95 so `pickBest` behavior can be verified.

## Schema

All records use `schema_id: ims.schedule.v1`. This fixture deliberately does not
depend on the `rims.slice1a` schema.

## Record Count

- 33 total evidence records
- 2 surfaced breach-driver conflicts
- 1 suppressed non-breach conflict
