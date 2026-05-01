# Portfolio Abstract Canonical Schema

**schema_id:** `modernization_portfolio_abstract`
**schema_version:** `0.1.0`

This document defines the canonical file format for a domain-agnostic modernization portfolio. The canonical file is the single-source-of-truth input to the stitching pipeline; all legacy view files (plants.json, gantt_tasks_by_plant.json, etc.) are derived outputs.

---

## Envelope (top-level metadata)

| Field             | Type   | Required | Description |
|-------------------|--------|----------|-------------|
| `schema_id`       | string | yes      | Always `"modernization_portfolio_abstract"` |
| `schema_version`  | string | yes      | Semver (e.g. `"0.1.0"`) |
| `bundle_id`       | string | yes      | Stable, URL-safe identifier for this dataset (e.g. `"energy-demo-v1"`) |
| `bundle_version`  | string | yes      | Semver for the dataset release |
| `domain_id`       | string | yes      | Domain namespace (e.g. `"energy-demo"`, `"airline-demo"`) |
| `created_at`      | string | yes      | ISO-8601 timestamp |
| `generated_by`    | string | no       | Script or tool that produced the file |
| `sources`         | array  | yes      | Provenance records (see below) |

## Sources (provenance)

Each entry in `sources[]` documents an upstream data source that contributed to the canonical file.

| Field           | Type   | Required | Description |
|-----------------|--------|----------|-------------|
| `source_id`     | string | yes      | Stable identifier (e.g. `"src-energy-legacy"`) |
| `label`         | string | yes      | Human-readable description |
| `snapshot_date` | string | no       | ISO-8601 date of the source snapshot |

Every work item and dependency carries a `source_id` field referencing one of these sources.

---

## Collections

### anchors[]

Anchors are the top-level grouping entities (plants, aircraft, facilities, etc.).

| Field          | Type   | Required | Description |
|----------------|--------|----------|-------------|
| `id`                     | string | yes      | Prefixed `A-{slug}` |
| `legacy_short_id`        | string | no       | Short display ID from legacy system |
| `name`                   | string | yes      | Display name |
| `eia_plant_id`           | number | no       | External registry identifier (EIA plant ID for energy) |
| `region`                 | string | no       | Geographic or organizational region |
| `category`               | string | no       | Domain grouping (fuel type for energy, fleet type for airline) |
| `status`                 | string | no       | Current status (e.g. `"modernization"`, `"active"`) |
| `capacity_mw`            | number | no       | Domain-specific capacity metric |
| `nameplate_mw`           | number | no       | Nameplate/rated capacity |
| `condition_score`        | number | no       | 0-100 asset condition index (risk of degradation without maintenance) |
| `annual_sustainment_opex`| number | no       | Annual sustainment operating expenditure (USD) |
| `infra_backlog`          | number | no       | Infrastructure backlog ($M) |
| `forced_outage_rate`     | number | no       | Forced outage rate (percentage) |
| `age_years`              | number | no       | Asset age in years |
| `owner`                  | string | no       | Owning organization |
| `start_date`             | string | no       | ISO-8601 date anchor became active |
| `retire_date`            | string | no       | ISO-8601 planned retirement date |
| `timezone`               | string | no       | IANA timezone |
| `tags`                   | array  | no       | Freeform string tags for filtering |
| `aliases`                | array  | no       | Alternate identifiers for entity resolution |
| `baseline`               | object | no       | `{ date, nameplate_mw }` baseline snapshot |
| `reliability`            | object | no       | `{ base_for }` reliability metrics |
| `sustainment`            | object | no       | `{ needs[] }` sustainment catalog (needs, resources, supply chain) |
| `projectTypes`           | object | no       | Domain-specific project type metadata |
| `components`             | object | no       | Asset component inventory |
| `meta`                   | object | no       | Domain-specific extension fields |

### work_items[]

Work items are tasks, milestones, procurement activities, etc. associated with anchors.

| Field               | Type    | Required | Description |
|---------------------|---------|----------|-------------|
| `id`                | string  | yes      | Prefixed `W-{slug}` |
| `anchor_id`         | string  | yes      | References an anchor `id` |
| `parent_id`         | string  | no       | References another work item `id` (assembly hierarchy) |
| `title`             | string  | yes      | Display name |
| `kind`              | string  | yes      | One of: `milestone`, `execution`, `procurement`, `assembly`, `integration_gate` |
| `start`             | string  | yes      | ISO-8601 date |
| `end`               | string  | yes      | ISO-8601 date |
| `baseline_start`    | string  | no       | ISO-8601 date (for slack/deadline computation) |
| `baseline_end`      | string  | no       | ISO-8601 date (for slack/deadline computation) |
| `risk_score`        | number  | no       | 0-100 risk indicator |
| `source_id`         | string  | yes      | References a `sources[].source_id` |
| `start_locked`      | boolean | no       | Whether start date is locked |
| `end_locked`        | boolean | no       | Whether end date is locked |
| `lock_reason`       | string  | no       | Explanation for lock |
| `cost_capex_usd`    | number  | no       | Capital expenditure |
| `cost_opex_annual_usd` | number | no    | Annual operating expenditure |
| `cost_total_p50_usd`   | number | no    | P50 total cost estimate |
| `cost_total_p90_usd`   | number | no    | P90 total cost estimate |
| `cost_owners_usd`   | number  | no       | Owner's cost |
| `cost_per_day_usd`  | number  | no       | Daily cost rate |
| `delta_capacity`     | number  | no       | Abstract capacity delta (domain-specific units) |
| `legacy_type`       | string  | no       | Original type from legacy system (preserved for backward compat) |
| `meta`              | object  | no       | Domain-specific extension fields |

