# CC-2 state
schema_version: CC-STATE-1
workflow_id: CC-2
revision: 2
base_revision: 1
next_action: Kevin decides G6 - (1) merge this state successor, (2) preview deployment of github-mcp cc2-integration@0a2063a7 for a real two-client rerun of CC2-01/03/05/07/10, (3) assign distinct testers for L3 and L4 or waive, (4) open or defer the memory vote, (5) promotion to master
canonical_ref: main
based_on_sha: 6507359c633f86105059f58f2f19aceb6cb79b98
phase: P6
framing_version: CC-FRAME-2
framing_ref: https://github.com/rfkevin/project-mcp-collab/issues/13
plan_version: CC-PLAN-2/v1.0
plan_ref: https://github.com/rfkevin/project-mcp-collab/issues/16
execution_ref: https://github.com/rfkevin/github-mcp/tree/mcp/105856986/cc2-integration
contract_ref: https://github.com/rfkevin/github-mcp/blob/0a2063a76b554bdcdd4fd4ba28bad0308fc6749d/docs/collaboration/contract.md
acceptance_ref: docs/coordination/cc2/acceptance-results.md

Placement defines authority: this file on a task branch is a proposal; it becomes the approved snapshot only when Kevin merges it to main.
Format: CC-STATE-1, parsed by github-mcp src/collab/state.ts (L1). Reference values are plain URLs or paths so that the context tool lists readable sources.
Revision 1 is the P5 opening decided by Kevin on 2026-10-05 and announced in #16 (5994794062); it was never transcribed here. Revision 2 transcribes the cycle as of P6.
History: the CC-1 snapshot (revision 4, P5) stays immutable at main 6507359c; docs/coordination/history/cc1-state-rev4.md points to it. CC-1 is not closed by this file; its owner closure stays pending.

## Owner decisions
Names are declared labels, not authenticated identities. Each line states how the decision reached the record; none is attested by Kevin in public unless linked.
- Kevin, 2026-10-05, coordinator chat, reported by Codex in #16 (5994794062): P5 opened; staging github-mcp branch mcp/105856986/cc2-integration from master cd8089ae; no promotion to master before the final packet and Kevin's decision.
- Kevin, 2026-10-05/06, reported by the substitutes in #16: Codex unavailable; Sol reviews L2 and Vibe reviews L4 in Codex's slot; Grok tests L1, L2 and L6 in place of Muse Spark; Cline unavailable then authors L7 on portalshall.
- Kevin, 2026-10-06, direct instruction in Claude's chat, no public attestation: Claude authors L3 and L6, closes L7/G5 (PR #18) and prepares this state successor for G5-F1.
- Kevin, 2026-10-06, reported by Sol in #16 (6013325692): GPT-5.6 Sol is substitute coordinator for the rest of P6/G5 to G6. Owner authority is not transferred.
- Kevin, 2026-10-06, direct instruction in Claude's chat: the cycle is in P6.
Kevin alone decides waivers, gate acceptance, merges to main or master, promotion and deployment.

