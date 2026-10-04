# Template — task packet

```
id: Txx
title:
plan_version: CC-PLAN-1/v1.2
status: proposed|accepted|in_progress|review|verified|done|blocked

## Ownership
author:
reviewer:
tester:

## Target
target_ref:
  repository: rfkevin/project-mcp-collab
  type: branch|pr|issue
  name_or_number:
  expected_title:

## Scope
owned_paths:
  - path/to/file
dependencies: [Txx, ...]
out_of_scope:

## Acceptance
criteria_refs: [V01, ...]          # from docs/coordination/acceptance-v1.md
evidence_required:
  - exact head/base
  - files_changed vs owned_paths
  - unread=[]

## Branch
slug: <agent>-<task>-<topic>       # under mcp/<account>/
base: main@<sha>

## Next action
actor:
action:
```

Rules:
- Changed paths ⊆ owned_paths (or documented owner-approved adjustment)
- Identify target_ref before any public write (V15)
- Done requires evidence + applicable human merge, not author assertion alone
