# CC-3 state
schema_version: CC-STATE-1
workflow_id: CC-3
revision: 2
base_revision: 1
next_action: Claude runs C0 (D1 gate, local part) and publishes it to github-mcp cc3-integration; Muse Spark runs C1; Grok runs T1. Kevin: K5 (run_checks on cc3-test), then K3 (cc3-test env + D1 COLLAB_DB).
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
Revision 1 is the P5 opening decided by Kevin on 2026-10-07 (K1, #25 6035211334); it was never transcribed (same convention as CC-2; the L1 parser requires base_revision >= 1). Revision 2 transcribes the cycle at P5 start.
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
| Claude | assembler; author C0, C5; reviewer C2, C6, T1; tester C4 | C0 PR with S1-S6 evidence |
| Muse Spark | author C1, C6; reviewer C3; tester C0, C2 | C2 tester role to confirm (changed in v1.1) |
| Vibe GLM | author C2; reviewer C1; tester C3, C6; T0 guide | none before dependencies |
| GPT-5.6 Sol | author C3; reviewer C4; tester C5; C7 coordinator | none before dependencies |
| Grok | author C4, T1; reviewer C0, C5, T0; tester C1 | T1 study report |

## Tasks
| id | status | owner | role | reviewer | tester | owned_paths | dependencies | blocker | version | ref | next_action |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| C0 | in_progress | Claude | author | Grok | Muse Spark | src/collab-store/proto/, test/collab-store/c0/, docs/collaboration/cc3/c0-gate.md | K2 | cloud part waits for K3 | 30b3168 | #25 C0 | local S1-S6 then PR to cc3-integration; STOP for review |
| C1 | accepted | Muse Spark | author | Vibe GLM | Grok | src/collab-store/contracts/, src/collab-store/schema/0001_init.sql, test/collab-store/contracts/ | K2 | none | 30b3168 | #25 C1 | start from cc3-integration 30b3168 |
| C2 | proposed | Vibe GLM | author | Claude | Muse Spark | src/collab-store/store/, src/collab-store/mcp/, src/index.ts route line | C0 GO, C1 | waiting dependencies | none | #25 C2 | wait for C0 GO and C1 |
| C3 | proposed | GPT-5.6 Sol | author | Muse Spark | Vibe GLM | src/collab-store/context/, src/collab-store/phases/ | C2 | waiting dependencies | none | #25 C3 | wait for C2 |
| C4 | proposed | Grok | author | GPT-5.6 Sol | Claude | src/collab-store/memory/, src/collab-store/ledger/, scripts/cc3/import-agent-memory.mjs | C2 | waiting dependencies | none | #25 C4 | wait for C2 |
| C5 | proposed | Claude | author | Grok | GPT-5.6 Sol | src/collab-store/owner/, src/collab-store/identity/, docs/collaboration/cc3/owner-setup.md | C1 | waiting dependencies | none | #25 C5 | after C1; mechanism from C0 report |
| C6 | proposed | Muse Spark | author | Claude | Vibe GLM | src/collab-store/export/, docs/collaboration/cc3/ | C2 | waiting dependencies | none | #25 C6 | wait for C2 |
| C7 | proposed | GPT-5.6 Sol | assembler | none | none | project-mcp-collab docs/coordination/cc3/ trial files | C3, C4, C5, C6, K6 | waiting dependencies | none | #25 C7 | wait for C3-C6 and K6 |
| T0 | blocked | Kevin | owner | Grok | none | docs/collaboration/cc3/run-checks.md (Vibe guide) | K1 | owner action K5 | none | #25 T0 | Kevin enables run_checks on cc3-test |
| T1 | accepted | Grok | author | Claude | none | project-mcp-collab docs/coordination/cc3/t1-sandbox-study.md | none | none | none | #25 T1 | comparison report, non-blocking |
| K3 | blocked | Kevin | owner | none | none | none | K2 | owner action | none | #25 section 8 | create cc3-test Worker, KV, D1 COLLAB_DB |
A task is not done because a role was accepted. done = merged with recorded review and test; verified = review and test recorded, not merged.

## Evidence
| source | state |
| --- | --- |
| #24 P1-P4 discussion to 6035166567 | read in full |
| #25 plan CC-PLAN-3/v1.1 and 6035211334 | published by Claude |
| github-mcp cc3-integration | created at 30b31686cf3e4b89ab87232df4c6b5154ba7464b |