### dependencies[]

Dependencies define ordering and timing relationships between work items.

| Field             | Type    | Required | Description |
|-------------------|---------|----------|-------------|
| `id`              | string  | yes      | Prefixed `D-{number}` |
| `predecessor_id`  | string  | yes      | References a work item `id` |
| `successor_id`    | string  | yes      | References a work item `id` |
| `type`            | string  | yes      | One of: `"FS"`, `"SS"`, `"FF"`, `"SF"` |
| `lag_days`        | number  | no       | Lag in days (default 0) |
| `source_id`       | string  | yes      | References a `sources[].source_id` |
| `inferred`        | boolean | no       | `true` if this dependency was inferred, not explicit |
| `confidence_tier` | string  | no       | Confidence level: `"high"`, `"medium"`, `"low"` |
| `evidence_refs`   | array   | no       | Array of strings referencing supporting evidence |

### suppliers[]

Supply chain vendor/supplier records. Empty in MVP; reserved for future use.

| Field      | Type   | Required | Description |
|------------|--------|----------|-------------|
| `id`       | string | yes      | Prefixed `S-{slug}` |
| `name`     | string | yes      | Supplier name |
| `category` | string | no       | Supplier category |
| `meta`     | object | no       | Extension fields |

### parts[]

Parts/components in the supply chain. Empty in MVP; reserved for future use.

| Field         | Type   | Required | Description |
|---------------|--------|----------|-------------|
| `id`          | string | yes      | Prefixed `PT-{slug}` |
| `name`        | string | yes      | Part name |
| `supplier_id` | string | no      | References a supplier `id` |
| `lead_time_days` | number | no   | Standard lead time |
| `meta`        | object | no       | Extension fields |

### procurement_maps[]

Mapping between work items and parts/suppliers. Empty in MVP; reserved for future use.

| Field          | Type   | Required | Description |
|----------------|--------|----------|-------------|
| `id`           | string | yes      | Prefixed `PM-{slug}` |
| `work_item_id` | string | yes      | References a work item `id` |
| `part_id`      | string | yes      | References a part `id` |
| `quantity`     | number | no       | Quantity required |
| `meta`         | object | no       | Extension fields |

### overlays[]

Constraints and overlay windows (blackout periods, maintenance windows, etc.).

| Field     | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `id`      | string | yes      | Prefixed `O-{slug}` |
| `scope`   | string | yes      | `"anchor"` or `"work_item"` |
| `target_id` | string | yes    | References the scoped entity |
| `start`   | string | yes      | ISO-8601 date |
| `end`     | string | yes      | ISO-8601 date |
| `pattern` | string | yes      | Overlay type (e.g. `"blackout"`, `"preferred"`) |
| `label`   | string | no       | Display label |
| `style`   | object | no       | Rendering hints (color, opacity, etc.) |

### operational_requirements[]

Time-series requirements that define operational floors, demand curves, etc.

| Field       | Type   | Required | Description |
|-------------|--------|----------|-------------|
| `id`        | string | yes      | Prefixed `R-{slug}` |
| `label`     | string | yes      | Display name |
| `region`    | string | yes      | Region this requirement applies to |
| `semantics` | string | yes      | `"minimum_required"`, `"forecast_demand"`, `"target"` |
| `unit`      | string | no       | Unit of measure (e.g. `"MW"`) |
| `time_series` | array | yes    | Array of `{ date: string, value: number }` |

### profiles[]

Additional named profiles or curves. Empty in MVP; reserved for future use.

| Field       | Type   | Required | Description |
|-------------|--------|----------|-------------|
| `id`        | string | yes      | Prefixed `P-{slug}` |
| `label`     | string | yes      | Display name |
| `kind`      | string | yes      | Profile type |
| `data`      | array  | yes      | Profile data points |

---

## ID Conventions

All IDs must be unique within their collection. Prefix conventions:

