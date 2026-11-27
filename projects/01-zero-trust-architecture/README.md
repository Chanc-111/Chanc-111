# 🛡️ Projet 01 — Conception et mise en place d’une architecture Zero Trust

<sub>Projet personnel · En cours de conception et de mise en œuvre</sub>

## 🎯 Contexte et objectif

À la suite d’un incident impliquant l’utilisation frauduleuse d’identifiants VPN dérobés, une entreprise de taille intermédiaire souhaite renforcer la sécurité de son système d’information **on-premises**.

L’objectif est de faire évoluer progressivement l’architecture vers une approche Zero Trust en :
- vérifiant les accès selon l’identité et le contexte, dans la mesure des capacités de la solution retenue ;
- appliquant le principe du moindre privilège ;
- limitant les communications entre systèmes aux seuls flux nécessaires, grâce à la microsegmentation.

Les ressources métier restent hébergées localement. Seul le plan de contrôle de la solution ZTNA est hébergé par son fournisseur et accessible via Internet.

## 🧱 Architecture existante — état de référence

L’architecture initiale repose sur cinq zones réseau :

| VLAN | Affectation | Description |
|---|---|---|
| 10 | Postes utilisateurs | Postes de travail des collaborateurs sur le réseau local |
| 20 | Serveurs | Serveurs hébergeant les services et applications internes |
| 30 | Administration | Postes dédiés à l’administration des systèmes et équipements |
| 40 | Gestion des équipements réseau | Interfaces d’administration des pare-feux, commutateurs et autres équipements |
| 50 | Accès distants | Zone logique d’arrivée des connexions VPN ; les postes distants n’intègrent pas le VLAN 10 |

Le routage et le filtrage entre les VLAN sont assurés par un pare-feu. Les communications entre systèmes d’un même VLAN ne traversent généralement pas ce pare-feu.

**Ressources de référence :** Active Directory, serveur de fichiers, serveur métier, accès VPN et postes d’administration dédiés.

[Voir le schéma de l’architecture existante](diagrams/01-existing-architecture.mmd)

## 🔐 Limites identifiées

- Le VPN ouvre un périmètre réseau relativement étendu après authentification.
- L’appartenance à un VLAN ne garantit pas l’autorisation d’accéder à chaque ressource.
- Les communications entre serveurs d’un même VLAN échappent généralement au filtrage inter-VLAN.
- Les accès privilégiés doivent être contrôlés indépendamment de la localisation réseau.

## 🏗️ Architecture cible — principes

La cible repose sur trois dimensions complémentaires :

1. **ZTNA — accès par identité et contexte :** tous les accès utilisateurs aux ressources internes, sur site comme à distance, y compris les accès d’administration, passent par le contrôle ZTNA. La localisation réseau ne suffit pas à autoriser un accès.
2. **Moindre privilège :** chaque utilisateur et chaque système dispose uniquement des accès et opérations nécessaires.
3. **Segmentation et microsegmentation :** le pare-feu réseau conserve le filtrage inter-zones ; les pare-feux hôtes contrôlent plus finement les communications entre serveurs, y compris au sein du VLAN serveurs.

Le VPN traditionnel est remplacé, pour les accès utilisateurs, par le modèle ZTNA. Le VLAN 50, dédié à l’arrivée des connexions VPN, n’a donc plus vocation à exister dans la cible sous cette forme.

Le ZTNA ne remplace ni les permissions applicatives, ni les ACL réseau, ni les pare-feux hôtes.

[Voir le schéma de l’architecture cible](diagrams/02-target-architecture.mmd)

## 🧩 Périmètre initial de microsegmentation

La première itération concerne :
- les contrôleurs de domaine Active Directory ;
- le serveur de fichiers ;
- le serveur métier.

Les postes utilisateurs et d’administration restent hors de ce périmètre initial. La microsegmentation repose sur les pare-feux hôtes. Le pare-feu réseau conserve le filtrage inter-VLAN ; les ACL réseau sont hors périmètre de cette première itération.

La matrice des flux sera définie après identification des dépendances réellement nécessaires.

## 🧰 Technologies et environnement envisagés

| Fonction | Technologie | Rôle |
|---|---|---|
| Accès Zero Trust | Twingate Starter | Contrôle des accès aux ressources internes |
| Pare-feu réseau principal | Stormshield SNS EVA | Filtrage inter-VLAN |
| Pare-feu complémentaire | FortiGate-VM | Expérimentations ciblées et comparaison |
| Émulation réseau | EVE-NG | Construction de la topologie |
| Virtualisation | VMware Workstation | Hébergement des machines virtuelles |
| Annuaire et DNS | Windows Server 2022 | AD DS et DNS |
| Serveur de fichiers cible | Windows Server 2022 | SMB, permissions NTFS et groupes AD |
| Serveur applicatif | Debian 13 | Application web interne, par exemple Nginx |
| Connecteur ZTNA | Debian 13 ou Ubuntu Server | Twingate Connector |
| Microsegmentation | Windows Defender Firewall et nftables | Filtrage des communications entre serveurs |

Stormshield est le pare-feu réseau de référence. FortiGate sera utilisé dans des expérimentations ciblées, sans être systématiquement placé dans le chemin réseau principal. Les prérequis et limites des offres gratuites seront vérifiés avant déploiement.

## 🔄 Parcours d’accès ZTNA

Le terminal initie une demande d’accès via le client Twingate. Le plan de contrôle évalue les règles d’accès ; si la demande est autorisée, le trafic rejoint le connecteur situé dans le SI local, puis la ressource publiée.

[Voir le diagramme du parcours ZTNA](diagrams/03-ztna-access-flow.mmd)

## 🧪 Cahier de recette — ZTNA

La première campagne se limite à deux contrôles : l'identité et les autorisations de l'utilisateur, puis la posture de sécurité du terminal.

| ID | Test | Résultat attendu |
|---|---|---|
| ZTNA-01 | Un utilisateur authentifié et autorisé tente d'accéder à une ressource qui lui est attribuée | Accès accordé |
| ZTNA-02 | Un utilisateur authentifié, mais non autorisé à accéder à cette ressource, tente une connexion | Accès refusé |
| ZTNA-03 | Un utilisateur autorisé se connecte depuis un terminal conforme aux critères de sécurité définis | Accès accordé |
| ZTNA-04 | Le même utilisateur tente d'accéder à la ressource depuis un terminal qui ne respecte pas au moins un critère de sécurité | Accès refusé |

**Point de vigilance :** les critères de posture effectivement disponibles (par exemple, chiffrement du disque, pare-feu ou antivirus) ainsi que leur prise en charge dans l'offre Twingate Starter devront être vérifiés avant l'exécution de ZTNA-03 et ZTNA-04. Si cette fonctionnalité n'est pas incluse dans l'offre retenue, ces deux tests seront adaptés ou reportés ; ils ne seront pas considérés comme réalisés.

Chaque test devra conserver une preuve reproductible : configuration appliquée, identité et état du terminal utilisés, résultat observé et journal correspondant lorsque disponible. Aucun test ne sera déclaré réussi avant son exécution.

## 📌 Avancement

Le projet est au stade du cadrage et de la conception. Les technologies, configurations et résultats seront précisés au fil de la construction du laboratoire. Les éléments envisagés ne sont pas présentés comme déjà déployés ou testés.

**Démarche :** comprendre → concevoir → mettre en œuvre → tester → documenter.
