# Déploiement de Wazuh avec Docker Compose

## Pourquoi Docker ?

Docker a été choisi pour isoler les composants Wazuh, simplifier leur cycle de vie et rendre le laboratoire plus facile à reproduire. Docker Compose fournit un mécanisme commun pour démarrer et inspecter le Manager, l’Indexer et le Dashboard.

## Préparation du serveur

La VM serveur Wazuh utilise Ubuntu Server 24.04. Docker Engine et le plugin Compose ont été installés depuis le dépôt officiel Docker pour Ubuntu.

Séquence générale d’installation :

1. Mettre APT à jour et installer les prérequis (`ca-certificates`, `curl`).
2. Créer le répertoire `/etc/apt/keyrings`.
3. Télécharger la clé GPG officielle Docker et définir les permissions de lecture.
4. Ajouter le dépôt APT officiel Docker.
5. Actualiser les métadonnées des paquets.
6. Installer Docker Engine, le client, containerd, Buildx et le plugin Compose.
7. Activer et démarrer Docker.
8. Vérifier le service et exécuter l’image de test `hello-world`.

Documentation officielle Docker : https://docs.docker.com/engine/install/ubuntu/

## Déploiement Wazuh

Le déploiement suit la documentation officielle Wazuh Docker :

https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html

Le laboratoire utilise le répertoire de déploiement `single-node` du dépôt Wazuh Docker. Commandes usuelles à exécuter depuis ce répertoire :

```bash
docker compose up -d
docker compose ps
docker compose logs --tail=100
```

Utiliser `docker compose restart <service>` lorsqu’un redémarrage ciblé suffit. Éviter les opérations destructrices sur les volumes lorsqu’ils contiennent des données à conserver.

## Vérifications

Vérifier que les conteneurs attendus fonctionnent :

```bash
docker compose ps
```

Valider les règles du Manager avant de le redémarrer :

```bash
docker compose exec -u 0 wazuh.manager \
  /var/ossec/bin/wazuh-analysisd -t
```

Une validation réussie se termine sans erreur de syntaxe. Vérifier le code de retour et les journaux au lieu de supposer qu’une commande silencieuse signifie qu’elle a réussi.

## Versions

Le laboratoire a été documenté avec Wazuh 4.14.7 et OpenSearch 2.19.5 au moment des tests. Consulter la documentation officielle avant d’utiliser des versions plus récentes : les commandes peuvent évoluer.
