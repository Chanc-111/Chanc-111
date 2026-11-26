# 03 — Architecture cible

> Architecture envisagée : les mécanismes ci-dessous restent à concevoir, déployer et valider.

## Accès aux ressources avec le ZTNA

Twingate est envisagé pour fournir un accès contrôlé aux ressources internes. Les autorisations seront définies par ressource et par groupe d'utilisateurs. La présence sur un VLAN ne constituera pas, à elle seule, une autorisation.

## Moindre privilège

- Groupes AD adaptés aux fonctions.
- Permissions de partage et NTFS sur le serveur de fichiers.
- Séparation des comptes d'usage courant et d'administration.
- Accès administratifs limités aux ressources nécessaires.

## Microsegmentation

Des règles de pare-feu sur les hôtes seront étudiées afin de limiter les communications entre serveurs aux seuls flux nécessaires. Cette mesure complète le ZTNA : le contrôle de l'accès d'un utilisateur à une ressource ne remplace pas le filtrage des communications entre ressources.

## Points à vérifier

- Les ressources ne sont accessibles qu'aux identités autorisées.
- Les communications serveur-à-serveur non nécessaires sont bloquées.
- La révocation d'un compte ou d'une autorisation entraîne la perte de l'accès correspondant, selon les mécanismes de la solution.
