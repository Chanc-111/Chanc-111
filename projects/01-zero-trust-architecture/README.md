# 🛡️ Architecture Zero Trust

<sub>Projet personnel de conception et de mise en œuvre progressive · En cours</sub>

## 🎯 Objectif
Faire évoluer un système d'information local vers une approche Zero Trust en renforçant la vérification des accès aux ressources et en limitant les communications réseau au strict nécessaire.

Le scénario repose sur une entreprise fictive dont l'architecture initiale utilise une segmentation en VLAN et un accès distant VPN. Le point de départ est la compromission d'identifiants VPN : être authentifié sur le réseau ne doit pas donner un accès implicite à l'ensemble du SI.

## 🧱 Périmètre du laboratoire
| Zone | Contenu |
|---|---|
| VLAN 10 — Utilisateurs | Postes de travail |
| VLAN 20 — Serveurs | Active Directory / DNS, serveur de fichiers, intranet |
| VLAN 30 — Administration | Postes d'administration |

**Socle technique envisagé :** Windows Server 2022 (AD DS / DNS et fichiers), Debian 13 (intranet), Twingate (ZTNA), Stormshield (filtrage réseau), pare-feu hôtes Windows et Linux, EVE-NG et virtualisation locale.

La première phase reste centrée sur le SI local. Le plan de contrôle Twingate est un service opéré par l'éditeur ; cela ne signifie pas que les ressources métier du laboratoire sont hébergées dans un cloud.

## 🔐 Principes de sécurité étudiés
- **ZTNA :** accès par ressource et par identité, plutôt qu'un accès réseau étendu après connexion VPN.
- **Moindre privilège :** droits AD, permissions SMB/NTFS et séparation des comptes standards et administrateurs.
- **Microsegmentation :** filtrage local des communications entre serveurs, en complément du contrôle d'accès ZTNA.
- **Vérification :** tests d'accès autorisés et refusés, contrôle des flux et observation des journaux.

> Le ZTNA et la microsegmentation répondent à des besoins différents : Twingate contrôle l'accès des utilisateurs aux ressources publiées ; les pare-feu des hôtes limitent les communications entre systèmes. L'un ne remplace pas l'autre.

## 🗺️ Schémas d'architecture
- [Architecture existante](diagrams/01-existing-architecture.mmd)
- [Architecture cible Zero Trust](diagrams/02-target-architecture.mmd)
- [Parcours d'accès ZTNA](diagrams/03-ztna-access-flow.mmd)

## 🧪 Validation prévue
1. Accès autorisé à une ressource via Twingate.
2. Refus d'un accès direct non autorisé.
3. Refus d'accès à une ressource pour une identité non habilitée.
4. Respect des droits sur le partage de fichiers.
5. Blocage des flux inter-serveurs non nécessaires.
6. Révocation effective d'un compte ou d'une autorisation.

Les contrôles ne seront considérés comme validés qu'après exécution et conservation de preuves.

## 📌 Avancement
Le projet est au stade du cadrage et de la conception. Les technologies, configurations et résultats seront précisés au fil de la construction du laboratoire. Les éléments envisagés ne sont pas présentés comme déjà déployés ou testés.

**Démarche :** comprendre → concevoir → mettre en œuvre → tester → documenter.
