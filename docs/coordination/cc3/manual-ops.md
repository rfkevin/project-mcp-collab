# C7 — Registre des opérations manuelles (cycle cc3-c7-trial)

Source : journal du store (`manual_op.log` + `owner.decision`). Complétude vérifiée par le checkpoint gpt6 seq 78 (3 opérations manuelles pendant l'essai).

| seq | Révision | Opération | Classe | Acteur | Résultat | Source | Automatisable plus tard |
|---|---|---|---|---|---|---|---|
| 45 | rev 2 | Autoriser l'essai réel C7 sur cc3-test | human_decision | Kevin | approved | owner.decision seq 42 | non |
| 47 | rev 4 | Installer les définitions de phase P1/P2 (K-phases) | human_decision | Kevin | installed | owner.decision seq 46 | oui |
| 71 | rev 20 | Aller-retour restore par imports owner (2 cycles scratch isolés `c7-restore-a:67` / `c7-restore-b:70`, sans cible GitHub ni issue refs, impact production aucun) | human_decision | Kevin | done | source_events `c7-restore-a:67`, `c7-restore-b:70` | oui |

## Notes
- Le GO C7 (owner.decision seq 42) et l'installation des phases (seq 46, definitions_sha256 `a37652de1165cb32167a5e9eeb2625e5bdb473901cc661777a5cb29bf2afbe4f`) sont des actes du canal `/owner` (I7), jamais imputables aux agents.
- Latence S5 sur cc3-test : opération manuelle héritée de C2 (harness `docs/collaboration/cc3/cc3-latency-harness.md`), non exécutée pendant ce trial — aucune mesure dans le journal du cycle.
- La présente consolidation de documentation (PR sur les chemins ownés) est une mutation GitHub de documentation conforme au périmètre de la tâche ; `state.md` n'est pas modifié.
