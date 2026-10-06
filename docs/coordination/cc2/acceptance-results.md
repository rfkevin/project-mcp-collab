# CC-2 — L7 closure and G5 gate (acceptance results)

`board: #16 | phase: P6 | lot: L7 closure / G5 | author: Claude (substitute: L7 author/coordinator Muse Spark absent; owner order 2026-10-06) | reviewer: GPT-5.6 Sol (expected) | tester: Grok (expected) | code: github-mcp cc2-integration@0a2063a7 (tree == PR #43 head 284074d, CI 4/4) | master: cd8089ae (unchanged) | date: 2026-10-06`

## Résumé (français)

- **G5 n'est pas franchi pour l'acceptation opérationnelle.** Le code CC-2 (L1–L6) est intégré sur `cc2-integration` et vert en CI. Les revues sont consignées pour chaque lot ; un test indépendant est consigné pour L0, L1, L2, L5, L6 et L7, mais **aucun pour L3 ni L4** dans les sources relues ; l'essai L7 sur portalshall est fusionné (revue Sol agree, test Grok pass). Mais les parcours outillés critiques (rejoindre, reprendre, amorcer, mémoire) n'ont jamais été exécutés par un client réel, parce que le serveur MCP déployé n'expose pas les nouveaux outils.
- **Deux blocages concrets** : (1) aucun état canonique CC-2 n'existe — `WORKFLOW_STATE.md` reste CC-1 / P5 rév. 4, donc l'outil de contexte annoncerait le mauvais cycle ; (2) aucun déploiement candidat du code CC-2 n'a été testé par un client.
- **Ce qui est prouvé** : amorçage additif sur un vrai dépôt (prévisualisation `ready`, application, second passage `unchanged`, empreintes SHA-256 vérifiées machine), reprise par checkpoint, remplacement d'agents absents avec rôles distincts, renouvellement des revues après changement de head.
- Décision suivante : G6 appartient à Kevin (voir « Next actions »). Rien n'est fusionné vers main/master ni déployé par cette clôture.

## Lot status (evidence at exact heads)

| Lot | PR | Head merged | Review (verdict at head) | Independent test | State |
| --- | --- | --- | --- | --- | --- |
| L0 baseline | collab #17 | 1ded98d | Sol | Cline pass (#16 5995655413) | merged main |
| L1 contracts | github-mcp #36 | 5202b01 | Vibe `changes_requested` (5995359059) → Vibe applied corrections as owner-mandated corrector (5995511758); re-verification by Grok substituting Codex (5995802561) | Grok `pass_with_limits` (5995802561; local suite not_tested, CI inspected) | merged staging |
| L2 context | #37 | 669fc6c | Sol `agree` substituting Codex (5999597321) after `changes_requested` (5999395502) | Grok `pass_with_limits` substituting Muse Spark (5999658768) | merged staging |
| L3 bootstrap | #42 | 0355d96 | Grok `agree` (#16 6010609825), Vibe `agree` (#16 6010599520) | none recorded in sources read; CI 4/4 + this closure's replay (simulation) | merged staging |
| L4 receipts | #38 | 6f14eb0 | Vibe `agree` substituting Codex (5999981193) | **none found**: Cline test still pending at #16 6002191114; no later verdict in #38 or #16 | merged staging |
| L5 memory | #40 | d50062e | Sol `agree` (6003225061) | Vibe pass (#16 6010404481) | merged staging |
| L6 guide | #43 | 284074d | Sol `agree` (reported by Grok #16 6010977933; not re-read) | Grok pass (#16 6010977933) | merged staging |
| L7 trial (app side) | portalshall #3 | fef60d9 | Sol `agree` (reported by Grok; not re-read) | Grok pass (portalshall #3 6012556685; echoed #16 6012557277) | merged portalshall master 8e6b163 |

Comment ids without a prefix are on the lot's own PR. Verdicts for L1, L2, L4 and the L7 test were re-read for this revision (Sol F2 on #18); the two Sol verdicts marked « not re-read » are cited from the tester's report.

