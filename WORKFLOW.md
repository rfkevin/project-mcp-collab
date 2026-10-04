# WORKFLOW — Collaboration Croisée phases

plan_ref: docs/coordination/plan-v1.2.md
state_ref: WORKFLOW_STATE.md
contract_ref: docs/coordination/state-contract.md

## Phases
| Phase | Name | Key rule |
| --- | --- | --- |
| P1 | Independent proposals | Private prep → publish → STOP. No reading peers until owner signal. |
| P2 | Cross-read / revise / private vote | Read peers; publish only real deltas/objections; vote stays private |
| P3 | Convergence / roles / private revote | Re-read; adjust plan/roles; revote privately |
| P4 | Assembly | Assembler publishes versioned plan + task board; owner validates scope |
| P5 | Implementation | Execute assigned lots; one task = one branch = one PR |
| P6 | Review / correct / manual merge / lessons | Independent review+test; owner merge; append durable lessons |

Owner (Kevin) alone authorizes phase transitions. Record decision in state.

## Roles
| Role | Duty |
| --- | --- |
| Assembler / PM | Synthesize plan, maintain board, process findings |
| Author | Deliver owned_paths at exact head; no self-validation as sole evidence |
| Reviewer | Full changed-path read; verdict with unread=[] and no unhandled blocker |
| Tester | Independent scenario execution; pass/fail/not_tested |
| Owner | Phase transitions, main merges, arbitration of blockers |

Author ≠ Reviewer ≠ Tester for major lots (V10).

## Transitions
1. Read approved state (revision N) + plan_version
2. Confirm owner decision for next phase (or explicit exception)
3. Update state as proposal (revision N+1, base_revision N) on task branch
4. Human merge to main establishes approved snapshot

## Objection contract
Required fields: id, problem, impact, evidence/scenario, proposed_correction, severity, status, owner, version, ref
Status: open → resolved_with_evidence | owner_arbitrated
Silence / majority / green CI does not close a blocker (V05).

## STOP boundaries (V02)
- After P1 publish: do not read peers until owner opens P2
- After P2 publish: do not start P3 work until owner opens P3
- Owner exceptions must be recorded in state

## Review record (minimum)
head_sha, base_sha, files_changed(paths/count), files_read(paths/count), unread, scope_deviations, evidence, verdict
Agree only if unread empty and no unhandled blocker (V14).
