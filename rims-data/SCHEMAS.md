# Dataset Schemas (Usage & Relationships)


## Forecasts
### load_forecast.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| date | date (YYYY-MM-DD) | — | Time-series alignment for charts and rollups. |
| region | string | — | Regional demand slices aligned to capacity floor. |
| peak_mw | number | — | Demand baseline vs capacity; adequacy comparison. |
| expected_for_pct | integer | — | — |


## Plants & Capacity


### deliverability_clean.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| project_id | string | — | Join key used to relate to other tables and drive graph links. |
| region | string | — | Regional breakdown for adequacy and filtering. |
| deliverable_by_p50 | date (YYYY-MM-DD) | — | Gates capacity credit by interface readiness (P50/P90). |
| deliverable_by_p90 | date (YYYY-MM-DD) | — | Gates capacity credit by interface readiness (P50/P90). |
| limiting_interface | string | — | — |

**Relationships**

- deliverability_clean.csv.project_id ↔ cod_slippage_report.csv.project_id
- deliverability_clean.csv.project_id ↔ project_cost_curve.csv.project_id
- deliverability_clean.csv.project_id ↔ project_milestones.csv.project_id
- deliverability_clean.csv.project_id ↔ project_permitting.csv.project_id
- deliverability_clean.csv.project_id ↔ project_risks.csv.project_id
- deliverability_clean.csv.project_id ↔ project_supply_chain.csv.project_id
- deliverability_clean.csv.project_id ↔ projects_norm.xlsx.project_id


### plants.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| plant_id | string | Primary plant identifier (string). Example: `PL-00123`. | Join key to link to projects, deliverability, and sustainment. |
| plant_name | string | Plant name (string). Example: `Big River Station`. | Shown in charts/tooltips; used for search, grouping, legends. |
| eia_plant_id | integer | External EIA plant ID (integer). Example: `55678`. | Join key to link to projects, deliverability, and sustainment. | 
| fuel | string | Primary fuel/technology (string). Example: `gas`. | Filters and tech breakdowns; informs reliability/maintenance heuristics. |
| status | string | Operational status (string). Example: `operating`. | Controls inclusion in capacity/adequacy curves; UI coloring. |
| nameplate_mw | integer | Installed nameplate capacity (MW, number). Example: `250`. | Feeds fleet capacity and adequacy margin calculations. |
| reliable_mw | integer | Reliable/deliverable capacity (MW, number). Example: `225`. | Feeds fleet capacity and adequacy margin calculations. |
| condition_score | integer | Condition/health score (integer, unitless). Example: `7`. | Prioritizes inspections/refurbishments; flags at‑risk plants. |
| annual_sustainment_opex_$ | integer | Typical annual sustainment O&M spend (USD, number). Example: `1500000`. | Budget planning and O&M benchmarking. |
| infra_backlog_$ | number | Deferred infrastructure backlog (USD, number). Example: `4200000`. | Backlog visualization and capex planning. |
| forced_outage_rate | number | Historical forced outage rate (fraction 0–1, number). Example: `0.06`. | Reliability overlays and risk scoring. |
| age_years | number | Plant age (years, number). Example: `19`. | Timeline alignment and age‑based planning. |
| id | string | Primary plant identifier (string). Example: `PL-00123`. | Join key to link to projects, deliverability, and sustainment. |
| name | string | Plant name (string). Example: `Big River Station`. | Shown in charts/tooltips; used for search, grouping, legends. |
| capacity_mw | integer | Installed nameplate capacity (MW, number). Example: `250`. | Feeds fleet capacity and adequacy margin calculations. |
| start_date | date (YYYY-MM-DD) | Commercial operation/start date (ISO date). Example: `2003-06-30`. | Timeline alignment and age‑based planning. |
| retire_date | date (YYYY-MM-DD) | Retirement date (ISO date). Example: `2035-12-31`. | Timeline alignment and age‑based planning. |
| owner | string | Owning utility/operator (string). Example: `Acme Power LLC`. | Owner/operator filters. |
| region | string | Balancing area / ISO code (string). Example: `NEISO`. | Regional adequacy, policy scoping, and filters. |

**Relationships**

- plants.csv.plant_id ↔ projects_norm.xlsx.plant_id
- plants.csv.plant_id ↔ sustainment_needs.csv.plant_id
- plants.csv.plant_id ↔ sustainment_work_aligned.csv.plant_id
- plants.csv.plant_id ↔ milestone_resources.csv.plant_id



## Policy & Constraints
### policy_constraints.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| constraint_id | string | Unique constraint ID (string). Example: `PC-00045`. | Joins constraints to affected tasks/assets. |
| scope | string | Scope selector (string): `regional`, `plant`, `fuel`, `project`, `milestone`. Example: `regional`. | Determines which selectors (region/plant/fuel/project/milestone) apply. |
| plant_id | string | Direct scoping to asset/task ID (string). Example: `P-1234`. | Defines scope and targeting for enforcement. |
| plant_name | string | Human‑readable plant name (string). Example: `Big River Station`. | Defines scope and targeting for enforcement. |
| region | string | Region/BA/ISO (string). Example: `MISO`. | Defines scope and targeting for enforcement. |
| fuel | string | Fuel/technology (string). Example: `wind`. | Defines scope and targeting for enforcement. |
| constraint_type | string | Constraint kind (string): Environmental, Seasonal Moratorium, Curfew, Crew Cap, etc. Example: `Seasonal Moratorium`. | Selects rule template and parameter(s) used by the checker. |
| constraint_name | string | Short name (string). Example: `Fish Spawning Moratorium`. | Selects rule template and parameter(s) used by the checker. |
| pattern | string | Wildcard/regex pattern to target items (string). Example: `SW-*`. | Defines scope and targeting for enforcement. |
| value | string | Limit value (string/number). Example: `500`. | Selects rule template and parameter(s) used by the checker. |
| target | string | Metric/field affected (string). Example: `outage_days`. | Selects rule template and parameter(s) used by the checker. |
| effective_from | date (YYYY-MM-DD) | Active start date (ISO). Example: `2025-06-01`. | Limits enforcement to the active window. |
| effective_to | date (YYYY-MM-DD) | Active end date (ISO). Example: `2025-09-30`. | Limits enforcement to the active window. |
| compliance_cost_$ | number | Estimated compliance/mitigation cost (USD, number). Example: `250000`. | Feeds cost‑aware trade‑offs and reporting. |
| enforcement_risk | string | Qualitative risk rating (string). Example: `High`. | Prioritizes alerts and UI emphasis. |
| notes | string | Narrative description and citation/source (string). Example: `Ref: 40 CFR Part 122`. | Displayed in detail panes and exports for auditability. |


