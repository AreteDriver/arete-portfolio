# FT-CH-002 — manual review and reproduction

**Status:** G1 execution verified; same-environment repeatability verified; G2 independent published-package reproduction pending; G3 source-level duplicate review completed (token estimates unverified); G4 publication blocked.

- Source: https://github.com/browser-use/browser-use
- Revision: `aa093a843da5edbf323c0b4145c9b6a0db3ecde8`
- Input: `AGENTS.md`
- Expected SHA-256: `a88c42c6635b09b590108a5e1a161886f3dcedab824e69a080d5de8ccd575ab5`
- Analyzer: `context-hygiene==0.3.2`
- First aggregate evidence: https://github.com/AreteDriver/arete-portfolio/actions/runs/39543944466/job/119037751455
- Finding-content fingerprint repeatability: https://github.com/AreteDriver/arete-portfolio/actions/runs/39545477949/job/119042572435 — two audits within the same job produced identical canonicalized SHA-256 fingerprints for all five deadweight findings and all 113 staleness results; aggregate metrics also matched (113 segments, 6,008 tokens, 277 recoverable, grade A). This confirms same-environment consistency only, not correctness or independent reproduction.
- Repeatability evidence: https://github.com/AreteDriver/arete-portfolio/actions/runs/39544805338/job/119040438800 — two audits within one job matched on segments (113), tokens (6,008), recoverable tokens (277), staleness (0.13), grade (A), and all finding counts (deadweight 5, contradictions 0, compression 0, staleness 113). Log marker: `same_environment_repeat=true`. This does **not** constitute independent reproduction or manual verification.
- Reported: 113 segments, 6,008 tokens, 277 estimated recoverable tokens, five deadweight candidates, zero contradictions, zero compression candidates, staleness score 0.13, grade A.

## Isolated reproduction

Run in a disposable Linux environment with no production secrets. The upstream checkout is **data only**: do not install upstream dependencies or execute its code. Use a separate environment for the analyzer. The following is a reviewer procedure, **not evidence that reproduction was performed**.

```bash
set -euo pipefail
mkdir -p /tmp/arete-ft-ch-002
cd /tmp/arete-ft-ch-002
git -c core.hooksPath=/dev/null clone --no-checkout --filter=blob:none \
  https://github.com/browser-use/browser-use.git upstream
git -C upstream -c core.hooksPath=/dev/null checkout --detach aa093a843da5edbf323c0b4145c9b6a0db3ecde8
printf '%s  %s\n' 'a88c42c6635b09b590108a5e1a161886f3dcedab824e69a080d5de8ccd575ab5' 'upstream/AGENTS.md' | sha256sum -c -
python3.11 -m venv analyzer-venv
analyzer-venv/bin/python -m pip install 'context-hygiene==0.3.2'
CONTEXT_HYGIENE_TELEMETRY=0 analyzer-venv/bin/ctx-hygiene audit upstream/AGENTS.md --format json > report.json 2> audit-stderr.txt
analyzer-venv/bin/python - <<'PY'
import json
r=json.load(open('report.json'))
for key in ('grade','total_segments','total_tokens','tokens_recoverable','staleness_score'):
    print(key, r.get(key))
for key in ('deadweight','contradictions','compression_candidates','staleness_results'):
    print(key+'_count',len(r.get(key, [])))
PY
```

Do not commit `report.json` or paste unreviewed raw findings into public CI logs. Store the raw report in a private reviewer workspace; inspect and redact before publishing.

## Five-finding review matrix

Complete one row per analyzer deadweight candidate after opening `report.json`. Use the tool's exact finding locator or quoted text to map each candidate back to the pinned file. If the report lacks a reliable locator, record **inconclusive**, not a guess.

