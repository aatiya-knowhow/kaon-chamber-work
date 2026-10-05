# DEMD-01: Demo story

Archived 5 October 2026 from Chamber commit `305a55a`. Superseded: Rivermark is abandoned for the public corpus library, and the product direction changed. The mocks remain on Chamber branch `prototype/demo-mocks`.

Design session, 30 September 2026, Andrew Atiya with Claude. Inputs: the
[demo and corpus handoff](../../../reports/2026-09-30-demo-and-corpus-handoff/README.md),
the [product vision](https://github.com/KnowHow-Software-Inc/kaon-chamber/blob/305a55a/docs/product/vision.md), the
[business context](https://github.com/KnowHow-Software-Inc/kaon-chamber/blob/305a55a/docs/product/business-context.md) and Andrew's core process
diagram.

## Purpose

One prototype build with two goals:

1. **Demo.** Convince a build-first buyer that Rare Kaon defines software from the
   business's own records and proves it against real past cases.
2. **Foundation.** Mock each foundational capability, so the MVP specification starts from
   screens and data shapes that were tried.

The prototype is the Svelte app in `web/`, reading fixtures. It has no Python API.

## Decisions

| Decision | Rationale |
| --- | --- |
| The audience is the build-first buyer. | It is how `AGENTS.md` and `decisions.md` define Chamber, the only unprompted buying signals came from such buyers, and the vertical software deck already makes the argument. The growth-lab framing is a second pass on the same spine. |
| The demo follows the seven steps of the [core process](https://github.com/KnowHow-Software-Inc/kaon-chamber/blob/305a55a/docs/product/vision.md#core-process), one beat each. | The demo and the MVP use one sequence. The viewer sees records become a tested build. |
| Open on the buyer's tool. End on replay results. | The buyer asked for outcomes, not data. The deck's proof is the traced specification and the replayed cases. |
| Add screens for steps 1 to 3: stock schema choice, a refinement round, the expert review queue, and processing status. | These steps make Chamber different, and the MVP builds them first. The review view is the MVP's first UI (`decisions.md`, deferred items). The current mocks do not show them. |
| Deepen steps 5 to 7: requirements with evidence and acceptance criteria, replay against a stand-in build with pass and fail, and a comparison of two map versions. | These carry the deck's claims. The current specification and replay screens are thin, and nothing covers step 7. |
| Keep three or four insight screens on the demo route: exposures, the process map, the money flow, and the life of one job. Other screens stay in the app, off the route. | Insight alone reads as an analytics tool, the category Rare Kaon positions against. |
| Say in the demo that Rivermark is synthetic. | The deck uses no synthetic demonstration data. A live demo on Rivermark must not look like a customer result. |
| Use the v2 fixture until the v3 corpus lands. Beats that need real timing wait for v3. Rebuild the fixture once. | v2 has almost no waits. Two rebuilds cost more than one. |
| Mock data shapes in `web/src/lib/contracts.ts`. They feed the Python OpenAPI contracts later. | The prototype informs the MVP contract without building the API first. |

## Beats

| Step | Beat | The viewer sees | State on 30 September | Informs in the MVP |
| --- | --- | --- | --- | --- |
| Frame | The buyer's tool | The tool the buyer wants, and Rivermark as a synthetic stand-in business | Not built | — |
| 1 | Start from what field service firms look like | The fixed layer and stock categories for field service | Not built. No stock schema exists. | Stock schema store; fixed-layer model |
| 2 | Fit it to this business | One refinement round: categories in the firm's own words, coverage, overlap, thin categories, and an expert confirming them | Not built | Refinement loop, `Judge` interface, review view |
| 3 | Read the records | Files read, classified, skipped and low-confidence, by source and kind | Counts only, on the overview | Ingestion runs; source formats; missing answers kept apart from low probabilities |
| 4 | What the records show | Exposures, the process map, the money flow, and the life of one job with an exception | Built on v2 from a hand extraction | Map queries that agents read |
| 5 | Specify the tool | Requirements that cite evidence and state acceptance criteria, and exceptions that interviews would miss | 14 requirements, thin | Specification export that cites evidence IDs |
| 6 | Replay real cases | Past cases replayed against the stand-in build: pass or fail, beside what the business did | 5 cases, no build | Replay case export; the Verify step |
| 7 | When the business changes | Map version 2 against version 1. A failed case traced to a wrong term, then fixed. | Not built | Schema freeze and versions; map comparison |

## Rejected

- **Open on the overview dashboard.** It leads with analysis, which the buyer rejected.
- **Growth-lab framing first.** Chamber's definition and the buying signals are
  build-first.
- **Build the MVP backend first.** The demo must run before an engagement. The mocks shape
  the specification.
- **Business context inside this feature.** It outlives the prototype. It is in
  `docs/product/`.

## Open questions

1. **The stand-in build for step 6.** Either a small working rules module and screen that
   the replay cases run against, or mocked results. Recommendation: a working module,
   because step 6 is the proof and it prototypes the MVP's Verify step.
2. **The tool Rivermark wants.** It must follow from the map. Candidates from the v2
   exposures: job completion with evidence filing to procedure, quote follow-up, and
   contract renewals. Choose from the v3 map.
3. **The source of step 2 data.** Hand-written, or a small real refinement round on
   Rivermark (an LLM proposes and a judge classifies). A real round also prototypes the
   MVP loop.
4. **Named individuals.** The Alex Ward exposure names a person. The trust rules in the
   [business context](https://github.com/KnowHow-Software-Inc/kaon-chamber/blob/305a55a/docs/product/business-context.md#trust-rules) forbid that
   without the person's participation. Decide before the demo.
5. **Length and format.** Live-driven by a Rare Kaon lead or self-guided, and the target
   length.
