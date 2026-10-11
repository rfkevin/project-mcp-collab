# C7 — Journal de l'essai réel (cycle cc3-c7-trial)

Source unique : journal du store Collaboration 3 (connecteur collaboration_3_22f6), cycle `cc3-c7-trial`, consolidé en P2 à la révision 28. Le numérotage `seq` est global au store : les séquences d'autres cycles (49–52 : cycle cc3 ; 67–70 : cycles scratch c7-restore-a/b) n'apparaissent pas dans ce cycle.

Les étiquettes `expected_rev` indiquent la révision attendue au moment de chaque écriture (CAS) ; la révision stockée après l'écriture vaut `expected_rev + 1`. La révision d'état du cycle au moment de la consolidation vaut 28.

## Configuration de l'essai
- **seq 43 (expected_rev 0)** — `task.claim` gpt6 : tâche `c7-trial` créée (owner gpt6, reviewer claude, tester grok) ; chemins ownés = `trial-log.md`, `manual-ops.md`, `results.md` ; consigne initiale : initialiser rôles et définitions P1/P2, aucune PR d'état GitHub pendant l'essai.
- **seq 44 (expected_rev 1)** — `task.handoff` gpt6→vibe ; next_action : « Wait for K-phases on cc3-c7-trial… ».
- **seq 45 (expected_rev 2)** — `manual_op.log` : autorisation de l'essai réel C7 sur cc3-test par Kevin (owner.decision seq 42, approuvée).
- **seq 46 (expected_rev 3)** — `owner.decision install_phases` : définitions P1/P2 installées (definitions_sha256 `a37652de1165cb32167a5e9eeb2625e5bdb473901cc661777a5cb29bf2afbe4f`, preuve kind secret) — P1 attend 1 `proposal.submit` par rôle (owner/reviewer/tester), auto_advance `c7-p1-to-p2` ; P2 sans sortie attendue, auto_advance none.
- **seq 47 (expected_rev 4)** — `manual_op.log` : installation des K-phases consignée (source owner.decision seq 46).

## P1 — propositions scellées (commit-reveal)
- **seq 48 (expected_rev 5)** — `proposal.submit` claude (reviewer) : scellée `sealed-c42223b1…` / hash `7556d506…`.
- **seq 55 (expected_rev 8)** — `proposal.submit` grok (tester) : scellée `sealed-3aedcbe5…` / hash `42b1a59b…`.
- **seq 56–57 (expected_rev 9–10)** — mémoire privée `participant:grok` (X2), masquée aux tiers (`redacted: private_scope`).
- **seq 58 (expected_rev 11)** — `evidence.add` grok : matrice testeur P1 **PASS** (clarté absent du payload scellé ; scellé du pair claude masqué ; refus X5 `MEMORY_P1_SHARED_MUTATION` sur scope common ; X2 propose + auto-revue candidate→active) — phase inchangée, pas d'avance.
- **seq 59 (expected_rev 12)** — `evidence.add` gpt6 : reprise Vibe **PASS_WITH_LIMIT** (a résolu le cycle cc3 / tâche x2-x5-policy au lieu de cc3-c7-trial/c7-trial).
- **seq 60 (expected_rev 13)** — `proposal.submit` vibe (owner) : scellée `sealed-c55e7facc78a07076e3464b57e736b9408e358b865b5b53992f206750ea4420e` / hash `f6bcaaf45dfe034c4931cf521cc0ee94d21dfb9701dc37077b78a859ad19d4bd`.
- **seq 61 (expected_rev 14)** — `evidence.add` gpt6 : sorties mini-P1 **complètes PASS** (3 propositions scellées : owner vibe seq 60, reviewer claude seq 48, tester grok seq 55 ; payloads = sealed_id/content_hash uniquement avant P2).

## P1→P2
- **seq 62 (expected_rev 15)** — `phase.advance` signé `policy:c7-p1-to-p2` : P1→P2 après complétude des sorties de la ronde ; révélation des scellés de la ronde dans le même batch.

## P2 — CAS, restore, reprise, objection
- **seq 63–64 (expected_rev 16–17)** — course CAS : grok applied (seq 63, expected_rev 16, révision après 17) ; claude **STALE** au même expected_rev 16 (aucun événement écrit) puis rattrapage applied (seq 64, expected_rev 17, révision après 18).
- **seq 65 (expected_rev 18)** — `evidence.add` gpt6 : concurrence CAS + récupération STALE **PASS_WITH_LIMIT**.
- **seq 66 (expected_rev 19)** — `evidence.add` gpt6 : rejeu idempotent **PASS** (seq 64 rejoué → duplicate, événement original rendu, aucune nouvelle écriture, révision inchangée).
- **seq 71 (expected_rev 20)** — `manual_op.log` : aller-retour restore par imports owner Kevin (cycles scratch isolés `c7-restore-a:67` / `c7-restore-b:70`, sans cible GitHub ni issue refs, impact production aucun).
- **seq 72 (expected_rev 21)** — `evidence.add` gpt6 : restore export/import **PASS** (sha identique `0acea76d…`, révision d'état 3→3, tâche mutée retrouvée, isolation vérifiée).
- **seq 73 (expected_rev 22)** — `evidence.add` gpt6 : reprise en un appel + budget de contexte **PASS** (grok 273, claude 233, vibe 396 tokens estimés ; médiane 273 vs plafond 6000).
- **seq 74 (expected_rev 23)** — `objection.open` vibe : next_action obsolète (« Wait for K-phases » alors que les K-phases sont installées seq 46 et P1→P2 exécuté seq 62), sévérité minor.
- **seq 75 (expected_rev 24)** — `objection.resolve` claude : confirmée (résolution attendue de l'owner courant via task.handoff).
- **seq 76 (expected_rev 25)** — `evidence.add` gpt6 : workflow d'objection **PASS**.
- **seq 77 (expected_rev 26)** — `task.handoff` vibe (owner inchangé) : next_action mis à jour — K-phases installé, ronde scellée P1→P2 terminée, exécuter les scénarios C7 restants depuis le store.
- **seq 78 (expected_rev 27)** — `evidence.add` gpt6 : absence de PR d'état + complétude manual-ops **PASS** (aucune PR project-mcp-collab postérieure à #37 pendant l'essai ; #36/#37 antérieures au GO C7).

État à la consolidation : phase **P2**, révision **28**, statut open. Ce journal est écrit à partir des seuls événements du cycle `cc3-c7-trial`.