| Collection               | Prefix | Example |
|--------------------------|--------|---------|
| anchors                  | `A-`   | `A-PL-N03` |
| work_items               | `W-`   | `W-P-019` |
| dependencies             | `D-`   | `D-1` |
| suppliers                | `S-`   | `S-acme-corp` |
| parts                    | `PT-`  | `PT-turbine-blade` |
| procurement_maps         | `PM-`  | `PM-proj1-blade` |
| overlays                 | `O-`   | `O-summer-blackout` |
| operational_requirements | `R-`   | `R-ERCOT` |
| profiles                 | `P-`   | `P-seasonal-curve` |

---

## Versioning

- **schema_version**: Follows semver. Breaking changes increment the major version.
  - `0.x.y` — pre-1.0 development; minor bumps may include breaking changes.
  - `1.0.0+` — stable; only backward-compatible additions in minor/patch.
- **bundle_version**: Independent semver for the dataset. Bumped when data content changes.

---

## Validation Rules

1. **Unique IDs**: Every `id` within a collection must be unique.
2. **Referential integrity**:
   - Every `work_items[].anchor_id` must reference an existing `anchors[].id`.
   - Every `work_items[].parent_id` (if set) must reference an existing `work_items[].id`.
   - Every `dependencies[].predecessor_id` must reference an existing `work_items[].id`.
   - Every `dependencies[].successor_id` must reference an existing `work_items[].id`.
   - Every `work_items[].source_id` must reference an existing `sources[].source_id`.
   - Every `dependencies[].source_id` must reference an existing `sources[].source_id`.
3. **Date validity**: For every entity with `start` and `end`, `start <= end`.
4. **Required fields**: All fields marked "Required" must be present and non-null.
5. **Floor curve coverage**: At least one `operational_requirements` entry should span the demo timeline.

---

## Mapping Guidance: Canonical to Legacy View Files

### anchors[] -> plants.json

| Canonical Field | Legacy Field | Notes |
|-----------------|-------------|-------|
| `id` (strip `A-` prefix) | `id` | Legacy IDs do not carry prefix |
| `name` | `name`, `plant_name` | |
| `category` | `fuel` | Domain-specific grouping |
| `region` | `region` | |
| `capacity_mw` | `capacity_mw` | |
| `nameplate_mw` | `nameplate_mw` | |
| `status` | `status` | |
| `owner` | `owner` | |
| `start_date` | `start_date` | |
| `retire_date` | `retire_date` | |
| `baseline` | `baseline` | Nested object preserved |
| `reliability` | `reliability` | Nested object preserved |

### work_items[] -> gantt_tasks_by_plant.json

| Canonical Field | Legacy Field | Notes |
|-----------------|-------------|-------|
| `id` (strip `W-` prefix) | `id` | |
| `anchor_id` (strip `A-` prefix) | `plantId` | |
| `parent_id` (strip `W-` prefix or null) | `parentId` | |
| `title` | `title` | |
| `legacy_type` | `type` | Original type value for backward compat |
| `start` | `start` | |
| `end` | `end` | |
| `risk_score` | `risk_score` | |
| `start_locked` | `start_locked` | |
| `end_locked` | `end_locked` | |
| `lock_reason` | `lock_reason` | |
| `cost_*` fields | `cost_*` fields | All cost fields mapped 1:1 |

### dependencies[] -> gantt_dependencies_by_plant.json

| Canonical Field | Legacy Field | Notes |
|-----------------|-------------|-------|
| `id` (strip `D-` prefix, parse int) | `id` | Legacy uses numeric IDs |
| `predecessor_id` (strip `W-` prefix) | `predecessorId` | |
| `successor_id` (strip `W-` prefix) | `successorId` | |
| `type` | `type` | Legacy uses numeric: FS=0, SS=1, FF=2, SF=3 |
| `lag_days` | `lag_days` | |

### operational_requirements[] -> load_forecast_by_region.json

| Canonical Field | Legacy Field | Notes |
|-----------------|-------------|-------|
| `region` | Object key | Legacy uses region as top-level key |
| `time_series[].date` | `[].date` | |
| `time_series[].value` | `[].load_mw` | Field name is domain-specific in legacy |

---

## Immutable Base + Mutation Layer

The canonical `portfolio_abstract.json` is an **immutable baseline**. Scenario mutations are stored in a separate `scenario_mutations.json` file. The stitching pipeline applies mutations to produce scenario-specific derived outputs. This separation ensures:

- Clean version control of the baseline
- Reproducible scenario generation
- Easy diff between scenarios

---

## Energy Demo Sample Bundle

The file `portfolio_abstract.json` in this directory is generated from the legacy energy demo view files by `scripts/generate_portfolio_abstract.js`. It includes:

- All 17 plant anchors
- All 249 work items (20 top-level projects + 229 child milestones/tasks)
- All 361 dependencies
- Load forecast operational requirements for 5 regions (ERCOT, MISO, NEISO, PJM, SPP)
