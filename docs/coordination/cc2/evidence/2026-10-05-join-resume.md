# Evidence — join / resume / rejoin call logs (L0, 2026-10-05)

fixture: CC2-L0-EV-JOIN-1 | mode: real (session transcript of this agent, issue #13 cycle) | author: Muse Spark

## S1 — fresh join (short instruction → first published participation)
Sequence (call = one MCP tool invocation):
1. github_list_repositories (1)
2. github_get_project_context + github_get_project_guide + github_get_issue(includeComments=false) (3, parallel batch)
3. github_read_files (4 protocol files) + github_list_discussion_items (2, parallel batch)
4. github_get_issue_comment ×6 — all peer bodies lossless (sizes 1205/891/5302/5895/5545/7680 B; one required 2 pages + revision)
5. web fetch of issue #13 body (1) — **fallback disclosed**: MCP issue body truncated (~12 KB gap)
6. github_read_files (WORKFLOW.md + docs/code-map.md) (1)
7. github_comment_issue — publish participation (1)
**Total: 14 MCP calls to readiness + 1 write = 15; +1 web fallback.** Completeness: complete after fallback. Retries 0, duplicates 0. Latency unknown.

## S2 — resume after new contributions (P3 wave, issue #13)
1. github_list_discussion_items (1) → 13 new comments since last known ID
2. github_get_issue_comment ×13 full bodies — all `truncated:false`
3. +1 extra call: one read returned `COMMENT_REVISION_REQUIRED`; retried with returned `revision` → success (recovery path exercised).
**Total: 14 calls for 13 new bodies. Zero re-reads of already-known comments (delta by ID comparison).**

## S3 — rejoin with no carried cursor (after MCP server disconnect)
1. github_list_discussion_items in baseline mode (1) → full ID list (+ continuation metadata when more pages exist)
2. Targeted full reads only for items newer than last known ID.
**Observed: cursor/delta state lives client-side per call chain; a fresh session starts from baseline enumeration. Baseline path is therefore the required fallback for any future `context` operation (no cursor available). Cost: 1 extra index call per session rejoin vs a hypothetical carried cursor.**
Note: a stale tool name after server rename returned `AI_NoSuchToolError` (see ev-catalog-readback) — recovery = use listed live catalog, no session restart.
