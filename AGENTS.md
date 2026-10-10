# AGENTS — mandatory protocol

## Read order (every task)
1. This file
2. WORKFLOW_STATE.md (CC-2 only); on CC-3 tasks also read docs/coordination/cc3/state.md and docs/coordination/cc3/manual-resume.md
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

## CC-3 manual relay (on activation only)
- Within an already owner-authorized phase and assigned role, resume live task/PR state and finish all safe role actions without a new 'Go' per microstep; publish a concise handoff with exact SHA evidence.
- Source of live CC-3 task status: coordination issue #31 and the linked github-mcp PRs; CC-3 state.md is an approved snapshot, not a live board.
- Full operating steps and STOP boundaries: docs/coordination/cc3/manual-resume.md. This does not wake other clients, auto-approve reviews/tests or authorize merges.
