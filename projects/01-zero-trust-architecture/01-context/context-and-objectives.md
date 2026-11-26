# 01 — Contexte et objectifs

## Contexte

Le projet s'appuie sur une entreprise fictive disposant d'un système d'information hébergé localement : postes utilisateurs, postes d'administration et serveurs internes.

L'architecture initiale utilise une segmentation en VLAN et des contrôles réseau périmétriques. Le scénario de départ considère qu'un attaquant a obtenu des identifiants VPN légitimes. Le projet vise à étudier les possibilités d'accès offertes par cette situation et à réduire l'exposition des ressources.

## Problématique

L'accès au réseau interne ne doit pas être assimilé à une autorisation générale d'accéder aux ressources. L'identité, les droits associés et les règles propres à chaque ressource doivent être pris en compte.

## Objectifs

1. Contrôler les accès aux ressources au moyen d'un modèle ZTNA.
2. Appliquer le moindre privilège aux comptes et autorisations.
3. Réduire les communications réseau au strict nécessaire.
4. Limiter les possibilités de déplacement latéral.
5. Vérifier les mesures par des tests d'accès autorisés et refusés.

## Périmètre initial

- Active Directory et DNS.
- Serveur de fichiers.
- Serveur intranet.
- Postes utilisateurs et d'administration.
- Équipements et mécanismes de contrôle réseau nécessaires au laboratoire.

Les ressources SaaS et l'intégration cloud sont exclues de cette première phase.

## Contraintes

Le laboratoire privilégie les solutions gratuites ou disposant d'une offre gratuite adaptée. Les ressources matérielles disponibles imposent un déploiement progressif. Les fonctionnalités des offres seront vérifiées avant d'être retenues. Aucun secret ou identifiant ne sera publié.
