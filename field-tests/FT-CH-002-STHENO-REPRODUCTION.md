# FT-CH-002: independent Stheno reproduction

Sanitized reproduction evidence for AreteDriver/arete-portfolio. This document is an evidence record, not a case study.

**Outcome: PASS for the requested published-package reproduction.** Executed 2026-10-21T06:00:06.270763+00:00 on Stheno, independently of the cited GitHub Actions environment. All requested input, version, segmentation, finding, reason, and estimated-token checks passed. This result establishes cross-environment reproducibility for one pinned input, and confirms five exact duplicates. It does not establish general analyzer accuracy or authorize upstream edits.

## Input and environment

- Source: [browser-use/browser-use](https://github.com/browser-use/browser-use/tree/aa093a843da5edbf323c0b4145c9b6a0db3ecde8); commit `aa093a843da5edbf323c0b4145c9b6a0db3ecde8`.
- Input: `AGENTS.md`; SHA-256 `a88c42c6635b09b590108a5e1a161886f3dcedab824e69a080d5de8ccd575ab5`. Verified before and after the audit.
- Runner: Ubuntu 26.04.1 LTS, Linux 7.0.0-38-generic, x86_64; CPython 3.14.4.
- Analyzer: published PyPI `context-hygiene==0.3.2`, installed from the public index in a fresh virtual environment, without editable installs or local analyzer source.
- Wheel SHA-256: `201684e2562e45cbf7d625a9000a962e8f86089cf5aaa2034a186619447cdef9`; matches public PyPI release metadata.
- Audit: `CONTEXT_HYGIENE_TELEMETRY=0 ctx-hygiene audit AGENTS.md --format json`; exit 0, valid JSON, empty stderr. Runtime telemetry check returned false; no telemetry database was created.
- Disposable private workspace; analyzer state redirected there with `CONTEXT_HYGIENE_DIR`. Directory mode 0700, raw report mode 0600. A fresh data-only Git repository fetched the exact upstream commit; `git show` extracted the input. No upstream code or dependencies were executed or installed. The extracted input was made read-only.

## Results and reason review

113 segments; 6,008 estimated tokens; 277 estimated recoverable tokens; grade A; staleness score 0.13. Counts: five deadweight findings, zero contradictions, zero compression candidates, 113 staleness results. These match the [published baseline protocol](https://github.com/AreteDriver/arete-portfolio/blob/bff13970b89d4c42ec9d84cc45a3d462e054ca1e/field-tests/FT-CH-002-REVIEW.md).

All five raw reason strings were checked against the exact allowlisted strings below. Independent text extraction confirmed byte-for-byte identical segment contents in each pair. Independent extraction also matched the installed 0.3.2 parser's segment-content hashes. Indices are zero-based analyzer segments, not line numbers.

| Index | Earlier index | Verified analyzer reason | Words | Analyzer / independent proxy | Independent cl100k_base |
|---|---|---|---|---|---|
| 37 | 32 | `exact duplicate of segment 32` | 21 | 27 | 36 |
| 38 | 33 | `exact duplicate of segment 33` | 70 | 91 | 118 |
| 39 | 34 | `exact duplicate of segment 34` | 84 | 109 | 138 |
| 40 | 35 | `exact duplicate of segment 35` | 24 | 31 | 59 |
| 41 | 36 | `exact duplicate of segment 36` | 15 | 19 | 43 |
| Total | | | 214 | 277 | 394 |

The 0.3.2 wheel uses tiktoken when available and otherwise estimates each nonempty segment as `max(int(word_count * 1.3), 1)`. The fresh baseline environment had no tiktoken. Independent arithmetic, without importing the analyzer's estimator, yielded `27 + 91 + 109 + 31 + 19 = 277`. Rounding applies separately to each segment; applying the formula once to all 214 words would yield 278.

For an independent tokenizer measurement, a second virtual environment installed tiktoken 0.14.0 and directly encoded the extracted text with `cl100k_base`. The sum of individual segment counts is **394**; joining those five sections with two newline characters yields **396**. Token counts depend on encoding and boundaries. The tokenizer environment did not alter the baseline analyzer environment.

## Discrepancies and limits

- No discrepancy against the requested baseline metrics, five indices, or five reason strings.
- **277 is a reproduced heuristic estimate, not a measured model-token saving.** The separate tokenizer measurement is 394 per-segment tokens, 117 above the proxy estimate. Neither measurement proves runtime cost savings or that removing the sections preserves intended behavior.
- Python 3.14.4 differs from the protocol's example Python 3.11. The published package declares Python >=3.10, and this run passed on Stheno's installed Python.
- Exact duplication is confirmed; whether repetition is intentional remains a contextual judgment. No upstream removal was attempted. No broader accuracy, publication-readiness, or case-study claim is made.

## Evidence and handling

Only this summary and `FT-CH-002-STHENO-RESULTS.json` are prepared for repository review. Raw JSON reports, stderr, dependency freeze, installation metadata, input text, local database, and tokenization outputs remain in the private Stheno workspace and were not copied into the portfolio. The private workspace location is withheld from this repository. Temporary evidence may be removed by Stheno's normal temporary-file cleanup; this is not an archival-storage claim.

Raw audit SHA-256: `24c44eaa71405a422d2ea8ba9510678f9baca42e866e70e4bea1da10173c992e`.
Empty audit stderr SHA-256: `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`.
Dependency-freeze SHA-256: `1d470971b648ef4db868acc2b8f1c9b05c054c50363d6342f9644d3073907243`.
Installation metadata SHA-256: `8da8879a60e51a76d27216a994fdd1fe36cf285777e8b7a091f4a3f89f70550f`.

Installed implementation hashes and per-segment content hashes are included in the sanitized JSON. Only reviewed allowlisted findings metadata is included; complete raw reports, secrets, user account names, network addresses, and credential paths are withheld. G4 content review and explicit repository publication authorization are recorded in the [review](FT-CH-002-REVIEW.md). See the [case study](FT-CH-002-CASE-STUDY.md); no upstream edit is proposed.
