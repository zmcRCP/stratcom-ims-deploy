# STRATCOM Demo Script: Source Refresh, Capacity Breach, And Mitigation Decision

## Demo Goal

Show R-IMS as the schedule-compatible, AI-native program intelligence layer
around existing source schedules. Each program keeps authoring in its own
schedule tool. R-IMS ingests those schedules, reconciles them into a common
portfolio view, maps schedule changes to a synthetic capacity dimension, and
shows leaders a future deterrence availability gap before the decision window
closes.

The demo should not feel like users are manually creating a toy delay. It should
feel like a normal data refresh: updated source data arrives, the portfolio view
changes, R-IMS highlights the predicted breach, and the user compares mitigation
options before locked dates and long-lead actions are missed.

## Current Data Checkpoints

- Scenario: `stratcom-triad-v0_1`
- Main action: `Update data` in the main control row with the dropdowns
- Focus controls: status and triad leg focus matching anchors/tasks without
  removing aggregate portfolio capacity
- Capacity unit: synthetic deterrence availability points (DAP)
- Baseline expectation: aggregate capacity meets the modeled floor before
  `Update data`
- Source-refresh effect: only the Sentinel T2 FOC source task changes during
  `Update data`
- Source-refresh date change: `Sentinel T2 FOC (delayed)` moves from
  `2030-05-17 to 2030-06-16` to `2033-05-17 to 2033-06-16`
- Source-refresh movement: Sentinel T2 FOC moves later by 1096 days
- Post-refresh gap: approximately `-33.86 DAP` below the modeled requirement
- Blocked mitigation: `Blocked: delay Minuteman III Retirement`
- Workable mitigation: `Works: delay B-2 Retirement`
- Required action: `Order B-2 sustainment parts`, with must-act-by date
  `2030-01-23`
- Decision close: applying the B-2 mitigation shows a decision card with the
  moved B-2 tasks, delay amounts, new dates, required actions, source basis, and
  Minuteman III blocked-constraint citation

## Visual Encoding Notes

- Triad-leg rails carry the base task identity.
- Top bars on child/grandchild tasks mean deterrence capacity delta only:
  red means capacity decrease, green means capacity increase, and no top bar
  means no modeled capacity delta.
- Do not explain blue top bars as capacity impact. Blue modernization coloring
  is not used for the capacity top-bar channel.
- Dependency arrows are intentionally pruned for the demo: intra-anchor schedule
  logic remains, but only one or two cross-anchor demo constraints should be
  visible.

## Talking Track

1. Start in the STRATCOM scenario at the aggregate portfolio view. Point out
   that the capacity chart is synthetic deterrence
   availability points. The important thing is the relationship between source
   schedules, future capacity, and decision windows.

2. Establish why R-IMS is in the room. Sentinel fielding, Minuteman III
   retirement, and B-2 sustainment/retirement all live in separate source
   schedules. R-IMS does not replace those authoring tools. It reconciles them
   into the portfolio view leadership needs.

3. Walk through the focus controls. Use status or triad leg to focus attention
   on new builds, modernization, or retirements. Emphasize that the capacity
   chart stays aggregate; filtering away the portfolio would hide the gap R-IMS
   is meant to reveal.

4. Explain the baseline. Before the data refresh, aggregate capacity stays above
   the modeled floor in the narrated horizon. The portfolio looks acceptable
   because the current source data still assumes Sentinel T2 FOC arrives in
   June 2030.

5. Click `Update data` in the main control row. Explain that this represents
   refreshed data arriving from existing schedule/source systems. R-IMS
   reconciles the update, moves the affected source-backed schedule item, and
   recomputes the portfolio capacity view.

6. Pause in the post-update data-diff state. The demo should first answer
   "what changed?" before jumping into mitigation. In TANV, show the same-row
   dashed shadow for the prior Sentinel T2 FOC window, the arrow to the
   refreshed delayed task, and the large movement callout. Use the panel to
   say: Sentinel T2 FOC moved later by 1096 days, from
   `2030-05-17 to 2030-06-16` to `2033-05-17 to 2033-06-16`.

7. Show the newly predicted breach. The capacity chart should now show the
   visible red breach overlay below the modeled floor. Explain that the gap is
   not caused by hiding other data; this is the aggregate portfolio after
   reconciliation. The gap is roughly `-33.86 DAP` in the modeled transition
   window.

