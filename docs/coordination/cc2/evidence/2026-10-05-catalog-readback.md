# Evidence — catalog surfaces, publish/readback, limits (L0, 2026-10-05)

fixture: CC2-L0-EV-CAT-1 | author: Muse Spark

## S7 — catalog / task guidance observed surfaces (this client)
| # | Surface | Observation | Mode |
| --- | --- | --- | --- |
| 1 | Pre-reconnect catalog | server tools named `kevin_mcp__*` (~30 tools) | real |
| 2 | Post-reconnect catalog | same tools renamed `kevin_codage__*`; session continued without client restart | real |
| 3 | Error surface | stale `kevin_mcp__*` call → `AI_NoSuchToolError` whose message enumerated the full live tool list | real |
| 4 | Second client | **unavailable → not_tested** | not_tested |
Historical corroboration (not re-measured): AGENT_MEMORY 2026-10-04-vibe-catalog-drift (two agents, different catalogs, same day).
Guidance implication: never hard-code tool names in guides/templates; re-enumerate after reconnect errors; catalogue display ≠ permission (baseline §4.4).

## S4 — long comment full read + edit detection
- Full read: comment 5990064913 (7680 B) via offset 0 → 4000, then 4000 → null with `revision` (2 pages). First continuation attempt without `revision` returned `COMMENT_REVISION_REQUIRED`; recovered by supplying the returned revision. **Both recovery paths documented, both exercised for real.**
- Edit detection: index items expose `createdAt`/`updatedAt`; a `updatedAt != createdAt` diff would flag an edit. No edit occurred in observed data and no comment-edit tool exists on this server → **real edit test = not_tested**; mechanism = inspection only.

## S5 — publish → identity → readback
- Write: github_comment_issue on #16 → returned id 5994821084 (single confirmation, no timeout).
- Readback: github_get_issue_comment(5994821084) → 3000 B, `revision f91a5760…`, `truncated:false` — identity and content match.
- **Anomaly (cause unknown)**: stored body begins with the header line twice (« Commentaire MCP … Muse Spark » ×2) while the submitted body had it once. Same duplication pattern visible in several peer comments → likely server-side envelope behavior, not client retry. Flag for TOOL_IMPROVEMENTS (candidate, not registered here).
- Prior uncertainty handled per W4: an earlier github_comment_issue attempt on issue #13 failed with `fetch failed`; index was re-read before any retry → comment confirmed absent → published once later. **No duplicate created.**
- L0 delivery itself: first apply_changes attempt failed `fetch failed`; remote re-read (branch tree) confirmed files absent → safe re-issue. Cause: MCP server outage mid-write (tool catalog disappeared). Delivery completed via disclosed git-CLI fallback (same branch, same content, no force push).

## S6 — memory candidate (captured; promotion deferred to L5)
- id: CC2-L0-cand-01 | scope proposed: common memory + pointer to central registry | evidence class: real.
- Fact: mid-session tool namespace change (see S7 rows 1-3); session recovered using the error-listed catalog.
- Limit: n=1 session, n=1 client. Advice: guidance must not hard-code tool names. Next: L5 evaluates promotion; catalog/version stamp remains existing candidate P-cand-3 in rfkevin/github-mcp TOOL_IMPROVEMENTS.md.

## Unknowns (explicit)
- All latencies: unknown (no timing surface).
- Serialized envelope bytes of responses: unknown.
- GitHub API call counts behind each MCP call: unknown (server-internal).
- Second-client catalog: not_tested.
