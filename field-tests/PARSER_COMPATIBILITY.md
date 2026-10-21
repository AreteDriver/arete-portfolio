# Agent Lint parser compatibility review

**Status:** Source inspection only; no external test runs.

Reviewed current default-branch source:
- [Models](https://github.com/AreteDriver/agent-lint/blob/main/src/agent_lint/models.py): `WorkflowFormat` enumerates `gorgon`, `langchain`, `crewai`, `generic`.
- [Gorgon parser](https://github.com/AreteDriver/agent-lint/blob/main/src/agent_lint/parsers/gorgon.py): top-level `steps` list; typed steps such as `claude_code`, `openai`, `loop`, `parallel`, `shell`, `checkpoint`; nested step handling.
- [LangChain parser](https://github.com/AreteDriver/agent-lint/blob/main/src/agent_lint/parsers/langchain.py): YAML `nodes` list and `edges` list; this is not a claim that arbitrary LangGraph Python projects can be analyzed.
- [CrewAI parser](https://github.com/AreteDriver/agent-lint/blob/main/src/agent_lint/parsers/crewai.py): YAML `agents` and `tasks` lists, not arbitrary CrewAI Python source.
- [Generic parser](https://github.com/AreteDriver/agent-lint/blob/main/src/agent_lint/parsers/generic.py): `steps` list/map or flat `agents`, `tasks`, `pipeline` list; best-effort classification.

## Pilot implications
1. Find external workflow YAML files with a matching **structure**, not merely a framework name.
2. Review the parser's format auto-detection and command options before execution.
3. Treat parser failures, empty-step outputs and unsupported schema as **inconclusive/ineligible**, not clean audits.
4. Run controlled fixture checks before evaluating third-party repositories.
5. Record source/tool commit SHA for reproducibility; the linked default-branch source is a discovery reference only.