Staging is 31 commits ahead of master `cd8089ae`; promotion is a human decision (G6).

## Scenario results (current, supersedes no earlier CC-2 table)

Legend: pass / partial / fail / not_tested × real / simulation / inspection. `sim-real-data` = merged code executed locally on real repository content ([evidence](evidence/2026-10-06-g5-local-replay.md)); never a deployed tool call.

| ID | Result | Mode | Evidence | Gap to close |
| --- | --- | --- | --- | --- |
| CC2-01 join | partial | real (legacy tools) + sim-real-data | portalshall trial §CC2-01 (6 calls); replay: context returns CC-1/P5 | CC-2 state + deployed `github_collab_context` |
| CC2-02 phase boundary | partial | real (legacy) | portalshall §CC2-02; P1 exclusion unit-tested (L2) | tool-path in P1 not run |
| CC2-03 cross-chat resume | pass | real (legacy, no cursor) + sim-real-data | portalshall §CC2-03; replay: 515 B checkpoint, `resumed=true`, nothing to reread | second client |
| CC2-04 long/edited discussion | partial | real (legacy) | revision continuation on #13/#16 (L0 S2/S4, L7) | L2 edited-source detection via tool |
| CC2-05 additive bootstrap | pass | real application + sim-real-data | portalshall `ready` → apply a8921c6 → `unchanged`; SHA-256 = pins; github-mcp/collab `ready` (registry only, existing files kept) | tool-path call |
| CC2-06 stale head | pass | real (precheck) | portalshall base re-read; every apply in this cycle used `expectedHeadSha` | — |
| CC2-07 lost write confirmation | not_tested | — (unit tests only, L4) | — | inject on candidate deployment |
| CC2-08 partial compound | partial | real (legacy atomic batch) | `github_apply_changes` atomic commits; L4 receipts unit-tested | receipts via tool |
| CC2-09 concurrent votes | not_tested | — | — | second live agent + vote round |
| CC2-10 memory through phases | not_tested | — (L5 unit tests) | candidates nominated below, no ballot opened | open electorate, publish |
| CC2-11 memory corrections | not_tested | — (L5 unit tests) | — | same |
| CC2-12 permission/catalogue | partial | real + inspection | read 24 tools / full 40 tools measured (L6 profile-decision); deployed catalogue lacks collab tools | deployed catalogue check |
| CC2-13 owner decision / state lag | **fail** | real observation + sim-real-data | work proceeded on direct owner orders (no blocking) but canonical state never transcribed CC-2: context would show CC-1/P5 without `sync_pending` | publish CC-2 state (owner) |
| CC2-14 agent unavailable | pass | real | Cline absent → Claude authored L3/L6 (distinct reviewers/testers); Muse Spark absent → Grok tested L6, Cline authored L7; no approval fabricated | — |
| CC2-15 efficiency | data only | real | L0 S1 join 15 calls; L7 join 6 calls; L7 total 34 calls, 0 blind retry, 0 duplicate | not comparable (different inputs, L0 §3 rule); no speed/token claim |
| CC2-16 conflict / review renewal | pass | real | L5 #39 append-only conflict → clean rebase #40; L3 #41 (master) → #42 (staging); Grok `changes_requested` at 51489ff renewed `agree` at 0355d96; CI re-run per head | — |
| CC2-17 final memory collection | partial | inspection | inventory below; no electorate opened | L5 vote + publication |
| CC2-18 real user route | partial | real | owner launched lots with short prompts; agents joined #16 and published handoffs; many owner interventions (merges, reassignments) | two participants on one task via tool-path |

Critical cases still lacking real tool-path evidence (C4 rule): join, resume, bootstrap, publish, memory. **Product not marked complete.**

## Findings

