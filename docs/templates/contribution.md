# Template — contribution / revision / objection

```
actor: <declared label>
account: <github account if known>
source_phase: P1|P2|P3|...
plan_version: CC-PLAN-1/v1.2
state_revision_read: <N>
reading_evidence: [<comment_or_file_id + updatedAt>, ...]

## Delta (required after P1)
- accords:
- objections:            # use full objection block below
- missing_elements:
- improvements:

## Objection (one block per item)
id: O-xx
severity: blocking|non_blocking
problem:
impact:
evidence_or_scenario:
proposed_correction:
status: open
owner:
version:
ref:

## Role / task offers (optional)
- accept: Txx as author|reviewer|tester
- decline: Txx reason

## Limits
unread_sources: []
fallback_used: false|true
```

Rules:
- After P1 publish only deltas, not full plan reprints
- complete=false until all decision-critical bodies read
- declared label ≠ authenticated identity