## Project & Schedule

### cod_slippage_report.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| project_id | string | Unique project identifier. Example: `PRJ-200`. | Joins report rows to `projects_norm.csv` for schedule and metadata context. |
| name | string | Project display name. Example: `BESS Expansion 200 MW`. | Used in tables, charts, and filters for readability. |
| fuel | string | Primary fuel/technology. Example: `Battery`, `Gas`, `Wind`. | Supports technology breakdowns and policy‑sensitive views. |
| region | string | ISO/BA/region code. Example: `ERCOT`, `MISO`, `PJM`. | Enables regional grouping and capacity adequacy comparisons. |
| baseline_end_for_comparison | date (YYYY-MM-DD) | Baseline COD used for slip comparison (ISO date). Example: `2026-06-30`. | Defines the reference finish date against which slippage is measured. |
| cod_p50 | date (YYYY-MM-DD) | Current projected COD at P50 (ISO date). Example: `2026-08-15`. | Midpoint estimate for commercial operation; used in variance analysis. |
| cod_p90 | date (YYYY-MM-DD) | Current projected COD at P90 (ISO date). Example: `2026-10-01`. | Conservative COD estimate; used for risk‑aware planning. |
| p50_slip_days_vs_baseline | integer | Difference between `cod_p50` and baseline end (days, integer). Example: `+46`. | Quantifies schedule variance; positive = slip, negative = pull‑in. |
| p90_slip_days_vs_baseline | integer | Difference between `cod_p90` and baseline end (days, integer). Example: `+93`. | Measures conservative schedule variance for risk dashboards. |
| gating_driver_p50 | string | Primary driver of P50 variance (string). Example: `Long‑lead transformer`. | Explains root cause of mid‑range schedule movement. |
| gating_driver_p90 | string | Primary driver of P90 variance (string). Example: `Permit appeal window`. | Explains worst‑case gating that pushes COD later. |
| risk_days_added_p50 | integer | Risk‑attributed delay included in P50 (days, integer). Example: `18`. | Shows how modeled risks translate into mid‑range delay. |
| risk_days_added_p90 | integer | Risk‑attributed delay included in P90 (days, integer). Example: `45`. | Shows risk stacking under conservative assumptions. |


**Relationships**

- cod_slippage_report.csv.project_id ↔ deliverability_clean.csv.project_id
- cod_slippage_report.csv.project_id ↔ project_cost_curve.csv.project_id
- cod_slippage_report.csv.project_id ↔ project_milestones.csv.project_id
- cod_slippage_report.csv.project_id ↔ project_permitting.csv.project_id
- cod_slippage_report.csv.project_id ↔ project_risks.csv.project_id
- cod_slippage_report.csv.project_id ↔ project_supply_chain.csv.project_id
- cod_slippage_report.csv.project_id ↔ projects_norm.xlsx.project_id


### dependencies_norm.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| source_work_item_id | string | ID of the predecessor task (string). Accepts canonical `P-*` / `SW-*` / `M-*` or source IDs. Example: `SW-1043`. | Canonicalized and validated; edge emitted only when both tasks exist. |
| target_work_item_id | string | ID of the successor task (string). Accepts canonical `P-*` / `SW-*` / `M-*` or target IDs. Example: `M-MS_proj_010_commissioning`. | Canonicalized and validated; edge emitted only when both tasks exist. |
| type | string | Precedence relationship: `FS` (Finish‑Start), `SS` (Start‑Start), `FF` (Finish‑Finish), `SF` (Start‑Finish); also accepts 0/1/2/3. Example: `SS`. | Selects which endpoints are tied (FS/SS/FF/SF) for feasibility and ripple checks. |
| min_lag_d | integer | Minimum enforced lag (calendar days, integer). Negative allowed for overlap. Example: `-2`. | Offsets successor timing; drives earliest/latest dates and slack. |
| typ_lag_d | integer | Typical/target lag (calendar days, integer). Example: `5`. | Offsets successor timing; drives earliest/latest dates and slack. |
| max_lag_d | integer | Maximum permissible lag (calendar days, integer). Example: `30`. | Offsets successor timing; drives earliest/latest dates and slack. |
| critical | boolean | Marks dependency as critical to path integrity (boolean). Example: `true`. | Elevates visualization and gating in risk analysis. |
| note | string | Rationale or comment (string). Example: `Cure concrete before electrical tie‑in`. | Shown in dependency inspector and exports for explainability. |
| policy_dep_type | string | Policy‑driven dependency category (string). Example: `environmental`. | Filters/tagging for policy‑origin edges; supports compliance views. |
| constraint_id | string | Reference to `policy_constraints.csv` (string). Example: `PC-0042`. | Joins to policy constraints for audit trail and rule inspection. |
| window_id | string | Shared outage/window identifier (string). Example: `WIN-Q3-2026-A`. | Groups tasks that must align within the same outage/time window. |

