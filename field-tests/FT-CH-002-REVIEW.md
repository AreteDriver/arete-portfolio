# FT-CH-002 — manual review and reproduction

**Status:** G1 execution verified; same-environment repeatability verified; G2 independent published-package reproduction passed on Stheno; G3 five exact duplicates and analyzer reasons verified, with independent token measurements recorded (human sign-off pending); G4 case-study publication blocked.

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

Updated from the private Stheno report and independent exact-text comparisons. The rationale column now records verified analyzer reason strings. Word-proxy estimates and independent cl100k_base measurements are distinct; see the [Stheno reproduction](FT-CH-002-STHENO-REPRODUCTION.md). Exact duplication does not establish that removal preserves intended behavior. Human sign-off remains pending.

| Candidate | Source line(s) | Tool's rationale | Necessary context or cross-reference | Reviewer classification | Token estimate | Decision |
|---|---|---|---|---|---|---|
| DW-01 | 380–386 | `exact duplicate of segment 32` | Earlier identical Agent History block | Confirmed exact duplication | 27 word proxy; 36 cl100k_base | No upstream edit |
| DW-02 | 387–397 | `exact duplicate of segment 33` | Earlier identical access methods block | Confirmed exact duplication | 91 word proxy; 118 cl100k_base | No upstream edit |
| DW-03 | 398–408 | `exact duplicate of segment 34` | Earlier identical analysis methods block | Confirmed exact duplication | 109 word proxy; 138 cl100k_base | No upstream edit |
| DW-04 | 409–414 | `exact duplicate of segment 35` | Earlier identical structured-output example | Confirmed exact duplication | 31 word proxy; 59 cl100k_base | No upstream edit |
| DW-05 | 415–419 | `exact duplicate of segment 36` | Earlier identical Structured Output block | Confirmed exact duplication | 19 word proxy; 43 cl100k_base | No upstream edit |

Review each against these questions: Is it duplicated verbatim? Is it restating a requirement for a different scope? Does removing it change behavior or compliance? Is it a useful reminder rather than deadweight? Is the token estimate grounded in actual text? Classify as **confirmed redundancy**, **false positive**, or **inconclusive** and record rationale.

If proposing an edit, prepare a minimal diff in a disposable copy and compare before/after analyzer output. Do not send unsolicited upstream PRs. No public claim of five *confirmed* issues until all five have been inspected.

## Reproduction acceptance

G2 requires matching input hash, tool version, nonempty parser output and matching candidate counts. G3 requires a completed five-row matrix and human inspection. G4 requires attribution, license/privacy review and explicit limitations. Keep both clean and positive tests in the evidence ledger.


## Candidate locator evidence (2026-10-21, historical checkpoint)

Run: https://github.com/AreteDriver/arete-portfolio/actions/runs/39545957085/job/119044056869

The five `deadweight` finding objects contain `reason`, `segment_index`, and `tokens_recoverable`. The workflow disclosed only allowlisted numeric locators: **segment indices 37, 38, 39, 40, and 41**, in reported order. Same-environment finding-content fingerprints matched. These indices are analyzer segments, **not source-file line numbers**; mapping requires the analyzer segmentation rules or a locally reviewed report. At this checkpoint, finding text and reasons had not been evaluated and independent reproduction remained pending. The subsequent Stheno run below supersedes that reproduction status; human sign-off and case-study publication remain pending.


## Source-level duplicate verification (2026-10-21, before Stheno reproduction)

Inspected `AreteDriver/context-hygiene` generic Markdown parser (default branch) and the pinned `browser-use/browser-use` `AGENTS.md` at `aa093a843da5edbf323c0b4145c9b6a0db3ecde8`. The parser splits markdown sections on headings (`#` through `######`) into zero-based SYSTEM segments. Reconstructing that segmentation yielded **113 segments**, matching the audited count. Exact string comparison confirmed five duplicates:

| Flagged index | Earlier identical index | Pinned source lines | Section heading |
|---|---|---|---|
| 37 | 32 | 380–386 | Agent History |
| 38 | 33 | 387–397 | Access useful information |
| 39 | 34 | 398–408 | Analysis methods |
| 40 | 35 | 409–414 | Structured output (when using output_model_schema) |
| 41 | 36 | 415–419 | Structured Output |

**Reviewer conclusion:** All five flagged segments are exact-text duplicates of preceding segments in this pinned file. This confirms duplicate detection for these five cases; it does not verify token savings independently, approve upstream edits, or demonstrate effectiveness on other repositories. That initial source inspection used the analyzer repository's default-branch parser. The subsequent Stheno run verified the installed published `0.3.2` parser and completed G2. No raw finding report was published.


## Independent Stheno reproduction (2026-10-21)

The [sanitized reproduction summary](FT-CH-002-STHENO-REPRODUCTION.md) and [allowlisted results](FT-CH-002-STHENO-RESULTS.json) record the completed run on Ubuntu 26.04.1 LTS, Linux 7.0.0-38-generic, x86_64, CPython 3.14.4. A fresh virtual environment installed published `context-hygiene==0.3.2`; the wheel hash and pinned input hash were verified. Telemetry was disabled and analyzer state stayed in the disposable private workspace.

The audit exited 0 with valid JSON and empty stderr: **113 segments; five deadweight findings at indices 37–41; 277 estimated recoverable tokens**. The remaining baseline metrics matched. Each reason string matched `exact duplicate of segment N` for earlier indices 32–36. Independent text extraction and the installed parser agreed on all five content hashes and exact duplication.

Independent word-proxy arithmetic reproduced **277** (`27 + 91 + 109 + 31 + 19`). A separate tokenizer environment using tiktoken 0.14.0 and cl100k_base measured **394** tokens summed per segment (`36 + 118 + 138 + 59 + 43`), or **396** when the five texts are joined with blank lines. The baseline analyzer environment did not contain tiktoken. Thus 277 is a reproduced heuristic estimate, not a measured model-token saving.

## Review qualifications

The earlier package-index networking failure is historical; this Stheno run successfully fetched the published package. All five analyzer reasons were inspected through exact-string checks against the private report, and all duplicate pairs were independently compared. The completed matrix records those technical checks; the protocol's human G3 sign-off remains pending.

Repetition may be intentional for separate readers or contexts. No upstream removal is authorized, no behavior-preserving fix is claimed, and this single pinned input does not demonstrate general analyzer accuracy. Raw reports remain private on Stheno; only sanitized evidence is proposed for this repository. G4 case-study publication remains blocked.
