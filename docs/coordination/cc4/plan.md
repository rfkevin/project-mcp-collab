# CC-PLAN-4/v0.1 — API integration, then workflow automation

status: P3 assembled proposal — **not owner-approved**
cycle: `cc4-design`
framing: rfkevin/project-mcp-collab#41
sources: sealed proposals seq 91 (Vibe GLM), 92 (Grok), 93 (Claude), 95 (GPT-6 canonical); P2 evidence seq 99–106
assembler: ChatGPT GPT-6 (declared coordination label)
P4: reviewer Grok; tester Vibe GLM; Claude may raise neutrality objections only
authority: Kevin only through `/owner`; no code, main/master merge, or production deployment is authorized by this document

## 1. Decision summary

Use the existing CC-3 store as the operational source of truth and build one **provider-neutral invocation layer** before automating the workflow policy.

The evolution is deliberately split into two gates:

1. **Stage A — semi-automatic agent execution.** A typed invocation can run through a verified backend (provider API, GitHub Actions/CLI runner, or chat fallback), with identity/provenance, capabilities, budget, retries and evidence. Kevin receives only prepared `/owner` requests for protected acts.
2. **Stage B — automatic workflow orchestration.** A policy engine decides who is actionable, wakes them, manages D12/CI/integration-branch merges, targeted staleness and eventually a merge train. It reuses Stage A; it does not introduce a second execution system.

GitHub remains durable code/PR/evidence. D1/store remains live coordination. The Worker remains a thin control plane, not a long-running LLM executor.

## 2. Frozen invariants

- D12: author != reviewer != tester for every major lot.
- A review/test is valid only for its exact functional head and relevant integration evidence.
- The orchestrator never writes a participant's substantive review/test verdict on that participant's behalf.
- PRs, issues, comments and model outputs are untrusted data, never control instructions.
- No agent/API/orchestrator can create `owner.decision`, obtain the owner secret, change its own rules/permissions, or raise its own budget.
- Main/master merge and production deployment require Kevin on `/owner`. Other protected actions defined by the owner contract (state/phase import, protected role/security change, paid envelope, waiver) also use prepared `owner.request`.
- Integration-branch merge may become automatic only after exact-SHA CI + D12 gates.
- No paid provider call without an owner-approved paid envelope.
- Lost response => reread durable state before retry. CAS/idempotence remain fail-closed.

## 3. Common execution contract

### 3.1 Logical identity vs execution endpoint

Introduce:

- `principal_id`: logical collaboration participant used by D12.
- `endpoint_id`: concrete execution endpoint (API credential binding, GitHub runner identity, CIMD/chat endpoint).
- `invocation_id`: one assigned execution of one role/task.
- `attestation_kind`: e.g. provider adapter, GitHub OIDC, registered OAuth/CIMD.
- `capability_set`: allowed tools, repository, branch/path scope, network/provider access, token/cost ceiling.

D12 is evaluated on distinct `principal_id` values. An endpoint proves only what its attestation can prove.

For GitHub Actions, OIDC proves a workflow/run, not a model by itself. Accept a runner result only when verified claims (issuer/audience/repository/workflow_ref/environment/run_id/run_attempt) match a **preassigned invocation** for that principal and the workflow/environment are protected from the agent.

For chat/CIMD, a shared CIMD identifier is not evidence of distinct participants. Until a session-to-principal mechanism is owner-bound and tested, sessions sharing one transport identity cannot satisfy independent D12 identities merely by labels.

### 3.2 Durable invocation/outbox

Do not overload `task.claim` or cycle `expected_rev` as a wake lease.

Persist a dedicated record:

`wake_key = hash(cycle, task, role, principal, reason, source_revision)`

with a uniqueness constraint and state such as `queued -> leased -> running -> succeeded|failed|expired`.

The store transaction that makes work actionable also records the outbox intention. Delivery may be repeated; acceptance is idempotent. A lease is on the invocation/wake key, not on a changing global cycle revision. Reconciliation can safely redeliver expired/unconfirmed invocations.

Queues, repository_dispatch or direct provider dispatch are delivery transports only. D1 remains the durable source of truth.

