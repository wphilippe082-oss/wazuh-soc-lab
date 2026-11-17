# Intégration des terminaux et collecte de télémétrie

## Terminal Windows

Le terminal Windows 11 a été enrôlé à l’aide du paquet MSI de l’agent Wazuh. L’agent a été configuré avec l’adresse du Manager, un groupe et un nom d’agent unique.

Sysmon a également été installé et configuré avec un fichier XML afin d’enrichir la télémétrie Windows. Lorsque la configuration de l’agent le permet, Wazuh collecte les journaux Windows, notamment Security et Sysmon.

### Exemple de validation

L’événement Windows Security `4624` a été identifié localement puis retrouvé dans Wazuh. Un événement capturé a été associé à la règle Wazuh `60106` (`Windows Logon Success`).

Point d’attention : l’événement observé présentait `LogonType = 5`, qui correspond à une ouverture de session de service. Il ne faut donc pas le présenter comme une connexion interactive d’un utilisateur.

## Terminal Ubuntu

Le client Ubuntu a été enrôlé avec l’agent Wazuh. Les exercices ont porté sur :

- Security Configuration Assessment (SCA).
- Collecte des événements d’authentification SSH.
- Configuration de File Integrity Monitoring (FIM).
- Collecte des journaux UFW.
- Collecte des journaux Fail2ban.

## Exercice UFW et Fail2ban

Fail2ban a été configuré pour surveiller les échecs répétés d’authentification SSH. Lors d’un test contrôlé, Kali Linux a généré des tentatives SSH échouées vers le client Ubuntu. Après le seuil configuré de deux tentatives, Fail2ban a banni l’adresse IP source et ajouté une règle de blocage dans UFW.

L’agent Wazuh a été configuré pour collecter les journaux locaux pertinents afin que les événements puissent être examinés de manière centralisée.

## Publication sécurisée

Ne pas publier les clés d’agent, mots de passe d’enrôlement, identifiants du Manager, certificats privés ou journaux non expurgés. Masquer les noms d’hôtes, noms d’utilisateurs et adresses IP si les captures révèlent des informations à garder privées.
