# G5-F1 evidence — CC-2 state successor, parsed and joined with the merged code

`date: 2026-10-06 | author: Claude | input: WORKFLOW_STATE.md proposed on branch mcp/105856986/claude-cc2-state | code: github-mcp cc2-integration@0a2063a7 (src/collab/state.ts, context.ts) | mode: simulation-on-real-data (local execution of merged modules, no deployed tool call)`

## Parse

- Proposed `WORKFLOW_STATE.md`: `CC-STATE-1`, not legacy, 17 tasks, 8 roles, 7 171 B; no parser error.
- CC-1 rev 4 (`6507359c:WORKFLOW_STATE.md`) still parses as `legacy-v1`, `workflow_id: CC-1`; `docs/coordination/history/cc1-state-rev4.md` points to that immutable blob.

## Join matrix (`buildCollabContext`, participant only, no task id)

| Participant | Cycle | Task returned | next_action |
| --- | --- | --- | --- |
| Kevin | CC-2 / P6 / r2 | none (all owner items are `blocked` on his decision) | global `next_action` |
| GPT-5.6 Sol | CC-2 / P6 / r2 | G6 `in_progress` | prepare the G6 decision path |
| Vibe GLM | CC-2 / P6 / r2 | G5-F3 `proposed` | link-target fix |
| Claude | CC-2 / P6 / r2 | G5-F1 `review` | review then test at exact head |
| Codex, Grok, Muse Spark, Cline, unknown | CC-2 / P6 / r2 | none | global `next_action` |

Each named participant resolves to **at most one** actionable task, so the short prompt « Reprends ton lot » works with `participant` alone (no `AMBIGUOUS_TASK`). A call with neither participant nor task id returns `AMBIGUOUS_TASK` listing G5-F1, G6, T-L3, T-L4, G5-F3: the expected targeted clarification.

Before this successor, the same calls on main returned `CC-1 / P5 / r4`, `legacy-v1`, no task (see `2026-10-06-g5-local-replay.md`).

## Other checks

- Guidance for an author in P6: `read_changed_paths`, `review_and_test_independently`, `request_or_apply_correction`, `report_lessons`; `ownerDecisionRequired=true`.
- Staleness: a client that has already seen revision 3 gets `stale=true` with « A newer owner revision exists ».
- Sources are plain URLs and paths (no Markdown link text), so `unread` lists locations a client can open; this avoids G5-F3 at data level without changing code.
- Task lookups by id: `T-L3` `proposed` (blocker: owner assignment), `L4` `blocked` (G5-F6), `PROMOTE` `blocked` (owner decision).
- Checkpoint for Claude: 484 B.

## Limits

- Not a deployed `github_collab_context` call (G5-F2 unchanged).
- `based_on_sha` records the preparation base (main `6507359c`). The deployed tool compares only `observedRevision`; the library check on `observed.sha` would flag a different head as stale, which is the CC-STATE-1 rule.
- `acceptance_ref` resolves once PR #18 is merged.
