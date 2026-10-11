# C7 — Registre des opérations manuelles (cycle cc3-c7-trial)

Source : journal du store (`manual_op.log` + `owner.decision`). Complétude vérifiée par le checkpoint gpt6 seq 78 (4 opérations manuelles pendant l'essai, clôture du snapshot final incluse).

| seq | expected_rev | Opération | Classe | Acteur | Résultat | Source | Automatisable plus tard |
|---|---|---|---|---|---|---|---|
| 45 | expected_rev 2 | Autoriser l'essai réel C7 sur cc3-test | human_decision | Kevin | approved | owner.decision seq 42 | non |
| 47 | expected_rev 4 | Installer les définitions de phase P1/P2 (K-phases) | human_decision | Kevin | installed | owner.decision seq 46 | oui |
| 71 | expected_rev 20 | Aller-retour restore par imports owner (2 cycles scratch isolés `c7-restore-a:67` / `c7-restore-b:70`, sans cible GitHub ni issue refs, impact production aucun) | human_decision | Kevin | done | source_events `c7-restore-a:67`, `c7-restore-b:70` | oui |
| 80 | expected_rev 29 | Import de la base du snapshot final C7 (`import_state` de l'état initial CC-STATE-1, state_revision 2, content_sha256 `60b4d75e…`, sans cible GitHub ni issue refs) | human_decision | Kevin | done | owner.decision seq 79 | oui |

## Opération manuelle issue de la revue PR #38 — clôturée

- **Import owner de l'état initial du cycle `cc3-c7-trial` sur `/owner`**, puis export `cc-state-1` validé par le parser L1 — nécessaire pour clore le scénario obligatoire « Snapshot final exportable » du GO (owner.request seq 41, owner.decision seq 42). Clôturée : import effectué par Kevin (owner.decision `import_state` seq 79, consigné manual_op seq 80) ; export final **PASS** (evidence seq 81) — cc-state-1 révision 3 sur base 2, sha `ad6080171f49b009b8c6143f38e1d2987cad1b314cf4dd727f8c386083f72a15`, parser validé, hash stable au second export. Aucune opération manuelle ouverte restante sur ce scénario.

## Notes
- Le GO C7 (owner.decision seq 42) et l'installation des phases (seq 46, definitions_sha256 `a37652de1165cb32167a5e9eeb2625e5bdb473901cc661777a5cb29bf2afbe4f`) sont des actes du canal `/owner` (I7), jamais imputables aux agents.
- Latence S5 sur cc3-test : opération manuelle héritée de C2 (harness `docs/collaboration/cc3/cc3-latency-harness.md`), non exécutée pendant ce trial — aucune mesure dans le journal du cycle.
- La présente consolidation de documentation (PR sur les chemins ownés) est une mutation GitHub de documentation conforme au périmètre de la tâche ; `state.md` n'est pas modifié.
