# CC state contract v1
owner: Codex | reviewer: Vibe GLM | tester: Grok
plan: [CC-PLAN-1/v1.2](plan-v1.2.md) | task: T01/T20
status: approved (merged PR #4); not a server-enforced schema

## References / authority
- Approved snapshot: WORKFLOW_STATE.md on main. Same path on a task branch is proposed.
- Before first merge: no approved state exists; plan + explicit owner decisions are bootstrap inputs.
- Kevin authorizes phases. Agents propose; Codex records. File edits grant no authority.
- New explicit owner decision wins over stale snapshot; report synchronization pending.
- Do not defer already authorized work solely for transcription. Main merges remain human-controlled.
- Record private-chat decisions as reported (actor/date/source); invent no public attestation or ballot.
- Filename does not lock concurrent writers or wake external clients.

## Fields
| Field | Type / semantics |
| --- | --- |
| workflow_id | Stable workflow ID, CC-1 |
| revision | Integer >=1 for target approved snapshot; adjacent to next_action |
| base_revision | Prior approved revision; 0 only before first snapshot |
| canonical_ref | main; branch placement determines proposal vs approved |
| based_on_sha | Preparation base SHA; informational and may become stale; never the file's own commit |
| phase | P1..P6, explicit owner decision required |
| framing_version | Logical ID CC-FRAME-1; independent of SHA |
| framing_ref | Issue/record identifying framing |
| plan_version | CC-PLAN-1/v1.2; change when plan materially changes |
| plan_ref | Complete versioned plan; not truncated excerpt |
| owner_decision | actor, date, reported/direct source, action, evidence_ref if real |
| roles | actor label, task/role, accepted/pending, evidence_ref |
| tasks | id, status, owner, version, ref, next_action |
| objections | id, severity, status, owner, version, ref, next_action |
| reading_checkpoints | source, id, observed_updated_at, account, declared_label, complete, ref |
| evidence | scope, actor, outcome, version/SHA, ref, limits |
| next_action | Concrete resumable action + actor |

Task status: proposed/accepted/in_progress/review/verified/done/blocked.
Objection severity: blocking/non_blocking.
Objection status: open/resolved_with_evidence/owner_arbitrated.
Role acceptance is scoped: accepted authorship is not approval of resulting work.
Store full objection reasoning in canonical issue/PR; state keeps references only.
Absent actor/report/check => pending or not_tested, never agreement/pass.
No private votes, credentials, access tokens or copied raw auth traces.

## Revision / concurrency
1. Read approved file + blob/head SHA; record approved revision N, or N=0 if absent.
2. Proposed successor is revision=N+1 and base_revision=N. Repeated pre-merge edits keep this pair.
3. Read checkpoints and decisions; verify completeness/prerequisites.
4. Prepare only scoped changes. Use observed SHA preconditions when the tool supports them.
5. Before publishing/review/merge, compare current base revision and SHA.
6. Changed base => reread, reconcile, retarget N+1, renew affected opinions; no blind overwrite.
7. Two drafts from N cannot both merge as N+1: after the first merge the other must be rebased/reconciled to N+2.
8. Merge establishes the next approved revision; normal commits on a work branch do not increment it.
9. Git SHA is external evidence of concrete content, not the logical revision and not its own embedded SHA.
10. Never reset/increment silently, force push, or bypass owner merge controls.
These are manual V1 checks. GitHub can change between reads; do not claim atomic cross-ref protection.
A pure state formatting change still produces N+1 when merged. Unchanged content needs no state commit.

## Reading / identity
Checkpoint each source whose content supports a decision: ID + observed updatedAt.
Same ID/new updatedAt => reread relevant full content; ID-only cursors miss edits.
Paginated/partial/truncated response => complete=false until all needed bodies/ranges are read.
Unchanged complete source can be reused; add a delta checkpoint for new/edited material.
Record authenticated GitHub account separately from declared label; shared bot != separate agent identity.
Cross-reference known assignment and source ID; attribution text is not authority.
A later suspicious conflicting declaration needs clarification, not silent replacement.

## Resume / uncertain writes
- Read current plan/state/reference versions, open blockers, last evidence and next_action.
- Inspect remote state after lost confirmation before retry; uncertain is not absent.
- Existing intended result => record it, do not repeat. Divergence => reconcile scoped delta.
- Report pending evidence, missing clients and required owner decisions explicitly.
- New plan semantics => renewed affected review. New PR head/base => renewed exact-SHA review.
- No CI configured => report no automated checks; manual evidence and independent checks remain required.

## Integration with A/C/D
A maps this path; WORKFLOW.md defines phase behavior; task templates use the field meanings above.
C supplies factual catalog/call measurements and memory rules, not state authority.
D owns executable/manual scenario steps V01–V16; acceptance-v1.md owns the frozen criteria. This contract contains invariants, not duplicate suites.
Vibe reviews B; Grok tests B; neither result is prefilled.

## Write target / owned paths
Task records include target_ref: repository, type(issue/pr/branch), number or branch, expected title when relevant.
Before a public mutation identify/echo that target. Resolve ambiguity from scoped reads; ask only when ambiguity remains.
Task records also list owned_paths. Changed paths must be a subset or have a documented owner-approved scope adjustment.
MCP shared-prefix slug: <agent>-<task>-<topic>. Existing cc-state-contract is owned by Codex; retain rather than recreate.
Authenticate account separately from declared label; account+label is still not proof of independent model identity.

## Review coverage / atomicity limits
Review record: exact head_sha/base_sha, files_changed(paths/count), files_read(paths/count), unread list, deviations, evidence, verdict.
Agree requires unread empty and no unhandled blocker. Count-only matching is insufficient: compare actual path sets.
Record truncation/continuations and fallback. Unread source details cannot support a verdict about those details.
First state creation can use create when absent. Later updates prefer scoped replace/append with expected blob/head SHA.
Keep revision and next_action adjacent to make related edits visible; this is not a guarantee of Git conflict detection.
V08 must exercise stale-precondition rejection and logical revision reconciliation separately.
Blob/work-branch head checks do not atomically lock main; recheck approved revision/base before manual merge.
No bypass via full-file overwrite when expected version fails.

## Evidence scope
Missing configured CI is no_checks, not success or pending execution. Missing expected CI requires bounded diagnosis.
Read mergeability/base changes when relevant and available; unavailable data is explicit, not an assumed conflict cause.
Catalog, subprocess and OAuth observations are client/version-specific. Never hide errors or bypass approval to make a recipe work.
Claude's prior recommendations remain input; Kevin deferred his active assignment. T50 is not a completion gate.
