# CC-2 baseline — measurable decisions (L0)

plan: CC-2-PLAN v1.0 (issue #16) | lot: L0 | author: Muse Spark | reviewer: GPT-5.6 Sol | tester: Cline
branch: mcp/105856986/muse-l0-baseline | base: project-mcp-collab/main `f8d2b0d338e8e4ca6a1bec8bdce3de21b64068e9` (plan recorded `dd16faf` — superseded, observed before branch creation)
date: 2026-10-05 | status: proposal for review | mode per row: real | simulation | inspection | not_tested

## 0. Scope
Raw baseline of today's manual CC workflow (tools as they exist 2026-10-05), for the G1 decision. Numerical budgets are NOT set here — G1 sets them from these observations (C1 rule). No metrics in WORKFLOW_STATE tables. Evidence fixtures: `docs/coordination/cc2/evidence/`.
Measurement honesty: unobservable = `unknown`, never 0. No token claims from bytes (V12). Latency is not exposed by this MCP → `unknown` in every row; samples report call/payload/completeness only.

## 1. Scenario results (summary; raw logs in evidence/)
| # | Scenario | Mode | Calls | Payload read (B) | Completeness | Retries/dupes | Evidence |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Fresh join (repo + issue only → first published participation, issue #13) | real | 15 MCP + 1 web fallback | ~12 KB issue body via web; 6 comments lossless; 6 protocol files | complete after fallback; MCP issue-body = truncated (gap disclosed) | 0/0 | ev-join-resume |
| 2 | Resume after new contributions (P3 batch: 13 new comments) | real | 1 index + 13 full reads (+1 revision retry) | 13 comments, all lossless | complete (`truncated:false` ×13) | 0/0 (1 recovered `COMMENT_REVISION_REQUIRED`) | ev-join-resume |
| 3 | New chat resumes without previous cursor (post-disconnect rejoin) | real | 1 baseline index + targeted full reads | — | complete; no cursor state carried across sessions | 0/0 | ev-join-resume |
| 4 | Read long comment fully + detect edited earlier comment | real (pagination) / not_tested (edit) | 2 (offset 0 → 4000, revision) | 7680 B comment in 2 pages | complete; edit-detection mechanism = `updatedAt` in index; **no edit tool exists → real edit test not_tested** | 0/0 (1 revision-required error, recovered with `revision`) | ev-catalog-readback |
| 5 | Publish authorized contribution, obtain identity, read back | real | 1 write + 1 readback | 3000 B readback, `revision` recorded | complete; **anomaly: stored body carries duplicated header line** (submitted once; cause unknown; same pattern in peer comments) | prior attempt on issue #13 failed `fetch` → verified-by-index non-publication before retry → **no duplicate** | ev-catalog-readback |
| 6 | Capture memory candidate + sources for scope decision | inspection | n/a | — | candidate captured below; promotion deferred to L5 policy | n/a | ev-catalog-readback |
| 7 | Catalogue/task guidance in ≥2 client surfaces | real (this client) / not_tested (2nd client) | — | — | this session observed 2 catalog states (pre/post server tool rename) + 1 error surface exposing full tool list; **second client unavailable → not_tested** | n/a | ev-catalog-readback |

## 2. Paired read-only scenario (×3, same ref `f8d2b0d3`, same files)
Reference operation: read `AGENTS.md` (1749 B) + `WORKFLOW.md` (2285 B) at a fixed SHA.
| Run | A: one batched call | B: two isolated calls | Payload A = B | Complete |
| --- | --- | --- | --- | --- |
| 1 | 1 call | 2 calls | 4034 B | yes/yes |
| 2 | 1 call | 2 calls | 4034 B | yes/yes |
| 3 | 1 call | 2 calls | 4034 B | yes/yes |
| **median** | **1** | **2** | 4034 B | 6/6 |
| range | [1,1] | [2,2] | — | — |
- Argument bytes (approx, char-count method, disclosed as approximate): A ≈ 160 B/call; B ≈ 190 B/round.
- Serialized envelope/response overhead: **unknown** (not measurable from this side). Latency: **unknown**.
- Retries/duplicates: 0 both arms. Blob SHAs identical across all 6 reads (`0653b470`, `401dcd3f`).
- Signal: batching halves call count for multi-file reads — consistent with TOOL_TIPS R3; no new claim beyond n=3.
Raw: `evidence/2026-10-05-paired-reads.md`.

## 3. Predeclared comparison criteria (before any facade/profile exists — binding on G1+)
- **Reference scenarios**: S1 join (15 calls observed) and paired-read P (§2), plus S5 publish+readback (2 calls).
- **Identical evidence required** in any A/B: same refs/SHAs, same file set, same completeness (`truncated:false`), same correctness outcome.
- **Primary metric**: MCP call count per scenario (round-trip proxy). **Secondary**: serialized argument bytes (approx method disclosed), returned payload bytes (from file `size` fields), retries, duplicates, human interventions.
- **Selection threshold (from C1)**: a facade/profile is adopted only if it removes **≥1 model↔MCP round trip** in the targeted multi-step scenario **or** lowers total serialized context across paired runs, with: no correctness loss, no hidden increase in retries/interventions, GitHub traffic/maintenance tradeoffs disclosed.
- **Acceptable regression**: +0 retries, +0 duplicates, completeness never reduced; latency stays `unknown` unless a real timing surface appears.
- **Numeric byte/latency budgets: deferred to G1** (no speculative 6/12 KB or 40 B figures carried over).

## 4. Recommendations to G1 (evidence-based, not decisions)
1. **Context envelope**: first ~2 KB should carry phase/revision/next_action/role — justified by observed issue-body truncation (~12 KB MCP limit) and comment pagination (~4000 B pages in this session). Sources cited by ID/SHA, never by copied body.
2. **Exchange facade criterion**: baseline publish+readback = 2 calls (S5). A combined operation must reach 1 call with independent per-step outcomes (D04) to qualify under §3; otherwise no new write path.
3. **Unavailable-client fallback**: baseline index enumeration (S3) must remain a supported path — delta cursors are not portable across chats/sessions (observed loss after disconnect).
4. **Profile/catalogue**: S7 not complete (one client) → G4 profile decision stays `not_tested` until a second surface is observed; catalogue presentation never equals permission (D10).
5. **Edit detection**: index `updatedAt` diff is the only available mechanism today; comment edit/delete remains registry candidate (IMP PR #35), not a cycle dependency.

## 5. Memory candidate (captured, NOT submitted — L5 owns promotion)
- id: `CC2-L0-cand-01` | proposed scope: common memory (protocol) + central registry pointer | evidence class: real observation.
- Fact: server tool namespace changed mid-session (`kevin_mcp__*` → `kevin_codage__*`); stale call returned `AI_NoSuchToolError` listing the live catalog; session resumed without restart.
- Limit: one session, one client. Advice: guidance must not hard-code tool names; re-enumerate catalog after reconnect errors. Next: L5 evaluates; TOOL_IMPROVEMENTS candidate = catalog/version stamp (existing P-cand-3).

## 6. Acceptance self-check (for reviewer/tester)
- Reviewer reproducibility: every row in §1/§2 points to an evidence file with call counts, refs, SHAs, revisions — recomputable from the fixtures.
- Tester rerun targets: paired-read P (deterministic, cheap), S5 readback (1 call), S3 reindex (1 call).
- Separation: each row labeled real/simulation/inspection/not_tested; no single-run speed or token claim anywhere.
- not_tested visible: S4-edit, S7-second-client, latency everywhere.

STOP: PR ready for GPT-5.6 Sol (reviewer) and Cline (tester). No phase claim, no merge, no WORKFLOW_STATE edit.

