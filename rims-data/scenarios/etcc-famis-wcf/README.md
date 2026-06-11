# FAMIS-WCF ETCC Scenario Bundle

This scenario materializes the Data-Normalization ETCC program-schedule proof
artifacts into an R-IMS portfolio abstract bundle.

## Source Proof Artifacts

Generated from:

`data-normalization://testbeds/etcc_program_schedule_v0/proof_artifacts.py`

Staged from proof output:

- `runs/etcc-program-schedule/etcc-program-schedule-v0-direct-fill/evidence_records.jsonl`
- `runs/etcc-program-schedule/etcc-program-schedule-v0-direct-fill/coverage_report.json`
- `runs/etcc-program-schedule/etcc-program-schedule-v0-direct-fill/rims_visibility_summary.json`
- `runs/etcc-program-schedule/etcc-program-schedule-v0-direct-fill/rims_visibility_summary.md`

## Loader Contract

- Primary R-IMS schedule input: `etcc_portfolio_abstract.json`.
- Compatibility alias: `portfolio_abstract.json`.
- Detail sidecar: `source_detail_records.jsonl`.
- This directory intentionally does not include `evidence_records.jsonl`,
  `evidence_records.ndjson`, or `evidence_records.json` so the scenario loader
  does not treat Data-Normalization proof records as the primary schedule input.
- The provenance join key is `meta.detail_ref`, populated from
  `scenario_finding_id` when present.

## Preserved Proof Signals

- Conflicted elements: 6.
- Incomplete/gap elements: 2.
- Weak-support elements: 7.
- Timeline views intentionally materialize only schedule-grounded ETCC findings.
  Scope gaps and supporting confirmed facts remain source-backed through
  `source_detail_records.jsonl` and the findings workspace instead of becoming
  fake chart rows.
- Leaf work items preserve `meta.knowledge_confidence`, `meta.gap`,
  `meta.conflict`, `meta.weak_support`, `meta.source_refs`, and
  `meta.detail_ref`.

## Validation

```bash
node scripts/validate_portfolio_abstract.js public/rims-data/scenarios/etcc-famis-wcf/etcc_portfolio_abstract.json
node scripts/convert_canonical_to_views.js public/rims-data/scenarios/etcc-famis-wcf/etcc_portfolio_abstract.json public/rims-data/scenarios/etcc-famis-wcf --domain etcc --validate
```
