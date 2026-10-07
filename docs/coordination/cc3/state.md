# CC-3 state
schema_version: CC-STATE-1
workflow_id: CC-3
revision: 4
base_revision: 3
next_action: Kevin: K3 (steps in docs/collaboration/cc3/cc3-test-setup.md, github-mcp PR #56; send the KV and D1 ids to Claude), then K5. Claude: reruns C0 on cc3-test after K3; may start C5 (C1 is merged). Vibe GLM: C2 starts when Kevin accepts the C0 gate (cloud rerun after K3, or an explicit owner decision to start on the local GO). cc3-integration is at 11c26eb.
canonical_ref: main
based_on_sha: 3499633e94ee5bc688641b4ed77333cbea01e804
phase: P5
framing_version: CC-FRAME-3
framing_ref: https://github.com/rfkevin/project-mcp-collab/issues/24
plan_version: CC-PLAN-3/v1.1
plan_ref: https://github.com/rfkevin/project-mcp-collab/issues/25
execution_ref: https://github.com/rfkevin/github-mcp/tree/mcp/105856986/cc3-integration
contract_ref: https://github.com/rfkevin/project-mcp-collab/issues/25
acceptance_ref: https://github.com/rfkevin/project-mcp-collab/issues/25

Placement defines authority: this file on a task branch is a proposal; it becomes the approved CC-3 snapshot only when Kevin merges it to main.
Revision 1 is the P5 opening decided by Kevin on 2026-10-07 (K1, #25 6035211334); it was never transcribed (same convention as CC-2; the L1 parser requires base_revision >= 1). Revision 2 transcribed the cycle at P5 start. Revision 3 (merged with PR #26) refreshed the operational facts after review 6035804243. Revision 4 refreshes them again after the merges of github-mcp #54, #55 and #57 and the recorded reviews and tests; invariants, plan, matrix and owner decisions are unchanged.
CC-2 stays in WORKFLOW_STATE.md (rev 4) because it still has open owner items (D12, G5-F2, MEM, PROMOTE); this file does not replace or close it. Whether CC-3 should later move to WORKFLOW_STATE.md is an owner decision.
This file is the last state written by PR: from C2/C7 onward the live CC-3 state moves to the collab store, and this path receives exports (collab_export, CC-STATE-1).

## Owner decisions
Names are declared labels, not authenticated identities. No owner channel exists yet (I7, lot C5): every line below is reported, not verified.
- Kevin, 2026-10-07, direct instruction in Claude's chat: Claude is the CC-3 assembler (P4).
- Kevin, 2026-10-07 11:36 Paris, direct instruction in Claude's chat ("Vas-y"), recorded in #25 (6035211334): K1, CC-PLAN-3/v1.1 validated (scope, matrix section 6, I7 requirement); phase P5 opens.
- Claude on Kevin's go-ahead, 2026-10-07: K2 done, github-mcp branch mcp/105856986/cc3-integration created from cc2-integration 30b3168.
Kevin alone decides waivers, gate acceptance, merges to main or master, promotion and deployment.

## Roles
| Actor | Scoped acceptance/assignment | Pending evidence |
| --- | --- | --- |
| Kevin | owner | K3, K4, K5, K6, K7, K8 |
| Claude | assembler; author C0, C5; reviewer C2, C6, T1; tester C4 | C0 cloud rerun after K3; T1 review recorded and merged; C5 can start (C1 merged); K3 prep PR #56 awaits the ids |
| Muse Spark | author C1, C6; reviewer C3; tester C0, C2 | C1 delivered and merged (PR #57); C0 test recorded (PASS on the local part, #25 6036401950); roles accepted in #25 6035552091 (C2 tester confirmed) |
| Vibe GLM | author C2; reviewer C1; tester C3, C6; T0 guide | C1 review recorded (agree, #25 6037373333); three non-blocking recommendations carried to C2; C2 waits for the C0 gate |
| GPT-5.6 Sol | author C3; reviewer C4; tester C5; C7 coordinator | none before dependencies |
| Grok | author C4, T1; reviewer C0, C5, T0; tester C1 | C0 review recorded (agree at 5ddb41b); T1 merged; C1 test recorded (PASS, #25 6037275221) |

## Tasks
| id | status | owner | role | reviewer | tester | owned_paths | dependencies | blocker | version | ref | next_action |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| C0 | in_progress | Claude | author | Grok | Muse Spark | src/collab-store/proto/, test/collab-store/c0/, docs/collaboration/cc3/c0-gate.md | K2 | cloud rerun waits for K3; GO D1 is final only after it | 5ddb41b | github-mcp#54 merged; #25 C0 | Local part done: review agree (Grok), test PASS (Muse Spark, 6036401950), merged. Claude reruns S1-S6 on cc3-test after K3 and updates c0-gate.md |
| C1 | done | Muse Spark | author | Vibe GLM | Grok | src/collab-store/contracts/, src/collab-store/schema/0001_init.sql, test/collab-store/contracts/ | K2 | none | d44f5e6 | github-mcp#57 merged; #25 C1 | Delivered: review agree (Vibe GLM), test PASS (Grok); three recommendations carried to C2 |
| C2 | proposed | Vibe GLM | author | Claude | Muse Spark | src/collab-store/store/, src/collab-store/mcp/, src/index.ts route line | C0 GO, C1 | C0 GO is local only; owner acceptance of the gate pending | none | #25 C2 | Vibe GLM starts when Kevin accepts the C0 gate; carry the three C1 recommendations (assertDistinctRoles no-op, events.model_meta, proto vs 0001_init.sql overlap) |
| C3 | proposed | GPT-5.6 Sol | author | Muse Spark | Vibe GLM | src/collab-store/context/, src/collab-store/phases/ | C2 | waiting dependencies | none | #25 C3 | wait for C2 |
| C4 | proposed | Grok | author | GPT-5.6 Sol | Claude | src/collab-store/memory/, src/collab-store/ledger/, scripts/cc3/import-agent-memory.mjs | C2 | waiting dependencies | none | #25 C4 | wait for C2 |
| C5 | accepted | Claude | author | Grok | GPT-5.6 Sol | src/collab-store/owner/, src/collab-store/identity/, docs/collaboration/cc3/owner-setup.md | C1 | none | 11c26eb | #25 C5 | Claude can start from cc3-integration 11c26eb; mechanism from the C0 report (Access preferred, secret fallback) |
| C6 | proposed | Muse Spark | author | Claude | Vibe GLM | src/collab-store/export/, docs/collaboration/cc3/ | C2 | waiting dependencies | none | #25 C6 | wait for C2 |
| C7 | proposed | GPT-5.6 Sol | assembler | none | none | project-mcp-collab docs/coordination/cc3/ trial files | C3, C4, C5, C6, K6 | waiting dependencies | none | #25 C7 | wait for C3-C6 and K6 |
| T0 | blocked | Kevin | owner | Grok | none | docs/collaboration/cc3/run-checks.md (Vibe guide) | K1 | owner action K5 | none | #25 T0 | Kevin enables run_checks on cc3-test |
| T1 | done | Grok | author | Claude | none | github-mcp docs/collaboration/cc3/t1-sandbox-study.md | none | none | 8a6dc6e | github-mcp#55 merged; #25 T1 | Delivered and merged; the three non-blocking remarks of Claude's review are left to the author; location moved from the plan |
| K3 | blocked | Kevin | owner | none | none | none | K2 | owner action; ids not yet sent | none | #25 section 8; github-mcp#56 | create the cc3-test KV and D1, send the two ids to Claude, then follow docs/collaboration/cc3/cc3-test-setup.md in PR #56 |
A task is not done because a role was accepted. done = merged with recorded review and test; verified = review and test recorded, not merged.

## Evidence
| source | state |
| --- | --- |
| #24 P1-P4 discussion to 6035166567 | read in full |
| #25 plan CC-PLAN-3/v1.1 and 6035211334 | published by Claude |
| github-mcp cc3-integration | created at 30b31686cf3e4b89ab87232df4c6b5154ba7464b |
| github-mcp PR #54 (C0) | merged into cc3-integration (a0faea3), head 5ddb41b, CI 4/4; Grok review agree (6035586117, 6035586721); Muse Spark independent test PASS on the local part (#25 6036401950); vitest.config.mts deviation approved by both; GO D1 is local, final after the cloud rerun |
| #25 6035552091 (Muse Spark) | acceptances confirmed (C1 author, C0 and C2 tester, C3 reviewer, C6 author, C7); posted through a human-credential CLI, declared not an owner decision |
| github-mcp PR #55 (T1) | merged (ed5ef37), head 8a6dc6e; Claude review agree (github-mcp#55 comment 6035930987), CI 4/4; study moved from project-mcp-collab to github-mcp docs/collaboration/cc3/ |
| github-mcp PR #57 (C1) | merged (11c26eb), head d44f5e6, CI 4/4; Vibe GLM review agree (#25 6037373333), Grok test PASS (#25 6037275221); three non-blocking recommendations for C2 |
| github-mcp PR #56 (K3 prep) | draft, head d4941a8, CI 4/4; cc3-test wrangler block with two id markers and the setup guide; the deploy workflow is not in the PR (GitHub Actions files are protected for the MCP) and Kevin adds it; not merged; the branch is behind cc3-integration without conflicts |
| project-mcp-collab PR #26 | merged into main (3499633) after GPT-5.6 Sol review agree at f4f2e1d (6035961577, 6035971936) |
| github-mcp cc3-integration | head 11c26eb after the merges of #54, #55 and #57 |
