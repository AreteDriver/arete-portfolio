# Reproducing five duplicate parser segments in Browser Use documentation

**Status: approved for repository publication.** Reproduction verified; human G3 approved for confirmed exact duplication only. AreteDriver approved G4 and explicitly authorized publication and merge on 2026-10-21. Publication target: the portfolio repository; website deployment is tracked separately.

Context Hygiene 0.3.2 reported five duplicate segments in a pinned Browser Use instruction file. A separate Linux run reproduced the result using the published package, and exact-text checks confirmed each duplicate. The exercise also exposed two limits worth reporting: five parser segments span two repeated documentation sections, and the analyzer's 277-token estimate differs from the 394 tokens measured with a separate tokenizer.

## Test record

| Field | Value |
|---|---|
| Case | FT-CH-002 |
| Upstream | [browser-use/browser-use](https://github.com/browser-use/browser-use) |
| Upstream license | [MIT; copyright 2024 Gregor Zunic](https://github.com/browser-use/browser-use/blob/aa093a843da5edbf323c0b4145c9b6a0db3ecde8/LICENSE) |
| Revision | `aa093a843da5edbf323c0b4145c9b6a0db3ecde8` |
| Input | Root `AGENTS.md` |
| Input SHA-256 | `a88c42c6635b09b590108a5e1a161886f3dcedab824e69a080d5de8ccd575ab5` |
| Analyzer | Published PyPI `context-hygiene==0.3.2` |
| Analyzer artifact | Wheel SHA-256 `201684e2562e45cbf7d625a9000a962e8f86089cf5aaa2034a186619447cdef9`; no source-commit mapping asserted |
| Environment | Stheno; Ubuntu 26.04.1 LTS; Linux 7.0.0-38-generic; x86_64; CPython 3.14.4; isolated virtual environment |
| Run timestamp | `2026-10-21T06:00:06.270763+00:00` |
| Audit command | `CONTEXT_HYGIENE_TELEMETRY=0 ctx-hygiene audit AGENTS.md --format json` |
| Exit status | 0; valid JSON; empty stderr |
| Result | Cross-environment reproduction passed; five exact duplicate parser segments confirmed |
| Reviewer | Codex agent plus AreteDriver human G3 sign-off for exact duplication only; no upstream endorsement claimed |
| Publication / contact | Repository publication authorized; no upstream issue, PR or private report |

## Why this input?

Browser Use is independently maintained and exposes a public Markdown instruction file that the analyzer can read statically. Candidate screening established the file's existence before the recorded audit. The portfolio's cohort-selection table still says unselected; this case does not establish a preregistered or representative sampling process. Its purpose is to examine one observed result, not estimate prevalence across repositories.

## What reproduced

The Stheno run used a disposable workspace, fetched the exact upstream Git object, extracted the input without running upstream code, and verified its SHA-256 before and after analysis. The published analyzer was installed in a fresh virtual environment; telemetry was disabled and analyzer state remained inside that workspace.

The output matched the earlier GitHub Actions baseline: 113 segments, 6,008 estimated tokens, five deadweight findings, 277 estimated recoverable tokens, grade A and staleness score 0.13. It also reported zero contradictions, zero compression candidates and 113 staleness results. Matching output demonstrates reproducibility for this input and package. Grade A and zero counts do not establish correctness of the other analysis passes.

All five exact reason strings were checked against the private report. Independent text comparisons mapped flagged indices 37–41 to identical earlier indices 32–36:

| Flagged index | Earlier index | Verified reason | Source lines | Word-proxy estimate | cl100k_base tokens |
|---|---|---|---|---|---|
| 37 | 32 | exact duplicate of segment 32 | 380–386 | 27 | 36 |
| 38 | 33 | exact duplicate of segment 33 | 387–397 | 91 | 118 |
| 39 | 34 | exact duplicate of segment 34 | 398–408 | 109 | 138 |
| 40 | 35 | exact duplicate of segment 35 | 409–414 | 31 | 59 |
| 41 | 36 | exact duplicate of segment 36 | 415–419 | 19 | 43 |
| Total | | | | 277 | 394 |

The [pinned source](https://github.com/browser-use/browser-use/blob/aa093a843da5edbf323c0b4145c9b6a0db3ecde8/AGENTS.md#L380-L419) provides the context. The generic parser splits on heading-shaped lines even inside fenced code. Three flagged segments begin at Python comments in an example. These five fragments cover two repeated sections, Agent History and Structured Output; they are not five independent upstream defects or independent instructions. No individual fragment was proposed for deletion.

## What the token numbers mean

The baseline environment had no tiktoken installation. Version 0.3.2 therefore used a word-count proxy, applying `int(words * 1.3)` separately to each nonempty segment, with a minimum of one. Independent arithmetic reproduced 27 + 91 + 109 + 31 + 19 = 277. The five segments contain 214 whitespace-delimited words; applying the formula once to that combined word count would yield 278 because rounding boundaries differ.

A second isolated environment used tiktoken 0.14.0 with cl100k_base. Encoding each segment separately produced 394 tokens in total; joining them with blank lines produced 396. This tokenizer measurement was separate from the published-package baseline, and did not change that environment. Neither number measures a model's actual input bill, behavioral effect, or savings in a running agent.

## Reproduction recipe

These commands are a reader recipe, not an additional execution result. Use an isolated Linux environment. The raw reports and installation metadata are private local outputs. Do not upload or commit them.

```bash
set -euo pipefail
umask 077
case_dir=$(mktemp -d /tmp/ft-ch-002.XXXXXXXX)
cd "$case_dir"
git init upstream
git -C upstream -c core.hooksPath=/dev/null fetch --depth=1 \
  https://github.com/browser-use/browser-use.git \
  aa093a843da5edbf323c0b4145c9b6a0db3ecde8
test "$(git -C upstream rev-parse FETCH_HEAD)" = \
  aa093a843da5edbf323c0b4145c9b6a0db3ecde8
git -C upstream show \
  aa093a843da5edbf323c0b4145c9b6a0db3ecde8:AGENTS.md > AGENTS.md
printf '%s  AGENTS.md\n' \
  a88c42c6635b09b590108a5e1a161886f3dcedab824e69a080d5de8ccd575ab5 \
  | sha256sum -c -
chmod 400 AGENTS.md
python3 -m venv analyzer-venv
analyzer-venv/bin/python -m pip install \
  --index-url https://pypi.org/simple --only-binary=:all: \
  --report install.json 'context-hygiene==0.3.2'
analyzer-venv/bin/python -m pip freeze --all > dependencies.txt
CONTEXT_HYGIENE_TELEMETRY=0 CONTEXT_HYGIENE_DIR="$case_dir/state" \
  analyzer-venv/bin/ctx-hygiene audit AGENTS.md --format json \
  > report.json 2> audit-stderr.txt
```

Verify the installed wheel hash against the recorded artifact, and record Python and resolved dependency versions. The analyzer's transitive dependencies were not version-locked by this recipe, so future installations can differ. Install tiktoken only in a separate environment for the supplemental measurement; installing it in the baseline environment changes token-count behavior.

## Outcome and limitations

The published package reproduced the five exact duplicate parser segments on an environment separate from GitHub Actions. This verifies a narrow behavior on one pinned input. It does not establish general analyzer accuracy, usefulness of deleting repeated text, author intent, behavior preservation, runtime savings or a security defect. No upstream changes were made.

The reproduction environment is separate; the reviewer is the same Codex agent that prepared the evidence. AreteDriver inspected the repeated source sections and approved G3 for confirmed exact duplication only on 2026-10-21, accepting parser and token-count limitations. Broader evaluation needs other inputs, including clean examples and false positives, before making claims about accuracy or value.

## Attribution and disclosure review

Credit for Browser Use and its documentation belongs to its contributors. The pinned repository LICENSE names Gregor Zunic and the MIT license. Context Hygiene's published 0.3.2 wheel carries Business Source License 1.1 with James C. Young (AreteDriver) as licensor; it is not represented here as MIT-licensed software. This draft uses original explanatory prose, numeric results, short reason strings and links; it redistributes neither upstream source nor the analyzer artifact.

The draft includes no raw report, complete source excerpt, personal account path, network address or credential material. No security vulnerability was identified by this duplicate check, and maintainer outreach is not proposed. Temporary private evidence on Stheno was rehashed during review; its location is withheld. A checksum identifies evidence but does not make the private report publicly verifiable or guarantee retention.

[PR #19](https://github.com/AreteDriver/arete-portfolio/pull/19) contains the sanitized reproduction evidence and this case study. Human G3 approval is limited to confirmed exact duplication; G4 content approval and explicit repository publication/merge authorization were received on 2026-10-21. This approval does not authorize removing documentation from Browser Use. Website deployment is separate from repository publication.
