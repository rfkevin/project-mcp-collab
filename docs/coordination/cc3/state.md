# CC-3 state
schema_version: CC-STATE-1
workflow_id: CC-3
revision: 5
base_revision: 4
next_action: C3 (GPT-5.6 Sol), C4 (Grok) and C6 (Muse Spark) start from cc3-integration 298ef4d (C2 merged). Claude starts C5 (owner channel and identity). Vibe GLM: C2 follow-ups (S5 latency on cc3-test with the deployed Worker, recorded in c0-gate.md; model_meta in the collab output schema) and the T0 guide docs/collaboration/cc3/run-checks.md for Grok's review. Kevin: merges as reviews and tests land; agents recreate their Cc3 connector (mcp:checks is now required on cc3-test). cc3-integration is at 298ef4d.
canonical_ref: main
based_on_sha: 62bc4703fefccb235ba647bf34a9615421a10ad9
phase: P5
framing_version: CC-FRAME-3
framing_ref: https://github.com/rfkevin/project-mcp-collab/issues/24
plan_version: CC-PLAN-3/v1.1
plan_ref: https://github.com/rfkevin/project-mcp-collab/issues/25
execution_ref: https://github.com/rfkevin/github-mcp/tree/mcp/105856986/cc3-integration
contract_ref: https://github.com/rfkevin/project-mcp-collab/issues/25
acceptance_ref: https://github.com/rfkevin/project-mcp-collab/issues/25

Placement defines authority: this file on a task branch is a proposal; it becomes the approved CC-3 snapshot only when Kevin merges it to main.
Revision 1 is the P5 opening decided by Kevin on 2026-10-07 (K1, #25 6035211334); it was never transcribed (same convention as CC-2; the L1 parser requires base_revision >= 1). Revision 2 transcribed the cycle at P5 start. Revision 3 (merged with PR #26) refreshed the operational facts after review 6035804243. Revision 4 refreshes them again after the merges of github-mcp #54, #55, #56 and #57, the recorded reviews and tests, the deployment of cc3-test (K3), the cloud rerun of C0 and the owner's acceptance of the C0 gate; invariants, plan and matrix are unchanged. Revision 5 records the merges of github-mcp #58 to #63 (C0 cloud, K5, actionable errors, S5 CI stability, C2), the first verified run_checks on cc3-test, and the owner's substitution of Grok for Muse Spark as C2 tester; the matrix is otherwise unchanged. Owner decision lines remain reported, not authenticated.
CC-2 stays in WORKFLOW_STATE.md (rev 4) because it still has open owner items (D12, G5-F2, MEM, PROMOTE); this file does not replace or close it. Whether CC-3 should later move to WORKFLOW_STATE.md is an owner decision.
This file is the last state written by PR: from C2/C7 onward the live CC-3 state moves to the collab store, and this path receives exports (collab_export, CC-STATE-1).

## Owner decisions
Names are declared labels, not authenticated identities. No owner channel exists yet (I7, lot C5): every line below is reported, not verified.
- Kevin, 2026-10-07, direct instruction in Claude's chat: Claude is the CC-3 assembler (P4).
- Kevin, 2026-10-07 11:36 Paris, direct instruction in Claude's chat ("Vas-y"), recorded in #25 (6035211334): K1, CC-PLAN-3/v1.1 validated (scope, matrix section 6, I7 requirement); phase P5 opens.
- Claude on Kevin's go-ahead, 2026-10-07: K2 done, github-mcp branch mcp/105856986/cc3-integration created from cc2-integration 30b3168.
- Kevin, 2026-10-07 15:04 Paris, in Claude's chat: reported that github-mcp PR #56 was merged into cc3-integration (observed by Claude: merged, wrangler.jsonc on cc3-integration holds the real ids).
- Kevin, 2026-10-07 16:45 Paris, direct instruction in Claude's chat ("1 on demarre", answering the options in Claude's previous message): accepts the C0 gate on correctness (real D1) and starts C2; the 50 ms p95 latency criterion (S5) moves to C2 and is measured on cc3-test with a deployed Worker.
- Kevin, 2026-10-07 19:04 Paris, in Claude's chat ("oui"): Claude makes tool errors actionable (github-mcp #62); merged by Kevin.
- Kevin, 2026-10-07 19:43 Paris, in Claude's chat ("je te laisse décider"): Claude decides the fix of the flaky C0 S5 test (github-mcp #63); merged by Kevin.
- Kevin, 2026-10-07, reported by Grok in github-mcp#60 (6043996431): Grok replaces Muse Spark as C2 tester (Muse Spark unavailable). No public owner attestation.
- Kevin, 2026-10-07 23:27 Paris, in Claude's chat ("Tu peux faire ta part"): Claude writes this revision and starts C5.
Kevin alone decides waivers, gate acceptance, merges to main or master, promotion and deployment.

