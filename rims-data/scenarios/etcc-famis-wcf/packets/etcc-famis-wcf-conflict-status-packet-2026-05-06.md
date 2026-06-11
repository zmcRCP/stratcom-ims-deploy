# ETCC Conflict / Status Packet - etcc-famis-wcf

**Generated:** 2026-05-06
**Packet content hash:** b8c778f1e70e1d07d6e35a4708472baa77f9419b749323867fbc98f8166ea21d
**Source change:** add-etcc-conflict-status-packet-v0_2

---

## Caveats

- Source documents are real FAMIS-WCF artifacts processed through the Data-Normalization pipeline. The reconciliation reflects the pipeline's extraction coverage, not a complete program audit.
- The demo does not claim the contractor IMS is wrong -- it claims the sources disagree and that disagreement is worth a conversation.
- The Year 5 option exercise window date is derived from the contract record. It is not legal advice and does not constitute a contracting determination.
- R-IMS surfaces discrepancies. Adjudication, corrective action requests, and contract authority determinations are human decisions made outside R-IMS.
- The packet does not represent live MS Project integration.
- The packet does not represent canonical SSUP ingestion (that path is tracked under the Data-Normalization SSUP changes).

---

## Schedule Conflicts (6)


- **Government tracking shows the ATO review accepted and closed while the contractor IMS still shows it in progress.** - confidence 0.88
  - Compared field: Status
  - Compared values: Government tracking: accepted_closed | Contractor IMS: in_progress
  - Date context: 2023-07-01 to 2024-03-31
  - (detail_ref: ato-review-status-mismatch)


- **Government and contractor schedules disagree on the Release 3.0 deployment milestone date.** - confidence 0.90
  - Compared field: Finish date
  - Compared values: Government tracking: 2024-11-15 | Contractor IMS: 2024-09-30
  - Date context: 2024-11-15 to 2024-11-15
  - (detail_ref: release-3-deploy-date-slip)


- **Government tracking shows Release 3.0 development and integration testing slipping past the contractor baseline finish.** - confidence 0.88
  - Compared field: Finish date
  - Compared values: Government tracking: 2024-10-31 | Contractor IMS: 2024-08-31
  - Date context: 2023-06-01 to 2024-10-31
  - (detail_ref: release-3-dev-baseline-drift)


- **Government tracking and the contractor IMS disagree on percent complete for Release 3.0 development and integration testing.** - confidence 0.89
  - Compared field: Percent complete
  - Compared values: Government tracking: 70 | Contractor IMS: 95
  - Date context: 2023-06-01 to 2024-10-31
  - (detail_ref: release-3-dev-percent-complete-mismatch)


- **Government and contractor schedules disagree on the Release 4.0 deployment date.** - confidence 0.91
  - Compared field: Finish date
  - Compared values: Government tracking: 2025-09-15 | Contractor IMS: 2025-11-30
  - Date context: 2025-09-15 to 2025-09-15
  - (detail_ref: release-4-deploy-date-slip)


- **Government tracking and the contractor IMS disagree on percent complete for Release 4.0 development and integration testing.** - confidence 0.90
  - Compared field: Percent complete
  - Compared values: Government tracking: 30 | Contractor IMS: 85
  - Date context: 2025-04-01 to 2025-08-29
  - (detail_ref: release-4-dev-percent-complete-mismatch)


---

## Scope Gaps (2)


- **P00010-funded work is present in the government schedule but missing from the contractor IMS.** - confidence 0.91
  - Coverage by source: Government tracking schedule: present (P00010) | Contractor IMS: missing | USAspending award record: present
  - Date context: 2023-10-01 to 2025-09-30
  - (detail_ref: funded-scope-missing-from-contractor-ims)


- **The Year 5 option appears in the government schedule but not in the contractor IMS.** - confidence 0.91
  - Coverage by source: Government tracking schedule: present (P00035) | Contractor IMS: missing | USAspending award record: present
  - Date context: 2025-12-17 to 2025-12-17
  - (detail_ref: year-5-option-not-reflected-in-contractor-ims)


---

## Confirmed Facts (7)


- **The government schedule records the ATO review as accepted and closed.** - confidence 0.95
  - Confirming source(s): Government tracking schedule
  - Reported value: accepted_closed
  - (detail_ref: accepted-ato-review)


- **The USAspending award record states the current period of performance end date directly.** - confidence 0.95
  - Confirming source(s): USAspending award record
  - Reported value: 2026-12-20
  - (detail_ref: current-period-of-performance-end)


- **The USAspending award record states the current total obligation directly.** - confidence 0.95
  - Confirming source(s): USAspending award record
  - Reported value: 41937842.45
  - (detail_ref: current-total-obligation)


- **The government schedule records the Release 2.0 production deployment milestone.** - confidence 0.95
  - Confirming source(s): Government tracking schedule
  - Reported value: 2022-09-15
  - (detail_ref: release-2-deploy-complete)


- **The government schedule records the Year 2 option exercise milestone.** - confidence 0.95
  - Confirming source(s): Government tracking schedule
  - Reported value: 2022-12-19
  - (detail_ref: year-2-option-exercised)


- **The government schedule records the Year 3 option exercise milestone.** - confidence 0.95
  - Confirming source(s): Government tracking schedule
  - Reported value: 2023-12-11
  - (detail_ref: year-3-option-exercised)