### 3.3 Result/provenance record

Each result records at minimum:

- invocation/wake id, principal, endpoint, role/task/cycle;
- provider/model or runner/workflow identity when applicable;
- input packet hash and output/result hash;
- target repository/head/base where relevant;
- tool calls or action receipts needed for audit;
- retry/attempt number, timestamps and terminal status;
- measured token/minute/operation usage and cost;
- evidence references.

Do not archive unlimited raw prompts by default. Preserve hashes, bounded packets and the minimum replay/audit material; durable GitHub artifacts hold code and formal evidence.

## 4. Executor backends

One broker contract supports multiple executors; no backend becomes the architecture.

| Backend | Best use | Identity/provenance | Initial policy |
| --- | --- | --- | --- |
| Provider API | reasoning, review, structured outputs, non-repo work | adapter-held credential + provider request metadata + preassigned invocation | enable only after primary-doc + live capability probe |
| GitHub Actions + agent CLI | checkout, coding, tests, builds, repo-bound agent work | protected workflow/environment + GitHub OIDC bound to invocation | preferred executor for repo-heavy work when verified |
| Chat/CIMD | hybrid participation when no headless endpoint is available | registered endpoint/session rules; labels never enough | fallback; produce ready-to-paste wake instruction and measure human relay |

Claude's subscription-backed runner path is a valid candidate, not a universal assumption. P2 verified that upstream `claude-code-action` supports `ANTHROPIC_API_KEY` or `CLAUDE_CODE_OAUTH_TOKEN` generated for eligible Pro/Max usage. Equivalent API/CLI/auth/cost claims for every other participant must be verified from primary sources before activation.

### 4.1 Participant capability matrix — required before real dispatch

The system must maintain a record per participant:

| Participant | Chat endpoint | Provider API | CLI/Actions | Auth | Cost/quota | Provenance | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Claude | known chat participant | optional; not assumed | Claude runner candidate verified at documentation level | protected runner secret/token + OIDC | subscription/API limits require live probe | OIDC + invocation binding | probe required |
| GPT-6 declared participant | known chat participant | do not assume chat model == API model | unverified | unverified | unverified | must be endpoint-specific | probe required |
| Grok | known chat participant | unverified in this cycle from primary source | unverified | unverified | unverified | must be endpoint-specific | probe required |
| Vibe GLM | known chat participant | unverified in this cycle from primary source | non-interactive mode unverified | unverified | unverified | must be endpoint-specific | probe required |

No row becomes `active` from a secondary pricing blog or model self-report.

## 5. Control plane and wake/retry

Initial control plane:

- existing Cloudflare Worker: authorization, lightweight policy, adapters/dispatch entrypoints;
- D1: tasks, events, invocation/outbox, budget ledger, owner-request state;
- one periodic reconciliation Cron: expired leases, unconfirmed dispatches, dead-letter escalation;
- Queues: optional delivery accelerator once measured useful; never the journal;
- GitHub Actions: repo executor and CI, not the central coordinator;
- Workflows: optional later for durable long waits after benchmark, not a Stage A dependency.

Verified P2 infrastructure facts, checked against primary documentation:
- Workers Free: 100,000 requests/day, 10 ms CPU/request, 50 external subrequests/request, 5 Cron Triggers/account.
- Queues Free: 10,000 operations/day; Free retention is 24 h.
- Workflows Free: current included quota includes 3,000 steps/day and 1 GB-month. The old “billing not before 2026-08-10” wording is historical and is not used as a planning fact.
- GitHub Actions: standard runners are free for public repositories; GitHub Free includes 2,000 minutes/month for private-repository usage.

Sources:
- https://developers.cloudflare.com/workers/platform/limits/
- https://developers.cloudflare.com/changelog/post/2026-02-04-queues-free-plan/
- https://developers.cloudflare.com/workflows/reference/pricing/
- https://docs.github.com/en/billing/concepts/product-billing/github-actions

Provider/model prices are **not frozen in this plan**. Seq 104 showed that the OpenAI names/prices in seq 95 were incorrect. Every paid backend gets an activation-time price-sheet snapshot from the provider's primary documentation and a live cost probe.

