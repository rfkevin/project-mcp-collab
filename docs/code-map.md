# Code map

Domain → primary files. Update on moves.

## Protocol / navigation
| Domain | Paths |
| --- | --- |
| Entry | README.md |
| Mandatory agent protocol | AGENTS.md |
| Phase/role rules | WORKFLOW.md |
| Approved shared state | WORKFLOW_STATE.md |
| State field contract | docs/coordination/state-contract.md |
| Frozen plan baseline | docs/coordination/plan-v1.2.md |
| Acceptance criteria V01–V16 | docs/coordination/acceptance-v1.md |
| Contribution template | docs/templates/contribution.md |
| Task template | docs/templates/task.md |

## Knowledge (lot C)
| Domain | Paths |
| --- | --- |
| MCP usage tips | TOOL_TIPS.md |
| Append-only lessons | AGENT_MEMORY.md |
| Server tool improvements | rfkevin/github-mcp TOOL_IMPROVEMENTS.md |

## Validation (lot D)
| Domain | Paths |
| --- | --- |
| Scenario procedures (design) | docs/validation/scenarios.md |
| Scenario results (trial) | issue #3: Vibe 5980904417 (A/B), Antigravity 5981035321 (C), Vibe 5982627823 (V04/V12 supplement), ChatGPT 5982634909 (renew) |

## Execution board
| Domain | Paths |
| --- | --- |
| Live task allocation | issue #3 |
| Design history | issue #2 |
| Framing / debate | issue #1 |

## Ownership (current board)
| Lot | Author | Primary paths | Delivery |
| --- | --- | --- | --- |
| T10/A | Grok | README, AGENTS, WORKFLOW, docs/code-map, docs/templates/* | #6 merged; map #9/#11 |
| T20/B | Codex | WORKFLOW_STATE, docs/coordination/* | #4 + state #8 (rev 2); rev 3 under T60 #10 |
| T30/C | Vibe | TOOL_TIPS, AGENT_MEMORY | #5 merged (a888c72) |
| T40/D | ChatGPT | docs/validation/scenarios.md | design #7; trial verified (limits: V04-edit/V08/V09 sim; V16b/c not_tested) |
| T60 | Codex | WORKFLOW_STATE findings | PR #10 open (rev 3); ChatGPT final review + Kevin acceptance |

Updated 2026-10-04 for T60 #10 and trial supplements. Trial combined SHA: 1336c650.
