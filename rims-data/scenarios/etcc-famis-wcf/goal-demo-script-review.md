# Independent Review: ETCC Goal Demo Script

## Overall Verdict

Not compelling as written -- the script describes a useful capability but has no narrative arc, no stakes, and no answer to the PM's most important follow-up question: "now what do I do?"

---

## Compelling Story?

The STRATCOM script works because it has a concrete decision arc with real stakes at every beat: baseline looks fine -> data refresh reveals a breach -> obvious option is blocked -> workable option exists but requires a specific action by a specific date. The audience experiences urgency at each step. They understand what is at risk and what must happen.

The ETCC script has none of this structure. It is an inventory walk: here are conflicts, here are gaps, here are confirmed items. There is no inciting event, no escalating tension, and no resolution. The audience learns that sources disagree -- but they probably already suspected that. The demo does not show them what that disagreement costs or what changes if they act on it.

The actual data behind this scenario (five records in source_detail_records.jsonl) contains two genuinely interesting findings:

1. P00010-funded work and the Year 5 option appear in the government schedule and contract record but are missing entirely from the contractor IMS. That is not a minor discrepancy -- it means the contractor's schedule is silent on a funded contract modification and a contract option. A PM looking at only the contractor IMS would have an incomplete picture of the program.

2. The Release 4.0 deployment date disagreement has a confidence of 0.86, meaning the pipeline is not fully certain which source to trust.

Neither of these findings is framed with stakes in the script. The Year 5 option gap in particular is exactly the kind of thing that causes programs to miss option exercise windows. That is the hook the script needs and does not use.

Compared directly to STRATCOM: STRATCOM has a must-act-by date (`2030-01-23`), a blocked option that eliminates the easy path, and a decision card that closes the loop. ETCC has a closing line that explicitly says R-IMS does not propose adjudication. That is technically accurate but it is the wrong place to end the story.

---

## How A PM Would Use This

The script does not answer this question clearly. "Program health check" is stated in the Demo Goal but it is vague. A PM walks into this demo and learns that sources disagree. That is background knowledge they likely already have from their daily work -- contractors and government schedules are never perfectly aligned; anyone running a program knows that. What R-IMS needs to show is that it surfaces a specific discrepancy that is invisible inside any single source, and that seeing it changes a decision or prevents a specific failure.

The closest the script gets to a concrete use case is in step 4: "The contractor's schedule does not show this work. The contract record does. That is a material discrepancy." That is a strong sentence. But the script does not set it up with a consequence. What is the PM's exposure if they rely on the contractor IMS alone and miss the P00010-funded work? What review, report, or meeting is the PM about to walk into where this would matter? Without that context, the discrepancy is interesting but not actionable.

The audience for this demo is unstated. "A reviewer" appears in step 2, but that could mean a DCMA reviewer, a program office PM, a contracting officer, or an oversight body. Each of those audiences has a different reason to care about schedule reconciliation and a different workflow afterward. The script should name one.

---

## What Other Tools Are Necessary

The script correctly acknowledges that R-IMS does not propose adjudication. But it stops there without telling the PM what happens next. In practice, a PM who finds these discrepancies would need to:

- Open a corrective action request (CAR) or request for information (RFI) to the contractor asking why P00010-funded work is absent from the IMS. That involves a contract vehicle, a POC, and a response timeline -- none of which R-IMS tracks.
- Coordinate with the contracting officer on the Year 5 option timeline. Option exercise windows are driven by the contract, not the schedule. R-IMS shows that the option is unrepresented in the contractor IMS, but the PM needs a contract modification tracker and the contracting officer's input to act on it.
- Reconcile the Release 4.0 date conflict against the integrated baseline review (IBR) record or most recent CDRL to determine which date has contract authority. That requires a document library and potentially a legal determination.

The script is honest that it does not do these things, but it leaves the PM with a list of discrepancies and no clear handoff to any system or workflow that would let them close the loop. The demo ends at the point where the PM's actual work begins. That is a problem for a tool that wants to be positioned as a program intelligence layer rather than a report generator.

---

## Other Weaknesses

- **No concrete dates anywhere in the talking track.** STRATCOM has specific dates on every finding (2030-01-23, 1096 days, etc.). ETCC uses "N conflicts, N gaps, N confirmed" as placeholders and never fills them in. Even as a goal-state script, this reads as unfinished. A skeptical reviewer in a demo meeting will ask "what are the actual numbers?" and the presenter will not have an answer.

- **"Enriched run" is used twice without explanation.** Steps 3 and 5 refer to "the enriched run" as if the audience knows what that means. The term appears nowhere else in the script. It will read as jargon that signals the demo is not fully production-ready.

- **The confidence score (0.86 for the Release 4.0 conflict) is never surfaced in the script.** The data has it. The script mentions a "red confidence badge" but does not explain what the 0.86 score means or why it should matter to the PM. This is a missed opportunity to show provenance, and it will prompt a skeptic to ask "how confident is R-IMS in any of this?"

- **The closing line undercuts the product.** "No one had to manually cross-reference a spreadsheet" is a true and useful claim. But leading with the manual-labor savings frames this as a time-saving tool, not a decision-support tool. The STRATCOM close lands on the consequence ("see the gap early enough that there is still a decision to make"), which is a more compelling positioning. The ETCC close lands on the method, which is weaker.

- **The "Caveats" section is understated about the scenario's real limitation.** The extraction depth is explicitly incomplete: only five records exist. A skeptical reviewer who asks "how much of the program did R-IMS actually cover?" has no good answer in this script. That needs to be addressed directly rather than buried in a caveat.

- **The scope gaps section promises a "coverage matrix" but the data does not have enough records to fill one.** The script implies a matrix with multiple rows and sources as columns. The actual data has two gap findings. That is a list, not a matrix. If the enriched-run data expansion is prerequisite for this, the script should not describe the matrix as if it already exists.

---

## Suggestions

1. **Name the audience and the meeting.** Pick one: "The contracting officer's representative is heading into a monthly program review and needs to know which schedule to trust." That one sentence gives the demo a concrete use case and tells the PM why they should care before the first click.

2. **Add a stake to the Year 5 option gap.** If the Year 5 option is in the contract record but missing from the contractor IMS, there is a risk that the contractor does not have the work planned or resourced. Add one sentence to step 4 that names that consequence: "If the contractor's schedule doesn't reflect this option, they may not have it resourced. The option exercise window is X. This discrepancy is worth a conversation before that window closes." Even as synthetic data, giving it a date makes it real.

3. **Fill in the N placeholders.** Even in a goal-state script, use the actual counts from source_detail_records.jsonl: 1 conflict, 2 gaps, 2 confirmed. Placeholder "N"s signal an unfinished draft to any reader.

4. **Explain the 0.86 confidence score out loud.** In step 3, when showing the red confidence badge, say what it means: "R-IMS found conflicting values in two sources and cannot determine from those sources alone which is authoritative. That is a 0.86 confidence -- high enough to flag, not high enough to call resolved." This turns a badge into a provenance story.

5. **Give the PM a next action at the close.** The STRATCOM close ends with a required action and a must-act-by date. The ETCC close should end with an equivalent: "This is the conversation you need to have with the contractor before the next IPT." Even a vague next action is better than ending with "R-IMS does not adjudicate."

6. **Add an operator checklist equivalent.** STRATCOM has a checklist of exact click steps. ETCC has prose steps but no checklist. A presenter will lose the thread in a real meeting without it.
