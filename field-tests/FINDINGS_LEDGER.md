# Findings ledger

## Verified execution evidence

| Case ID | Repository@SHA | Analyzer | Execution | Report summary | Independent reproduction | Manual findings review | Published case study |
|---|---|---|---|---|---|---|---|
| FT-CH-001 | `anthropics/claude-code@49b8c35f8c57493c2f0eba1429ccaa9638ab8712` (`CLAUDE.md`) | `context-hygiene==0.3.2` | **PASS** — exit 0; valid JSON | Grade A; 2 segments; 430 estimated tokens; 0 recoverable; staleness 0.1; second run: contradictions 0, deadweight 0, compression candidates 0, staleness results 2 | Pending | Pending | No |
| FT-CH-002 | `browser-use/browser-use@aa093a843da5edbf323c0b4145c9b6a0db3ecde8` (`AGENTS.md`) | `context-hygiene==0.3.2` | **PASS** — exit 0; valid JSON | Grade A; 113 segments; 6,008 tokens; 277 recoverable (4.6%); staleness 0.13; 5 deadweight candidates; 0 contradictions; 0 compression candidates; 113 staleness results | Pending | Pending | No |

**Follow-up evidence:** [GitHub Actions run 39543333571, job 119035834116](https://github.com/AreteDriver/arete-portfolio/actions/runs/39543333571/job/119035834116), after merge commit `984bd3938f3e0db189b179bde3ba3017cda86f60`. The stricter validator passed with nonempty parsed content and all four finding collections present. Heuristic counts were contradictions 0, deadweight 0, compression candidates 0, and staleness results 2. These are unreviewed analyzer outputs, not independently validated findings.

**Primary evidence:** [GitHub Actions run 39541571190, job 119030275457](https://github.com/AreteDriver/arete-portfolio/actions/runs/39541571190/job/119030275457), executed on portfolio merge commit `9df5e586cc792d48839fa48afa48d86a21b7815e`.

**Input SHA-256:** `0fafa0a267c1a7320c4d9ccb4acaa27bdb7062e34cb23569f32a7a472ed0c7ac`.

The job logs verify that Python 3.11.17 installed Context Hygiene 0.3.2, checked out the pinned upstream revision, and ran `ctx-hygiene audit upstream/CLAUDE.md --format json`. The command exited 0 and its output parsed as JSON with the reported metrics above. The workflow deleted the raw JSON report at job completion. Therefore **the underlying findings were not independently inspected**, and grade A must not be represented as proof of correctness or absence of issues.

**Classification:** successful execution; finding classification **inconclusive pending review**. Do not label as confirmed finding, clean, or false positive until the raw report has been reviewed and the result reproduced.

## FT-CH-002 evidence

**Job:** [GitHub Actions run 39543944466, job 119037751455](https://github.com/AreteDriver/arete-portfolio/actions/runs/39543944466/job/119037751455), after merge commit `6370194cd91e1c66e45108d8e49759336a17437a`.

**Input SHA-256:** `a88c42c6635b09b590108a5e1a161886f3dcedab824e69a080d5de8ccd575ab5`.

The analyzer completed and its JSON passed strict validation. Five deadweight candidates and 277 estimated recoverable tokens are **unreviewed heuristic outputs**. No raw findings were retained. Finding classification remains **inconclusive pending manual inspection and independent reproduction**; do not describe them as confirmed issues or upstream defects.

## Next evidence gate

1. Independently reproduce the pinned file and analyzer version in an isolated environment.
2. Capture and review the full JSON report; redact any sensitive content before publishing.
3. Record parser behavior, number of findings, manually verified findings and false positives.
4. Complete the [case study template](CASE_STUDY_TEMPLATE.md) and protocol G4 gate.

Allowed finding outcomes: **confirmed finding**, **clean/no finding**, **false positive**, **inconclusive**, **execution failure**. Workflow execution status is recorded separately from findings classification.
