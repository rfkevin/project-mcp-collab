# Acceptance criteria v1
plan: [CC-PLAN-1/v1.2](plan-v1.2.md) | execution: [issue #3](https://github.com/rfkevin/project-mcp-collab/issues/3)
owner: T01/Codex criteria; T40/ChatGPT procedures; reviewer: Grok
status: frozen criteria (CC-PLAN-1/v1.2); executed results are recorded in docs/validation/scenarios.md and issue #3, not here

## Evidence contract
case, actor, client/catalog/time if relevant, exact version/head/base, steps, expected, observed, evidence_ref, limitations.
Outcomes: pass/fail/not_tested. Distinguish simulation, author inspection, independent trial and real-client observation.
No secret/auth traces, invented model identity or unperformed reviewer result.
Task A templates link here; D adds procedures/results in docs/validation/scenarios.md, without copying this whole matrix.

## Complete matrix
| ID | Scenario | Acceptance criterion |
| --- | --- | --- |
| V01 | Normal workflow/navigation | Phase inputs/outputs/owner decision/next actor present; locate task from map; final combined-SHA links and IDs resolve |
| V02 | Phase independence | P1 private preparation; asynchronous publication then STOP; P2 peer reading only after owner signal; P2 publication then STOP until P3; owner exceptions recorded; no fake technical secrecy proof |
| V03 | Missing actor and attribution | Missing contribution stays pending; no assumed assent; source ID/account/cross-reference distinct from untrusted declared label; owner substitution explicit |
| V04 | Long/paginated/edited sources | Required bodies completely read; pages/continuation and ID+updatedAt checked; distinguish issue body vs comment tools; partial/fallback recorded |
| V05 | Blocking objection | Blocker remains open despite green CI, silence or majority until verified resolution/owner arbitration |
| V06 | Changed plan/head/base | Affected logical-plan opinions renewed; PR review tied to new exact head/base; no silent reuse of stale agreement |
| V07 | Interrupted/resumed agent | Recover approved version, open blockers, evidence, uncertainty, next action; do not repeat successful or uncertain writes blindly |
| V08 | Concurrent stale state | Stale blob/work-head precondition refuses mutation; separately reconcile main revision/base before merge; preserve concurrent edits; no assumed atomic main lock |
| V09 | Lost write confirmation | Reread remote state before retry; distinguish existing result, absent result and unresolved uncertainty |
| V10 | Independent checks/knowledge | Author/reviewer/tester separated for major lots; useful append-only lessons; no private ballots or rewritten history |
| V11 | Compact agent format | Independent agent recovers all constraints/dependencies/evidence from shared notation and navigation; no unmeasured savings claim |
| V12 | Known batchable operations | Use available grouping for independent known reads/coherent changes; prerequisites first; compare calls, argument bytes, response bytes, supplied context and time at equivalent information |
| V13 | Budget/partial batch | Missing items/errors/continuation explicit; complete relevant reading before verdict; unavailable capability marked not_tested |
| V14 | Complete review coverage | Compare actual changed-path set to owned_paths; exact head/base and files_changed/files_read/unread recorded; agree only with unread empty and no unhandled blocker |
| V15 | Wrong-target prevention | Task target_ref includes repo/type/number-or-branch/title; identify target before public write; resolve ambiguity before mutation; test without posting to unintended targets |
| V16 | No CI vs missing CI vs running CI | No configured checks => manual doc evidence/no automated pass; missing expected check => inspect check availability, mergeability/base/diagnostics before waiting; only observed running checks are pending |

## Execution allocation
- T10/A verification: Antigravity, independent of Grok.
- T20/B verification: Grok, independent of Codex.
- T30/C verification: ChatGPT + DeepSeek reading evidence; DeepSeek's added T00 review requires acknowledgment.
- T40 end-to-end: Vibe A/B cases, Antigravity C cases; Grok reviews D design/evidence.
- Authorship of a server tool must be disclosed. Independent discussion-delta verification: DeepSeek or Antigravity; never Claude alone.
- Claude's historical advice is retained; T50 final consultation is deferred/unassigned by Kevin.
- New actor recommendations are not already-run tests. T01 matrix acknowledgments remain explicit.

## Reporting limits
Counting calls alone can hide large argument/context costs; report raw values and interpretation separately.
Compatible tokenizer unavailable => tokens not_measured. Human-readable tool-improvement proposals stay in French.
Transient/sandbox/client observations are scoped; do not suppress error output as a universal remedy.
Additional runtime tests are only needed if executable implementation is later authorized. This V1 adds no runtime stack.
