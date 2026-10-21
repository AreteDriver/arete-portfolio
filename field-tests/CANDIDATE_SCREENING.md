# Initial external candidate screening

**Status:** File-existence verification only, not completed audits or final cohort selection. GitHub file blob SHA is **not** a repository commit SHA.

| Candidate | Verified file | File blob SHA | Screening |
|---|---|---|---|
| [anthropics/claude-code](https://github.com/anthropics/claude-code) | [CLAUDE.md](https://github.com/anthropics/claude-code/blob/main/CLAUDE.md) | `410ad0f67663043920889a2b4cc380867acc1e3b` | Exists; Context Hygiene candidate |
| [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) | [AGENTS.md](https://github.com/langchain-ai/langgraph/blob/main/AGENTS.md) | `5e5f0c00bf74a0b6997f94e4772fba25408bfd3f` | Exists; Context Hygiene candidate |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | [AGENTS.md](https://github.com/browser-use/browser-use/blob/main/AGENTS.md) | `d8f2f6150b237b1a4bbd52e564831460f5083a86` | Exists; Context Hygiene candidate |
| [openai/openai-agents-python](https://github.com/openai/openai-agents-python) | [AGENTS.md](https://github.com/openai/openai-agents-python/blob/main/AGENTS.md) | `6cf28ce7e03d61f7b7c689b63d9f2ba5adf998df` | Exists; Context Hygiene candidate |

## Rejected or unconfirmed probes
Root-path checks for `openai/codex:AGENTS.md`, `crewAIInc/crewAI:CLAUDE.md`, `continuedev/continue:AGENTS.md`, `microsoft/autogen:AGENTS.md`, `modelcontextprotocol/servers:CLAUDE.md`, and `aider-ai/aider:CONVENTIONS.md` returned not found. This does **not** establish that those projects have no agent instructions elsewhere.

## Next gate
Verify each candidate's full pinned commit SHA, license and document suitability; run the Context Hygiene tool on a safe local copy, verify findings manually and publish only after the protocol's G4 gate. Agent Lint candidates require structurally compatible YAML and parser fixture validation first.
