# First-run execution and verification

The [Field Tests workflow](../.github/workflows/field-tests-manual.yml) is merged into `main`. **A merged workflow is not evidence of a completed evaluation.** Check [its Actions run history](https://github.com/AreteDriver/arete-portfolio/actions/workflows/field-tests-manual.yml) before classifying any result.

## Approved pinned inputs
The workflow enforces exact repository/commit/file triples; arbitrary commits are rejected.

| Repository | Full upstream commit SHA | Root input |
|---|---|---|
| `anthropics/claude-code` | `49b8c35f8c57493c2f0eba1429ccaa9638ab8712` | `CLAUDE.md` |
| `browser-use/browser-use` | `aa093a843da5edbf323c0b4145c9b6a0db3ecde8` | `AGENTS.md` |
| `openai/openai-agents-python` | `64cd5dfc5a1579870dcbed0ea42b27aa84033fee` | `CLAUDE.md` |

These source files were confirmed accessible at the listed commit revisions. **File existence is not a successful analyzer result.**

## Operator procedure
1. Open the [workflow runs](https://github.com/AreteDriver/arete-portfolio/actions/workflows/field-tests-manual.yml). Inspect the push-triggered run for the workflow merge commit, if present.
2. If no run exists, select **Run workflow** on `main`, choose an approved repository and paste its exact commit SHA and path from the table.
3. Inspect the job status and logs. The job prints tool version, upstream revision, file SHA256, audit exit code and selected JSON summary fields. A job success does **not** verify individual findings.
4. If the job fails, inspect the failing step. Do not classify a failed parse, unavailable package, quota failure, or checkout error as a clean result.
5. Independently reproduce the audit in an isolated environment using `context-hygiene==0.3.3` and `ctx-hygiene audit <file> --format json`. Manually review the full output, then redact before publishing.
6. Populate [the case study template](CASE_STUDY_TEMPLATE.md) and [findings ledger](FINDINGS_LEDGER.md) with the workflow URL, source commit, input hash, tool version, verification and outcome.
7. Publish only after the protocol's G4 gate.

## Security and remaining hardening
- The workflow does not run upstream project setup scripts, tests, submodules or LFS.
- Context Hygiene is pinned to **0.3.3**. The GitHub Actions dependencies currently use **major version tags**, not immutable commit SHAs. Pin reviewed action SHAs before considering the harness hardened.
- The analyzer is installed from the package index without a hash-locked dependency set; for a strict reproducibility claim, retain a verified lockfile or wheel hashes.
- The workflow **does not preserve raw analyzer findings**. It deletes them after printing non-sensitive metadata. A separate controlled reviewer run is required to inspect findings.
- Verify package availability, runtime behavior, and any parser limitations from actual job output.
- GitHub Actions status is independent of the unrelated Vercel hosting/billing deployment status. Do not change Vercel protections for this program.
- Never publish secrets, private information, or employer materials.

## Run ledger
No run ID or successful result has been verified in this document. Record the first real run ID and result after inspecting GitHub Actions.
