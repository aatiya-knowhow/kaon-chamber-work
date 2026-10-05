# Corpus library implementation — 3 October 2026

Archived 5 October 2026 from Chamber commit `305a55a`. Complete.

The [agreed design](../../features/corpus-library/design/CRPD-01-corpus-library.md)
uses one independent corpus repository and one Railway project. The library's
[verification record](https://github.com/KnowHow-Software-Inc/kaon-corpora/blob/main/docs/verification.md)
contains the transfer demonstrations and known limits. The initial library
release is [commit 49f1ce0](https://github.com/KnowHow-Software-Inc/kaon-corpora/tree/49f1ce0).

## Initial delivered resources

- Private repository: [KnowHow-Software-Inc/kaon-corpora](https://github.com/KnowHow-Software-Inc/kaon-corpora).
- Railway project: [kaon-corpora](https://railway.com/project/2ce07b65-e436-4a96-bf87-dda84c09c4a1), with five buckets in `iad`.
- Production environment: `32104c8f-8156-408f-a0d1-95f163e3958d`.
- No deployed service, volume or GitHub app connection is needed for the library.

The library catalog owns bucket IDs, named snapshots, manifest hashes and
acquisition recipes. Source files and manifests live in buckets and external
local caches. The repository contains research, metadata and access tools.

## Core phases

Phase 1 is complete. FCIC has 44 verified files, 188,836,885 bytes. Its manifest
records the one known broken publisher link. Uploaded bytes were read back and
hashed before publication.

Phase 2 is complete. The list/info/acquire/fetch CLI and canonical agent skill
work without Chamber imports. A fresh full FCIC fetch from a neutral directory
passed; a repeat from Chamber verified all 44 cached files without downloading
again. A separate skill-driven demonstration selected three early-deal PDFs
without later outcome references. The independent code review found no remaining
breaking issues. A discovered truncated-response risk was fixed and demonstrated.

Phase 3 is complete. The library has published McKinsey's 30-record Purdue E2E slice (18,414,836 bytes),
El Faro's 13-file safety/maintenance selection (18,635,578 bytes), and Horizon's
17-file mismatch response and investigation slice (79,184,243 bytes). Each passed
a full fetch with manifest and file-hash verification. Enron's selected TREC 2010
release published seven packages, 9,530,556,230 bytes, with no failures. All
uploaded bytes passed readback hashing and all sizes match publisher headers.
The normal fetch command verified all seven retained cached files; a separate
selected-file fetch downloaded and verified a 10,776,222-byte mapping file.
The five collections contain 111 files/packages, 9,835,627,772 bytes in total.

The optional overview remains deferred. The [browsing guide](https://github.com/KnowHow-Software-Inc/kaon-corpora/blob/main/docs/browsing.md)
uses Railway's Files view and documents optional Cyberduck settings. The browser
requires user sign-in; Cyberduck was not installed on the setup machine.

## Chamber integration and limits

Chamber has a [thin skill reference](https://github.com/KnowHow-Software-Inc/kaon-chamber/blob/305a55a/.agents/skills/corpus-library/SKILL.md)
and [integration note](https://github.com/KnowHow-Software-Inc/kaon-chamber/blob/305a55a/docs/development/corpus-library.md). The 84 source reports
and ranking moved to the corpus repository with provenance. The original unranked
input remains unchanged; its SHA-256 is
`8327cdc47709a68d2aede5806510da595d7cdd52b93867e63e635a6e440e74d3`.

Chamber currently exposes a health endpoint and has no ingestion entry point.
Access and integrity were demonstrated; parsing, classification and replay were
not. Source/reference/derivative roles do not establish a replay cutoff. Preserve
manifest identity and select eligible source records for each experiment.

The initial selected library is about 9.84 GB. At Railway's current storage rate,
a full month is approximately $0.15 before account credits, with workspace-wide
rounding. Direct bucket downloads and S3 operations are free. See the
[verified billing source](https://docs.railway.com/storage-buckets/billing).


## Expansion after manual verification

The user confirmed hosted-file browsing, a native Office-file check, and an
independent three-file FCIC fetch from a fresh Chamber chat. These checks passed.
They requested the remaining public records for the sampled collections.

The expansion is complete in [corpus commit aa2cb39](https://github.com/KnowHow-Software-Inc/kaon-corpora/tree/aa2cb39).
The earlier pilot snapshots remain available.

| Added snapshot | Published files | Bytes |
| --- | ---: | ---: |
| El Faro full docket | 519 | 867,943,113 |
| McKinsey full collection | 114,930 | 93,937,042,106 |
| Horizon public evidence | 19,640 | 182,315,617,282 |

Every upload passed readback hashing. Completed storage inventories and fresh
selected-file fetches passed. Horizon accounts for all 19,704 nonempty advertised
downloads: 19,640 stored files and 64 HTTP 404 exceptions. Separate coverage
metadata records 31 pages without attachments and 40 zero-byte placeholders.
McKinsey records 22 video IDs without downloadable artifacts. One Horizon
publisher size was stale; its current header agrees with the acquired bytes.
The library's verification record owns the detailed evidence and snapshot pins.

All eight snapshots total 135,200 files/packages and 286,956,230,273 payload bytes.
Approximate storage is $4.31/month before credits and workspace rounding, plus
metadata. Large acquisitions used temporary compute and roughly $14 for one
upload pass, plus retries. Both transfer deployments are stopped. Redundant
Horizon staging was removed after verification against the completed snapshot.

Enron archives remain intact. A complete archive inventory measured 928,430
files and 26,288,299,095 unpacked bytes across native attachments and publisher
email text. Both compressed hashes matched, with no unsafe paths or links.
Extract a separate external view when a consumer needs individual files; no
publisher download is needed. The library also supports manifest-only fetching
for selection before downloading a large collection.

Next work that does not require ingestion is to select workflow-sized record
sets, establish replay cutoffs and review expected facts against the original
records. Full mixed collections require this review before replay experiments.
