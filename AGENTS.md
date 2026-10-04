# AGENTS — mandatory protocol

## Read order (every task)
1. This file
2. WORKFLOW_STATE.md (approved on main)
3. docs/coordination/state-contract.md (if mutating state or templates)
4. WORKFLOW.md (phase/role rules)
5. docs/code-map.md
6. Task-specific sources (owned_paths, target_ref)

A summary never proves complete review. Record ID + updatedAt for decision sources.

## Authorization bounds
- Kevin authorizes phases, merges to main, and exceptions.
- File edits grant no authority.
- Role acceptance ≠ approval of resulting work.
- Missing/partial reading cannot support agreement on unread material.
- Private votes stay in private chats; never copy ballots into issues/state/memory.
- Declared agent labels are coordination names, not authenticated identities. Record GitHub account separately.

## Branch / write rules
- MCP branch slug: `<agent>-<task>-<topic>` under `mcp/<account>/`
- One coherent PR per lot; closed owned_paths list; exact head/base
- No force push, no assumed external-client activation
- Lost write confirmation → reread remote before retry
- Batch only known independent reads / coherent already-decided changes

## Contribution rules
- After P1: publish deltas/objections/references, not full plan copies
- Objection format: problem | impact | evidence/scenario | proposed correction | severity (blocking/non_blocking)
- Templates: docs/templates/contribution.md, docs/templates/task.md
- Operational content: agent-first compact English + tables. Tool-improvement proposals: human-readable French.

## Knowledge
- Temporary debate → issues/PRs
- Durable lessons → AGENT_MEMORY.md (append-only, owned by C)
- Tool improvements → rfkevin/github-mcp TOOL_IMPROVEMENTS.md (not a local competing backlog)
