# Knowledge System Design Notes

## Approach

This submission implements a deterministic, snapshot-grounded knowledge engine. It indexes all supplied pages in chronological order and answers from the most recent snapshot whose `taken_on` is on or before the requested `as_of` date. It also parses explicit dates within natural-language questions. No live website calls or external AI services are used.

Authoritative product pages take priority for deposit APYs, dividend rates, loan APRs, and mortgage rates. Structured Markdown tables are interpreted using account/product, balance tier, certificate term, jumbo/standard designation, vehicle model year, loan duration, mortgage type, and loan amount. Financial products with different qualifiers are not treated as contradictions. Generic lexical retrieval is a conservative fallback for other facts; rate pages supersede old calculators and promotional content. Missing rates are not guessed. Earlier fee information can be retained when a later crawl omitted the fee page, but is expressly marked as last-published/not reconfirmed.

`conflicts()` extracts observed value changes from qualified authoritative rate tables across snapshots and adds detected cross-page disagreements with out-of-date calculators, guidance pages, and historical reports. Resolutions favor the most applicable authoritative source as of the latest available snapshot. Conflict detection is precision-oriented; its score against the private conflict gold set cannot be determined from the public materials.

The three contract methods are implemented in `solution.py`; the grader-provided `contract` module is imported when available. Standalone fallback classes allow the source file to be imported without assuming a particular filesystem layout. The grade harness and data are never modified.

## Dependencies and environment

Target runtime: Python 3.11, standard library only. Local execution was verified on Python 3.13.5; the code also passes a Python 3.11 syntax grammar check. A native Python 3.11 interpreter was not available in this environment. No API keys, paid model endpoints, database servers, or network permissions are needed. The challenge data must remain private to the event and is deliberately not packaged with this submission.

## Validation

- Public offline scorer: **0.9690** over 40 questions (the theoretical ceiling with the bundled offline `must_include` checks, because one non-refusal list question has an empty required-token list).
- Bundled contract self-test with the supplied example conflict gold: **1.0000** over five questions and one conflict.
- 22 paraphrase/edge variants tied to known public facts: **22/22** token checks.
- 132 automatically generated time/tier queries from official published tables: **132/132** correct value and correct authoritative source URL (50 auto loan, 66 mortgage, 16 certificate). These are property tests, not the organizers' hidden eval.
- 23 additional manually authored domain/abstention probes: **23/23** checks.
- 40-question public scorer: approximately 4.2 seconds ingestion, 0.03 seconds p95 answer latency in this environment.

**Limitations:** Public paraphrase groups contain one question each; the reported 100% paraphrase consistency is therefore not a meaningful independent estimate of hidden paraphrase performance. The real hidden questions, expected conflict labels, and possible optional semantic judge settings were not provided. No hidden score is claimed.

## Additional variation and adversarial coverage (November 2026)

Another 336 locally authored, snapshot-grounded tests were added (261 surface variations plus 75 separately authored edge tests). They exercise abbreviated durations (`60 mo`, `10m`, `five yrs`), colloquial amounts (`15k`, `200k`, `15 grand`), alternate date spellings, 2026 snapshot transitions, auto model-year buckets, savings balance boundaries, mortgage amount thresholds, non-existent terms, certificate minimums, and explicit before-versus-now comparisons. All 336 pass after corrections. The pre-correction audited implementation passed 176 of the first 261; these are regression checks refined while repairing known failures, not an independently hidden test.

The separate testing package is intentionally excluded from the submission ZIP and repository. The actual hackathon data and internal gold labels remain unavailable to third-party viewers; do not commit the `snapshots` or `snapshots_zip` directories.
