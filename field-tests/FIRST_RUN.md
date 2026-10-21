# Controlled first-run procedure

The [manual GitHub Actions workflow](../.github/workflows/field-tests-manual.yml) is a **proposed execution harness**, not evidence that any audit has run.

## Operator checklist
1. Obtain the **full repository commit SHA** independently. Do not use the file blob SHAs in candidate screening.
2. Confirm the public repository license and inspect the chosen instruction file for privacy/security concerns.
3. Merge the workflow after review; `workflow_dispatch` generally requires the workflow to be present on the default branch.
4. Manually dispatch an allowlisted repository, full commit SHA and matching root file.
5. Record workflow run URL, commit, file SHA256, analyzer version and exit code.
6. Review raw output only in a controlled environment, redact as needed and verify findings manually before publication.
7. Create a case study with a reproduction command and classify verified, clean, false positive, inconclusive or execution failure.

## Safety and limitations
- The workflow checks out public upstream files without submodules or LFS and **does not run upstream setup scripts, tests or executables**.
- It uses a GitHub-hosted runner and installs the published Context Hygiene package; its version is recorded but **not pinned yet**. Pin a verified package release and action SHAs before treating this as a hardened benchmark harness.
- Raw analyzer output is not uploaded or automatically published. It is deleted at job completion. A local reviewer-run or a separately approved secure artifact workflow is required to retain redacted findings.
- The command's exit code alone does not prove that an audit succeeded or found a problem.
- The allowlist restricts eligible repository names, not the content of the pinned revision. An operator must verify the selected commit belongs to the intended upstream repository.
- No results have been obtained as part of preparing this workflow.
