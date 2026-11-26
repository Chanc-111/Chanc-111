# 🛡️ Projet 01 — Architecture Zero Trust

**Statut :** 🟡 En cours — cadrage et conception

## Présentation

Conception et mise en œuvre progressive d'une architecture Zero Trust pour un système d'information local fictif. Le laboratoire explore le contrôle des accès aux ressources, le moindre privilège et la limitation des communications latérales.

## Contexte

L'infrastructure initiale repose sur une segmentation en VLAN et des contrôles réseau périmétriques. Un scénario de compromission d'identifiants VPN sert de point de départ : l'authentification au réseau ne doit pas donner, à elle seule, un accès étendu aux ressources internes.

## Objectifs

- Contrôler les accès aux ressources à l'aide d'une solution ZTNA.
- Appliquer le moindre privilège aux utilisateurs et administrateurs.
- Limiter les communications entre serveurs aux flux nécessaires.
- Vérifier les contrôles par des tests reproductibles.

## Organisation

- [01 — Contexte](01-context/context-and-objectives.md)
- [02 — Architecture existante](02-existing-architecture/architecture.md)
- [03 — Architecture cible](03-target-architecture/architecture.md)
- [04 — Mise en œuvre](04-implementation/README.md)
- [05 — Validation](05-validation/test-results.md)
- [Schémas](diagrams/README.md)
- [Scripts](scripts/README.md)

## Environnement envisagé

Twingate, Stormshield, VLAN, Windows Server, Active Directory, Debian, Windows Defender Firewall, nftables et virtualisation locale.

Les choix définitifs seront précisés pendant la conception. Les éléments envisagés ne sont pas présentés comme déjà déployés ou validés.

## Démarche

**Comprendre → Concevoir → Mettre en œuvre → Tester → Documenter.**