## 6. Security model

1. External/repository/model text is placed in a data envelope. Immutable control instructions and capability policy come from protected server configuration.
2. Each invocation receives deny-by-default tool permissions. Repo, branch, paths, mutations, provider/network access and spending are explicit capabilities.
3. Provider credentials remain in isolated server/runner secret stores; never prompts, memory or collaboration events.
4. The owner secret is only entered by Kevin on `/owner`; no agent tool can read it.
5. A global `halt` on `/owner` prevents new invocations and mutating automated actions. Running executors check it before a write; leases expire safely.
6. Protected workflow/policy/budget configuration cannot be changed by the participant whose behavior it governs.
7. The dispatcher cannot synthesize reviewer/tester evidence. It only assigns invocations and records transport/provenance.
8. Retries are idempotent. A different payload under the same operation/wake identity is a conflict, not a retry.

## 7. Budget and observability

Measure from the first executable lot:

- input/cache/output tokens by provider/model;
- API cost reserved and actual;
- Actions minutes;
- Worker/Queue/Workflow usage;
- retries/timeouts/failures;
- latency per stage;
- number of Kevin interventions and manual chat relays.

Budget algorithm:
1. Free-capable path preferred when it satisfies the same acceptance contract.
2. Before a paid invocation, atomically reserve its worst-case configured cost.
3. If the remaining approved envelope is insufficient, fail closed and prepare an owner request; do not silently switch to paid fallback.
4. Release unused reserve after measured usage.
5. `/owner` shows spent/reserved/remaining per mission and globally; alerts at configurable warning thresholds and hard stop at 100%.

The reported target is Free first and roughly EUR 50 initially, but an exact paid envelope is effective only after Kevin approves the prepared budget request.

## 8. Prepared owner actions

Kevin must never prepare or paste JSON.

An owner request contains:

- `request_id`, action type and canonical payload;
- `payload_sha256`;
- exact target and preconditions (head/base/revision/config version);
- why it is requested;
- risk and expected effect;
- rollback/recovery procedure;
- maximum cost if relevant;
- expiry/revalidation rule.

`/owner` displays the request, budget and history and offers Approve/Refuse. On Approve the owner route:

1. recomputes the payload hash;
2. rechecks all preconditions;
3. aborts with no protected effect on any mismatch;
4. executes exactly that payload;
5. records the owner decision and result atomically or with an auditable operation receipt.

Refusal, timeout or silence leaves protected state unchanged. Reminders may be grouped; there is never approval by expiration.

Protected requests include, according to the owner contract: state/phase installation/import, protected role/security changes, paid budget envelope/waivers, main/master merge and production deployment. Automatic integration-branch merges are outside this protected set only after their D12/CI policy is owner-approved.

## 9. Delivery plan

All assignments below are **proposed**, not accepted authority until Kevin validates the plan. Each lot requires exact-head CI/evidence and distinct author/reviewer/tester.

### Stage A — semi-automatic execution

| Lot | Scope / exit test | Proposed author | Reviewer | Tester |
| --- | --- | --- | --- | --- |
| A0 hardening | strict op_id grammar; typed STALE exposes currentRevision; cycle discovery; principal/endpoint/session contract; one-output-per-required-phase guard | Vibe GLM | GPT-6 | Grok |
| A1 invocation core | invocation/outbox schema; UNIQUE wake_key; leases/retry/reconciliation; simulated executor; provenance + usage ledger | GPT-6 | Claude | Vibe GLM |
| A2 owner/budget gate | executable exact-hash owner requests; owner UI with no copy/paste; protected-action preconditions; budget envelope UI/contract; kill switch. Invocation reserve/settle hooks are integrated with A1 before A3 exit | Claude | Grok | Vibe GLM |
| A3 executor adapters | capability matrix; at least one verified **Free API** adapter and one verified Actions/CLI adapter; endpoint attestation; least-privilege tools. No paid provider is required for this lot | Grok | Vibe GLM | Claude |
| A4 semi-auto trial | one real mission: dispatch -> author result -> independent review/test -> prepared owner final action; chat fallback measured; no automatic main/prod | Vibe GLM | GPT-6 | Grok |

