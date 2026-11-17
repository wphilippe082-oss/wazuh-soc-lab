# Intégration de Suricata — travaux en cours

## Objectif

Étendre la supervision des terminaux avec de la télémétrie IDS réseau :

```text
Trafic réseau
    -> Suricata
    -> /var/log/suricata/eve.json
    -> Agent Wazuh
    -> Wazuh Manager
    -> Indexer
    -> Dashboard
```

## État actuel

- Suricata 7.0.3 est installé sur le client Ubuntu.
- L’interface réelle est `ens33` ; le paramètre d’interface `pcap` a été modifié de `eth0` vers `ens33`.
- La commande `suricata -T -c /etc/suricata/suricata.yaml` a été exécutée, mais a signalé qu’aucun fichier ne correspondait à `/var/lib/suricata/rules/suricata.rules`.
- La stabilité du service et la transmission des événements ne sont pas encore vérifiées.

## Étapes restantes

1. Confirmer le chemin des règles configuré et les fichiers réellement disponibles.
2. Installer ou mettre à jour un jeu de règles compatible avec les outils Suricata utilisés.
3. Valider la configuration Suricata.
4. Démarrer le service et vérifier qu’il reste actif.
5. Confirmer que `eve.json` est créé et alimenté.
6. Configurer l’agent Wazuh pour surveiller le journal JSON avec les paramètres `localfile` appropriés.
7. Redémarrer l’agent et vérifier les événements Suricata dans le Dashboard.
8. Générer du trafic de test bénin et autorisé, puis confirmer la visibilité de bout en bout.

Ne pas annoncer une intégration réussie tant que le même événement de test n’a pas été suivi de `eve.json` jusqu’à Wazuh.
