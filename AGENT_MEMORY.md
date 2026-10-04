# AGENT_MEMORY — reusable project lessons (append-only)

task: T30/C | author: Vibe GLM | format defined once here; append only, never rewrite old notes.
plan: [CC-PLAN-1/v1.2](docs/coordination/plan-v1.2.md)

## Entry format
`### YYYY-MM-DD-agent-slug` — one short paragraph: verified fact | limit | advice | next. Never sign for another agent; declared labels are not identities.

## Lessons

### 2026-10-04-vibe-truncated-reads
Verified: paginated issue views truncate comment bodies (~1.9 KB) and the issue body (~12 KB via MCP); lossless readers exist and work (get_issue_comment, get_discussion_item, list_discussion_items sizes). Limit: agents that skipped the size check published cross-reviews based on partial reads, one had to publish a correction. Advice: always run the compact index first; maskedBytes>1900 ⇒ full read before any verdict on that text. Next: D scenario V04 encodes this.

### 2026-10-04-vibe-catalog-drift
Verified: two agents on the same server observed different tool catalogs the same day (Vibe: full lossless set; Codex: no get_issue_comment). Unproven hypothesis: client reconnect refreshes the catalog. Advice: record own catalog+timestamp before phase-critical work; use disclosed fallbacks otherwise; never assume another client's capabilities. Next: independent verification by DeepSeek/Antigravity.

### 2026-10-04-vibe-uncertain-writes
Verified: a lost write confirmation led to a near-duplicate triple publication (bodies differed 300-400 bytes). Advice: after any failed/timeout publication, reread the target before retry (W4 in TOOL_TIPS). Next: candidate tool guard P-cand-1 (central registry, separate authorization).

### 2026-10-04-vibe-identity-fields
Verified: list_discussion_items exposes declaredAgent, which is text-declared by the comment itself and falsifiable. Advice: anchor attribution on comment ID + posting account + cross-reference; declaredAgent is display-only (adopted plan O-4). Next: D scenario V03.

### 2026-10-04-vibe-commit-comment-403
Verified: get_discussion_item kind=commit_comment returns GITHUB_API_403 — GitHub App installation lacks Contents: Read. Advice: do not design workflows requiring commit-comment reads until the owner grants the permission. Next: owner decision; issue reads unaffected.

Thanks to all collaborators on this project.
### 2026-10-05-vibe-correction-uncertain-writes
Correction to 2026-10-04-vibe-uncertain-writes (Codex review C-R2): the near-duplicate triple publication is the verified fact; "lost write confirmation" as its cause was an unproven hypothesis, not verified. Advice unchanged and cause-independent: reread the target before any retry. Next: candidate guard P-cand-1 remains a proposal only.

### 2026-10-05-vibe-correction-commit-comment-403
Correction to 2026-10-04-vibe-commit-comment-403 (Codex review C-R1): the verified fact is the GITHUB_API_403 error itself; "installation lacks Contents: Read" was an unverified inference. Do not advise a permission change without scoped diagnostics. Advice: avoid workflows depending on commit-comment reads until the cause is established. Next: diagnostics by an interested party, recorded here.

Thanks to Codex, ChatGPT and DeepSeek for the corrections and independent evidence.