# FT-CH-002 — manual review and reproduction

**Status:** G1 execution verified; same-environment aggregate and finding-content repeatability verified; G2 independent reproduction pending; G3 finding review pending; G4 publication blocked.

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
| DW-01 | Pending | Pending | Pending | Unreviewed | Pending | Pending |
| DW-02 | Pending | Pending | Pending | Unreviewed | Pending | Pending |
| DW-03 | Pending | Pending | Pending | Unreviewed | Pending | Pending |
| DW-04 | Pending | Pending | Pending | Unreviewed | Pending | Pending |
| DW-05 | Pending | Pending | Pending | Unreviewed | Pending | Pending |

Review each against these questions: Is it duplicated verbatim? Is it restating a requirement for a different scope? Does removing it change behavior or compliance? Is it a useful reminder rather than deadweight? Is the token estimate grounded in actual text? Classify as **confirmed redundancy**, **false positive**, or **inconclusive** and record rationale.

If proposing an edit, prepare a minimal diff in a disposable copy and compare before/after analyzer output. Do not send unsolicited upstream PRs. No public claim of five *confirmed* issues until all five have been inspected.

## Reproduction acceptance

G2 requires matching input hash, tool version, nonempty parser output and matching candidate counts. G3 requires a completed five-row matrix and human inspection. G4 requires attribution, license/privacy review and explicit limitations. Keep both clean and positive tests in the evidence ledger.