| ID | Severity | Finding | Owner | Next action |
| --- | --- | --- | --- | --- |
| G5-F1 | blocking | No CC-2 canonical `WORKFLOW_STATE.md` anywhere; main is CC-1 rev 4 P5 | Kevin (decision), Codex/L1 (draft) | Publish CC-STATE-1 successor for CC-2 (phase P6) on a task branch; keep CC-1 history; merge = Kevin |
| G5-F2 | blocking | CC-2 code never deployed to a client: every collab tool path is `not_tested` | Kevin | Connect one client to a preview deployment of `cc2-integration@0a2063a7` (owner's existing preview process), rerun CC2-01/03/05/07/10 |
| G5-F3 | minor | Context sources keep Markdown link text (`[label](path)`) as locations | Vibe GLM (L2) | Extract link target; add a fixture with linked refs |
| G5-F4 | info | CI on the staging merge commit runs Workers Builds only; full checks ran on PR heads (tree identical, verified) | — | Run full CI on the promotion PR to master |
| G5-F5 | info | Second client / second participant never observed | L7 tester pool | Required before final operational acceptance |
| G5-F6 | significant | L4 merged without a recorded independent test; L3 likewise (covered here only by simulation); L1 review re-verification and test were done by the same agent (D12 distinct roles not met) | Kevin, testers | Run the missing L3/L4 tests at the staging head, or record an explicit owner waiver |

## CC2-17 — memory candidates (nominated, not voted)

L5 policy: no candidate is promoted without an electorate and quorum. Electorate not opened in this cycle → every item is `nominated`, publication `pending`.

| ID | Scope | Status | Statement | Source |
| --- | --- | --- | --- | --- |
| CC2-M01 | global_usage | verified (2 obs) | Re-enumerate the catalogue after a tool-not-found error; never hard-code tool names in guidance | L0 cand-01 |
| CC2-M02 | mcp_internal | verified (2 obs) | Stored comment bodies can carry a duplicated header line; compare readback on content, not header | L0 S5, Cline 5995655413 |
| CC2-M03 | global_usage | verified | Append-only shared files (AGENT_MEMORY, code-map) conflict at the second merged lot; rebase from fresh staging, append only | Vibe 6010404481 |
| CC2-M04 | global_usage | verified | Code merged on staging is not a deployed tool; mark tool-path scenarios `not_tested-unavailable-in-deployed-MCP` | L7 trial F2 |
| CC2-M05 | global_usage | verified | A hand replay is not a tool call; running the merged module on real refs gives machine-verified evidence | Sol F1 + this closure |
| CC2-M06 | mcp_internal | verified | `github_apply_changes`: message ≤ 200 chars including the server trace; `restore` cannot create a new path (`expectedSha:null` rejected) | Claude, L3 |
| CC2-M07 | project | verified | When a new cycle opens, transcribe it in the canonical state before relying on the context tool | G5-F1 |
| CC2-M08 | mcp_internal | hypothesis | Vite `import.meta.glob` excludes the calling file; list it explicitly in path-existence tests | Claude, L6 |

## Next actions (G6, owner)

1. Decide G5-F1: authorize a CC-2 state successor (or record that CC-2 stays tracked on #16 only, and accept that the context tool is not usable for CC-2).
2. Decide G5-F2: preview deployment of `cc2-integration@0a2063a7`, then one real rerun of the critical scenarios by two clients.
3. Open the L5 electorate for CC2-M01…M08 (or defer explicitly).
4. Promotion staging → master and production: Kevin only, after 1–2.

`handoff: L7-closure/G5 | status=gate_not_passed(operational) code_level=pass | evidence=this file + evidence/2026-10-06-g5-local-replay.md | read=#16 index 45 items (new since 06:20 read in full), portalshall acceptance+trial (full), L0 baseline (full) | tests=local replay:pass:simulation, staging CI:4/4 on identical tree | objections=G5-F1,G5-F2 blocking; G5-F6 significant | next=Sol review + Grok test at exact head; owner decisions 1–4`
