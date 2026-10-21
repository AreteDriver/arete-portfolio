# Repository selection — pilot cohort

**Status:** Not selected. The table is a sampling plan, not a claim that these repositories have been tested.

## Sampling rules
- Ten different external repository owners when feasible; no Arete-controlled repositories.
- Public repository with pinned full commit SHA, accessible license, and relevant static input files.
- Aim for different project sizes, maintenance activity and ecosystems.
- Record selection before inspecting tool findings; include at least two deliberately ordinary/low-risk examples.
- Agent Lint requires a **supported workflow YAML dialect**; do not feed arbitrary YAML and label output meaningful.
- Context Hygiene requires suitable agent instructions or context documents. Assess support for each format before running.
- If no suitable files exist, mark ineligible and replace the candidate transparently.

| Slot | Tool | Repository URL | Pinned SHA | Input paths | Status |
|---|---|---|---|---|---|
| 01 | Context Hygiene | TBD | TBD | TBD | Unselected |
| 02 | Context Hygiene | TBD | TBD | TBD | Unselected |
| 03 | Context Hygiene | TBD | TBD | TBD | Unselected |
| 04 | Context Hygiene | TBD | TBD | TBD | Unselected |
| 05 | Context Hygiene | TBD | TBD | TBD | Unselected |
| 06 | Context Hygiene | TBD | TBD | TBD | Unselected |
| 07 | Agent Lint | TBD | TBD | TBD | Unselected |
| 08 | Agent Lint | TBD | TBD | TBD | Unselected |
| 09 | Agent Lint | TBD | TBD | TBD | Unselected |
| 10 | Agent Lint | TBD | TBD | TBD | Unselected |

Before selecting Agent Lint candidates, confirm which workflow schemas the current analyzer actually understands.