- `dependencies_norm.csv.source_work_item_id` ↔ `project_milestones.csv.milestone_id` *(when ID is `M-*`)*  
- `dependencies_norm.csv.target_work_item_id` ↔ `project_milestones.csv.milestone_id` *(when ID is `M-*`)*  
- `dependencies_norm.csv.source_work_item_id` ↔ `sustainment_work.csv.sustain_work_id` *(when ID is `SW-*`)*  
- `dependencies_norm.csv.target_work_item_id` ↔ `sustainment_work.csv.sustain_work_id` *(when ID is `SW-*`)*  
- *(If you emit project-level tasks)* `dependencies_norm.csv.(source|target)_work_item_id` ↔ `projects_norm.csv.project_id` *(when ID is `P-*`)*
- `dependencies_norm.csv.constraint_id` ↔ `policy_constraints.csv.constraint_id`  
- `dependencies_norm.csv.policy_dep_type` ↔ `policy_constraints.csv.constraint_type` *(categorical alignment; optional)*
- `dependencies_norm.csv.window_id` ↔ `sustainment_dependencies.csv.shared` *(group edges that must align within the same outage/constraint window)*
- `dependencies_norm.csv.(source|target)_work_item_id` ↔ `milestone_resources.csv.milestone_id` *(for edges referencing `M-*` milestones)*
**Semantics of `type`**  
- `FS` (Finish→Start), `SS` (Start→Start), `FF` (Finish→Finish), `SF` (Start→Finish). Numeric aliases: `0=FS, 1=SS, 2=FF, 3=SF`.

### project_cost_curve.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| project_id | string | Unique project identifier. Example: `PRJ-200`. | Primary join key linking to `projects_norm.csv`; ensures cost curve alignment with project scope and schedule. |
| date | date (YYYY-MM-DD) | Cost time series date. Example: `2026-06-30`. | Defines temporal resolution for cost accumulation and rollup over project duration. |
| capex_$ | integer | Capital expenditure (USD). Example: `25000000`. | Used in cumulative CAPEX curves, portfolio cost projections, and ROI analysis. |
| opex_$ | integer | Operating expenditure (USD). Example: `750000`. | Used in lifecycle OPEX rollups and annualized cost reporting. |
| idc_$ | integer | Interest during construction (USD). Example: `1250000`. | Integrated into total project financing cost; influences NPV and WACC calculations. |
| category | string | Classification of cost bucket or activity. Example: `Construction`, `Engineering`, `Commissioning`. | Enables segmentation of spending across phases for burn‑down and variance analysis. |
| note | string | Free‑text comments or annotations. Example: `Includes substation upgrade scope.` | Adds context in dashboards and cost curve exports. |
| base_cost_year | integer | Reference year for cost normalization. Example: `2024`. | Used for inflation adjustment and cost escalation modeling. |
| currency | string | Currency code (ISO 4217). Example: `USD`, `CAD`. | Used for currency conversion and unified reporting across projects. |


**Relationships**

- project_cost_curve.csv.project_id ↔ cod_slippage_report.csv.project_id
- project_cost_curve.csv.project_id ↔ deliverability_clean.csv.project_id
- project_cost_curve.csv.project_id ↔ project_milestones.csv.project_id
- project_cost_curve.csv.project_id ↔ project_permitting.csv.project_id
- project_cost_curve.csv.project_id ↔ project_risks.csv.project_id
- project_cost_curve.csv.project_id ↔ project_supply_chain.csv.project_id
- project_cost_curve.csv.project_id ↔ projects_norm.xlsx.project_id



### project_milestones.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| milestone_id | string | Unique milestone identifier. Example: `MS_proj_009_engineering`. | Primary key; used to join with dependencies and link milestones to parent projects. |
| project_id | string | Parent project identifier. Example: `PRJ-200`. | Joins milestone to project scope; enables aggregation and project-level timeline views. |
| name | string | Milestone name. Example: `Engineering Complete`, `Notice to Proceed`, `COD`. | Shown as label in Gantt views, tables, and milestone summaries. |
| type | string | Milestone classification (e.g., `Design`, `Construction`, `Permitting`, `Commercial`). | Drives color-coding, grouping, and phase logic. |
| planned_start_p50 | date (YYYY-MM-DD) | Expected start date (median, P50). Example: `2025-03-01`. | Defines baseline schedule window and used in uncertainty band generation. |
| planned_start_p90 | date (YYYY-MM-DD) | Conservative start date (P90). Example: `2025-03-15`. | Defines late-start bound for probabilistic schedule forecasts. |
| planned_end_p50 | date (YYYY-MM-DD) | Expected finish date (median, P50). Example: `2025-07-30`. | Primary target for schedule visualization and status calculations. |
| planned_end_p90 | date (YYYY-MM-DD) | Conservative finish date (P90). Example: `2025-08-10`. | Defines late-finish bound in probabilistic Gantt views. |
| duration_days_p50 | integer | Expected duration (calendar days) at P50 confidence. Example: `45`. | Used to compute nominal milestone span and baseline windows. |
| duration_days_p90 | integer | Expected duration (calendar days) at P90 confidence. Example: `60`. | Used for uncertainty envelopes and risk-based schedule adjustments. |
| gating_flag | boolean | Indicates whether milestone must complete before successors start. Example: `true`. | Enforces dependency gating in critical-path and feasibility logic. |
| external_dependency | string | Linked project or external dependency name. Example: `Transmission Line Energized`. | Connects cross-project dependencies and external gating events. |
| notes | string | Context, assumptions, or source references. Example: `Requires environmental clearance before mobilization.` | Displayed in milestone detail panels, tooltips, and reports. |
| _name_lower | string | Lower-cased normalized version of `name`. Example: `engineering complete`. | Used for duplicate detection, text joins, and case-insensitive matching. |
| _end | number | Internal end timestamp (epoch ms). Example: `1732924800000`. | Enables numeric comparison for sorting and dependency graph layout. |
| _start | number | Internal start timestamp (epoch ms). Example: `1730563200000`. | Enables numeric comparison for sorting and dependency graph layout. |
| _dt_pref | date (YYYY-MM-DD) | Preferred display date, typically midpoint between P50/P90 or latest update. Example: `2025-05-15`. | Used for unified milestone placement in hybrid deterministic/probabilistic timelines. |
| owner_id | string | Responsible organization or individual ID. Example: `ENG_TEAM_A`. | Joins to responsibility matrix for accountability tracking. |
| owner_name | string | Responsible organization or individual name. Example: `Engineering Division A`. | Shown in milestone dashboards and progress reports. |
| progress_pct | number | Completion percentage (0–100). Example: `80`. | Used for progress bars and phase completion summaries. |
| status | string | Current lifecycle status. Example: `Planned`, `In Progress`, `Complete`, `Deferred`. | Controls display state, highlighting, and filtering. |
| last_update_utc | date (YYYY-MM-DD) | Timestamp of last modification (UTC). Example: `2025-01-17T22:45Z`. | Enables sync logic and staleness detection for milestone records. |
| _phase_order | integer | Sequence index for phase ordering. Example: `3`. | Determines left-to-right milestone order within project phases. |
| _dur_days | number | Calculated duration (calendar days) derived from `_end – _start`. Example: `42`. | Used for rendering bar length and computing actual vs. planned variance. |
| filled_start | date (YYYY-MM-DD) | Auto-inferred or backfilled start date when explicit values are missing. Example: `2025-06-01`. | Used for baseline estimation or visualization when start data incomplete. |
| filled_end | date (YYYY-MM-DD) | Auto-inferred or backfilled end date when explicit values are missing. Example: `2025-07-15`. | Used for baseline estimation or visualization when end data incomplete. |


