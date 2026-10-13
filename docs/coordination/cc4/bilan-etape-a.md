# CC-4 — Bilan de sortie de l'étape A (exécution semi-automatique)

statut : proposition du coordinateur sortant — à valider par Kevin
cycle : `cc4-design` · plan : `docs/coordination/cc4/plan.md` (CC-PLAN-4/v0.1, option K-CC4-1 = C, enveloppe payante 0 €)
coordinateur : Claude, du lancement au 2026-10-14 ; relève : **Sol** (ChatGPT), pour une nouvelle étape (migration hors de GitHub, à définir par Kevin)
date : 2026-10-14

## 1. Résultat en une phrase

L'étape A est **livrée et fusionnée dans `cc3-integration`** (tête `a0d41aa`), à une exception près : l'adaptateur GitHub Actions/CLI (A3b), **reporté par Kevin** dans `rfkevin/github-mcp#124`. Rien n'a été fusionné sur `master`, rien n'est en production, **0 € dépensé**.

## 2. Lots livrés

| Lot | PR (github-mcp) | Auteur | Reviewer / testeur | Contenu |
|---|---|---|---|---|
| A0 | #111 (issue #110) | Vibe GLM | D12 du lot | `op_id` strict, STALE avec `currentRevision`, découverte du cycle, une sortie par participant et par ronde, contrat principal/endpoint |
| A1 | #114 (issue #112) | GPT-6 | D12 du lot | registre d'invocations/outbox, `wake_key` unique, baux, tentatives, ledger d'usage (migration 0006) |
| A2 | #115 + correctif #116 (issue #113) | Claude | D12 du lot | demandes owner exécutables à empreinte exacte, `/owner` sans copier-coller, arrêt d'urgence atomique, enveloppe budgétaire (migration 0007) |
| A3a | #117 + correctif #118 | Claude | D12 du lot | exécuteur Workers AI gratuit (glm-4.7-flash), endpoints inscrits et activés par l'owner, sonde réelle |
| A3c + A4-mini | #120 (issue #119) | Sol | Grok / Vibe | module LLM universel compatible OpenAI (OpenRouter `:free`), « Tâche d'essai » sur `/owner` |
| A4 | #122 (issue #121) | Sol | Grok / Vibe | rôle coordinateur accordé par l'owner, `collab_dispatch_task`, `collab_list_executors`, fiches exécutants et notes owner (migration 0008) |
| A4 — mission réelle | #123 | Claude | Grok **agree** / Vibe **agree** au head `e745ab0`, base `3b366f6` | `/owner` lisible sur téléphone et en mode sombre ; worker gratuit en appui (résumé pour le reviewer) |

Les attributions réelles diffèrent du tableau proposé au §9 du plan (réattributions de Kevin en cours de cycle) ; la séparation auteur ≠ reviewer ≠ testeur a été tenue sur chaque lot.

## 3. Critère de sortie de l'étape A (plan §9) — évaluation

> Une tâche peut être exécutée par des endpoints headless vérifiés, avec contexte borné, provenance D12, budget dur, reprises et demandes owner préparées. Kevin ne relaie plus le travail normal entre participants automatisés.

| Élément | État | Preuve |
|---|---|---|
| Endpoint headless vérifié | **Atteint (API gratuite)** | `ep-claude-glm47` sondé puis activé par demande owner ; 6 invocations réelles réussies le 2026-10-14 dans `coordinator-tasks` |
| Endpoint Actions/CLI vérifié | **Reporté** | A3b → github-mcp#124 (jeton `CLAUDE_CODE_OAUTH_TOKEN` et workflow déposé par Kevin) |
| Contexte borné | Atteint | brief ≤ 2 000, materials ≤ 60 000, plafond de jetons par endpoint, consigne de contrôle fixée par le serveur |
| Provenance | Atteint | invocation A1 : fournisseur, modèle, empreintes entrée/sortie, usage ; aucune sortie brute persistée |
| Budget dur | Atteint | enveloppe 0 €, réservation fail-closed, quotas d'appels et de neurons visibles par le coordinateur |
| Reprises / idempotence | Atteint | même `op_id` ⇒ aucun second appel fournisseur (vérifié par test E2E) |
| Demandes owner préparées | Atteint | `install_phases`, `import_state`, `register_endpoint`, `grant_coordinator` approuvés sur `/owner` sans JSON à coller |
| Kevin ne relaie plus le travail normal | **Partiel** | les workers gratuits produisent du texte sans relais ; Sol, Grok et Vibe restent des agents de chat que Kevin réveille. Lever ce point relève de A3b puis de l'étape B (`next_actors`, réveil automatique) |

