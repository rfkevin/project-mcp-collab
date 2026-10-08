# Code map

Domain → primary files. Update on moves.

## Protocol / navigation
| Domain | Paths |
| --- | --- |
| Entry | README.md |
| Mandatory agent protocol | AGENTS.md |
| Phase/role rules | WORKFLOW.md |
| Approved shared state | WORKFLOW_STATE.md (CC-2, CC-STATE-1 contract: github-mcp docs/collaboration/contract.md) |
| CC-3 approved snapshots | docs/coordination/cc3/state.md (snapshot, not a real-time board; post-audit rev 7 proposed in F6-A10) |
| CC-1 final snapshot (rev 4, read-only history) | docs/coordination/history/cc1-state-rev4.md |
| State field contract (CC-1) | docs/coordination/state-contract.md |
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
| Scenario procedures and executed-results pointers | docs/validation/scenarios.md |
| Scenario results (trial) | issue #3: Vibe 5980904417 (A/B), Antigravity 5981035321 (C), Vibe 5982627823 (V04/V12 supplement), ChatGPT 5982634909 (renew) |

## Execution board
| Domain | Paths |
| --- | --- |
| Live task allocation (CC-1 baseline) | issue #3 |
| CC-3 canonical plan | issue #25 |
| CC-3 post-audit correction board | issue #31; source audit rfkevin/github-mcp#79 |
| Design history | issue #2 |
| Framing / debate | issue #1 |

## Ownership (current board)
| Lot | Author | Primary paths | Delivery |
| --- | --- | --- | --- |
| T10/A | Grok | README, AGENTS, WORKFLOW, docs/code-map, docs/templates/* | #6 merged; map #9/#11 |
| T20/B | Codex | WORKFLOW_STATE, docs/coordination/* | #4 + state #8 (rev 2); rev 3 merged #10 |
| T30/C | Vibe | TOOL_TIPS, AGENT_MEMORY | #5 merged (a888c72) |
| T40/D | ChatGPT | docs/validation/scenarios.md | design #7; results pointers #12; trial verified (limits: V04-edit/V08/V09 sim; V16b/c not_tested) |
| T60 | Codex | WORKFLOW_STATE findings | #10 (rev 3), #11 and #12 merged; Kevin acceptance; consolidation PR |

Updated 2026-10-08 for CC-3 A10: CC-3 snapshot path, canonical plan #25, post-audit correction board #31 and Codex audit github-mcp#79 are now explicit. Older CC-1/CC-2 mappings remain for their still-open history.