**Dependency order:** A0 -> **A1 and A2 in parallel** -> A3 -> A4. The exact-hash owner-request executor does not depend on the invocation registry and is intentionally delivered as soon as A0 hardening establishes the authority/action contracts. A1 and A2 integrate their budget/invocation hooks before A3.

**Stage A exit:** a task can be executed through verified headless endpoints with bounded context, D12 provenance, hard budget, retries and prepared owner requests. Kevin is not relaying normal work between automated participants. No general next-action policy or merge-train automation is required yet.

### Stage B — workflow automation

| Lot | Scope / exit test | Proposed author | Reviewer | Tester |
| --- | --- | --- | --- | --- |
| B1 policy/wake | versioned `next_actors(state,event)`; DAG/dependencies; automatic wake/retry/fallback; no duplicate work under concurrent events | Claude | GPT-6 | Vibe GLM |
| B2 gates/integration | exact-SHA CI + D12 evidence gate; automatic merge to non-protected integration branch; fail closed on stale/missing proof | Grok | Claude | Vibe GLM |
| B3 targeted staleness / merge train | separate functional-head evidence from integration-SHA evidence; conflicts classified; invalidate only affected proof | GPT-6 | Grok | Vibe GLM |
| B4 full trial/audit | full P1->review/test->integration flow; crash/retry/STALE/budget/halt scenarios; independent global audit; owner receives final prepared request only | Claude | Grok | Vibe GLM |

**Stage B exit:** the workflow advances without Kevin relaying instructions; only protected owner decisions stop at `/owner`. The system can reconstruct why every automatic action was allowed.

### Debt deferred unless it blocks a gate

- cosmetic Evidence `state` rendering;
- full prose overlay in `collab_export` once bootstrap no longer depends on stale inherited fields;
- advanced memory semantic search;
- optional Workflows/DO optimization;
- noncritical `run_checks/CONTROLLER_CHANGED` improvements.

The CC-3 export inheritance defect must be fixed earlier if an automated bootstrap/trial demonstrates that it can regenerate stale authority/next-action fields.

## 10. Decisions for Kevin

### K-CC4-1 — first real executor strategy

This is the only unresolved architectural preference that materially changes Stage A ordering.

| Option | Meaning | Initial cost / Free compliance / prerequisite | Participant position |
| --- | --- | --- | --- |
| A — API-first | first real adapter is a provider API; Actions/CLI follows | **Free-compatible for the pilot:** use a currently verified free API/model. Prerequisite: live capability/tool/provenance probe. Initial paid envelope: **0** | GPT-6: prefer; Grok: prefer |
| B — subscription runner-first | first real adapter is Actions/CLI using an already-paid subscription where permitted | **Free/no new API billing for the pilot** if the reported Claude Pro path remains permitted and available. Prerequisite: protected runner/OIDC + live usage probe. Initial paid envelope: **0** | Claude: prefer |
| C — dual capability pilot | keep common broker neutral and require one API adapter + one Actions/CLI adapter before selecting a default | **Free-compatible for the pilot:** one verified free API endpoint + one verified subscription-backed runner, both within included quotas. Prerequisite: both probes. Initial paid envelope: **0** | Vibe: closest to proposal; GPT-6/Grok accept hybrid executors; Claude's runner remains represented |

**Assembler recommendation: C.** It adds one bounded adapter probe but prevents the architecture from being locked to a single provider/channel and directly answers the largest P2 uncertainty. The later default is chosen by measured cost, capability, provenance and reliability, not by assumption.

Kevin's reported “about EUR 50” is a later maximum target, **not a pilot spend**. If a required capability cannot be proven on the Free/subscription-included paths, automation stops and prepares a separate paid-budget owner request before any charge.

Prepared owner request after P4 should contain: selected option, exact Stage A scope/roles, initial paid envelope **0** for the pilot unless Kevin explicitly chooses otherwise, risks and rollback. Approval of this plan does **not** authorize production.

### Resolved in synthesis — no separate Kevin choice needed

