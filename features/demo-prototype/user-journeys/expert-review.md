# Customer expert reviews the schema

The review view is the MVP's first UI (`decisions.md`, deferred items). The demo shows a
short part of this flow in step 2.

## Actor

**Customer expert:** a person who does or runs the work, for example a service operations
manager. Not a data specialist. Their time is scarce, so a session is short.

## Trigger

The Rare Kaon lead invites the expert after a refinement round, with a request: confirm a
set of categories and label a sample of records.

## Flow

1. **Open the round.** The expert sees the round number, the records in scope, and the
   request: how many categories to review and how many records to label.
2. **Review categories.** For each proposed category the expert sees:
   - the name in the business's own words and a one-line definition;
   - how many records it covers, as a sum of probabilities;
   - three source sentences;
   - the nearest category that overlaps it.

   The expert confirms, renames, merges into another category, marks for a split with a
   note, or rejects.
3. **Label the sample.** For each sampled record the expert sees the source text, the
   category the classifier chose and its probability. The expert agrees, chooses another
   category, or marks "none fits". Records with a missing or failed answer show apart from
   low-probability ones.
4. **Check evidence.** For a finding, the expert sees the cited span with its surrounding
   text. The expert marks a wrong span or a wrong reading.
5. **See the effect.** After the next round the expert sees what changed: coverage, thin
   categories, and each of their decisions with its result.
6. **Freeze.** When the targets hold on a second sample, Rare Kaon freezes the schema
   version. The expert sees the version they helped fix.

Every decision records who made it, when, the schema version and the source span
(`decisions.md`, deferred items).

## Trust

- The expert sees only the sources the customer approved.
- Views are aggregate by default.
- The people in the map see it before the steering group does.

## Out of scope

- Editing source records.
- Writing queries or building views.
- Seeing another customer's data.