## Roles
| Actor | Scoped acceptance/assignment | Pending evidence |
| --- | --- | --- |
| Kevin | owner | K4, K6, K7, K8 (K3, K5 done); client re-consent with mcp:checks for every agent |
| Claude | assembler; author C0, C5; reviewer C2, C6, T1; tester C4 | C0 done; C2 review agree at 3a82d3f (6043813866); C5 starting; revision 5 of this file |
| Muse Spark | author C1, C6; reviewer C3; tester C0, C2 | C2 tested at ba7cf7b (PASS, #25 6042530743) before the head moved; replaced as C2 tester by Grok; C6 can start; publishes through the owner's machine, so availability follows Kevin's |
| Vibe GLM | author C2; reviewer C1; tester C3, C6; T0 guide | C2 merged (github-mcp#60, 298ef4d); follow-ups: S5 latency on cc3-test, model_meta output schema; T0 guide not yet written |
| GPT-5.6 Sol | author C3; reviewer C4; tester C5; C7 coordinator | C3 can start (C2 merged) |
| Grok | author C4, T1; reviewer C0, C5, T0; tester C1 (and C2 as substitute) | C0 cloud review agree (github-mcp#58 6041314672); C2 test PASS at 3a82d3f (github-mcp#60 6043996431, inspection and CI, not rerun locally); C4 can start |

## Tasks
| id | status | owner | role | reviewer | tester | owned_paths | dependencies | blocker | version | ref | next_action |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| C0 | done | Claude | author | Grok | Muse Spark | src/collab-store/proto/, test/collab-store/c0/, docs/collaboration/cc3/c0-gate.md | K2 | none | 2112ff9 | github-mcp#54, #58, #63 merged | Delivered: local part (review agree Grok, test PASS Muse Spark), cloud rerun on real D1 (#58, review agree Grok 6041314672), S5 CI stability (#63: p95 reported, median bounded after warm-up, after a flaky deploy run 37658176122; merged by Kevin without independent review). The S5 50 ms p95 criterion stays open under C2 |
| C1 | done | Muse Spark | author | Vibe GLM | Grok | src/collab-store/contracts/, src/collab-store/schema/0001_init.sql, test/collab-store/contracts/ | K2 | none | d44f5e6 | github-mcp#57 merged; #25 C1 | Delivered: review agree (Vibe GLM), test PASS (Grok); three recommendations carried to C2 |
| C2 | done | Vibe GLM | author | Claude | Grok (substitute for Muse Spark) | src/collab-store/store/, src/collab-store/mcp/, src/index.ts route line | C0 GO, C1 | none | 3a82d3f | github-mcp#60 merged (298ef4d) | Delivered: review agree Claude at 3a82d3f (6043813866, check:full 707 tests), test PASS Grok at 3a82d3f (6043996431; inspection and CI 4/4, not rerun locally), Muse Spark PASS at the earlier head ba7cf7b. Follow-ups for Vibe GLM: S5 50 ms p95 and append latency on cc3-test with the deployed Worker, recorded in c0-gate.md; model_meta in outputSchemas. Carried to C4 and C5: participant_id is client-declared, not bound to the token; collab tokens issued without required scope (refusal at call time); collab refresh without resource is routed to the GitHub provider |
| C3 | accepted | GPT-5.6 Sol | author | Muse Spark | Vibe GLM | src/collab-store/context/, src/collab-store/phases/ | C2 | none | 298ef4d | #25 C3 | GPT-5.6 Sol starts from cc3-integration 298ef4d |
| C4 | accepted | Grok | author | GPT-5.6 Sol | Claude | src/collab-store/memory/, src/collab-store/ledger/, scripts/cc3/import-agent-memory.mjs | C2 | none | 298ef4d | #25 C4 | Grok starts from cc3-integration 298ef4d |
| C5 | in_progress | Claude | author | Grok | GPT-5.6 Sol | src/collab-store/owner/, src/collab-store/identity/, docs/collaboration/cc3/owner-setup.md | C1 | none | 298ef4d | #25 C5 | Claude builds from cc3-integration 298ef4d; mechanism from the C0 report (Access preferred, secret fallback); binds participant_id to the token identity (C2 reserve) |
| C6 | accepted | Muse Spark | author | Claude | Vibe GLM | src/collab-store/export/, docs/collaboration/cc3/ | C2 | Muse Spark publishes through the owner's machine | 298ef4d | #25 C6 | Muse Spark starts from cc3-integration 298ef4d when Kevin's machine is available; named substitutes per plan otherwise |
| C7 | proposed | GPT-5.6 Sol | assembler | none | none | project-mcp-collab docs/coordination/cc3/ trial files | C3, C4, C5, C6, K6 | waiting dependencies | none | #25 C7 | wait for C3-C6 and K6 |
| T0 | in_progress | Kevin | owner | Grok | none | docs/collaboration/cc3/run-checks.md (Vibe guide) | K1 | none | 54a2c03 | #25 T0; github-mcp#59, #61 | K5 done: run_checks configured (#59), mcp:checks required at consent (#61). Claude verified run 37660768469 on 54a2c03 (quick, verifiedSuccess true, reuse on the second call). Remaining: Vibe GLM writes the guide, Grok reviews it |
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