- **Identity:** combine principal/endpoint separation with endpoint-specific attestation. OAuth/CIMD and GitHub OIDC are complementary transports, not rival identity models.
- **Queue vs Cron:** D1 outbox is authoritative; one reconciliation Cron is mandatory. Queue is an optional accelerator introduced only when benchmarked useful.
- **Merge train:** defer to B3; do not make it a prerequisite for proving Stage A execution.

## 11. P2 objections — disposition

| Objection | Resolution |
| --- | --- |
| V1: Vibe lacked per-participant execution/cost mapping | required capability matrix in §4.1; headless subscription runner is evaluated |
| V2: task+expected_rev lease permits duplicate work | replaced by dedicated invocation/outbox + UNIQUE wake_key lease |
| V3: runner authentication unspecified | endpoint attestation contract; protected GitHub OIDC path specified |
| V4: Workflows billing date/limits stale | historical date removed; current primary-doc quota only |
| C1: Actions-first only proven for Claude | Actions/CLI is one backend; no universal default; K-CC4-1 records disagreement |
| C2: task.claim is not an invocation lease | replaced by dedicated invocation registry |
| C3: OIDC proves workflow, not model | explicitly limited; claims must bind to preassigned principal/invocation under protected workflow |
| C4: “zero marginal” subscription wording / secondary sources | changed to no separate API charge only when verified; primary sources + live probe required |
| Grok: GPT-6 seq 95 vs 96 | seq 95 is canonical; seq 96 withdrawn |
| Grok: GPT-6 cost example weak/incorrect | provider price example removed; activation-time primary price sheet required |
| Grok: do not reintroduce Actions as coordinator | Worker/store remains control plane; Actions is executor only |
| Vibe: Grok Actions public/private quota wrong | corrected in §5 |
| Vibe: Grok CIMD detection underspecified | principal/endpoint/attestation contract and shared-CIMD restriction in §3.1 |
| Vibe: Grok cost range unsourced | removed as canonical planning fact |
| Vibe: Grok soft-lock ad hoc | replaced by durable invocation CAS/UNIQUE lease |

No P2 objection is blocking after these dispositions; P4 must verify the implementation plan actually preserves them.

## 12. Fact corrections applied

- GitHub Actions: public standard runners are free; 2,000 minutes/month is the GitHub Free private-repository quota.
- The OpenAI model names/prices from GPT-6 seq 95 are not carried forward; seq 104 found them wrong. Provider costs are verified from primary pricing at activation.
- The Workflows “billing not before 2026-08-10” sentence is obsolete historical context and removed.
- Secondary pricing/capability sources for xAI/GLM/Codex are not accepted as canonical evidence. They become primary-source + live-spike questions in A3.
- A “5 Cron” limit means five Cron Triggers/account, not a five-minute period.

## 13. Traceability matrix

Legend: **R** retained, **E** excluded/replaced, **K** decision for Kevin.

