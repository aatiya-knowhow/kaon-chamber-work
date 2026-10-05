# Demo mocks and corpus v3: handoff

Archived 5 October 2026 from Chamber commit `305a55a`. Closed: corpus v3 stopped on 1 October 2026 and Rivermark is abandoned.

Temporary. Session of 28–30 September 2026, Andrew Atiya with Claude. Delete when the
open items below are resolved or moved into `docs/decisions.md`.

## Where things stand

| Work | Location | State |
| --- | --- | --- |
| UI mocks | `prototype/demo-mocks` at `2dbd555`, `web/` | Built. Overview, exposures, strengths and opportunities, process map, jobs and timelines, findings, specification, replay cases, evidence panel. Drill-down: Business › Process › Case › Evidence. |
| Mock data | `web/fixtures/rivermark-demo.json`, built by `web/fixtures/build/` | 300 jobs, 1,829 events, 3,592 verbatim evidence spans from the 2,179-file Rivermark v2 drive. `check_parts.py` verifies every quote. |
| Process map v2 | Merged into `prototype/demo-mocks` at `8a5c9ce`, worktree `../kaon-chamber-process-map` | Built. ELK swimlane layout, draggable boxes, detail sliders, frequency and time views, period comparison. See [Process map v2](#process-map-v2). |
| Visuals | Merged into `prototype/demo-mocks` at `481a6b0`, worktree `../kaon-chamber-visuals` | Built. Money flow, life of a job, every job, deadline horizon, paper trail, title against actual work, workload calendar. See [Visuals](#visuals). |
| Corpus v3 | `rivermark-shared-drive`, branch `corpus-v3`; spec `_notes/brief-v3.md` | Running unattended on the dev VPS: pilot fixes, then the full render if the checks and the blind reader pass. |
| Corpus key | `rivermark-corpus-key` (private) | Sealed. Readers never open it. |

Process map v2 and the visuals are merged into `prototype/demo-mocks`. `2dbd555` then
moved the shared date and day-format helpers into `lib/dates.ts` and `lib/format.ts`. Next:
the demo branch to `main`.

## Process map v2

Screen `#/process`. Code in `web/src/screens/ProcessMap.svelte`,
`web/src/components/process/` and `web/src/lib/process.ts`. Adds `elkjs` 0.12.0. Every
box, line, label and comparison row still opens its jobs, then the job timeline, then the
source sentences.

| Part | Behaviour |
| --- | --- |
| Layout | ELK layered, two passes. Pass 1 sets columns and the order in each column; activities enter in their mean position in a job, so back lines are real loops. Lanes take the order with the least job-weighted lane distance (exhaustive search up to 8 lanes). Pass 2 pins each box to its lane row and routes spline lines. Edge labels are placed by the app, clear of boxes and other labels. |
| Lanes | A lane is the role that holds the work (`TimelineEvent.lane`, most common per activity). Lanes have rules, alternate fills and a highlight while a box moves. |
| Dragging | A box moves freely along its lane and snaps to a column on release. Dropping it below the last row adds a row to the lane. Positions are kept in `localStorage` for each kind of work (`kaon-chamber:process-map:positions:<work type>`). Reset layout clears them. |
| Detail | Activities slider keeps the activities most jobs reach (default 15 of 21, the old 5% cut-off). Paths slider keeps the busiest share of paths (default 10%), plus each box's busiest line in and out. Hidden activities are skipped: a line joins the shown steps directly. The step that the title names is always drawn. |
| Frequency / Time | Frequency: line width and label are jobs. Time: line colour in four bands and a label give the median wait. A wait is left out when either event has `dateRecorded: false`. |
| Period comparison | Median wait for each step in date ranges A and B, counted in the period in which the step began. Default split is the median dated event, so samples are similar. Steps need at least 4 dated waits in each period. |
| Title | Names the step with the most job-days of waiting, from steps with a median above 0 and at least 5 dated waits. |

Limits:

- On the v2 drive most waits are 0 days, so the time view and the comparison show few
  changes. Medians also hide changes in the slow tail: Attended → Evidence filed has a
  median of 0 days in both periods, but its mean rose from 0 to 7.1 days.
- ELK runs on the main thread. With all 21 activities and 149 paths, a layout takes about
  1 second. A 120 ms delay limits layouts while a slider moves. A Web Worker would remove
  the pause.
- A small filter change can move columns, because ELK lays out the whole map again.
- `docs/development/local-setup.md` does not yet name ELK or `components/process/`.
- Animated replay is not built. It waits for real timing (next step 4).

## Visuals

Seven screens in `web/src/screens/`, charts in `web/src/components/charts/`, derived data
in `web/src/lib/`. Each title is computed from the data and states the finding. Every mark
opens its jobs or its source sentences. No contract or fixture changed.

| Screen | Route | Shows |
| --- | --- | --- |
| Where the money goes | `#/money` | Quote register to invoice register as a flow: accepted, invoiced, kept; lost, lapsed, withdrawn and revised quotes; credit notes and unbilled work. Values after invoicing are under 1 px at true scale, so an inset draws them 100× larger. |
| Life of a job | `#/story/<job>` | Scrolling story, default RM-26082 Cedar Court. Files arrive in date order. At the rule step the axis zooms to the 3-day evidence gap under OP-01 v3.0. The open items hang past the snapshot. |
| Every job | `#/dots/<grouping>` | 300 squares coloured by outcome. They move between groupings by outcome, kind of work, customer and month opened. |
| Deadline horizon | `#/horizon` | Open items by age at the snapshot, by lane. Dates after the snapshot that a source names, marked firm, estimate or proposal. |
| Paper trail | `#/trail/<job>` | One job's files on a time axis, with an explicit break for long gaps. Arcs join a file to the file whose reference it names. |
| Title against actual work | `#/roles` | Ribbons from role titles to the activities that the records show, coloured by whether the title names the work. |
| Workload calendar | `#/workload/<person>` | Recorded actions per person per week, with a day grid for one person. A toggle removes register rows. |

Shared code: `lib/registers.ts` reads register rows from evidence spans. `lib/outcomes.ts`
gives one outcome per job. `components/charts/Tooltip.svelte` is the hover card.
`lib/dates.ts` holds all calendar arithmetic, in UTC. `lib/format.ts` gains `money()`,
`formatDays()` and `formatDayCount()`.

Limits:

- Register rows carry the snapshot date (`updated_at`), not the date the row was written.
  The charts dash them and say so.
- The fixture has no due dates. The horizon finds dates after the snapshot by scanning
  evidence text, and cites each one. A date with no year is kept only within 120 days after
  its source date.
- 66 closed jobs have no quote or invoice row in the registers.
- Workload counts are actions in the records, not hours. 64% of dated actions fall on a
  Monday because the records date jobs there. 88% of Owen Brooks's actions are register
  rows.
- "The title names the work" is a hand-written word match in `lib/people.ts`.
- Unbilled work (£780) is exposure exp-05's value less the credit notes, not register rows.
- Paper-trail links come only from references in cited sentences, not full file text.
- Chrome throttles a tab that is not visible. Scroll steps and entry motion then pause
  until the tab is shown.

## Why corpus v3

The v2 drive has almost no timing: 239 of 300 jobs have quote, acceptance, attendance and
invoice on one date. It also has uniform figures, filler, and contradictions nobody
designed, and its stories are thin. The process views show 0-day waits for that reason.
v3 builds the drive from a simulated business, with a sealed story catalogue and measured
signatures. The first pilot fixed the timing but leaked simulator output into the drive and
read like printed logs. The brief now adds an information boundary, texture targets, a
blind reader gate and no self-grading.

## Next steps

The demo direction is in
[DEMD-01](../../features/demo-prototype/design/DEMD-01-demo-story.md).

1. When the VPS run ends: a reader-side spot check of the new drive, using only a copy of
   `Rivermark Shared/` (no `.git`, no `_notes/`, no key).
2. Rebuild the mock fixture from the v3 drive with the pipeline in `web/fixtures/build/`.
   Run the extraction in fresh threads: this session saw story hints through the leaking
   v2 pilot. Point the process-map and visuals branches at the new fixture.
3. Freeze and hash Chamber's output, then run a fresh scorer against the key.
4. Visuals that wait for real timing: animated replay, river of open work, what-if slider.
5. Link the visuals. A file on the story shelf lights its paper-trail arcs. An open item on
   the horizon lights its job's square. A role ribbon opens who does each step on the
   process map. Put each visual's title on the overview as an entry point.
6. Show where a register and its documents disagree. For example, the register dates
   INV-26318 7 September and the invoice says 8 September.

## Open decisions

- Record in `docs/decisions.md`, held back on 28 September: the demo runs on the staging
  environment; every map record stores its producer (agent, Jev or person); specifications
  are external files that cite evidence IDs.
- `AGENTS.md` forbids committing corpus-derived artifacts. The Rivermark fixtures are
  synthetic and were committed at Andrew's request. Decide whether the rule should name
  customer data only.
- Fixture size, about 10 MB in git: Git LFS, or regenerate on demand.
- Process-mining rules exist in both `web/src/lib/process.ts` and
  `web/fixtures/build/insights.py`. Move them to one Python home when the API exists.
  `process.ts` now also holds path selection and period comparison.
- Period comparison uses the median only. Decide whether to add a tail measure (mean or
  75th percentile) once v3 timing arrives.
- Contract gaps from the visuals: a dated commitment (date, kind, owner, evidence) would
  replace the text scan in the horizon. Timesheet hours would let the workload calendar
  compare hours with recorded actions.
- Trust rules in `business-planning/venture-studio/approach.md` still forbid naming
  individuals. Andrew's position is that company records carry no such expectation.
  Update that document.