8. Click `Review mitigation options`. This is the second step. The delayed
   Sentinel diff overlay should stop competing for attention, and the timeline
   should shift to previewing what each mitigation option would move.

9. Inspect the blocked option: `Blocked: delay Minuteman III Retirement`.
   Hover, focus, or pin it. The UI should show the desired shadow move for
   `Minuteman III first block offline`: current dates
   `2032-07-08 to 2032-08-07`, desired dates
   `2033-07-08 to 2033-08-07`, moving later by 1 year. The related Minuteman
   tasks should show a red dashed blocked-constraint outline and
   `extension blocked` marker. Say that this is the obvious large lever, but it
   is not executable because approval, inspection, crew, and spares windows are
   no longer available inside the transition window.

10. Open or mention the blocked-constraint citation. The decision path cites
    `Minuteman III Extension Feasibility Review`, dated `2030-10-15`, from the
    synthetic demo evidence register. This is not real operational evidence; it
    is a demo-local basis showing why the option is blocked.

11. Inspect the viable mitigation: `Works: delay B-2 Retirement`. Hover, focus,
    or pin it. The TANV preview should show dashed target task bars and arrows
    for both applied schedule moves:
    - `B-2 first block offline` moves later by 771 days, from
      `2031-05-08 to 2031-06-07` to `2033-06-17 to 2033-07-17`.
    - `B-2 full retirement` moves later by 771 days, from
      `2031-06-07 to 2031-06-27` to `2033-07-17 to 2033-08-06`.

12. Inspect the required action under the B-2 option. Highlight
    `Order B-2 sustainment parts` and the two order tasks. The key point is the
    must-act-by date: `2030-01-23`. Say that the B-2 bridge remains available
    only if those long-lead sustainment parts are ordered soon.

13. Apply `Works: delay B-2 Retirement`. The capacity chart should recompute and
    show the breach closed or the margin restored in the modeled window. The
    applied mitigation is synthetic portfolio availability modeling; do not
    describe it as operational interchangeability between triad legs.

14. Use the decision summary to close. It should now summarize:
    - issue: refreshed data predicts a future capacity breach;
    - cause: Sentinel T2 FOC moved later by 1096 days;
    - options considered: blocked Minuteman III retirement delay, workable B-2
      retirement delay, and required B-2 sustainment-parts action;
    - decision selected: `Works: delay B-2 Retirement`;
    - schedule changes: both B-2 delayed tasks, delay amounts, and new dates;
    - result: modeled floor is restored in this sandbox path;
    - blocked constraint citation: `Minuteman III Extension Feasibility Review`,
      dated `2030-10-15`;
    - what needs to happen now: order B-2 sustainment parts by `2030-01-23`;
    - source basis: replacement fielding schedule, Minuteman III retirement
      schedule, and B-2 sustainment parts plan.

## Suggested Out-Loud Close

"No individual program manager owns this picture. The Sentinel team sees its
fielding schedule. The Minuteman III team sees its retirement schedule. The B-2
sustainment team sees parts and workforce constraints. R-IMS shows the
consequence of those schedules running in parallel: a capacity gap that appears
at the portfolio level and can disappear inside any single-program view. The
value is seeing that gap early enough that there is still a decision to make,
then making the required next action obvious."

## Operator Checklist

- Start with the baseline above the floor.
- Click `Update data` once.
- Confirm the first post-update state is the Sentinel T2 FOC diff, not
  mitigation review.
- Confirm the chart breach is visually obvious.
- Click `Review mitigation options`.
- Inspect blocked Minuteman III first; do not apply it.
- Inspect B-2 Retirement second; confirm both B-2 preview callouts are visible
  and not overlapping.
- Apply B-2 Retirement.
- Close on the decision summary and required action.

## Caveats

- The data is synthetic and unclassified.
- DAP is a normalized demo unit, not warheads, launchers, sortie generation, or
  operational force structure.
- The cross-leg or cross-program mitigation is synthetic portfolio availability
  modeling; it is not a claim that triad legs are operationally
  interchangeable.
- The Minuteman III evidence link is synthetic demo evidence, not a real
  operational document.
- The decision card is an in-product demo summary. Report export, evidence
  packet generation, approval workflow, and audit trail are intentionally
  deferred to a future evidence-backed decision packet change.
