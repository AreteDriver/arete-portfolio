# Findings ledger

## Verified execution evidence

| Case ID | Repository@SHA | Analyzer | Execution | Report summary | Independent reproduction | Manual findings review | Published case study |
|---|---|---|---|---|---|---|---|
| FT-CH-001 | `anthropics/claude-code@49b8c35f8c57493c2f0eba1429ccaa9638ab8712` (`CLAUDE.md`) | `context-hygiene==0.3.2` | **PASS** — exit 0; valid JSON | Grade A; 2 segments; 430 estimated tokens; 0 recoverable; staleness 0.1 | Pending | Pending | No |

**Primary evidence:** [GitHub Actions run 39541571190, job 119030275457](https://github.com/AreteDriver/arete-portfolio/actions/runs/39541571190/job/119030275457), executed on portfolio merge commit `9df5e586cc792d48839fa48afa48d86a21b7815e`.

**Input SHA-256:** `0fafa0a267c1a7320c4d9ccb4acaa27bdb7062e34cb23569f32a7a472ed0c7ac`.

The job logs verify that Python 3.11.17 installed Context Hygiene 0.3.2, checked out the pinned upstream revision, and ran `ctx-hygiene audit upstream/CLAUDE.md --format json`. The command exited 0 and its output parsed as JSON with the reported metrics above. The workflow deleted the raw JSON report at job completion. Therefore **the underlying findings were not independently inspected**, and grade A must not be represented as proof of correctness or absence of issues.

**Classification:** successful execution; finding classification **inconclusive pending review**. Do not label as confirmed finding, clean, or false positive until the raw report has been reviewed and the result reproduced.

## Next evidence gate

1. Independently reproduce the pinned file and analyzer version in an isolated environment.
2. Capture and review the full JSON report; redact any sensitive content before publishing.
3. Record parser behavior, number of findings, manually verified findings and false positives.
4. Complete the [case study template](CASE_STUDY_TEMPLATE.md) and protocol G4 gate.

Allowed finding outcomes: **confirmed finding**, **clean/no finding**, **false positive**, **inconclusive**, **execution failure**. Workflow execution status is recorded separately from findings classification.
