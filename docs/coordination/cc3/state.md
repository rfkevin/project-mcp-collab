# CC-3 state
schema_version: CC-STATE-1
workflow_id: CC-3
revision: 3
base_revision: 2
next_action: Muse Spark starts C1 from cc3-integration 30b3168 and tests C0 PR github-mcp#54 at 5ddb41b; Claude has reviewed T1 PR github-mcp#55 (agree at 8a6dc6e) and reruns C0 on cc3-test after K3; Kevin: K5 (run_checks on cc3-test), then K3 (cc3-test env + D1 COLLAB_DB), then merges after recorded review and test. The C0 GO stays local until the K3 cloud rerun.
canonical_ref: main
based_on_sha: 245ac672c36608662dedd034a406b400496e0233
phase: P5
framing_version: CC-FRAME-3
framing_ref: https://github.com/rfkevin/project-mcp-collab/issues/24
plan_version: CC-PLAN-3/v1.1
plan_ref: https://github.com/rfkevin/project-mcp-collab/issues/25
execution_ref: https://github.com/rfkevin/github-mcp/tree/mcp/105856986/cc3-integration
contract_ref: https://github.com/rfkevin/project-mcp-collab/issues/25
acceptance_ref: https://github.com/rfkevin/project-mcp-collab/issues/25

Placement defines authority: this file on a task branch is a proposal; it becomes the approved CC-3 snapshot only when Kevin merges it to main.
Revision 1 is the P5 opening decided by Kevin on 2026-10-07 (K1, #25 6035211334); it was never transcribed (same convention as CC-2; the L1 parser requires base_revision >= 1). Revision 2 transcribed the cycle at P5 start. Revision 3 refreshes only operational facts proven after that (C0 delivered and reviewed, T1 delivered, Muse Spark acceptances confirmed) after review 6035804243 on PR #26; invariants, plan, matrix and owner decisions are unchanged.
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
| Claude | assembler; author C0, C5; reviewer C2, C6, T1; tester C4 | C0 cloud rerun after K3; T1 review recorded (agree at 8a6dc6e) |
| Muse Spark | author C1, C6; reviewer C3; tester C0, C2 | C1 not started; C0 test of PR #54 not yet reported; roles accepted in #25 6035552091 (C2 tester confirmed) |
| Vibe GLM | author C2; reviewer C1; tester C3, C6; T0 guide | none before dependencies |
| GPT-5.6 Sol | author C3; reviewer C4; tester C5; C7 coordinator | none before dependencies |
| Grok | author C4, T1; reviewer C0, C5, T0; tester C1 | C0 review recorded (agree at 5ddb41b); T1 study delivered in PR #55, Claude review agree recorded, PR still draft |

## Tasks
| id | status | owner | role | reviewer | tester | owned_paths | dependencies | blocker | version | ref | next_action |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| C0 | review | Claude | author | Grok | Muse Spark | src/collab-store/proto/, test/collab-store/c0/, docs/collaboration/cc3/c0-gate.md | K2 | Muse Spark test pending; cloud part waits for K3 | 5ddb41b | github-mcp#54; #25 C0 | Muse Spark tests #54; Claude reruns on cc3-test after K3; merge by Kevin. Review agree by Grok at 5ddb41b; GO D1 is local only |
| C1 | accepted | Muse Spark | author | Vibe GLM | Grok | src/collab-store/contracts/, src/collab-store/schema/0001_init.sql, test/collab-store/contracts/ | K2 | none | 30b3168 | #25 C1 | Muse Spark starts from cc3-integration 30b3168; K2 branch exists |
| C2 | proposed | Vibe GLM | author | Claude | Muse Spark | src/collab-store/store/, src/collab-store/mcp/, src/index.ts route line | C0 GO, C1 | waiting dependencies | none | #25 C2 | wait for C0 GO and C1 |
| C3 | proposed | GPT-5.6 Sol | author | Muse Spark | Vibe GLM | src/collab-store/context/, src/collab-store/phases/ | C2 | waiting dependencies | none | #25 C3 | wait for C2 |
| C4 | proposed | Grok | author | GPT-5.6 Sol | Claude | src/collab-store/memory/, src/collab-store/ledger/, scripts/cc3/import-agent-memory.mjs | C2 | waiting dependencies | none | #25 C4 | wait for C2 |
| C5 | proposed | Claude | author | Grok | GPT-5.6 Sol | src/collab-store/owner/, src/collab-store/identity/, docs/collaboration/cc3/owner-setup.md | C1 | waiting dependencies | none | #25 C5 | after C1; mechanism from C0 report |
| C6 | proposed | Muse Spark | author | Claude | Vibe GLM | src/collab-store/export/, docs/collaboration/cc3/ | C2 | waiting dependencies | none | #25 C6 | wait for C2 |
| C7 | proposed | GPT-5.6 Sol | assembler | none | none | project-mcp-collab docs/coordination/cc3/ trial files | C3, C4, C5, C6, K6 | waiting dependencies | none | #25 C7 | wait for C3-C6 and K6 |
| T0 | blocked | Kevin | owner | Grok | none | docs/collaboration/cc3/run-checks.md (Vibe guide) | K1 | owner action K5 | none | #25 T0 | Kevin enables run_checks on cc3-test |
| T1 | review | Grok | author | Claude | none | github-mcp docs/collaboration/cc3/t1-sandbox-study.md | none | review recorded; PR still draft; merge by Kevin | 8a6dc6e | github-mcp#55; #25 T1 | Grok may apply the three non-blocking remarks; Kevin merges; non-blocking; location moved from the plan |
| K3 | blocked | Kevin | owner | none | none | none | K2 | owner action | none | #25 section 8 | create cc3-test Worker, KV, D1 COLLAB_DB |
A task is not done because a role was accepted. done = merged with recorded review and test; verified = review and test recorded, not merged.

## Evidence
| source | state |
| --- | --- |
| #24 P1-P4 discussion to 6035166567 | read in full |
| #25 plan CC-PLAN-3/v1.1 and 6035211334 | published by Claude |
| github-mcp cc3-integration | created at 30b31686cf3e4b89ab87232df4c6b5154ba7464b |
| github-mcp PR #54 (C0) | head 5ddb41b, CI 4/4; Grok review agree at that head (6035586117, 6035586721); vitest.config.mts deviation approved by Grok; Muse Spark test not reported; Kevin has not merged |
| #25 6035552091 (Muse Spark) | acceptances confirmed (C1 author, C0 and C2 tester, C3 reviewer, C6 author, C7); posted through a human-credential CLI, declared not an owner decision; C1 not started |
| github-mcp PR #55 (T1) | draft, head 8a6dc6e, Grok author, Claude reviewer; study moved from project-mcp-collab to github-mcp docs/collaboration/cc3/; Claude review agree at that head (github-mcp#55 comment 6035930987), CI 4/4, three non-blocking remarks |
| project-mcp-collab PR #26 review 6035804243 | GPT-5.6 Sol, changes_requested at 45adf67e: operational facts out of date; this revision answers it; re-review expected at the new head |
