# Exercices de détection et de réponse

## Règles Wazuh personnalisées

Le laboratoire a permis d’étudier les règles personnalisées dans `local_rules.xml`, notamment un cas d’usage de détection de processus PowerShell sous Windows.

Conserver les personnalisations locales séparées du jeu de règles par défaut de Wazuh. Avant d’appliquer une modification, valider la configuration depuis le conteneur Manager :

```bash
docker compose exec -u 0 wazuh.manager \
  /var/ossec/bin/wazuh-analysisd -t
```

Ne redémarrer le Manager qu’après une validation réussie.

## Leçon de dépannage : API du Manager indisponible

Une règle personnalisée dans `local_rules.xml` contenait une déclaration de champ invalide impliquant `win.eventdata.image`. Les journaux signalaient également une erreur de permission lors de l’ouverture du fichier des règles locales. Cette configuration invalide empêchait les services d’analyse du Manager et l’API de démarrer correctement.

La récupération a consisté à remplacer temporairement le fichier de règles locales présent dans le conteneur par une règle de test minimale valide, à le vérifier avec `wazuh-analysisd -t`, puis à redémarrer le Manager. L’API est alors redevenue disponible.

Important : la règle temporaire de récupération n’était pas la règle initiale de détection PowerShell. Cette dernière doit être retravaillée et validée avant d’être redéployée.

## Supervision SSH

Les échecs d’authentification SSH provenant de Kali ont été collectés par l’agent Ubuntu. L’exercice a permis de distinguer les événements individuels d’échec d’authentification des règles de corrélation qui détectent les tentatives répétées. Un grand nombre d’échecs individuels ne prouve pas, à lui seul, qu’une règle de corrélation de type brute force s’est déclenchée.
