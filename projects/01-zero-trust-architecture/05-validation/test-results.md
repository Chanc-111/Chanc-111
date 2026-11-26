# 05 — Validation

Les tests permettront de vérifier que les contrôles prévus fonctionnent réellement.

| ID | Scénario | Résultat attendu | Statut |
|---|---|---|---|
| T01 | Accès à une ressource autorisée via ZTNA | Accès autorisé | À tester |
| T02 | Accès direct sans passer par le mécanisme prévu | Connexion bloquée | À tester |
| T03 | Accès par un utilisateur non autorisé | Accès refusé | À tester |
| T04 | Accès au partage de fichiers | Droits conformes aux permissions | À tester |
| T05 | Communication serveur-à-serveur non nécessaire | Communication bloquée | À tester |
| T06 | Révocation d'un compte ou d'une autorisation | Accès correspondant révoqué | À tester |

Pour chaque test, consigner l'objectif, les prérequis, la procédure, le résultat attendu, le résultat observé, les preuves et les éventuels écarts. Aucun test ne sera marqué comme réussi avant son exécution.
