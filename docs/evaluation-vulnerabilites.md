# Évaluation des vulnérabilités avec Nessus Essentials

## Périmètre

Un scan local de type Advanced Scan a été effectué avec Tenable Nessus Essentials sur le serveur Wazuh.

Le scan s’est terminé en environ 15 minutes. La capture du rapport disponible affichait 31 vulnérabilités ou résultats.

## Résultats observés

- Un résultat de sévérité Medium : **ICMP Timestamp Request Remote Date Disclosure**, CVSS 4.0.
- Plusieurs résultats informatifs ou regroupés concernant SSL/TLS.
- Des résultats informatifs liés à HTTP et SSH.
- Des résultats de détection de services et de scan SYN Nessus.

Aucun résultat Critical ou High n’était visible dans la capture fournie.

## Interprétation

Il ne faut pas résumer ce résultat par « aucune vulnérabilité ». Le scan a remonté des résultats, dont un résultat Medium. La conclusion prudente est que le rapport affiché ne présentait aucun résultat Critical ou High, mais contenait un résultat Medium ainsi que plusieurs résultats informatifs ou regroupés.

Un scan Nessus représente une évaluation à un instant donné. Il complète la supervision continue et la détection d’événements dans Wazuh, sans les remplacer.

Avant de publier le rapport complet, masquer les adresses de cible, noms d’hôtes, sorties de plugins contenant des données sensibles et éventuels identifiants.
