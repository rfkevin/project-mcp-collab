# CC-3 state
schema_version: CC-STATE-1
workflow_id: CC-3
revision: 8
base_revision: 7
next_action: ChatGPT GPT-6 authors this post-CR-A/CR-B CC-3 snapshot proposal from rev 7; Claude reviews and Vibe GLM independently tests this exact PR head/base. Kevin alone merges to main. After merge Codex performs a new integrated global audit of github-mcp cc3-integration@ed9d1b5baf2a98fd837c41b8e56627468671e7f1; A11 and owner K4/K6 runtime proof remain to verify before any C7 trial. No phase change or production promotion is implied.
canonical_ref: main
based_on_sha: 76af341382f8f3ba8c106574216863be0ca3596b
phase: P5
framing_version: CC-FRAME-3
framing_ref: https://github.com/rfkevin/project-mcp-collab/issues/24
plan_version: CC-PLAN-3/v1.1
plan_ref: https://github.com/rfkevin/project-mcp-collab/issues/25
execution_ref: https://github.com/rfkevin/github-mcp/tree/mcp/105856986/cc3-integration
contract_ref: https://github.com/rfkevin/project-mcp-collab/issues/25
acceptance_ref: https://github.com/rfkevin/project-mcp-collab/issues/25

Placement defines authority: this file on a task branch is a proposal; it becomes the approved CC-3 snapshot only when Kevin merges it to main.
Revision 1 is the P5 opening decided by Kevin on 2026-10-07 (K1, #25 6035211334); it was never transcribed (same convention as CC-2; the L1 parser requires base_revision >= 1). Revision 2 transcribed the cycle at P5 start. Revision 3 (merged with PR #26) refreshed the operational facts after review 6035804243. Revision 4 refreshes them again after the merges of github-mcp #54, #55, #56 and #57, the recorded reviews and tests, the deployment of cc3-test (K3), the cloud rerun of C0 and the owner's acceptance of the C0 gate; invariants, plan and matrix are unchanged. Revision 5 records the merges of github-mcp #58 to #63 (C0 cloud, K5, actionable errors, S5 CI stability, C2), the first verified run_checks on cc3-test, and the owner's substitution of Grok for Muse Spark as C2 tester; the matrix is otherwise unchanged. Revision 6 records three owner role substitutions made on 2026-10-08 (Claude replaces Muse Spark as C3 reviewer; Vibe GLM replaces Grok as C5 reviewer; Claude replaces Grok as T0 reviewer), the C3 review, the C5 test, the C4 review and preliminary test, the T0 guide and its review, the re-pin of the run_checks controller (github-mcp#72), and the merges of C5 (github-mcp#64) and of the T0 guide (github-mcp#71). Revision 6 was first merged by mistake into the revision 5 branch (project-mcp-collab #29, after #28 had reached main); this copy brings it to main. Revision 8 proposes an updated checkpoint after github-mcp PRs #86 (CR-A) and #87 (CR-B) merged into cc3-integration. This records verified code-review and independent-test results but NOT global acceptance or C7 permission. Revision 7 is the post-audit A10 refresh: it records that C3, C4 and C6 are now merged, captures Codex audit github-mcp#79, opens the correction gate in project-mcp-collab#31, and makes the remaining C7 dependencies explicit. This remains a snapshot, not a real-time board. Owner decision lines remain reported unless an owner-channel proof is explicitly referenced.
CC-2 stays in WORKFLOW_STATE.md (rev 4) because it still has open owner items (D12, G5-F2, MEM, PROMOTE); this file does not replace or close it. Whether CC-3 should later move to WORKFLOW_STATE.md is an owner decision.
This file is the last state written by PR: from C2/C7 onward the live CC-3 state moves to the collab store, and this path receives exports (collab_export, CC-STATE-1).

## Owner decisions
Names are declared labels, not authenticated identities. The C5 owner-channel implementation is merged, but chat-sourced decisions below remain reported rather than owner-channel-authenticated unless an explicit proof is cited.
- Kevin, 2026-10-07, direct instruction in Claude's chat: Claude is the CC-3 assembler (P4).
- Kevin, 2026-10-07 11:36 Paris, direct instruction in Claude's chat ("Vas-y"), recorded in #25 (6035211334): K1, CC-PLAN-3/v1.1 validated (scope, matrix section 6, I7 requirement); phase P5 opens.
- Claude on Kevin's go-ahead, 2026-10-07: K2 done, github-mcp branch mcp/105856986/cc3-integration created from cc2-integration 30b3168.
- Kevin, 2026-10-07 15:04 Paris, in Claude's chat: reported that github-mcp PR #56 was merged into cc3-integration (observed by Claude: merged, wrangler.jsonc on cc3-integration holds the real ids).
- Kevin, 2026-10-07 16:45 Paris, direct instruction in Claude's chat ("1 on demarre", answering the options in Claude's previous message): accepts the C0 gate on correctness (real D1) and starts C2; the 50 ms p95 latency criterion (S5) moves to C2 and is measured on cc3-test with a deployed Worker.
- Kevin, 2026-10-07 19:04 Paris, in Claude's chat ("oui"): Claude makes tool errors actionable (github-mcp #62); merged by Kevin.
- Kevin, 2026-10-07 19:43 Paris, in Claude's chat ("je te laisse décider"): Claude decides the fix of the flaky C0 S5 test (github-mcp #63); merged by Kevin.
- Kevin, 2026-10-07, reported by Grok in github-mcp#60 (6043996431): Grok replaces Muse Spark as C2 tester (Muse Spark unavailable). No public owner attestation.
- Kevin, 2026-10-07 23:27 Paris, in Claude's chat ("Tu peux faire ta part"): Claude writes revision 5 and starts C5.
- Kevin, 2026-10-08 about 01:00 Paris, in Claude's chat ("Oui je vais demander a Claude de remplacer muse sur c3"): Claude replaces Muse Spark as C3 reviewer.
- Kevin, 2026-10-08, reported by GPT-5.6 Sol in github-mcp#64 (6049209227, 6049246920), #67 (6049208570, 6049246232) and #71 (6049209901): reassignments of Grok's review roles; the C4 and C5 lines were self-corrected by Sol and are superseded by the next two lines.
- Kevin, 2026-10-08 09:13 Paris, in Claude's chat: Grok stays and finishes C4; Muse Spark is relieved of the C5 review because C6 already loads it; Claude reviews T0.
- Kevin, 2026-10-08 09:18 Paris, in Claude's chat ("Vibe va le faire"): Vibe GLM reviews C5. Recorded in github-mcp#64 (6054734086).
- Kevin, 2026-10-08 09:21 Paris, in Claude's chat ("Oui"): Claude writes this revision.
- Kevin, 2026-10-08 09:41 Paris, in Claude's chat: reported that C5 is merged (observed: github-mcp#64 merged into cc3-integration e0e8484, after Vibe GLM's review agree 6054819060); T0 guide merged (github-mcp#71, 0075d81).
- Kevin, 2026-10-08, coordinator chat after the Codex audit: the audit correction wave is handled before C7; GPT-5.6 Sol creates project-mcp-collab#31 to organize A01-A11. This does not authorize production or main/master promotion.
- Kevin, 2026-10-08, coordinator chat: "Fais ta tâche je vais dire aussi au autre de faire de même". This authorizes GPT-5.6 Sol to execute the coordinator-side A10 refresh now.
- Kevin, 2026-10-08 20:41 Paris, in Claude's chat (reported by Claude in #31 6067359542 and PR #32 review 6068775540): Claude executes F2 and F4 as author under the #31 matrix.
- Kevin, 2026-10-08 22:47 Paris, in Claude's chat (reported by Claude in PR #32 review 6068775540): Claude is the independent reviewer for F6-A10. No distinct tester is inferred from that assignment.
- Kevin, 2026-10-09, current coordinator chat: ChatGPT GPT-6 replaces unavailable Sol for CC-3 operational coordination and CR-A/CR-B reviews; user authorizes a new A10 update with separate Claude review and Vibe GLM test. This is reported chat authority, not /owner proof; no phase promotion.
Kevin alone decides waivers, gate acceptance, merges to main or master, promotion and deployment.

## Roles
| Actor | Scoped acceptance/assignment | Pending evidence |
| --- | --- | --- |
| Kevin | owner | Merge rev 8 only after independent review/test; K4/K6 runtime proof, A11 disposition, final Codex audit, C7 gate and promotion decisions |
| Claude | assembler; author C0, C5, C6, F2, F4; reviewer C2, C3, T0, T1, F1, F6-A10; tester C4, F3, F5 | F2/F4 merged; F1 review complete; F3 formal PASS 6070204581; F5 formal test waits for final Grok head after Sol review; F6-A10 review must be renewed after this correction |
| Muse Spark | author C1; tester C0 | C1 merged; no longer C6 author after Kevin reassigned C6 to Claude |
| Vibe GLM | author C2; C4 final author/reprise; author F1, F3; reviewer C1, C5; tester C3; T0 guide | F1 and F3 merged after independent review/test |
| GPT-5.6 Sol | historical coordinator, author C3, reviewer C4/C6/F3/F4/F5 and author rev 7 A10 | unavailable for current follow-up; no past sign-offs transferred to substitute |
| Grok | original C4 author; author T1, F5; reviewer C0, F2; tester C1, C2, C6, F1, F4 | F1/F2/F4 evidence complete; F5 remains in progress and must finish A07/A09 coverage then resync to live integration |

## Tasks
| id | status | owner | role | reviewer | tester | owned_paths | dependencies | blocker | version | ref | next_action |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| C0 | done | Claude | author | Grok | Muse Spark | src/collab-store/proto/, test/collab-store/c0/, docs/collaboration/cc3/c0-gate.md | K2 | none | 2112ff9 | github-mcp#54, #58, #63 merged | Delivered: local part (review agree Grok, test PASS Muse Spark), cloud rerun on real D1 (#58, review agree Grok 6041314672), S5 CI stability (#63: p95 reported, median bounded after warm-up, after a flaky deploy run 37658176122; merged by Kevin without independent review). The S5 50 ms p95 criterion stays open under C2 |
| C1 | done | Muse Spark | author | Vibe GLM | Grok | src/collab-store/contracts/, src/collab-store/schema/0001_init.sql, test/collab-store/contracts/ | K2 | none | d44f5e6 | github-mcp#57 merged; #25 C1 | Delivered: review agree (Vibe GLM), test PASS (Grok); three recommendations carried to C2 |
| C2 | done | Vibe GLM | author | Claude | Grok (substitute for Muse Spark) | src/collab-store/store/, src/collab-store/mcp/, src/index.ts route line | C0 GO, C1 | none | 3a82d3f | github-mcp#60 merged (298ef4d) | Delivered with review/test. Post-audit store/idempotence/task findings A01/A05/A08 are tracked in F1 and do not rewrite the historical C2 result |
| C3 | done | GPT-5.6 Sol | author | Claude (for Muse Spark) | Vibe GLM | src/collab-store/context/, src/collab-store/phases/ | C2 | none | ff196c7 | github-mcp#70 merged (ad31913) | Delivered and merged after renewed review/test. Post-audit phase/wiring findings A07/A09 are tracked in F5 |
| C4 | done | Vibe GLM (reprise; Grok original) | author | GPT-5.6 Sol | Claude | src/collab-store/memory/, src/collab-store/ledger/, scripts/cc3/import-agent-memory.mjs | C2 | none | eb5f98a | github-mcp#67 merged (22d2571) | Final review AGREE by Sol and formal PASS by Claude at eb5f98a before merge. Post-audit atomic activation finding A03 is tracked in F3 |
| C5 | done | Claude | author | Vibe GLM (for Grok) | GPT-5.6 Sol | src/collab-store/owner/, src/collab-store/identity/, docs/collaboration/cc3/owner-setup.md | C1 | none | b0c0141 | github-mcp#64 merged (e0e8484) | Delivered. K4 owner-channel runtime setup and K6 client mappings remain C7 prerequisites; post-audit owner findings A02/A06 are tracked in F2 |
| C6 | done | Claude (for Muse Spark) | author | GPT-5.6 Sol | Grok (for Vibe GLM) | src/collab-store/export/, docs/collaboration/cc3/ | C2-C5 | none | 1fc4ac7 | github-mcp#75 merged (77ffeae) | Delivered after resync on merged C4 with renewed Sol review and Grok test. Post-audit export-coherence finding A04 is tracked in F4 |
| C7 | blocked | ChatGPT GPT-6 (substitutes for Sol) | coordinator | none | none | project-mcp-collab docs/coordination/cc3/ trial files | CR-A, CR-B, A10 refresh, A11, K4, K6 | global Codex re-audit and runtime owner proofs pending | none | #25 C7; #31; github-mcp#79 | Do NOT start until rev 8 is independently reviewed/tested/merged, Codex clears integrated corrections, A11 is verified, and K4/K6 runtime prerequisites are evidenced |
| F1 | done | Vibe GLM | author | Claude | Grok | github-mcp src/collab-store/store/ + related MCP guards | audit #79 | none | A01/A05/A08 | github-mcp#82 merged (bc22543) | Final head 9c004ba; Claude AGREE and Grok PASS renewed on base 8b1cb48 before Kevin merge |
| F2 | done | Claude | author | Grok | GPT-5.6 Sol | github-mcp src/collab-store/owner/ | audit #79 | none | A02/A06 | github-mcp#81 merged (af940248) | Final head 3baa575; Grok AGREE, Sol PASS, CI 4/4 before Kevin merge |
| F3 | done | Vibe GLM | author | GPT-5.6 Sol | Claude | github-mcp src/collab-store/memory/ | audit #79 | none | A03 | github-mcp#84 merged (e15bb4d) | Final head 63c9d13; Sol AGREE 6070119116 + Claude formal PASS 6070204581 at base bc22543 before Kevin merge |
| F4 | done | Claude | author | GPT-5.6 Sol | Grok | github-mcp src/collab-store/export/ | audit #79 | none | A04 | github-mcp#83 merged (8b1cb48) | Final head 0ef8f1a; renewed Sol AGREE + Grok PASS and CI 4/4 before Kevin merge |
| F5 | done | Grok | author | GPT-5.6 Sol | Claude | github-mcp C3 phases + C2-C6 MCP integration | audit #79 | none (historic post-audit lot) | A07/A09 | github-mcp#80 (merged; verify exact historical evidence if needed) | Original F5 work is historically complete in integration; new Codex CR-01/CR-03 findings were addressed separately by CR-A/#86, not retroactively counted as F5 approval |
| F6-A10 | in_progress | ChatGPT GPT-6 (substitute for Sol) | author/coordinator | Claude | Vibe GLM | docs/coordination/cc3/state.md, AGENT_MEMORY.md | CR-A and CR-B merged; post-audit #31 | independent review and test for rev 8 pending | revision 8 proposal | project-mcp-collab#32 (rev 7 merged), new PR pending | Review and test rev 8 at exact head/base; Kevin merges to main, then Codex audits combined github-mcp integration |
| F6-A11 | proposed | pending | author | pending | pending | github-mcp connector issue-body reading, coordinated with #26 | audit #79 | assignment not yet observed | A11 | project-mcp-collab#31; github-mcp#26,#79 | Avoid duplicate implementation; add paginated/revision-safe body continuation |
| CR-A | done | Claude | author | ChatGPT GPT-6 (Sol substitute) | Vibe GLM | github-mcp phases/owner/identity + tests | Codex CR-01, CR-03 | none for scoped PR | cd80f59 | github-mcp#86 merged into cc3-integration (695064d) | Reviewer AGREE 6085520149, independent Vibe PASS 6085571437, CI 4/4 at PR head; not global C7 clearance |
| CR-B | done | Vibe GLM | author | ChatGPT GPT-6 (Sol substitute) | Claude | github-mcp memory journal lifecycle + MCP + tests | Codex CR-02; CR-A merged | none for scoped PR | da6cd4e | github-mcp#87 merged into cc3-integration (ed9d1b5) | Reviewer AGREE 6087040929 after CRB-R1 fix; independent Claude PASS 6087251944; CI 4/4 at PR head; integrated global audit pending |
| T0 | done | Kevin | owner | Claude (for Grok) | none | docs/collaboration/cc3/run-checks.md (Vibe guide) | K1 | none | e9f4e9d | #25 T0; github-mcp#59, #61, #71, #72 | K5 done: run_checks configured (#59), mcp:checks required at consent (#61), controller re-pinned to master 774a311 after the feedback consolidation (#72, merged into cc3-integration 33d6602). Claude verified run 37660768469 on 54a2c03 (quick, verifiedSuccess true, reuse on the second call). Guide by Vibe GLM in github-mcp#71: Claude changes_requested at b8241c5 (6054654896), five corrections made, review agree at e9f4e9d (6054720155), CI 4/4. Merged by Kevin (0075d81) |
| T1 | done | Grok | author | Claude | none | github-mcp docs/collaboration/cc3/t1-sandbox-study.md | none | none | 8a6dc6e | github-mcp#55 merged; #25 T1 | Delivered and merged; the three non-blocking remarks of Claude's review are left to the author; location moved from the plan |
| K3 | done | Kevin | owner | none | none | none | K2 | none | db74838 | #25 section 8; github-mcp#56 merged | cc3-test is deployed: KV and D1 created, ids in wrangler.jsonc, GitHub environment, OAuth callback and runtime secrets set by Kevin, workflow added by Kevin (db74838). Workflow run 37626432208: build and deploy to cc3-test succeeded; the later curl steps failed, a likely first-deploy delay (the step log was not readable by Claude, so the cause is not confirmed). Claude checked /ready (200) and the OAuth discovery document (200) afterwards. No independent review: infrastructure operation merged by the owner |
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
| github-mcp PR #56 (K3 prep) | merged into cc3-integration by Kevin; head 6ba4e65 with the real KV and D1 ids, CI 4/4; the deploy workflow was added by Kevin because GitHub Actions files are protected for the MCP |
| github-mcp cc3-test deployment | run 37626432208 at db74838: install, check:full and wrangler deploy succeeded; the curl verification steps failed right after the first deploy; Claude then read /ready (200, status ready) and /.well-known/oauth-authorization-server (200); the step log itself was not read |
| github-mcp PR #58 (C0 cloud) | open draft, head ecc51e8; ci, Workers Builds and SonarCloud green; GitGuardian check did not report on this head (absent, not counted as passed); local stand-in run 9/9 green; real D1 run by Kevin, output pasted in chat, not archived; review by Grok pending |
| project-mcp-collab PR #26 | merged into main (3499633) after GPT-5.6 Sol review agree at f4f2e1d (6035961577, 6035971936) |
| github-mcp cc3-integration | head db748389a314233636f60f6cebf178e017baa12b, which includes the merges of #54, #55, #57 and #56 (K3) and the deploy-cc3-test.yml workflow added by Kevin; PRs #58 and #59 are based on it |
| github-mcp PR #59 (K5) | open draft, base db74838, head f32d791; one line in wrangler.jsonc env.cc3-test: GITHUB_CHECKS_CONFIG pinned to the master controller fc2da1d (agent-checks.yml present there); parser and wrangler dry-run checked locally; ci, Workers Builds and SonarCloud green, GitGuardian absent (not counted as passed); the GitHub App already has Actions read and write (reported by Kevin, not verified by Claude); not merged; client re-consent with mcp:checks still to do |
| github-mcp PRs #58, #59, #61, #62, #63, #60 | merged into cc3-integration in that order (8a73cf0, 8da576d, 8d2d998, 54a2c03, 311ca61, 298ef4d); #59, #61, #62 and #63 merged by Kevin without an independent review comment |
| github-mcp cc3-test deployment | run 37665752466 at 298ef4d succeeded; run 37658176122 at 54a2c03 failed on the C0 S5 p95 (154 ms) and passed on rerun |
| run_checks on cc3-test | earlier calls refused at 16:55 UTC with a generic message during the K5 rollout; run 37660768469 (quick, 54a2c03) success with verifiedSuccess true |
| github-mcp PR #68 (feedback consolidation of #33, #35, #65, #66) | merged into master (774a311) by Kevin; the original PRs are closed; master moved, so the run_checks controller pin fc2da1d became stale |
| github-mcp PR #72 (K5 re-pin) | merged into cc3-integration (33d6602) by Kevin; GITHUB_CHECKS_CONFIG controllerSha 774a311; between fc2da1d and 774a311 only the append-only journals changed |
| github-mcp PR #69 | closed (duplicate of #70 targeting master) |
| github-mcp PR #70 (C3) | open draft, head 3adb106, base cc3-integration; CI 4/4; Claude review agree (6049197196); test by Vibe GLM pending |
| github-mcp PR #67 (C4) | open draft, head 95b0f7e; ci failure (run 37700800978); GPT-5.6 Sol changes_requested (6048736293); Claude preliminary test changes_requested (6049235436) |
| github-mcp PR #64 (C5) | open draft, head b0c0141; CI 4/4; GPT-5.6 Sol test PASS_WITH_LIMITS (6047695154); reviewer changed to Vibe GLM (6054734086) |
| github-mcp PR #71 (T0 guide) | open, head e9f4e9d; CI 4/4; Claude review agree (6054720155) |
| github-mcp PR #73 (S5 harness) | open, head 38d7750, documentation only; CI 4/4; no reviewer named |
| github-mcp get_pull_request vs comment guard | after #72, get_pull_request still reported base 298ef4d for #67 and #71 while the review guard required the live base 33d6602; reported to Kevin as a tool inconsistency |
| github-mcp PR #64 (C5) merge | merged into cc3-integration (e0e8484) after Vibe GLM review agree (6054819060) |
| github-mcp PR #71 (T0) merge | merged into cc3-integration (0075d81) |
| project-mcp-collab PR #29 | merged into the revision 5 branch after #28 reached main, so revision 6 did not reach main; carried over by the later main snapshot |
| github-mcp PR #70 (C3) | merged; final head ff196c7998f1e0f2fd1ca9c2155c315e4d049e04, integration merge ad31913af948c1a0fdfb812f67fca1284eccb1be |
| github-mcp PR #67 (C4) | merged; final head eb5f98a86d7679a9cfe6882d08e599c61fae962d, integration merge 22d2571eb9371645b954a78714f2c85daf66944a after Sol AGREE + Claude PASS at the final head |
| github-mcp PR #75 (C6) | merged; final head 1fc4ac7a952e4a3a521a5488e5a1557b16207fa6, integration merge 77ffeae6ffb4b805303a3480b55871e03d748173 |
| github-mcp#79 Codex audit | audit of cc3-integration@77ffeae; A01-A04 P1, A05-A08 contract/guard findings, A09 traversal blocker, A10 stale coordination snapshot, A11 issue-body continuation gap |
| project-mcp-collab#31 | post-audit correction plan before C7; groups A01-A11 into F1-F6 and keeps promotion/merge authority with Kevin |
| github-mcp PR #81 (F2) | merged into cc3-integration as af940248; final head 3baa575, Grok reviewer AGREE + GPT-5.6 Sol tester PASS, CI 4/4 |
| github-mcp PR #83 (F4) | merged into cc3-integration as 8b1cb488; final head 0ef8f1a, renewed GPT-5.6 Sol AGREE + Grok PASS, CI 4/4 |
| github-mcp PR #82 (F1) | merged into cc3-integration as bc22543f; final head 9c004ba, Claude AGREE + Grok PASS renewed on the then-live base, CI 4/4 |
| github-mcp PR #84 (F3) | merged into cc3-integration as e15bb4d9; final head 63c9d13, GPT-5.6 Sol AGREE 6070119116 + Claude formal PASS 6070204581, CI 4/4 |
| github-mcp PR #80 (F5) | open at head 9c90f742 on old base 77ffeae; A07 replay/policy probes passed in Claude pre-test 6069270480, but A09 and in-PR D1/MCP regression coverage remain open; final resync to live integration e15bb4d required before Sol review/Claude formal test |
| project-mcp-collab PR #32 (F6-A10) | Claude CHANGES_REQUESTED 6068775540: snapshot F1-F5 facts/roles stale; corrected on the next head. Claude is reviewer; distinct tester still unassigned by owner |
| github-mcp PR #86 (CR-A) | merged; head cd80f59bc28b30d7c8cea3f9162d1680ed071352; review ChatGPT GPT-6 AGREE 6085520149; test Vibe GLM PASS 6085571437 at same head/base 221d634; CI 4/4; integration head after merge 695064d |
| github-mcp PR #87 (CR-B) | merged; head da6cd4ed2d9b34e7c47075fe66a6b99e2a64b30f; review ChatGPT GPT-6 AGREE 6087040929; test Claude PASS 6087251944 at same head/base 695064d; CI 4/4; integration head after merge ed9d1b5 |
| github-mcp cc3-integration after #87 | ed9d1b5baf2a98fd837c41b8e56627468671e7f1; deploy-cc3-test and Workers Builds succeeded; all four original PR checks were not observed on the merge SHA, so integrated CI verification is incomplete, not pass |
| project-mcp-collab PR #32 (A10 rev 7) | merged; this rev 8 update supersedes its operational snapshot, preserving history and unchanged phase P5 |