**Relationships**

- project_milestones.csv.project_id ↔ cod_slippage_report.csv.project_id
- project_milestones.csv.project_id ↔ deliverability_clean.csv.project_id
- project_milestones.csv.project_id ↔ project_cost_curve.csv.project_id
- project_milestones.csv.project_id ↔ project_permitting.csv.project_id
- project_milestones.csv.project_id ↔ project_risks.csv.project_id
- project_milestones.csv.project_id ↔ project_supply_chain.csv.project_id
- project_milestones.csv.project_id ↔ projects_norm.xlsx.project_id
- project_milestones.csv.milestone_id ↔  milestone_resources.csv.milestone_id 


### project_permitting.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| permit_id | string | Join key: unique permit record for a given project. (Used in **V1a**.) | Joins permits to the project and milestones. |
| project_id | string | Join key: unique permit record for a given project. (Used in **V1a**.) | Joins permits to the project and milestones. |
| authority | string | Permitting authority/agency (free text or code). (Used in **V1a**.) | Displayed for accountability and jurisdiction filters. |
| application_date | date (YYYY-MM-DD) | Permit attribute used in schedule gating and readiness. (Used in **V1a**.) |  |
| expected_approval_p50 | date (YYYY-MM-DD) | Expected approval date (P50/P90). (Used starting **V2**.) | Feeds feasibility windows and P50/P90 visuals. |
| expected_approval_p90 | date (YYYY-MM-DD) | Expected approval date (P50/P90). (Used starting **V2**.) | Feeds feasibility windows and P50/P90 visuals. |
| status | string | Permit lifecycle state (e.g., planned, submitted, in review, granted, denied). (Used in **V1a**.) | Controls alerts and pending‑approval flags. |
| gating_milestone_id | string | Milestone blocked until approval (joins to project_milestones). (Used in **V1a**.) | Locks the milestone until approval is received. |
| notes | string | Narrative context, conditions, contingencies. (Used in **V1a**.) | Shows review history and conditions. |

**Relationships**

- project_permitting.csv.project_id ↔ cod_slippage_report.csv.project_id
- project_permitting.csv.project_id ↔ deliverability_clean.csv.project_id
- project_permitting.csv.project_id ↔ project_cost_curve.csv.project_id
- project_permitting.csv.project_id ↔ project_milestones.csv.project_id
- project_permitting.csv.project_id ↔ project_risks.csv.project_id
- project_permitting.csv.project_id ↔ project_supply_chain.csv.project_id
- project_permitting.csv.project_id ↔ projects_norm.xlsx.project_id
- `project_permitting.csv.gating_milestone_id` → emits **FS** edges into `dependencies_norm` targeting `project_milestones.milestone_id`


### project_risks.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| risk_id | string | Join key linking a risk to its project. (Used starting **V2**.) | Joins risks to project views and exports. |
| project_id | string | Join key linking a risk to its project. (Used starting **V2**.) | Joins risks to project views and exports. |
| category | string | Risk classification (e.g., schedule, cost, supply, permit). (Used starting **V2**.) | Enables heatmap/group analysis. |
| description | string | Narrative description/details. (Used starting **V2**.) | Context for review and exports. |
| probability | number | Risk probability (0–100%). (Used starting **V2**.) | Feeds risk scoring and prioritization. |
| impact_days_p50 | integer | Schedule impact if realized (calendar days). (Used starting **V2**.) | Used in schedule contingency modeling. |
| impact_days_p90 | integer | Schedule impact if realized (calendar days). (Used starting **V2**.) | Used in schedule contingency modeling. |
| impact_cost_$ | integer | Cost impact if realized (USD). (Used starting **V3**.) | Used in cost contingency modeling. |
| affected_milestone_id | string | Project risk attribute for tracking and reporting. (Used starting **V2**.) |  |
| response | string | Planned response/mitigation action. (Used starting **V2**.) | Tracks actions and status. |
| owner | string | Risk owner (person or team). (Used starting **V2**.) | Accountability and routing. |
| notes | string | Narrative description/details. (Used starting **V2**.) | Context for review and exports. |

**Relationships**

- project_risks.csv.project_id ↔ cod_slippage_report.csv.project_id
- project_risks.csv.project_id ↔ deliverability_clean.csv.project_id
- project_risks.csv.project_id ↔ project_cost_curve.csv.project_id
- project_risks.csv.project_id ↔ project_milestones.csv.project_id
- project_risks.csv.project_id ↔ project_permitting.csv.project_id
- project_risks.csv.project_id ↔ project_supply_chain.csv.project_id
- project_risks.csv.project_id ↔ projects_norm.xlsx.project_id


### project_supply_chain.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| component_id | string | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |
| project_id | string | Project ID (string). Example: `PRJ-200`. | Parents component under project. |
| component_name | string | Component or package name (string). Example: `HV Transformer T1`. | Labeling, vendor mix, and aggregation. |
| vendor | string | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |
| alt_vendors | string | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |
| lead_time_days_p50 | integer | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |
| lead_time_days_p90 | integer | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |
| expedite_days | integer | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |
| on_critical_path | boolean | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |
| dependent_milestone_id | string | Milestone gated by component (string). Example: `MS_proj_200_install_ready`. | Creates FS edge component → milestone. |
| notes | string | Notes (string). Example: `Expedite available at +15%`. | Explainability and governance. |
| _key | string | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |
| component_owner_id | string | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |
| component_owner_name | string | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |
| component_progress_pct | integer | Supply item status/progress (string/integer). Example: `in‑progress`. | Risk and cost analysis; dashboard KPIs. |
| status | string | Supply item status/progress (string/integer). Example: `in‑progress`. | Risk and cost analysis; dashboard KPIs. |
| last_update_utc | date (YYYY-MM-DD) | Supply‑chain attribute. Example: `metadata`. | Used in supply‑chain gating. |

