# Notes de dépannage

Ce document consigne les incidents rencontrés et les méthodes de récupération utilisées. Il ne remplace pas l’inspection des journaux et de la configuration en cours.

## 1. Espace disque et LVM

L’espace alloué à la VM a été augmenté, puis la partition invitée, le volume physique et le volume logique ont été étendus. Les noms exacts des périphériques doivent être vérifiés avec `lsblk` avant toute commande de stockage.

Commandes d’inspection utiles :

```bash
lsblk
df -h
sudo pvs
sudo vgs
sudo lvs
```

Ne pas copier une commande `growpart`, `pvresize` ou `lvextend` sans avoir confirmé le disque et la partition ciblés.

## 2. Configuration Netplan

Un problème de connectivité de la VM a été attribué à une configuration Netplan incorrecte. Vérifier l’indentation YAML et les interfaces actives avant toute application.

```bash
ip -br addr
ip route
sudo netplan get
```

Conserver un accès console lors de la modification de la configuration réseau d’une VM.

## 3. Erreur HTTP 500 du Dashboard après changement de mot de passe

Après le changement du mot de passe administrateur, un problème côté navigateur a été résolu en supprimant les cookies puis en ouvrant une nouvelle session. C’est la résolution observée dans ce laboratoire ; elle ne doit pas être considérée comme l’explication de toute erreur HTTP 500.

## 4. API Wazuh Manager indisponible

Une règle personnalisée mal formée dans `local_rules.xml` et une erreur de permission empêchaient les services d’analyse du Manager de démarrer. Le fichier présent dans le conteneur a été temporairement remplacé par une règle minimale valide, vérifié avec `wazuh-analysisd -t`, puis le Manager a été redémarré.

La règle PowerShell prévue doit être reconstruite avec soin et validée avant déploiement.

## 5. Configuration des agents et utilisateurs

Certains problèmes initiaux étaient liés à une configuration incorrecte des utilisateurs et de leur indexation. Vérifier à la fois les rôles Wazuh et les rôles de sécurité de l’Indexer, puis confirmer les permissions avec un compte de test non administrateur.

## 6. Mauvaise interface Suricata (en cours)

Suricata référençait initialement `eth0`, alors que l’interface réelle du terminal Ubuntu était `ens33`. Le paramètre d’interface `pcap` a été modifié dans `/etc/suricata/suricata.yaml`.

Le test de configuration a ensuite signalé qu’aucun fichier ne correspondait au chemin de règles :

```text
/var/lib/suricata/rules/suricata.rules
```

La stabilité du service, l’installation du jeu de règles et la collecte de `eve.json` restent à valider. Ne pas déclarer l’intégration Suricata terminée tant qu’un événement n’est pas visible dans Wazuh.
