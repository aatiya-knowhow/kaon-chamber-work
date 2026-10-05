# CRPD-01: Corpus library

Archived 5 October 2026 from Chamber commit `305a55a`. Delivered; see the [implementation report](../../../reports/2026-10-03-corpus-library/README.md).

Agreed design, 3 October 2026, Andrew Atiya with Codex. Inputs:
[ranked corpora](https://github.com/KnowHow-Software-Inc/kaon-corpora/blob/main/research/ranked-lists/README.md),
[storage research](https://github.com/KnowHow-Software-Inc/kaon-corpora/blob/main/research/corpus-storage-2026-10-03/README.md), and
[Chamber's product vision](https://github.com/KnowHow-Software-Inc/kaon-chamber/blob/305a55a/docs/product/vision.md).

## Purpose

Give people and coding agents a low-cost library of real business records in
their published formats. People can browse originals. Chamber and future similar
projects can acquire a known corpus snapshot and test their own ingestion.
An optional overview connects files to the research and proposed experiments.

## Agreed boundaries

| Decision | Reason |
| --- | --- |
| One Railway project for the library, with one bucket per corpus. | One operational home; each corpus has a clear storage boundary. A new corpus adds a bucket, not another project. |
| One separate GitHub repository for the catalog, research, acquisition scripts and agent workflow. | Chamber and future projects consume the same library without copying its implementation. A new corpus adds a catalog entry, not another repository. |
| Actual corpus files live in Railway buckets, with local working copies outside repositories. | Git holds the tools and descriptions. Native records and large archives do not enter Git history or release assets. |
| Preserve published originals. | Parsing PDFs, Office files, email containers and archives is part of product R&D. Pre-extracted text cannot replace that input. |
| Use Railway's file browser and existing desktop tools first. | Browsing does not initially need a custom application. |
| Read-only storage credentials are optional. | These are recoverable public research corpora. Railway's bucket-wide credential is acceptable; snapshots, hashes and separate outputs support repeatability. |
| Deliver in phases with delegated implementation and direct demonstrations. | Andrew explicitly replaces the usual specification ceremony for this work. No S/V document series or requirements matrix is needed. |

The resources are the private [kaon-corpora repository](https://github.com/KnowHow-Software-Inc/kaon-corpora) and the [kaon-corpora Railway project](https://railway.com/project/2ce07b65-e436-4a96-bf87-dda84c09c4a1). Andrew created both before implementation. The production environment is `32104c8f-8156-408f-a0d1-95f163e3958d`. The library catalog owns bucket identifiers.

The library is independent of Chamber's container and PostgreSQL architecture.
It delivers source files; it does not implement Chamber's ingestion, classification
or business map. An ingestion run can be demonstrated only where the consumer
already has an appropriate entry point.

## Corpus and snapshot organization

A corpus has one stable catalog ID and one bucket. A snapshot identifies the
specific acquisition or release inside that bucket. Related Enron distributions
remain separate releases; their overlapping records and counts are not combined.

Keep the publisher's filenames and relative paths under a snapshot prefix, with
source IDs where needed to distinguish duplicate names. Each snapshot includes a
manifest with source URLs, acquisition time, original paths, byte counts, SHA-256
hashes and release terms. Record missing or failed downloads explicitly. Publish
a completed snapshot after acquisition and verification; an interrupted transfer
remains incomplete. Named completed snapshots are not overwritten in normal use.

A corpus catalog entry contains its history and scope, release and snapshot
locators, file formats, measured size, acquisition command, research links and
possible Chamber experiments. Keep one canonical copy of this research in the
corpus repo once it exists; Chamber retains a link and the rationale for using it.
Any later relocation of the current reports preserves provenance and fixes links.

Original source bytes, derived extraction and evaluation reference material have
separate locations and roles. A mirror's OCR is a derivative; a published PDF of
an email is still the available original, not a native mailbox. Transport archives
stay intact even if a separate unpacked view helps inspection. A replay run selects
its input files explicitly so later inquiry findings do not supply the answer.

The selected corpus ID, snapshot and file manifest travel with a run. Combining
corpora is explicit. Local cache paths include corpus and snapshot IDs; run outputs
are separate. These boundaries prevent accidental mixing without building a new
data platform.

## Human browsing

Start with Railway's Files tab for navigation and downloads. Configure Cyberduck
for desktop browsing and available Quick Look previews. Open downloaded Office
files in suitable local applications. Large mail containers and archives can
remain downloads until their inspection needs justify a dedicated tool.

A future web overview can add corpus descriptions, experiment ideas, snapshot and
file lists, filename/type filtering, PDF/image/text previews, publisher links and
original downloads. It reads the same catalog. Its server can generate signed
links so bytes travel directly from storage. Universal Office/mail rendering,
content indexing and a second catalog database are outside the initial scope.

## Agent workflow

The corpus repo owns a small CLI and an agent skill. The initial interface is
list, info and fetch, with a named snapshot and optional file selection. Exact
command names settle during implementation. Reuse an existing S3 transfer client
where it reduces code; acquisition adapters handle publisher-specific delivery.

A coding agent reads the catalog, selects a corpus and snapshot, obtains the
corresponding Railway bucket credentials through its available account access,
and fetches the selected originals. Credentials stay out of Git and command
output. The fetch reports its destination, snapshot identity, counts and failures.
The agent verifies hashes and passes a local directory to its consuming tool.

One host can share a completed cache across worktrees. Remote hosts fetch their
own copy; they cannot mount a laptop path. A container can mount the cached folder
read-only to prevent incidental parser writes, but this does not require a
read-only bucket key. Results and temporary extraction go elsewhere. Generic
access tooling has no dependency on Chamber's Python package or database.

## Delivery phases

### Phase 1: Establish the library with FCIC

Create the single corpus repo and Railway project. Add FCIC's bucket, first catalog
entry and repeatable acquisition script. Acquire the 44 currently reachable files
(about 189 MB), preserving the PDFs and workbook, and record the known broken link.
Publish the manifest and connect the existing browsing tools. The first usable
result is a browsable set of verified originals. The minimal repo starts here so
acquisition and resource setup are recorded from the first corpus.

### Phase 2: Make access reusable by agents

Generalize the working FCIC path into list/info/fetch and the agent skill. Document
host setup, credential retrieval, local caching and container access. Add a thin
reference from Chamber to the corpus workflow. Demonstrate use from Chamber and
from a neutral directory without importing Chamber code. Exercise repeat fetches
and separate run outputs. Use consumer ingestion only if its entry point exists;
file access alone does not prove parsing or classification.

### Phase 3: Add distinct real workloads

Add a connected McKinsey engagement and one Enron release, then El Faro and a
bounded Horizon case. Each acquisition has one named scope, one bucket and catalog
entry per corpus, plus separate snapshots for related releases. Use the ranked
research to choose connected records. Measure object counts, bytes and recurring
cost before expanding unknown-size archives. Preserve native artifacts and avoid
acquiring overlapping releases without an experiment that needs them.

### Phase 4: Optional research-linked overview

Use the browsing experience to decide whether a custom overview saves enough work.
If useful, add the small catalog-driven browser described above in the same corpus
repo, deployed in the same Railway project. This is an optional phase, not an
assumption that a separate application must be built.

## Implementation coordination

The parent agent owns sequencing, integration, resource identity and phase reports.
Implementation and bounded review go to **GPT-6.1 Sol, high reasoning** subagents.
Agents share the active worktree for each repository; do not create agent-specific
checkouts. Once the corpus repository exists, library agents work in that shared
checkout and Chamber integration stays in the current Chamber worktree.

Delegate non-overlapping work: one infrastructure/acquisition owner, one catalog
and workflow owner when the interface is settled, and a separate reviewer of the
completed phase. Parallelize only independent files and operations. One owner
performs each infrastructure mutation. Subagents return changed files, resource
IDs, commands/results, limitations and a concrete demonstration. The parent checks
those results, resolves material findings and commits coherent phase changes.

Continue through the agreed core phases without repeating design approval. Surface
new cost, scope or access decisions when they materially change this design. Keep
checks proportionate and follow repository rules on tests. Do not expand to all
ranked corpora or build the optional viewer merely because resources are available.