**Relationships**

- project_supply_chain.csv.project_id ↔ cod_slippage_report.csv.project_id
- project_supply_chain.csv.project_id ↔ deliverability_clean.csv.project_id
- project_supply_chain.csv.project_id ↔ project_cost_curve.csv.project_id
- project_supply_chain.csv.project_id ↔ project_milestones.csv.project_id
- project_supply_chain.csv.project_id ↔ project_permitting.csv.project_id
- project_supply_chain.csv.project_id ↔ project_risks.csv.project_id
- project_supply_chain.csv.project_id ↔ projects_norm.xlsx.project_id
- `project_supply_chain.csv.dependent_milestone_id` → emits **FS** edges into `dependencies_norm` targeting `project_milestones.milestone_id`  


### projects_norm.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| project_id | string | Unique project identifier (string). Example: `PRJ-200`. | Primary join key linking projects to milestones, dependencies, and supply chain data. |
| name | string | Project name (string). Example: `BESS Expansion 200 MW`. | Displayed in dashboards, exports, and Gantt timelines. |
| program | string | Program or portfolio grouping (string). Example: `Reliability` or `Decarbonization`. | Used for filtering, aggregation, and program-level reporting. |
| type | string | Project type classification (string). Example: `Build`, `Repower`, `Retire`. | Used in planning and categorization for summaries and comparisons. |
| fuel | string | Primary fuel or technology (string). Example: `Battery`, `Gas`, `Solar`. | Used for grouping, policy analysis, and capacity modeling. |
| plant_id | string | Host plant identifier (string). Example: `PL-00123`. | Joins to plants.csv; anchors project at a specific facility. |
| plant_name | string | Plant name (string). Example: `Big River Station`. | Displayed in project tables and visualizations; used for validation. |
| eia_plant_id | string | Official EIA (Energy Information Administration) plant ID (integer). Example: `5543`. | Used for regulatory cross-referencing and integration with public datasets. |
| start_planned | string | Planned project start date (ISO date). Example: `2025-01-15`. | Defines project initiation window for scheduling. |
| end_planned | string | Planned or target project end date (ISO date). Example: `2026-11-30`. | Defines completion milestone; key for schedule variance tracking. |
| status | string | Current project lifecycle state (string). Example: `Active`, `Delayed`, `Cancelled`. | Drives dashboard filters and milestone color coding. |
| capacity_mw | string | Installed capacity in MW (number). Example: `200`. | Used in fleet aggregation, adequacy curves, and performance summaries. |
| subtype | string | Sub‑classification within project type (string). Example: `Hybrid`, `Peaker`, `Transmission`. | Supports finer categorization in dashboards and reports. |
| delta_nameplate_mw | string | Change in nameplate capacity (MW, number). Example: `+200`. | Used in capacity delta rollups and adequacy simulations. |
| delta_reliable_mw | string | Change in reliable capacity (MW, number). Example: `+180`. | Feeds reliability assessments and system adequacy models. |
| capex_$ | string | Capital expenditure (USD). Example: `350000000`. | Used in capital budget summaries and ROI calculations. |
| annual_opex_$ | string | Annual operating expenditure (USD). Example: `5000000`. | Feeds cost forecasting and lifecycle O&M analysis. |
| start_p50 | string | Projected start date percentile (ISO date). Example: `2025-02-01` (start_p50). | Used for schedule risk analysis and scenario comparison. |
| start_p90 | string | Projected start date percentile (ISO date). Example: `2025-02-01` (start_p90). | Used for schedule risk analysis and scenario comparison. |
| end_p50 | string | Projected or target end date percentile (ISO date). Example: `2026-10-31` (end_p50). | Used for P50/P90 uncertainty analysis in completion forecasts. |
| end_p90 | string | Projected or target end date percentile (ISO date). Example: `2026-10-31` (end_p90). | Used for P50/P90 uncertainty analysis in completion forecasts. |
| schedule_risk_pct | string | Probability‑weighted schedule risk (%, number). Example: `15`. | Used to quantify likelihood of project delay; feeds Monte Carlo models. |
| outage_profile_id | string | Reference to outage or maintenance profile (string). Example: `OP‑BESS‑1`. | Used for linking downtime assumptions to fleet adequacy models. |
| regulatory_needed | string | Boolean/flag for regulatory approval requirement (1/0). Example: `1`. | Used to identify projects needing permits or commission review. |
| permit_lead_months | string | Estimated lead time for permits (months). Example: `9`. | Informs pre‑construction readiness and Gantt dependency modeling. |
| end_target_p50 | string | Projected or target end date percentile (ISO date). Example: `2026-10-31` (end_target_p50). | Used for P50/P90 uncertainty analysis in completion forecasts. |
| end_target_p90 | string | Projected or target end date percentile (ISO date). Example: `2026-10-31` (end_target_p90). | Used for P50/P90 uncertainty analysis in completion forecasts. |
| capex_total_p50_$ | string | Total capital cost at P50 confidence level (USD). Example: `375000000`. | Feeds cost range visualization and contingency modeling. |
| capex_total_p90_$ | string | Total capital cost at P90 confidence level (USD). Example: `375000000`. | Feeds cost range visualization and contingency modeling. |
| owners_costs_$ | string | Owner's internal costs (USD). Example: `12000000`. | Used for total project cost aggregation and accounting. |
| contingency_pct | string | Contingency percentage (%). Example: `10`. | Applied to CAPEX for cost risk modeling. |
| annual_opex_fixed_$ | string | Annual fixed O&M cost (USD). Example: `4500000`. | Used in long‑term operating cost models. |
| var_opex_$perMWh | string | Variable O&M cost (USD/MWh). Example: `2.5`. | Used in dispatch simulations and financial optimization. |
| wacc_pct | string | Weighted average cost of capital (%, number). Example: `6.5`. | Used for discounted cash flow and NPV calculations. |
| tax_rate_pct | string | Effective tax rate (%, number). Example: `25`. | Used in project financial modeling and net cost estimation. |
| book_life_years | string | Book depreciation life (years, number). Example: `30`. | Determines financial depreciation and accounting schedules. |
| base_cost_year | string | Cost reference year for inflation adjustments (integer). Example: `2023`. | Used in escalation modeling and cost normalization. |
| currency | string | Currency code (string). Example: `USD`. | Used for financial rollups and cross‑currency normalization. |

