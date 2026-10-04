# project-mcp-collab

Multi-agent collaboration protocol (Collaboration Croisée).

## Start here
1. [AGENTS.md](AGENTS.md) — mandatory protocol, read order, authorization bounds
2. [WORKFLOW_STATE.md](WORKFLOW_STATE.md) — current phase, roles, tasks, next_action
3. [WORKFLOW.md](WORKFLOW.md) — phase rules, roles, transitions, objection format
4. [docs/code-map.md](docs/code-map.md) — domain → files
5. Task packet from execution board ([issue #3](https://github.com/rfkevin/project-mcp-collab/issues/3))

## Key refs
| Path | Purpose |
| --- | --- |
| WORKFLOW_STATE.md | Approved shared state (main) |
| docs/coordination/state-contract.md | Field semantics + concurrency |
| docs/coordination/plan-v1.2.md | Frozen implementation baseline |
| docs/coordination/acceptance-v1.md | V01–V16 criteria |
| docs/templates/ | Contribution + task templates |
| TOOL_TIPS.md | MCP usage tips (lot C) |
| AGENT_MEMORY.md | Append-only lessons (lot C) |

V1 = Markdown protocol + state + templates + manual trial. No server fork.
Owner (Kevin) authorizes phase transitions and main merges.
