# cloud-itonami-lei-96950077l0tn7barox36

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Capgemini SE.**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**Capgemini SE**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: Capgemini SE
- **LEI (ISO 17442)**: [96950077L0TN7BAROX36](https://search.gleif.org/#/record/96950077L0TN7BAROX36) (GLEIF-verified)
- **Jurisdiction**: FR
- **Website**: https://www.capgemini.com
- **Ticker**: CAP (Euronext Paris)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `facts.edn` — 65 verified registry facts with per-fact provenance (9 about the
  entity itself, 56 direct children). **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
kbb --backend sci scripts/verify-facts.cljk           # check the recorded facts against the live sources
kbb --backend sci scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Fourteen GLEIF/ISO requests back the file (`CHECKED 14` when it was written,
2026-08-22T20:27Z, golden copy 2026-08-22T08:00Z) — the LEI record (legal name
`CAPGEMINI`, entity **ACTIVE**, registration **ISSUED**, next renewal
2026-10-24; the two statuses are different fields and are recorded separately),
its 38 ISINs (counted from `meta.pagination.total`, not mirrored — at this
issuer's volume they turn over), its managing LOU and LEI-issuer accreditation
(Insee, LEI `969500Q2MA9VBQ8BG884`), registration authority `RA000189`
(Register of Companies / Sirene, SIREN `330703844`), ISO 20275 legal form `TPNT`
(*Société européenne*), reporting exceptions at both consolidation levels
(`NO_KNOWN_PERSON` — Capgemini SE is the top of its own group), and **56 direct
children**, each mirrored as its own `:direct-child` entity across four
15-per-page requests (jurisdictions span FR/DE/IT/SG/GB/JP/AU/US-DE/NL/ES/…).
The `direct-parent` and `ultimate-parent` endpoints answered `404` because
GLEIF publishes the exception side of that pair for this entity, which the
checker treats as a fact rather than a failure.

The checker's exit codes are three, not two: `0` every recorded fact matches the
live sources, `1` a citation broke or a fact drifted, `3` the check could not be
performed at all — an absent `facts.edn`, or every request failing at the
transport level. A check that could not run must not be indistinguishable from a
check that ran and found nothing, so it refuses to report a pass rather than
exiting 0. All four outcomes were exercised before this landed: unmodified `0`;
`:registration/next-renewal-date` edited one year forward → `1` naming
`DRIFT gleif-lei-record :registration/next-renewal-date`; the `gleif-isins`
entity deleted → `1` naming `ADDED gleif-isins`; the GLEIF host rewritten to an
unresolvable name → `3` (`INCONCLUSIVE … refusing to report a pass`).

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