## Roles
| Actor | Scoped acceptance/assignment | Pending evidence |
| --- | --- | --- |
| Kevin | owner | G6 decisions listed in next_action |
| GPT-5.6 Sol | substitute coordinator (P6); L4 author; reviewer L2, L5, L6, L7, G5 | G6 decision path |
| Codex | assembler and L1 author; unavailable since 2026-10-05 | none while unavailable |
| Vibe GLM | L2 author; L1 corrector; reviewer L3, L4; tester L5 | G5-F3 link-target fix proposal |
| Grok | L5 author; reviewer L3; tester L1, L2, L6, L7, G5 | none recorded |
| Muse Spark | L0 author; L7 coordinator; unavailable since L6 | none while unavailable |
| Cline | L0 tester; L7 author (portalshall #3) | L4 test was assigned and is not recorded |
| Claude | L3 and L6 author; L7/G5 closure; this state successor | review and test of this state PR |

## Tasks
| id | status | owner | role | owned_paths | dependencies | blocker | version | ref | next_action |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| L0 | done | Muse Spark | author | docs/coordination/cc2/baseline.md | none | none | 1ded98d | project-mcp-collab#17 | none |
| L1 | done | Codex | author | src/collab/contracts.ts, src/collab/state.ts, src/collab/phase.ts | L0 | none | 5202b01 | github-mcp#36 | none; review and test were by one agent (G5-F6) |
| L2 | done | Vibe GLM | author | src/collab/context.ts, src/collab/reading-checkpoint.ts | L1 | none | 669fc6c | github-mcp#37 | none |
| L3 | blocked | Claude | author | src/collab/bootstrap.ts, src/collab/bootstrap-manifest.ts | L1, L2 | G5-F6: no independent test recorded | 0355d96 | github-mcp#42 | waits for T-L3 |
| L4 | blocked | GPT-5.6 Sol | author | src/collab/receipts.ts, src/collab/reconcile.ts, src/collab/publication.ts | L1 | G5-F6: no independent test recorded | 6f14eb0 | github-mcp#38 | waits for T-L4 |
| L5 | done | Grok | author | src/collab/memory/ | L1 | none | d50062e | github-mcp#40 | none |
| L6 | done | Claude | author | docs/collaboration/usage.md, docs/collaboration/troubleshooting.md, docs/collaboration/profile-decision.md | L1-L5 | none | 284074d | github-mcp#43 | none |
| L7 | done | Cline | author | portalshall docs/coordination/cc2-l7/ | L2-L6 | none | fef60d9 | portalshall#3 | none |
| G5 | verified | Claude | author | docs/coordination/cc2/acceptance-results.md, docs/coordination/cc2/evidence/ | L7 | none | 2c0e66b | project-mcp-collab#18 | Kevin merges or not; gate stays not passed operationally |
| G5-F1 | review | Claude | author | WORKFLOW_STATE.md, docs/coordination/history/ | G5 | none | this PR | branch mcp/105856986/claude-cc2-state | review then test at exact head; merge = Kevin |
| G6 | in_progress | GPT-5.6 Sol | assembler | none | G5 | none | 6013325692 | project-mcp-collab#16 | prepare the G6 decision path from G5-F1, G5-F2 and G5-F6 |
| T-L3 | proposed | unassigned | tester | none | L3 | owner assignment | 0355d96 | github-mcp#42 | Kevin assigns a tester distinct from Claude, Grok and Vibe GLM (eligible: GPT-5.6 Sol, Muse Spark, Cline, Codex) |
| T-L4 | proposed | unassigned | tester | none | L4 | owner assignment | 6f14eb0 | github-mcp#38 | Kevin assigns a tester distinct from Sol and Vibe GLM (eligible: Grok, Claude, Cline, Muse Spark, Codex) |
| G5-F2 | blocked | Kevin | owner | none | G5 | owner decision | 0a2063a | github-mcp cc2-integration | preview deployment, then real rerun of CC2-01/03/05/07/10 with two clients |
| G5-F3 | proposed | Vibe GLM | author | src/collab/context.ts | L2 | none | 0a2063a | acceptance-results.md G5-F3 | extract Markdown link targets as source locations; add a fixture |
| MEM | blocked | Kevin | owner | none | G5 | owner decision | CC2-M01..M08 | acceptance-results.md CC2-17 | open the L5 electorate or defer |
| PROMOTE | blocked | Kevin | owner | none | G5-F1, G5-F2, T-L3, T-L4 | owner decision | 0a2063a | github-mcp master cd8089a | promotion to master and production stay Kevin's |
A task is not done because a role was accepted. done = merged with recorded review and independent test; verified = review and test recorded, not merged.

## Evidence
| source | state |
| --- | --- |
| #16 plan and annexes C1-C4 | read in full |
| #16 discussion to 6013325692 | read in full |
| github-mcp PRs 36, 37, 38, 42, 43 | verdicts re-read (see acceptance-results.md) |
| portalshall #3 | merged 8e6b163, test 6012556685 |
| project-mcp-collab #18 | Sol agree 6012998068, Grok pass 6013290108 at 2c0e66b |
