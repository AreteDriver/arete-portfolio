# Evaluation and safety protocol

1. Select a public, independently maintained repository with an explicit public URL, accessible revision SHA, relevant configuration files and a documented license. Exclude Arete-controlled repositories from the external pilot.
2. Record selection before running the tool. Include clean/negative examples; do not select only targets likely to produce findings.
3. Fetch a pinned commit into an isolated, nonprivileged environment with no production credentials. **Do not execute arbitrary setup scripts, hooks, install commands or repository code**. Prefer static file inspection; review any tool operation that could launch subprocesses or access networks before running it.
4. Record tool version, command, exit status, input file hashes, stdout/stderr, timestamp and environment details. Redact secrets and personal information before any publication.
5. Verify each finding manually against the pinned source and rule definition. Classify as confirmed, false positive, inconclusive, or informational. Heuristic warnings are not verified security vulnerabilities.
6. For claimed fixes, test before and after on a disposable copy, preserving original source and showing a minimal diff. Never modify the upstream repository without maintainer consent.
7. Publish both positive and negative results with clear limitations, links to original authors and a reproduction recipe. If publication risks exposing a vulnerability, credential or personal information, do not publish details; follow the upstream security policy.
8. Contact maintainers only when there is a verified, actionable benefit; no automated outreach, mass PRs, or promotional issue filing.

## Required evidence
Repository URL, owner, license, pinned commit, selected path(s), tool version/commit, command, execution environment, result classification, redacted logs or hashes, reviewer decision, reproduction status, limitations, and publication/upstream-contact status.

## Gates
- **G0 candidate:** independently maintained, relevant files found.
- **G1 safe to run:** inputs and execution paths reviewed; no untrusted code execution.
- **G2 reproducible:** results repeat on pinned source and tool version.
- **G3 verified:** findings manually reviewed and limitations recorded.
- **G4 publishable:** attribution, license, privacy and responsible disclosure checks passed.

Do not use proprietary Toyota code, documentation or TLS BOLI. TLS BOLI remains local-only, without public-facing components.