**Relationships**

- projects_norm.xlsx.plant_id ↔ plants.csv.plant_id
- projects_norm.xlsx.plant_id ↔ sustainment_needs.csv.plant_id
- projects_norm.xlsx.plant_id ↔ sustainment_work_aligned.csv.plant_id
- projects_norm.xlsx.project_id ↔ cod_slippage_report.csv.project_id
- projects_norm.xlsx.project_id ↔ deliverability_clean.csv.project_id
- projects_norm.xlsx.project_id ↔ project_cost_curve.csv.project_id
- projects_norm.xlsx.project_id ↔ project_milestones.csv.project_id
- projects_norm.xlsx.project_id ↔ project_permitting.csv.project_id
- projects_norm.xlsx.project_id ↔ project_risks.csv.project_id
- projects_norm.xlsx.project_id ↔ project_supply_chain.csv.project_id
- projects_norm.csv.project_id ↔  milestone_resources.csv.project_id

### milestone_resources.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| milestone_id | string | Milestone this resource supports. Example: `MS_proj_009_engineering`. | Primary join to `project_milestones.csv`; lets the allocator and Gantt tie staffing to specific timeline bars. |
| resource_id | string | Resource/skill identifier (canonical skill name). Example: `Electrical HV`, `Relay Tech`, `Civil`. | Joins to `resource_capacity.csv.skill` and `resource_rates.csv.skill` for availability and cost. |
| qty_required_per_day | integer | Number of heads/crews required per working day. Example: `2`. | Multiplied by duration to compute effort and to check capacity conflicts. |
| duration_days | integer | Run length of the resource assignment (**calendar days**). Example: `10`. | Determines envelope width in staffing views; participates in overlap checks. |
| start_offset_days | integer | Offset from the milestone’s start (**calendar days**). Example: `-3` (pre-stage), `0`, `+2`. | Aligns pre-staging or lagged crews; feeds feasibility and ripple timing. |
| notes | string | Free-text context/assumptions. Example: `Weekend work 1.5×; requires outage window.` | Aids coordination, export governance, and audit trails. |
| project_id | string | Back-reference to parent project. Example: `PRJ-200`. | Enables portfolio views and direct joins to project cost/permit/supply data. |
| plant_id | string | Host plant identifier. Example: `PL-00123`. | Aligns staffing to site; joins to plant region/fuel for policy checks and routing. |
| region | string | ISO/BA/region code. Example: `ERCOT`, `MISO`, `PJM`. | Aligns with regional capacity, rates, and policy constraints in planning. |

**Relationships**

- milestone_resources.csv.milestone_id ↔ project_milestones.csv.milestone_id  
- milestone_resources.csv.project_id ↔ projects_norm.csv.project_id  
- milestone_resources.csv.plant_id ↔ plants.csv.plant_id  
- milestone_resources.csv.region ↔ plants.csv.region  
- milestone_resources.csv.resource_id ↔ resource_capacity.csv.skill  
- milestone_resources.csv.resource_id ↔ resource_rates.csv.skill  
- milestone_resources.csv.milestone_id ↔ dependencies_norm.csv.(source_work_item_id | target_work_item_id) *(where the ID is an `M-*` milestone)*

## Resources

### resource_capacity.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| region | string | Region/BA/ISO code (string). Example: `ERCOT`, `PJM`, `MISO`. | Aligns capacity with project/plant region to avoid cross‑region double counting. |
| skill | string | Skill or crew type (string). Example: `civil_foundation`, `electrical_hv`, `env_permitting`. | Matches demand by skill in staffing solver and dashboards. |
| max_concurrent | integer | Maximum number of parallel crews/heads available concurrently (integer). Example: `3`. | Caps simultaneous assignments for this skill across all active work. |
| notes | string | Narrative notes or sizing method (string). Example: `synthetic sizing based on portfolio scale`. | Context for planners; appears in staffing reports and exports. |
| owner_id | number | Owning org/vendor identifier (number/string as provided). Example: `101`. | Groups capacity by provider for contracting and SLA reporting. |


**Relationships**

- resource_capacity.csv.skill ↔ resource_rates.csv.skill
- resource_capacity.csv.skill ↔ sustainment_work_resources.csv.resource_id
- resource_capacity.csv.region ↔ plants.csv.region
- resource_capacity.csv.skill ↔ milestone_resources.csv.resource_id


### resource_rates.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| region | string | Region/BA/ISO code (string). Example: `PJM`, `MISO`, `ERCOT`. | Used to match resource market/location with plant/project region during planning. |
| skill | string | Skill or labor category (string). Example: `Relay Tech`, `Electrical HV`, `Civil`. | Used for skill-to-demand matching and to aggregate cost/availability by trade. |
| rate_$per_day | number | Base billing rate (USD per day). Example: `1200`. | Drives cost estimation for day-based resource plans and what‑ifs. |
| overtime_mult | number | Overtime pay multiplier (number). Example: `1.5`. | Applied to base rate for overtime hours/days when calculating costs. |
| base_cost_year | integer | Price basis year for rates (integer, YYYY). Example: `2024`. | Supports cost normalization and escalation to current dollars. |
| currency | string | Currency code (string). Example: `USD`. | Used for financial rollups and conversion where multi‑currency is present. |
| owner_id | string | Owning org/vendor/contractor identifier (string). Example: `VEND‑045`. | Used to group rates by supplier and to enforce vendor‑specific terms. |