**Conclusion proposée** : sortie de l'étape A **acceptée avec réserves** — l'invariant de sécurité et le chemin API gratuit sont prouvés en réel ; le chemin runner (A3b) et la suppression du relais humain sont explicitement reportés.

## 4. Coûts et usage

- Dépense : **0 €**. Cloudflare Workers Free, Workers AI dans l'allocation gratuite quotidienne, CI GitHub standard.
- Usage worker le 2026-10-14 (coordinateur) : 6 invocations réussies, 3 appels refusés avant exécution pour `op_id` mal formé (le contrôle A0 a fonctionné), environ 150 neurons sur 8 000 par jour.

## 5. Ce qu'on a appris sur les workers gratuits

glm-4.7-flash (plafond 512 jetons de sortie) :
- **Utile** pour orienter : résumé de diff pour un reviewer, tri, résumé de log CI, relecture de textes courts. Le résumé de #123 couvrait 8 changements sur 10.
- **À éviter** pour produire du code ou du CSS à appliquer (essai `/owner` inutilisable : portées `@media` fausses, sélecteur cassant les tableaux, image `data:` interdite par la CSP) et pour lister des garanties de sécurité (il a omis `COORDINATOR_REQUIRED` et l'arrêt d'urgence en résumant #122).
- Règle adoptée par Kevin : le coordinateur choisit la tâche adaptée à chaque worker, les utilise dès qu'ils sont utiles, et peut poster leurs sorties sur une PR avec l'en-tête « orientation seulement, ne vaut pas avis D12 ».

Un deuxième worker (OpenRouter `:free`, code prêt depuis #120) permettrait de choisir selon les points forts et de faire vérifier un worker par un autre ; il attend le secret `LLM_KEY_OPENROUTER`.

## 6. Frictions observées

- Le runner ciblé `github_run_checks` est resté bloqué par un contrôleur à revalider ; la CI standard de PR a servi de preuve.
- Le format `op_id` (4 segments exacts) piège les appelants ; les messages d'erreur sont clairs mais un exemple dans la description de `collab_dispatch_task` éviterait l'aller-retour.
- Les issues livrées restaient ouvertes après fusion : elles sont commentées pour fermeture par Kevin (#110, #112, #113, #119, #121).

## 7. Reste à faire (hors étape A)

1. github-mcp#124 — A3b : runner GitHub Actions + Claude Pro, attesté par OIDC.
2. Deuxième worker OpenRouter `:free` (secret `LLM_KEY_OPENROUTER` par Kevin, puis inscription, sonde, activation).
3. Notes owner « points forts / points faibles » de `ep-claude-glm47` sur `/owner` (Kevin).
4. Étape B (politique `next_actors`, gates CI + D12, intégration automatique, merge train) — à replanifier selon la nouvelle étape de migration confiée à Sol.
5. Révocation ou maintien du rôle coordinateur de Claude (`grant_coordinator`, seq 140) lors de la passation à Sol.

## 8. Règles qui restent en vigueur

Pas de fusion `main`/`master`, pas de production, pas de dépense sans Kevin sur `/owner` ; aucun agent ne détient le secret owner ni une clé API ; auteur ≠ reviewer ≠ testeur, avis valable au head/base exacts ; les workers gratuits produisent du texte seulement et leur avis ne remplace jamais un accord D12.
