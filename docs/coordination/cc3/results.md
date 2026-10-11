# C7 — Résultats consolidés de l'essai réel (cycle cc3-c7-trial)

Source unique : preuves `evidence.add` du journal du store (coordinateur gpt6, tester grok, owner vibe). Aucune donnée GitHub n'a servi à établir ces verdicts. Consolidation en P2, révision 28.

## Verdicts par scénario

| Scénario | Preuve (seq) | Verdict |
|---|---|---|
| Reprise en un appel + budget de contexte (3 participants) | 73 | **PASS** (grok 273, claude 233, vibe 396 tokens estimés ; médiane 273 vs plafond 6000) |
| Propositions P1 scellées (commit-reveal C3, 3 rôles D12) | 48, 55, 58, 60, 61 | **PASS** (payloads = sealed_id/content_hash uniquement avant P2 ; aucun clair dans le journal) |
| Avancée P1→P2 (policy `c7-p1-to-p2`, sorties de la ronde complètes) | 61, 62 | **PASS** |
| Concurrence CAS + récupération STALE | 63–65 | **PASS_WITH_LIMIT** |
| Rejeu idempotent après applied | 66 | **PASS** (duplicate, événement original rendu, aucune nouvelle écriture, révision inchangée) |
| Restore export/import (aller-retour) | 71, 72 | **PASS** (sha identique `0acea76d…`, révision d'état 3→3, tâche mutée retrouvée, cycles scratch isolés) |
| Garde X5 (mémoire partagée en P1) | 58 | **PASS** (refus `MEMORY_P1_SHARED_MUTATION` sur scope common, révision inchangée) |
| Mémoire privée X2 (propose + auto-revue bornée) | 56, 57, 58 | **PASS** (candidate→active, masquée aux tiers) |
| Workflow d'objection sur next_action obsolète | 74–76 | **PASS** (ouverte par l'owner, confirmée par le reviewer) |
| Absence de PR d'état pendant l'essai | 78 | **PASS** (aucune PR project-mcp-collab postérieure à #37 ; #36/#37 antérieures au GO) |
| Reprise générique « Reprends C7 » (1re tentative Grok) | 53 | **FAIL** |
| Reprise avec instruction explicite (source + cycle nommés) | 54, 73 | **PASS_WITH_LIMIT** |
| Résolution du cycle dédié par Vibe (1re reprise) | 59 | **PASS_WITH_LIMIT** (cycle cc3 / x2-x5-policy résolu au lieu de cc3-c7-trial / c7-trial) |

## Limites observées

1. **Premier resume générique Grok raté (seq 53).** L'instruction ultra-courte « Reprends C7 » a fait sélectionner l'instantané GitHub périmé au lieu du store vivant : Grok a rapporté « C7 pas ouvert, pas de tâche Grok, pas de GO owner » alors que le store vivant montrait P1/rev 6, tâche c7-trial (tester grok), GO owner seq 42 et phases installées seq 46. Interprétation consignée : défaillance de comportement de reprise/découverte ou sélection de source périmée, pas une preuve d'absence du mapping store/tâche/identité. À porter vers CC-4 comme problème d'utilisabilité.
2. **STALE structuré non affiché par le client Claude (seq 65).** Lors de la course CAS (expected_rev 16), le perdant Claude a reçu l'erreur typée STALE — le serveur inclut `currentRevision` et le delta à rejouer, mais le client n'a rendu que le message humain ; la récupération a exigé une lecture de delta supplémentaire. Les invariants tiennent (une seule écriture à expected_rev 16, aucune écriture du perdant, réutilisation du même op_id possible car le refus STALE ne crée pas d'enregistrement d'idempotence), mais le rendu de l'erreur structurée reste un défaut d'UX client.
3. **next_action obsolète corrigé (seq 74–77).** Le next_action de c7-trial disait encore « Wait for K-phases » après l'installation (seq 46) et l'avancée P1→P2 (seq 62) ; objection ouverte par l'owner (seq 74), confirmée par le reviewer (seq 75), corrigée par `task.handoff` owner inchangé (seq 77). Limite corrélée : la preuve seq 73 montre que la reprise en un appel servait le next_action partagé périmé — une instruction générique peut hériter d'un état de tâche non rafraîchi.

## Conclusion

Les scénarios C7 exécutés depuis le store seul tiennent : scellement commit-reveal, avancée policy bornée à la ronde, CAS fail-closed avec récupération STALE, idempotence, restore export/import bit-exact, gardes mémoire X5/X2, workflow d'objection, absence de PR d'état GitHub pendant l'essai. Les limites relevées sont des défaillances d'utilisabilité côté clients (découverte générique, rendu de l'erreur STALE structurée, fraîcheur du next_action), pas des défauts du store. Fusion de cette PR = Kevin uniquement ; `state.md` non modifié.