**Relationships**

- resource_rates.csv.skill ↔ resource_capacity.csv.skill
- resource_rates.csv.skill ↔ sustainment_work_resources.csv.resource_id
- resource_rates.csv.region ↔ plants.csv.region
- resource_rates.csv.skill ↔ milestone_resources.csv.resource_id

### `sustainment_needs_combined.csv`

### Columns
| Column | Type | Required | Description |
|---|---|---:|---|
| `need_id` | string | ✓ | Globally unique sustainment need ID (stable across recurrences). |
| `plant_id` | string | ✓ | Host plant ID; join key to `plants.csv`. |
| `category` | string | ✓ | Work category (e.g., “HRSG tube cleaning”, “Generator inspection”). |
| `cadence_days` | integer | ✓ | Recurrence interval in **days**. |
| `default_duration_days` | integer | ✓ | Default duration for a single occurrence in **calendar days**. |
| `anchor` | string |  | Recurrence anchor: `"plantStart" \| "plantStartPlusLead" \| "firstOfMonth"`. |
| `resources_json` | JSON (array) | ✓ | Resource plan **template** for any occurrence. See shape below. |
| `supply_chain_json` | JSON (array) | ✓ | Components/vendors/lead times/costs for this need. See shape below. |
| `dep_templates_json` | JSON (array) | ✓ | Dependency **templates** to instantiate per occurrence. |
| `policy_refs_json` | JSON (array) |  | Policy/moratoria references constraining scheduling. |
| `occurrence_cost_usd` | number | ✓ | Typical per-occurrence cost. |
| `risk_if_deferred` | enum | ✓ | One of `High`, `Medium`, `Low`. |
| `notes` | string | ✓ | Free text context; may be auto-filled if not provided. |

---

This section defines the **per-item schemas** for the JSON columns inside `sustainment_needs_combined.csv`. 

---

## `resources_json` (array of objects)

A resource plan **template** for any occurrence of the sustainment need. Each object represents one resource type required concurrently during the occurrence.

### Item Shape
| Field | Type | Required | Description | Constraints / Notes | Default |
|---|---|---:|---|---|---|
| `resource_id` | string | ✓ | Canonical resource type identifier (e.g., `mech_crew`, `I&C`, `electrician`). | Must match the allowed resource catalog / enum used for capacity modeling. Case-insensitive; stored lowercase except well-known mixed-case (`I&C`). | — |
| `qty_per_day` | number | ✓ | Average daily capacity required from this resource during the active window. | `> 0`. Use fractional values for partial allocation (e.g., `1.5` FTE). | — |
| `duration_days` | integer | ✓ | Number of calendar days this resource is needed for the occurrence. | `>= 1`. Often equals `default_duration_days`; can differ for staggered resources. | `default_duration_days` of need if omitted |
| `start_offset_days` | integer |  | Offset (days) from the occurrence **start** before this resource begins. | `>= 0`. Enables phased starts (e.g., scaffolding before main work). | `0` |



---

## `supply_chain_json` (array of objects)

Procurement items required to execute the need, not necessarily for every occurrence; treat as **per-need baseline** to seed per-occurrence procurement tasks.

### Item Shape
| Field | Type | Required | Description | Constraints / Notes | Default |
|---|---|---:|---|---|---|
| `component` | string | ✓ | Specific material/assembly/service (e.g., `Combustor liner kit`, `CT gearbox`). | Prefer concrete items over generic labels. | — |
| `vendor_type` | string | ✓ | Supplier class (e.g., `OEM/aftermarket`, `Instrumentation`, `Fabricator`). | Use controlled list for analytics; case-insensitive normalization. | — |
| `lead_time_days` | integer | ✓ | Typical procurement lead time. | `>= 0`. Used to back-schedule procurement tasks. | — |
| `expedite_days` | integer |  | Expedite lead time (if paid/rushed). | `>= 0` and `<= lead_time_days`. If present, enables mitigation options. | — |
| `unit_cost_estimate_usd` | number |  | Estimated unit cost for modeling. | `>= 0`. Use ranges in upstream curation; store the working estimate here. | — |
| `procurement_start_date` | string (YYYY-MM-DD) |  | Known/required start date for procurement activities (per program). | ISO-8601 date or empty. Per-need milestone; occurrence-specific dates are derived later. | — |


---

## `dep_templates_json` (array of objects)

Dependency **templates** that will be instantiated into concrete edges for each dated occurrence. Use to tie the work to procurement, milestones, or prior recurrences.

### Item Shape
| Field | Type | Required | Description | Constraints / Notes | Default |
|---|---|---:|---|---|---|
| `to_kind` | enum | ✓ | Target type for the dependency. | Allowed: `procurement`, `previous_occurrence`, `need`, `milestone`. | — |
| `to_ref` | string \| null |  | Identifier for the target when `to_kind` is `need` or `milestone`. | When `to_kind` = `procurement` or `previous_occurrence`, `to_ref` is typically `null`. | `null` |
| `type` | enum | ✓ | Dependency type between the **from** occurrence and the **to** target. | Allowed: `FS`, `SS`, `FF`, `SF`. | `FS` |
| `lag_days` | integer |  | Lag applied to the edge (positive = delay, negative = overlap). | Can be negative for overlap constraints; typically small integer. | `0` |
| `severity` | enum |  | Constraint severity. | Allowed: `hard`, `soft`. Soft edges may be violated with penalty. | `hard` |


---

## `policy_refs_json` (array of objects)

Links to policies/moratoria/blackout windows that constrain scheduling (e.g., environmental windows, seasonal peaks, capital freeze).

### Item Shape
| Field | Type | Required | Description | Constraints / Notes | Default |
|---|---|---:|---|---|---|
| `policy_id` | string |  | Direct reference to a policy in your policy catalog. | Mutually exclusive with `pattern` (provide one of them). | — |
| `pattern` | string |  | Pattern (glob/regex) that matches a set of policy windows. | Mutually exclusive with `policy_id`. | — |
| `severity` | enum |  | Constraint strength. | Allowed: `hard`, `soft`. | `hard` |



**Relationships**