- **The government schedule records the Year 4 option exercise milestone.** - confidence 0.95
  - Confirming source(s): Government tracking schedule
  - Reported value: 2024-12-10
  - (detail_ref: year-4-option-exercised)


---

## Source Streams (4)


- **row=16,task_uid=16,wbs_id=3.2.2,task_name=Release 3.0 Dev & Integration Test,field=finish_date** (`contractor-ims-v1-xlsx`)
  - Location: data-normalization://testbeds/etcc_program_schedule_v0/fixtures/contractor_ims_v1.xlsx
  - Snapshot date: 2026-05-06
  - Content hash: `8fc010f2066c5d9f7cb37878c9c1baed6a5a539faee3737d25441f9b81edde0e`


- **row=16,task_uid=16,wbs_id=3.2.2,task_name=Release 3.0 Dev & Integration Test,field=finish_date** (`gov-tracking-schedule-v1-xlsx`)
  - Location: data-normalization://testbeds/etcc_program_schedule_v0/fixtures/gov_tracking_schedule_v1.xlsx
  - Snapshot date: 2026-05-06
  - Content hash: `ee2a5d9d38c69de807e7a9577441bd5f0e42ba82dbaf0630cbb2eedaf8cf454b`


- **Data-Normalization ETCC proof artifacts** (`src-etcc-proof-artifacts`)
  - Location: (no uri)
  - Snapshot date: 2026-04-16
  - Content hash: `(no hash on this source - internal aggregator)`


- **record=award_json,field=contract_event_ref** (`usaspending-hc104722f0001-award-json`)
  - Location: data-normalization://testbeds/etcc_program_schedule_v0/fixtures/usaspending_HC104722F0001_award.json
  - Snapshot date: 2026-05-06
  - Content hash: `3b9f772ec53ac144f64935d71a5d727e8570395cf0ec3cdbeb82da5ff2b3f1d1`


---

## Needed Decisions (8)


- Confirm how to adjudicate: Government tracking shows the ATO review accepted and closed while the contractor IMS still shows it in progress. (see: ato-review-status-mismatch)

- Decide whether this gap requires contractor schedule correction or documented rationale: P00010-funded work is present in the government schedule but missing from the contractor IMS. (see: funded-scope-missing-from-contractor-ims)

- Confirm how to adjudicate: Government and contractor schedules disagree on the Release 3.0 deployment milestone date. (see: release-3-deploy-date-slip)

- Confirm how to adjudicate: Government tracking shows Release 3.0 development and integration testing slipping past the contractor baseline finish. (see: release-3-dev-baseline-drift)

- Confirm how to adjudicate: Government tracking and the contractor IMS disagree on percent complete for Release 3.0 development and integration testing. (see: release-3-dev-percent-complete-mismatch)

- Confirm how to adjudicate: Government and contractor schedules disagree on the Release 4.0 deployment date. (see: release-4-deploy-date-slip)

- Confirm how to adjudicate: Government tracking and the contractor IMS disagree on percent complete for Release 4.0 development and integration testing. (see: release-4-dev-percent-complete-mismatch)

- Decide whether this gap requires contractor schedule correction or documented rationale: The Year 5 option appears in the government schedule but not in the contractor IMS. (see: year-5-option-not-reflected-in-contractor-ims)


---

## References

Every material claim above carries a `(detail_ref: <id>)` annotation. The
following list enumerates every `detail_ref` emitted in this packet,
alphabetized and deduplicated. Each `detail_ref` resolves to a record in
`source_detail_records.jsonl` in the scenario folder.


- `accepted-ato-review` - The government schedule records the ATO review as accepted and closed.

- `ato-review-status-mismatch` - Government tracking shows the ATO review accepted and closed while the contractor IMS still shows it in progress.

- `current-period-of-performance-end` - The USAspending award record states the current period of performance end date directly.

- `current-total-obligation` - The USAspending award record states the current total obligation directly.

- `funded-scope-missing-from-contractor-ims` - P00010-funded work is present in the government schedule but missing from the contractor IMS.

- `release-2-deploy-complete` - The government schedule records the Release 2.0 production deployment milestone.

- `release-3-deploy-date-slip` - Government and contractor schedules disagree on the Release 3.0 deployment milestone date.

- `release-3-dev-baseline-drift` - Government tracking shows Release 3.0 development and integration testing slipping past the contractor baseline finish.

- `release-3-dev-percent-complete-mismatch` - Government tracking and the contractor IMS disagree on percent complete for Release 3.0 development and integration testing.

- `release-4-deploy-date-slip` - Government and contractor schedules disagree on the Release 4.0 deployment date.

- `release-4-dev-percent-complete-mismatch` - Government tracking and the contractor IMS disagree on percent complete for Release 4.0 development and integration testing.

- `year-2-option-exercised` - The government schedule records the Year 2 option exercise milestone.

- `year-3-option-exercised` - The government schedule records the Year 3 option exercise milestone.

- `year-4-option-exercised` - The government schedule records the Year 4 option exercise milestone.

- `year-5-option-not-reflected-in-contractor-ims` - The Year 5 option appears in the government schedule but not in the contractor IMS.


---

*This packet was generated by `scripts/etcc/generate_conflict_status_packet.js`,
not authored. To reproduce, run the generator against the same scenario bundle
and the same `--date`. See change `add-etcc-conflict-status-packet-v0_2` for the
governing design.*
