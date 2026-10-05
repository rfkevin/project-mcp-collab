# Evidence — paired read-only runs (L0, 2026-10-05)

fixture: CC2-L0-EV-PAIR-1 | mode: real | author: Muse Spark | repo: rfkevin/project-mcp-collab | ref: f8d2b0d338e8e4ca6a1bec8bdce3de21b64068e9
Operation: read AGENTS.md + WORKFLOW.md. Arm A = github_read_files (1 call, 2 files). Arm B = github_read_file ×2.
Latency: unknown (not exposed). Serialized envelope bytes: unknown (not measurable client-side).

## Samples
| run | arm | calls | args (approx B) | payload B (sum of file sizes) | truncated | retries | dupes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | A | 1 | ~160 | 4034 (1749+2285) | false | 0 | 0 |
| 1 | B | 2 | ~95 + ~95 | 4034 | false | 0 | 0 |
| 2 | A | 1 | ~160 | 4034 | false | 0 | 0 |
| 2 | B | 2 | ~95 + ~95 | 4034 | false | 0 | 0 |
| 3 | A | 1 | ~160 | 4034 | false | 0 | 0 |
| 3 | B | 2 | ~95 + ~95 | 4034 | false | 0 | 0 |
| median | A | 1 | ~160 | 4034 | — | 0 | 0 |
| median | B | 2 | ~190 | 4034 | — | 0 | 0 |

Integrity: AGENTS.md blobSha 0653b470c0fcba2f015cd0ecb670949575026e70 and WORKFLOW.md blobSha 401dcd3ff20017c83de9f59648800b775357b636 identical in all 6 reads; `partial:false` in every batch response.
Method note: arg bytes = character count of the JSON argument object as written by the client (approximate, disclosed). Payload = exact file `size` fields returned by the server. No token or speed claim.
Reproduction: run the 3 calls of either arm at the same ref; expected values above; divergence = report, do not average away.
