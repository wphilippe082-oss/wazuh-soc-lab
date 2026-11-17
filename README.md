# Laboratoire SOC avec Wazuh

Laboratoire pratique de centre opérationnel de sécurité (SOC) construit autour de **Wazuh**, **Docker Compose**, de postes Windows et Ubuntu, de Sysmon, UFW, Fail2ban et Nessus Essentials.

Ce dépôt documente l’architecture, le déploiement, l’intégration des terminaux, la collecte de télémétrie de sécurité, les exercices de détection et les difficultés rencontrées.

> **État du projet :** la pile Wazuh et la collecte de télémétrie des terminaux ont été testées. L’intégration de Suricata est encore en cours et n’est pas présentée comme terminée.

## Objectifs

- Déployer un environnement SIEM/XDR Wazuh avec Docker.
- Centraliser les événements de terminaux Windows et Linux.
- Appliquer le contrôle d’accès par rôles (RBAC) et le principe du moindre privilège.
- Collecter les événements Windows Security et Sysmon.
- Superviser les événements d’authentification et de sécurité Linux.
- Tester UFW et Fail2ban sur le terminal Ubuntu.
- Évaluer le serveur Wazuh avec Nessus Essentials.
- Étendre la visibilité réseau avec Suricata.

## Architecture générale

```text
                  Postes des analystes SOC
                    soc-analyst-01 / 02
                             |
                       HTTPS / RBAC
                             |
+---------------------+      |      +-------------------------------+
| Terminal Windows    |------|----  |       Serveur Wazuh           |
| Wazuh Agent         |      |      |       Docker Compose          |
| Sysmon              |      |      |                               |
| Journaux Security   |      |      |  +-------------------------+  |
+---------------------+      |      |  | Wazuh Manager           |  |
                             |      |  +-------------------------+  |
+---------------------+      |      |  | Wazuh Indexer           |  |
| Terminal Ubuntu     |------------>|  +-------------------------+  |
| Wazuh Agent         |             |  | Wazuh Dashboard         |  |
| UFW + Fail2ban      |             |  +-------------------------+  |
| FIM / journaux      |             +-------------------------------+
| Suricata            |                             |
+---------------------+                     Alertes centralisées
                                                    |
                                             Investigation SOC

Kali Linux sert de source de tests contrôlés pour certains exercices SSH et réseau.
Nessus Essentials est utilisé séparément pour l’évaluation des vulnérabilités du serveur Wazuh.
```

### Réseau du laboratoire

| Système | Rôle | Adresse documentée |
|---|---|---|
| Serveur Wazuh | Manager, Indexer, Dashboard | `192.168.85.129` (NAT) ; `10.126.210.210` (accès ponté) |
| Client Ubuntu | Terminal Linux, UFW, Fail2ban | `192.168.85.130` |
| Kali Linux | Machine source de tests contrôlés | `192.168.85.131` |
| Windows 11 | Terminal Windows, Sysmon | DHCP / réseau de l’hôte |

Les adresses sont celles observées pendant les exercices. Elles peuvent varier selon la configuration du réseau virtuel.

## Composants

| Composant         | Fonction                                                                           |
| ----------------- | ---------------------------------------------------------------------------------- |
| Wazuh Manager     | Communication avec les agents, analyse des événements et génération d’alertes      |
| Wazuh Indexer     | Stockage et indexation des alertes                                                 |
| Wazuh Dashboard   | Recherche, supervision et investigation                                            |
| Wazuh Agent       | Collecte et transmission de la télémétrie des terminaux                            |
| Sysmon            | Télémétrie détaillée des processus, événements système et activités réseau Windows |
| UFW               | Pare-feu du terminal Ubuntu                                                        |
| Fail2ban          | Détection des échecs d’authentification répétés et bannissement temporaire         |
| Nessus Essentials | Évaluation des vulnérabilités du serveur Wazuh                                     |
| Suricata          | Source IDS ; surveille la télemetrie reseau                                        |

## Avancement

- [x] Installation de Docker Engine et Docker Compose
- [x] Déploiement Wazuh Single-Node
- [x] Rétablissement et vérification de l’API et du Dashboard Wazuh
- [x] Création de deux comptes analystes SOC avec objectif d’accès en lecture seule
- [x] Enrôlement d’agents Windows et Ubuntu pendant les exercices
- [x] Vérification de la collecte des événements Windows Security, dont l’événement 4624
- [x] Installation et configuration de Sysmon sur Windows
- [x] Exercices SCA et collecte des événements SSH sur Ubuntu
- [x] Test conjoint d’UFW et Fail2ban pour le blocage SSH
- [x] Étude du processus de création et de validation de règles Wazuh personnalisées
- [x] Scan du serveur Wazuh avec Nessus Essentials
- [ ] Validation du démarrage de Suricata et de son jeu de règles
- [ ] Collecte de `eve.json` par l’agent Wazuh
- [ ] Vérification des événements réseau dans le Dashboard
- [ ] Revue finale des captures et de la reproductibilité

## Arborescence du dépôt

```text
wazuh-soc-lab-fr/
├── README.md
├── .gitignore
├── docs/
│   ├── architecture.md
│   ├── deploiement-docker-wazuh.md
│   ├── integration-terminaux.md
│   ├── detection-et-reponse.md
│   ├── evaluation-vulnerabilites.md
│   ├── depannage.md
│   └── integration-suricata.md
├── configs/
│   ├── wazuh/regles_locales_exemple.xml
│   ├── sysmon/README.md
│   └── suricata/README.md
├── evidence/README.md
└── scripts/README.md
```

## Démarrage

Consulter le guide de [déploiement Docker et Wazuh](docs/deploiement-docker-wazuh.md), puis le guide d’[intégration des terminaux](docs/integration-terminaux.md).

Ce dépôt est un carnet technique et un projet de documentation. Il ne contient volontairement aucune clé privée, aucun mot de passe, aucune clé d’agent, aucun secret API ni journal brut de terminal.

## Sécurité et utilisation responsable

- Utiliser ce laboratoire uniquement sur des systèmes et réseaux dont on est propriétaire ou pour lesquels on dispose d’une autorisation explicite.
- Ne jamais publier de mots de passe, hashes de mots de passe, clés API, clés d’enrôlement, clés privées ou journaux non expurgés.
- Vérifier les adresses IP et noms d’hôtes avant de réutiliser les commandes.
- Relire chaque configuration avant déploiement.
- Valider les règles Wazuh personnalisées avant de redémarrer le Manager.
- Limiter les tests Nessus et Kali aux cibles autorisées.

## Leçons retenues

La partie la plus formatrice du projet a été le diagnostic des interactions entre services, permissions, interfaces réseau, stockage et fichiers de configuration — et pas uniquement l’exécution des commandes d’installation. Le guide de dépannage retrace les incidents rencontrés et les vérifications utilisées.

## Auteur

**WILSON Philippe**  
Stagiaire analyste SOC | Systèmes, réseaux et cybersécurité

GitHub : [@wphilippe082-oss](https://github.com/wphilippe082-oss)
