# 02 — Architecture existante

> Point de départ du scénario ; les détails seront complétés pendant la modélisation du laboratoire.

## Segmentation initiale

| VLAN | Usage | Ressources principales |
|---|---|---|
| 10 | Utilisateurs | Postes de travail |
| 20 | Serveurs | Active Directory, serveur de fichiers, intranet |
| 30 | Administration | Postes d'administration |

Les communications inter-VLAN sont contrôlées par le pare-feu. Dans le scénario initial, l'accès distant repose sur un VPN.

## Ressources

- **Active Directory / DNS :** identités du domaine et résolution de noms.
- **Serveur de fichiers :** partage SMB, permissions NTFS et groupes AD.
- **Intranet :** application web interne hébergée sur Debian.
- **Postes :** postes utilisateurs et postes dédiés à l'administration.

## Points à analyser

- Quels utilisateurs et équipements peuvent atteindre chaque ressource ?
- Quels flux sont nécessaires au fonctionnement des services ?
- Quelles autorisations sont accordées après une connexion VPN ?
- Quelles protections empêchent les communications latérales entre serveurs ?