- sustainment_needs.csv.plant_id ↔ plants.csv.plant_id
- sustainment_needs.csv.plant_id ↔ projects_norm.xlsx.plant_id
- sustainment_needs.csv.plant_id ↔ sustainment_work_aligned.csv.plant_id
- sustainment_needs.csv.plant_id ↔ work_items.csv.plant_id


### projects_norm.csv

| Column | Type | Description | How It's Used |
|---|---|---|---|
| project_id | string | Unique project identifier (string). Example: `PRJ-200`. | Primary join key linking projects to milestones, dependencies, and supply chain data. |
| name | string | Project name (string). Example: `BESS Expansion 200 MW`. | Displayed in dashboards, exports, and Gantt timelines. |
| program | string | Program or portfolio grouping (string). Example: `Reliability` or `Decarbonization`. | Used for filtering, aggregation, and program-level reporting. |
| type | string | Project type classification (string). Example: `Build`, `Repower`, `Retire`. | Used in planning and categorization for summaries and comparisons. |
| fuel | string | Primary fuel or technology (string). Example: `Battery`, `Gas`, `Solar`. | Used for grouping, policy analysis, and capacity modeling. |
| plant_id | string | Host plant identifier (string). Example: `PL-00123`. | Joins to plants.csv; anchors project at a specific facility. |
| plant_name | string | Plant name (string). Example: `Big River Station`. | Displayed in project tables and visualizations; used for validation. |
| eia_plant_id | string | Official EIA (Energy Information Administration) plant ID (integer). Example: `5543`. | Used for regulatory cross-referencing and integration with public datasets. |
| start_planned | string | Planned project start date (ISO date). Example: `2025-01-15`. | Defines project initiation window for scheduling. |
| end_planned | string | Planned or target project end date (ISO date). Example: `2026-11-30`. | Defines completion milestone; key for schedule variance tracking. |
| status | string | Current project lifecycle state (string). Example: `Active`, `Delayed`, `Cancelled`. | Drives dashboard filters and milestone color coding. |
| capacity_mw | string | Installed capacity in MW (number). Example: `200`. | Used in fleet aggregation, adequacy curves, and performance summaries. |
| subtype | string | Sub‑classification within project type (string). Example: `Hybrid`, `Peaker`, `Transmission`. | Supports finer categorization in dashboards and reports. |
| delta_nameplate_mw | string | Change in nameplate capacity (MW, number). Example: `+200`. | Used in capacity delta rollups and adequacy simulations. |
| delta_reliable_mw | string | Change in reliable capacity (MW, number). Example: `+180`. | Feeds reliability assessments and system adequacy models. |
| capex_$ | string | Capital expenditure (USD). Example: `350000000`. | Used in capital budget summaries and ROI calculations. |
| annual_opex_$ | string | Annual operating expenditure (USD). Example: `5000000`. | Feeds cost forecasting and lifecycle O&M analysis. |
| start_p50 | string | Projected start date percentile (ISO date). Example: `2025-02-01` (start_p50). | Used for schedule risk analysis and scenario comparison. |
| start_p90 | string | Projected start date percentile (ISO date). Example: `2025-02-01` (start_p90). | Used for schedule risk analysis and scenario comparison. |
| end_p50 | string | Projected or target end date percentile (ISO date). Example: `2026-10-31` (end_p50). | Used for P50/P90 uncertainty analysis in completion forecasts. |
| end_p90 | string | Projected or target end date percentile (ISO date). Example: `2026-10-31` (end_p90). | Used for P50/P90 uncertainty analysis in completion forecasts. |
| schedule_risk_pct | string | Probability‑weighted schedule risk (%, number). Example: `15`. | Used to quantify likelihood of project delay; feeds Monte Carlo models. |
| outage_profile_id | string | Reference to outage or maintenance profile (string). Example: `OP‑BESS‑1`. | Used for linking downtime assumptions to fleet adequacy models. |
| regulatory_needed | string | Boolean/flag for regulatory approval requirement (1/0). Example: `1`. | Used to identify projects needing permits or commission review. |
| permit_lead_months | string | Estimated lead time for permits (months). Example: `9`. | Informs pre‑construction readiness and Gantt dependency modeling. |
| end_target_p50 | string | Projected or target end date percentile (ISO date). Example: `2026-10-31` (end_target_p50). | Used for P50/P90 uncertainty analysis in completion forecasts. |
| end_target_p90 | string | Projected or target end date percentile (ISO date). Example: `2026-10-31` (end_target_p90). | Used for P50/P90 uncertainty analysis in completion forecasts. |
| capex_total_p50_$ | string | Total capital cost at P50 confidence level (USD). Example: `375000000`. | Feeds cost range visualization and contingency modeling. |
| capex_total_p90_$ | string | Total capital cost at P90 confidence level (USD). Example: `375000000`. | Feeds cost range visualization and contingency modeling. |
| owners_costs_$ | string | Owner's internal costs (USD). Example: `12000000`. | Used for total project cost aggregation and accounting. |
| contingency_pct | string | Contingency percentage (%). Example: `10`. | Applied to CAPEX for cost risk modeling. |
| annual_opex_fixed_$ | string | Annual fixed O&M cost (USD). Example: `4500000`. | Used in long‑term operating cost models. |
| var_opex_$perMWh | string | Variable O&M cost (USD/MWh). Example: `2.5`. | Used in dispatch simulations and financial optimization. |
| wacc_pct | string | Weighted average cost of capital (%, number). Example: `6.5`. | Used for discounted cash flow and NPV calculations. |
| tax_rate_pct | string | Effective tax rate (%, number). Example: `25`. | Used in project financial modeling and net cost estimation. |
| book_life_years | string | Book depreciation life (years, number). Example: `30`. | Determines financial depreciation and accounting schedules. |
| base_cost_year | string | Cost reference year for inflation adjustments (integer). Example: `2023`. | Used in escalation modeling and cost normalization. |
| currency | string | Currency code (string). Example: `USD`. | Used for financial rollups and cross‑currency normalization. |
| condition_score_delta | integer | delta to plant condition score when project is complete | Used for risk analysis and capacity calculations. |

