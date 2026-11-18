# Architecture et topologie réseau

## Conception

Le laboratoire repose sur un déploiement Wazuh Single-Node installé sur Ubuntu Server 24.04. Le Manager, l’Indexer et le Dashboard fonctionnent dans des conteneurs Docker pilotés par Docker Compose.

Les agents des terminaux transmettent leur télémétrie au Manager. Celui-ci analyse les événements à l’aide des décodeurs et règles ; les alertes sont ensuite indexées et consultées dans le Dashboard.

## Composants

- **Wazuh Manager :** communication avec les agents, analyse des événements et génération d’alertes.
- **Wazuh Indexer :** stockage et recherche des alertes.
- **Wazuh Dashboard :** interface d’investigation.
- **Windows 11 :** agent Wazuh, Sysmon et collecte des événements Windows Security.
- **Client Ubuntu :** agent Wazuh, exercices SCA/FIM, journaux SSH, UFW et Fail2ban, Suricata
- **Kali Linux :** source de tests contrôlés et autorisés.
- **Nessus Essentials :** outil d’évaluation des vulnérabilités.

## Réseau

| Nœud | Adresse / interface | Remarque |
|---|---|---|
| Serveur Wazuh | `192.168.85.129`, NAT `ens33` | Réseau de laboratoire côté Manager |
| Serveur Wazuh | `10.126.210.210`, ponté `ens37` | Chemin de gestion supplémentaire |
| Client Ubuntu | `192.168.85.130` | Terminal Linux |
| Kali | `192.168.85.131` | Source de tests autorisés |
| Windows 11 | DHCP / réseau hôte | Terminal Windows |



## Limites et frontières de confiance

Il s’agit d’un environnement pédagogique, et non d’une architecture de production hautement disponible. Le modèle Single-Node concentre les services sur une seule VM. Les accès ponté et NAT doivent être réévalués avant toute exposition en dehors du réseau isolé du laboratoire.


