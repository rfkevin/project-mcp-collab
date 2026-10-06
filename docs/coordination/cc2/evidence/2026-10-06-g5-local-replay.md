# G5 evidence — local replay of merged CC-2 code on real repositories

`date: 2026-10-06 | author: Claude (G5 closure) | code: github-mcp cc2-integration@0a2063a76b554bdcdd4fd4ba28bad0308fc6749d (tree == L6 PR head 284074d, CI ci/GitGuardian/SonarCloud/Workers 4/4 green) | mode: simulation-on-real-data`

## Method

The deployed MCP used by every trial client does not expose `github_plan_project_bootstrap` or `github_collab_context` (code merged on staging, not deployed). To obtain machine-verified evidence instead of a hand replay, the **merged modules themselves** (`src/collab/bootstrap.ts`, `bootstrap-manifest.ts`, `context.ts`) were bundled with esbuild and executed with Node 22 against files read by `git show <ref>:<path>` from fresh clones. Same inputs the tool adapter reads (target files + registry at one commit). This is **not** a deployed tool call: transport, OAuth and GitHub API paths are not exercised.

## Bootstrap planner (L3) — raw output

| Repository @ ref | Status | Operations | Entries |
| --- | --- | --- | --- |
| portalshall @ `7befe67e` (before L7 trial) | `ready` | AGENTS.md, AGENT_MEMORY.md, docs/collaboration/bootstrap-manifest.json | create ×3 |
| portalshall @ master `8e6b163a` (after PR #3 merge) | `unchanged` | none | unchanged ×3 |
| github-mcp @ cc2-integration `0a2063a7` | `ready` | docs/collaboration/bootstrap-manifest.json | AGENTS.md kept, AGENT_MEMORY.md kept, registry create |
| project-mcp-collab @ main `6507359c` | `ready` | docs/collaboration/bootstrap-manifest.json | AGENTS.md kept, AGENT_MEMORY.md kept, registry create |

SHA-256 of the files applied by hand in portalshall (Cline, commit a8921c6), recomputed here:

- `AGENTS.md` = `c420140ef1e55577173b92f2853f70ad6935256951545f5ecde1353f535a9f58` = manifest pin.
- `AGENT_MEMORY.md` = `0f4024a98aa5b94eff2aa968539af4b098c5e816c1a750cc3a8fa5a9e82fbafc` = manifest pin.
- `docs/collaboration/bootstrap-manifest.json` = `9874cb414090c44d1f03471013a54246f7055a909bcb2ae3eef32823970fbf38`, byte-identical to `renderRecord(BOOTSTRAP_MANIFEST)` (planner returns `unchanged`).

This closes the L7 limit « transcription not machine-verified » and the pending « second preview `unchanged` » check, in simulation mode.

## Context envelope (L2) on the real canonical state

Input: `project-mcp-collab@main 6507359c:WORKFLOW_STATE.md`, participant `Claude`, no task id.

- `cycle`: `workflowId=CC-1`, `phase=P5`, `revision=4`, `schemaVersion=legacy-v1`, `stale=false`.
- `task`: null. `guidance.role`: consultant, `ownerDecisionRequired=true`.
- `peerProposalExclusion.active=false` (P5: peer proposals readable).
- `nextCheckpoint`: 515 B. Resume with that checkpoint: `resumed=true`, `scopeMatch=true`, `toReread=[]`, `missing=[]`, `rescanRequired=false`.
- `coverage.unread` contains raw Markdown link text as locations, e.g. `[complete plan](docs/coordination/plan-v1.2.md)`, `[V01-V16](docs/coordination/acceptance-v1.md)`.

Findings:

1. **No CC-2 canonical state exists** on any branch of project-mcp-collab (all `WORKFLOW_STATE.md` revisions are `workflow_id: CC-1`, latest rev 4, phase P5). A CC-2 participant using the delivered context tool would be told « CC-1 / P5 » with no `sync_pending` signal.
2. Source extraction keeps Markdown link syntax instead of the link target (minor, L2 owner).
3. Checkpoint resume on unchanged sources behaves as specified.

## Reproduce

Clone the three repositories, check out `cc2-integration@0a2063a`, bundle a script importing `planBootstrap`, `BOOTSTRAP_MANIFEST`, `EMBEDDED_TEMPLATES`, `buildCollabContext` from `src/collab/`, feed it `git show <ref>:<path>` contents; compare with the table above.
