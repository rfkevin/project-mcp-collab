# TOOL_TIPS — observed MCP capabilities, recipes and limits

task: T30/C | author: Vibe GLM | reviewers: Codex (review), ChatGPT + DeepSeek (tests)
plan: [CC-PLAN-1/v1.2](docs/coordination/plan-v1.2.md) | state: [WORKFLOW_STATE.md](WORKFLOW_STATE.md)
status: merged (PR #5, a888c72); amendments proposed in the consolidation PR | All observations are client-specific and time-stamped; refresh before relying on them.

## 1. Observed client catalog (Vibe GLM client, 2026-10-04, github-mcp worker via MCP connector)

Verified working this session (used, not just listed):
- github_get_issue — issue + paged comments (20/page) via nextCommentsPage; issue BODY truncates (bodyTruncated flag observed at ~12 KB body); comment bodies truncate around ~1.9 KB in this view.
- github_get_issue_comment — lossless masked body + UTF-8 pagination (offset/limit 1..12000, revision hex64 required when offset>0). Read 8 KB+ bodies fully.
- github_list_discussion_items — compact index: id, author account, declaredAgent (UNTRUSTED), createdAt/updatedAt, maskedBytes, url; cursor resume; kind issue_comment|pull_request_comment + number required.
- github_get_discussion_item — kinds pull_request_review / pull_request_review_comment / commit_comment; requires pullNumber only for pull_request_review; SENSITIVE_FILE refused.
- github_read_files — up to 10 files/ranges at one immutable SHA; non-truncated reads observed.
- github_apply_changes — atomic batch (append/create/replace/restore) with expectedSha preconditions; replace anchors must be short and unique.
- github_get_project_context, github_list_repositories, github_list_issues, github_list_pull_requests, github_get_pull_request, github_compare_refs, github_get_commit, github_comment_issue, github_comment_pull_request, github_create_branch, github_open_pull_request.
Known limits observed:
- get_discussion_item kind=commit_comment → GITHUB_API_403 observed (2026-10-04). Cause NOT verified: could be installation permissions, repository scoping or another server condition. Recorded as an observed error only; no permission change advised without scoped diagnostics.
- Cross-client catalog differences are REAL: Codex reported github_get_issue_comment absent in his catalog on 2026-10-04. Hypothesis (unproven): reconnect refreshes tool list. Each agent MUST record own catalog+time before Phase-critical reads. Never assume another client has your tools.
- Paginated views truncate ~1.9 KB/comment: DO NOT base cross-review verdicts on github_get_issue comment bodies alone. Check maskedBytes in the index first; >1.9 KB ⇒ read via github_get_issue_comment.

## 1b. Observed client catalog (Claude client, same github-mcp worker; added by the consolidation PR, lot-C owner may amend)

- Catalog is per conversation: github_get_issue, github_get_issue_comment, github_list_issues, github_list_discussion_items, github_get_discussion_item, github_get_discussion_delta, github_comment_issue and github_create_issue appeared via tool_search after the conversation had started (cause unknown: refresh or deployment).
- Bounds seen: issue body returned 12,000 B of 23,396 B (bodyTruncated=true); get_issue comment excerpts 2,000 B. Lossless paths exist for comments, reviews, inline comments and commit comments, not for bodies (P-cand-2).
- Counts: a public GitHub page snapshot held 12-14 of 20 comments on issue #3 while github_list_discussion_items listed 20. Count items with the index, never from a web page.
- Approval prompts: 5 calls (3 reads, 2 writes) returned `No approval received`. A refused write is unconfirmed until read back (W4); one comment was retried after the owner's instruction and no duplicate was seen afterward.
- Sandbox: can clone public repos and run their test suites; no push credentials, so writes go through the MCP only.

## 2. Read recipes (bounded, evidence-safe)

R1 Long comment: list_discussion_items (sizes) → get_issue_comment offset 0 → follow nextOffset with revision. Loop until nextOffset=null. Request an appropriate supported limit; observed pagination differs across clients: Vibe reported 4000-byte pages (10,426 B with limit=12000 still paged at offsets 0/4000/8000; 8071 B → 3 calls, V04 trial); Codex retrieved the same 10,426 B comment in ONE call with limit=12000 (nextOffset=null). No cause for the discrepancy is established. Follow returned nextOffset with revision; do not infer completeness from the requested limit or a fixed page count.
R2 Bulk discussion review: index FIRST (list_discussion_items maskedBytes) when available; paginated get_issue excerpts only as fallback. Full reads (R1) for items >1900 bytes. This session: 13 comments, 11 needed full reads.
R3 Multi-file state/plan read: read_files batch at one SHA (4 files/1 call, zero truncation). Prefer over per-file reads.
R4 Before any write: read_files at current head to get blob SHAs (expectedSha preconditions) — one call for all targets.
R5 CI: ci_status at exact SHA with expectedChecks declared; get_failure_report for annotations (gives file/line/error, no raw logs).
R6 Follow a discussion without rereading it: github_get_discussion_delta (kind issue_comment|pull_request_comment + number). No cursor ⇒ baseline: newest `limit` entries (no bodies) + nextCursor that acknowledges everything. With cursor ⇒ added/modified/deleted ids; reuse nextCursor each call; hasMore=true ⇒ call again. Read changed items with get_issue_comment. Limits: newest 300 items tracked; ≥1000 comments ⇒ DISCUSSION_TOO_LARGE_FOR_DELTA; an edit invisible after secret masking is not reported; INVALID/FOREIGN/STALE cursor ⇒ restart without cursor. Contract: https://github.com/rfkevin/github-mcp/blob/master/docs/discussion-delta.md (other repository). DISCLOSURE: authored by Claude. Independent evidence: DeepSeek's baseline on issue #3 matched the index (comment 5980474094); ChatGPT (non-author, PR #14 comment 5988954087) independently verified baseline enumeration, exact-body follow-up read, and INVALID_CURSOR recovery (retryable=false → restart without cursor, no silent reset). The added/modified/deleted classifications remain without independent run (cursor not surfaced in that client; no edit/delete actions available). Do not treat R6 as fully runtime-verified.

## 3. Write recipes and uncertainty rules

W1 Branch: github_create_branch (task slug; auto-prefix mcp/<account>/) with expectedBaseSha read beforehand.
W2 Batch edits: github_apply_changes single commit; keep replace anchors SHORT+unique (long multi-line anchors risk spurious conflicts); read the file back after create-type ops before committing more.
W3 PR: github_open_pull_request expectedHeadSha; poll ci_status at that exact SHA; bilan via github_comment_pull_request with expectedHeadSha+expectedBaseSha.
W4 LOST WRITE CONFIRMATION ⇒ reread target (list comments / read files) BEFORE retry; never blind re-publish. Observation: a near-duplicate triple publication occurred on 2026-10-04 (three bodies differing ~300-400 bytes). Hypothesis (unproven): lost write confirmation followed by retries. Rule stands regardless of cause: if your tool call failed/timed out, the comment may already exist — reread first.
W5 Group only independent changes; a dependent mutation waits for its prerequisite evidence (per plan rules).

## 4. Raw metrics convention (for D)

Record per scenario: calls, argument bytes (approx), returned bytes (approx), pages/continuations, time, completeness. Report RAW values; interpretation belongs to D (ChatGPT). No token claims without compatible tokenizer. Example from this session (issue #1 cross-review, 2026-10-04, raw values only): 1 index call + 7 full-body reads (~31 KB bodies) + 2 paged list calls ≈ 10 calls; argument bytes minimal (IDs only). Comparison scenarios must be measured, not estimated.

## 5. Fallbacks (disclosed, not silent)

F1 Issue BODY truncated by MCP (12 KB observed) ⇒ read via web GitHub page; DISCLOSE the fallback and what remained unread. A truncated read is not review evidence (V04/V14).
F2 Missing tool in client catalog ⇒ use paged reads + F1, record limitation; never claim a read you did not make.
F3 403 on commit_comment reads ⇒ observed error, cause unverified. Do not rely on commit-comment reads; if needed, run scoped diagnostics (GitHub API docs for the installation) and record findings before proposing any permission change.

## 6. Improvement pointers

Tool-improvement proposals live ONLY in rfkevin/github-mcp TOOL_IMPROVEMENTS.md (clear French, existing IDs/branch+PR workflow). Notable candidates from this project (proposals, NOT registered, NOT authorized):
- P-cand-1: quasi-duplicate detection guard for comment publication (after lost confirmations).
- P-cand-2: lossless issue BODY reading (body truncation is the last truncation gap; comments are solved).
- P-cand-3: catalog/version stamp in a lightweight tools/info endpoint to make cross-client capability checks cheap.
Do not implement from this file; separate owner-authorized task/PR required.