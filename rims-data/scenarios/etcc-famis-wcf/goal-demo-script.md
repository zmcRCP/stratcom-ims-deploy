# ETCC Goal Demo Script: Schedule Reconciliation Across Three Sources

**Status:** runnable against the enriched ETCC bundle and findings workspace

---

## Current State

This script now describes the current enriched ETCC scenario state.

| Aspect | Current staged bundle / UI |
|---|---|
| Schedule conflicts | 6 |
| Scope gaps | 2 |
| Confirmed source-facts | 7 |
| Findings surface | findings-first ETCC workspace |
| TANV scoping | schedule-grounded findings only; gaps and supporting facts stay out of fake bar rendering |

---

## Demo Goal

Show R-IMS as the reconciliation layer over existing program source documents.
The FAMIS-WCF program has a government tracking schedule, a contractor IMS, and
USASpending contract records. These three sources are never in perfect agreement.
R-IMS ingests and reconciles them, surfaces where they conflict or are silent on
the same funded work, and gives a program reviewer a clear picture of what they
can rely on and what needs a conversation -- before they walk into a review meeting
and rely on only one source as truth.

**Audience:** A contracting officer's representative (COR) or program manager
heading into a monthly program review. They have access to the contractor IMS and
the government schedule. They do not have time to manually cross-reference them
against the contract record. They need to know: is what the contractor is showing
me complete and consistent with what the contract says?

---

## Setup

- Scenario: `FAMIS-WCF ETCC Analysis`
- Layout: TANV (schedule view) on left, findings workspace on right, provenance
  detail for the selected finding
- Data: enriched extraction -- 6 schedule conflicts, 2 scope gaps, 7 confirmed facts

---

## Talking Track

### 1. Open the scenario and establish the situation

Select `FAMIS-WCF ETCC Analysis`. Before clicking anything, establish the context:

"This is a real program with three source streams: a government tracking schedule,
a contractor IMS, and the USASpending contract record. Each one is authoritative
for something different. R-IMS pulled from all three and reconciled what it found.
What you're about to see is where they agree and where they do not."

Point out that R-IMS is not the authoring system for any of this data. None of
the sources were modified. The reconciliation is read-only.

### 2. Orient to the finding counts

Point to the summary header in the findings workspace:
**6 conflicts - 2 gaps - 7 confirmed**.

"Six places where sources report different values for the same item. Two places
where work that exists in the contract record and government schedule is simply
absent from the contractor IMS. And seven items where the sources agree -- those
are the areas you can rely on going into your review."

Pause here. Let the ratio land. The contractor IMS is not wrong everywhere -- but
it is silent on funded work.

### 3. Walk the schedule conflicts

Click into the **Schedule Conflicts** section. Lead with the Release 4.0
deployment date disagreement:

"Government schedule says one date. Contractor IMS says another. R-IMS flagged
this with a confidence of 0.91 -- meaning it found the conflict clearly but cannot
determine from the source documents alone which date has contract authority. That
determination requires a human: check the IBR record or the most recent CDRL.
What R-IMS is telling you is that this question is open and you should not assume
either source is right."

Click the row to navigate to that task on the TANV. Point out the red confidence
badge. Open the source detail to show exactly what each source said.

Then walk two or three more conflicts from the enriched run. The point is
pattern, not exception:

"This is not one bad data point. R-IMS found six places where the contractor
schedule and the government schedule disagree on dates or status values. Before
today, seeing these required opening both systems and comparing them manually.
Here they are in one view."

### 4. Walk the scope gaps -- this is the critical section

Click into the **Scope Gaps** section. Show the coverage matrix. Two rows, three
source columns: government schedule, contractor IMS, USASpending.

**P00010-funded work:**

"P00010 is a funded contract modification. It appears in the government schedule
and in the USASpending transaction record. It does not appear in the contractor
IMS at all. That means the contractor's schedule is silent on work the government
has already funded. A reviewer looking only at the contractor IMS would have no
visibility into this scope. That is a material gap."

**Year 5 option:**

"The Year 5 option appears in the government schedule and the USASpending award
record. It is absent from the contractor IMS. The option exercise window closes
in Q3 2026. If the contractor does not have this work planned and resourced in
their IMS, there is a risk they are not positioned to execute it when the option
is exercised -- or that it influences the government's decision on whether to
exercise at all. This is the conversation you need to have before that window
closes."

Click the Year 5 option row and keep the conversation in the scope-gap matrix
and provenance detail. This is a coverage and contract-review question, not a
true contractor schedule task. The important point is absence from the
contractor plan, not synthetic placement on the chart.

### 5. Show what is confirmed -- set the contrast

Expand the **Confirmed** section. Walk two items:

"ATO review status: confirmed directly by the government schedule. Period of
performance end date: confirmed by the USASpending award record. These are areas
where R-IMS found consistent, authoritative source backing. You can rely on
these going into your review."

Contrast with the gaps and conflicts: "This is what the good side of reconciliation
looks like. Where sources agree, you can trust the picture. Where they do not, you
now know exactly which questions to bring to the contractor."

### 6. Close on the decision the PM now has

"You are heading into a program review. Before R-IMS, you had a contractor IMS
and a government schedule that you probably assumed were mostly aligned. They are
not fully aligned. You have six date or status conflicts that need a
contract-authority determination. You have two funded work items that the
contractor's schedule does not show. And you have a Year 5 option exercise
window coming up in Q3 2026 with no evidence the contractor has it planned.

That is the conversation you need to have at the review -- or before it. R-IMS
did not tell you what to decide. It told you what to ask."

---

## Key Phrases

- "The contractor's schedule is silent on work the government has already funded."
- "Six conflicts. Two gaps. Seven confirmed. That is the reconciliation picture."
- "0.91 confidence means R-IMS found the conflict clearly but a human has to
  determine which source has contract authority."
- "The option exercise window closes in Q3 2026. Does the contractor have this
  planned?"
- "Where sources agree, you can trust the picture. Where they do not, you now
  know exactly which questions to bring."

---

## Operator Checklist

- [ ] Open `FAMIS-WCF ETCC Analysis` from the scenario picker.
- [ ] Confirm findings workspace is open and shows 6 conflicts - 2 gaps - 7 confirmed.
- [ ] Click Schedule Conflicts. Lead with Release 4.0 date conflict.
- [ ] Click the Release 4.0 row -- confirm TANV navigates to that task and badge is red.
- [ ] Open source detail. Confirm it shows both source values and the 0.91 score.
- [ ] Walk 2-3 additional conflict rows to show pattern.
- [ ] Click Scope Gaps. Walk coverage matrix -- P00010 row first, Year 5 option second.
- [ ] Click Year 5 option row -- confirm provenance detail opens from the scope-gap selection without relying on a timeline bar.
- [ ] Expand Confirmed section. Walk ATO review and PoP end date.
- [ ] Close on the option exercise window and the question the PM now has.

---

## Caveats For This Demo

- Source documents are real FAMIS-WCF artifacts processed through the
  Data-Normalization pipeline. The reconciliation reflects the pipeline's
  extraction coverage, not a complete program audit.
- The demo does not claim the contractor IMS is wrong -- it claims the sources
  disagree and that disagreement is worth a conversation.
- The Year 5 option exercise window date is derived from the contract record.
  It is not legal advice and does not constitute a contracting determination.
- R-IMS surfaces discrepancies. Adjudication, corrective action requests, and
  contract authority determinations are human decisions made outside R-IMS.