| Candidate | Source line(s) | Tool's rationale | Necessary context or cross-reference | Reviewer classification | Token estimate | Decision |
|---|---|---|---|---|---|---|
| DW-01 | 380–386 | Exact duplicate of segment 32 | Earlier identical Agent History block | Confirmed exact duplication | Not independently measured | No upstream edit |
| DW-02 | 387–397 | Exact duplicate of segment 33 | Earlier identical access methods block | Confirmed exact duplication | Not independently measured | No upstream edit |
| DW-03 | 398–408 | Exact duplicate of segment 34 | Earlier identical analysis methods block | Confirmed exact duplication | Not independently measured | No upstream edit |
| DW-04 | 409–414 | Exact duplicate of segment 35 | Earlier identical structured-output example | Confirmed exact duplication | Not independently measured | No upstream edit |
| DW-05 | 415–419 | Exact duplicate of segment 36 | Earlier identical Structured Output block | Confirmed exact duplication | Not independently measured | No upstream edit |

Review each against these questions: Is it duplicated verbatim? Is it restating a requirement for a different scope? Does removing it change behavior or compliance? Is it a useful reminder rather than deadweight? Is the token estimate grounded in actual text? Classify as **confirmed redundancy**, **false positive**, or **inconclusive** and record rationale.

If proposing an edit, prepare a minimal diff in a disposable copy and compare before/after analyzer output. Do not send unsolicited upstream PRs. No public claim of five *confirmed* issues until all five have been inspected.

## Reproduction acceptance

G2 requires matching input hash, tool version, nonempty parser output and matching candidate counts. G3 requires a completed five-row matrix and human inspection. G4 requires attribution, license/privacy review and explicit limitations. Keep both clean and positive tests in the evidence ledger.


## Candidate locator evidence (2026-10-21)

Run: https://github.com/AreteDriver/arete-portfolio/actions/runs/39545957085/job/119044056869

The five `deadweight` finding objects contain `reason`, `segment_index`, and `tokens_recoverable`. The workflow disclosed only allowlisted numeric locators: **segment indices 37, 38, 39, 40, and 41**, in reported order. Same-environment finding-content fingerprints matched. These indices are analyzer segments, **not source-file line numbers**; mapping requires the analyzer segmentation rules or a locally reviewed report. No finding text or reasons have been manually evaluated. G2 independent reproduction and G3 manual classification remain pending; publication remains blocked.


## Source-level duplicate verification (2026-10-21)

Inspected `AreteDriver/context-hygiene` generic Markdown parser (default branch) and the pinned `browser-use/browser-use` `AGENTS.md` at `aa093a843da5edbf323c0b4145c9b6a0db3ecde8`. The parser splits markdown sections on headings (`#` through `######`) into zero-based SYSTEM segments. Reconstructing that segmentation yielded **113 segments**, matching the audited count. Exact string comparison confirmed five duplicates:

| Flagged index | Earlier identical index | Pinned source lines | Section heading |
|---|---|---|---|
| 37 | 32 | 380–386 | Agent History |
| 38 | 33 | 387–397 | Access useful information |
| 39 | 34 | 398–408 | Analysis methods |
| 40 | 35 | 409–414 | Structured output (when using output_model_schema) |
| 41 | 36 | 415–419 | Structured Output |

**Reviewer conclusion:** All five flagged segments are exact-text duplicates of preceding segments in this pinned file. This confirms duplicate detection for these five cases; it does not verify token savings independently, approve upstream edits, or demonstrate effectiveness on other repositories. The current source inspection uses the analyzer repository's default-branch parser, not a verified installed `0.3.2` source artifact; G2 independent installed-version reproduction remains pending. No raw finding report was published.


## Review qualifications

The five classifications above are based on independent exact-text comparison of the pinned public input after reconstructing the documented Markdown segmentation. They do **not** quote the raw analyzer `reason` strings, which remain withheld. The rationale column therefore records reviewer reasoning, not the analyzer's exact rationale. `277` recoverable tokens is the analyzer's estimate and has not been independently measured. The repeated block might be intentional for separate readers or contexts; no upstream removal is authorized by this review. An attempt to fetch the published `context-hygiene==0.3.2` package in a separate execution environment was blocked by unavailable package-index networking; it is not an independent reproduction result.
