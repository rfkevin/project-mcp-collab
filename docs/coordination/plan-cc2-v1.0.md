# CC-2 plan — Intégration de la V1 dans github-mcp

plan_version: CC-2-PLAN-1/v1.0 | framing: issue #13 (complète #1, inchangée) | status: proposal (branch; owner merge = approved)
based_on_sha: dd16faf0e7143a7728923d812ad3378a44e28258 | assembled from: P1 5984463500/5984468835/5984580784/5990064913/5991261705 + P2/P3 convergences (all lossless, refs in #13)

## 0. Scope and non-goals
- Integrate the V1 collaboration protocol (CC-1, this repo) into the `rfkevin/github-mcp` connector as a thin coordination layer above existing services.
- No duplication of GitHub primitives, no parallel system, no overwrite of existing files without an explicit owner decision.
- Out of scope this cycle: agent notification/activation by API, technical P1 isolation (visibility gates), token claims without compatible measurement.

## 1. Consolidated decisions (from P2/P3 consensus; owner arbitrations marked ⚖)
| ID | Decision | Basis |
| --- | --- | --- |
| D1 | github-mcp = capability layer; collaboration = thin composition layer reusing internal services | all P1; Codex §2; GPT O3 |
| D2 | Single canonical state: `WORKFLOW_STATE.md` (V1 filenames, deterministic sections, explicit revision) — one owner-merged approval path. Structured record may later become an adapter, never a second editable authority. Coordination branch: dropped for V2 | Muse O-2; Codex corr.1; Cline B.1; GPT §2 |
| D3 | Memory invariant: append-only everywhere; growth bounded by curated pointer entries (themselves append-only). Synthesis/rewrite withdrawn | Vibe+Grok objection; GPT explicit withdrawal 5991998054 §3 |
| D4 | P1 isolation this cycle: disclosure-only — mandatory read-coverage line per contribution; prior peer reading = observer entry (Muse Spark precedent). No secrecy claims on public GitHub; technical isolation → improvement registry | Codex §4; GPT O4; Muse O-1 |
| D5 | Phase authority: owner decision ≠ agent contribution; future runtime = dedicated operation with authority checks, never comment interpretation | GPT O5; Codex §4 |
| D6 | Pagination/continuation: follow returned nextOffset + revision; never hard-code page size (observed client divergence, PR #14 R1) | Codex corr.5; Cline OBJ-04 |
| D7 | Metrics out of canonical state (phase summaries / registry only); single taxonomy pass|fail|not_tested × real|simulation|inspection | Cline B.6; GPT §7 |
| D8 | Scaffolding: check-only diagnostic first, then explicitly authorized additive creation; adopt compatible existing files | Vibe G-2; Codex §4; GPT O7 |
| D9 | Coordinated writes: operation_id + receipt + expectedSha/revision + pending/reconcile states; partial success never auto-republished | Codex §2; GPT O6 |
| D10 | Client profile = UX/context optimization only, never a permission boundary; server-side enforcement separate | Muse O-4; GPT §8 |
| D11 | Interface: contracts-first, composition-first; thin context/exchange module evaluated by measured comparison before any public API freeze | GPT §5; Cline B.2; Muse O-3 |
| ⚖ A1 | Installation access to rfkevin/github-mcp (404 Grok/Cline; successful reads GPT/Vibe) — grant, or scope this cycle docs-only | Cline OBJ-02 — blocking for lots B-E |
| ⚖ A2 | Assembler (private votes in Kevin's chat) + role matrix (proposal in §4) | Muse §4; GPT §8 |
| ⚖ A3 | Freeze gate: lots B/C/D start only after owner validates lot A contracts version | Muse D-1 — recommended adopt |

## 2. Lots
| Lot | Deliverable | Depends | Owner gate |
| --- | --- | --- | --- |
| A | Contracts: canonical state schema (deterministic sections), phase authority ops, read-coverage disclosure line in contribution template, partial-outcome/recovery contract, taxonomy alignment, continuation rules | — | freeze gate ⚖A3; includes Muse D-2 deltas (template + taxonomy in A) |
| B | Context routing (short instruction → state → phase rules → targeted sources) + bootstrap check-only/additive | A frozen | needs ⚖A1 |
| C | Exchange: coordinated writes, receipts, concurrency, phase enforcement | A frozen; parallel B | needs ⚖A1 |
| D | Integration guide (G-1) + opt-in client profile (G-3 lessons → github-mcp AGENT_MEMORY) | A; after B/C | needs ⚖A1 |
| E | Independent multi-client trials on both repos; measured comparison vs manual V1 path (calls, arg/response bytes, latency, errors, retries, duplicates, human interventions, completeness) | after A-D | real vs simulation distinguished |

## 3. Acceptance criteria (candidate set for lot E)
Join from short instruction (≤10 words) with ≤3-4 targeted calls; P1 peer exclusion verified by disclosure; authorized phase transition; interruption/ambiguous-write resume without duplicate effects; concurrent state updates without loss; long/edited comments read losslessly; partial batch explicit; append-only journals protected; legacy tool behavior unchanged. not_tested stays visible; no fabricated CI state.

## 4. Role matrix (proposal — Kevin arbitrates ⚖A2; author ≠ reviewer ≠ tester per lot, V10)
| Lot | Author | Reviewer | Tester |
| --- | --- | --- | --- |
| A | Codex | Vibe GLM | Grok |
| B | Cline | Grok | Vibe GLM |
| C | GPT-5.6 Sol | Codex | Cline |
| D | Vibe GLM | GPT-5.6 Sol | Muse Spark |
| E | Muse Spark (coordinator) | Grok | GPT-5.6 Sol |
Availability stated by Cline, Muse Spark, GPT-5.6 Sol; Grok/Codex/Vibe by participation history; no availability inferred; each assignee explicitly accepts or declines at assignment. Access to github-mcp verified per assignee before any connector lot (⚖A1).

## 5. Process rules carried over (V1, unchanged)
Owner-only phase transitions and merges; silence ≠ validation; declaredAgent is attribution, not authority; private votes in coordinator chat only; append-only AGENT_MEMORY; central improvement registry in github-mcp (already has PR #35 pending for comment edit/delete); reading records with unread:[] before verdicts; expectedSha/expectedHeadSha on every write; reread before replay after uncertain results.

## 6. Open items recorded for later cycles
Technical P1 isolation (visibility design); comment edit/delete tools (IMP PR #35); automatic agent activation; structured state adapter; token measurement with compatible tokenizer.