| Seq | Q | Proposal element | Disposition |
| --- | --- | --- | --- |
| 91 Vibe | Q1 | API + Actions + chat hybrid | R — §4; contributes to K-CC4-1 option C |
| 91 | Q2 | webhook/store -> queue; deterministic wake id | R — store/outbox + deterministic wake; transport generalized |
| 91 | Q2 | lease on task + expected_rev | E — replaced by invocation lease (§3.2), V2 |
| 91 | Q2 | chat ready-to-paste fallback | R — §4/A4 |
| 91 | Q3 | Worker + Cron + Queues + Workflows | R partly — Worker/D1/Cron; Queue optional; Workflows later |
| 91 | Q4 | one OAuth client per agent; orchestrator has no task role | R conceptually — endpoint registry + orchestrator non-substantive |
| 91 | Q5 | untrusted content, role allowlists, secrets, kill switch | R — §6 |
| 91 | Q6 | Free first, measure cost, EUR50 cap/alerts | R — §7; exact paid envelope remains owner-approved |
| 91 | Q7 | hash-bound prepared owner requests | R — §8 |
| 91 | Q8 | N0 debt then orchestrator/owner/budget/chat/trial | R restructured — A0–A4 |
| 91 | Q9 | probe chat APIs, Workflows, webhook reliability, cost, OAuth | R — capability probes/A3/A4 |
| 92 Grok | Q1 | API-first + Actions CI + optional CLI/cloud | R as backend set; API-first preference -> K-CC4-1 A |
| 92 | Q2 | store append wake + op_id + soft lock | R wake concept; E soft lock -> invocation lease |
| 92 | Q3 | Worker/D1/Cron/Queue + Actions; no DO lot1 | R — §5 |
| 92 | Q4 | server participant id, exact SHA, owner-approved client mapping | R — §3; shared CIMD tightened |
| 92 | Q5 | data-not-instruction, allowlist, halt | R — §6 |
| 92 | Q6 | rough EUR cost range | E — unsourced; use measured ledger/primary prices |
| 92 | Q7 | owner queue + exact hash/CAS | R — §8 |
| 92 | Q8 | wake, owner UI, auto integration, API, merge-train later | R restructured — A/B lots |
| 92 | Q9 | chat unknown, CPU/race/budget risks | R — A3/A4/B trials |
| 93 Claude | Q1 | Actions/CLI subscription-first; APIs complement; chat fallback | R as backend set; first-backend preference -> K-CC4-1 B |
| 93 | Q2 | pure next_actors + repository_dispatch + Actions concurrency + Cron | R in B1/control plane; E task.claim as lease |
| 93 | Q3 | Worker decisions, Actions long work, Workflows later | R — §5 |
| 93 | Q4 | GitHub OIDC runner provenance + session mapping | R with limitation — §3.1 |
| 93 | Q5 | protected workflows, minimal token permissions, per-env secrets, halt | R — §6 |
| 93 | Q6 | subscription marginal-cost strategy + model/provider estimates | R strategy only; E unverified/secondary price claims |
| 93 | Q7 | prepared owner request + budget/history/halt | R — §8 |
| 93 | Q8 | debt lot, runner pilot, D12 runners, budget/API/chat, trial | R restructured — A0–A4 |
| 93 | Q9 | verify Codex/GLM runner, subscription limits, OIDC mapping | R — A3 capability matrix |
| 95 GPT-6 | thesis | Stage A execution then Stage B policy, contracts shared | R — plan structure |
| 95 | Q1 | provider-neutral Invocation Broker; Actions specialized; chat fallback | R — §§3–4; API-first preference -> K-CC4-1 A |
| 95 | Q2 | transactional outbox + UNIQUE wake_key + CAS invocation | R — §3.2 |
| 95 | Q3 | Worker/D1/Queues; Workflows benchmark later | R with Queue optional — §5 |
| 95 | Q4 | principal_id vs endpoint_id; invocation provenance | R — §3.1 |
| 95 | Q5 | immutable control, capability manifests, isolated secrets, halt | R — §6 |
| 95 | Q6 | cost ledger/reservation; specific OpenAI price example | R ledger/reservation; E price example after seq104 correction |
| 95 | Q7 | exact-hash prepared owner actions | R — §8 |
| 95 | Q8 | A0–A5 then B1–B4 | R, consolidated to A0–A4/B1–B4 |
| 95 | Q9 | provider variance, Workflows CPU, private Actions quota, account compromise, chat wake | R — A3/A4 and security |

Every Q1–Q9 element of the four canonical P1 proposals is represented above. Seq 96 is intentionally absent because GPT-6 withdrew it in P2.

## 14. P4 acceptance checklist

Grok review and Vibe test at the exact PR head/base must verify:

1. traceability table covers seq 91/92/93/95 with no silent loss;
2. all P2 objections in §11 are actually resolved by the plan;
3. only primary-source external facts are treated as facts;
4. Stage A can run on Free infrastructure except provider/runner usage explicitly gated by capability/budget;
5. D12 identity/provenance cannot be forged by the orchestrator;
6. double delivery cannot create two accepted invocations/results;
7. owner approval executes exactly the presented payload or nothing;
8. no main/master or production action occurs without owner approval;
9. Stage A and B are separable gates but share one execution contract;
10. rollback/halt/budget fail closed.

After P4, Kevin receives one prepared `/owner` validation request for the plan and K-CC4-1. No implementation starts before that approval.
