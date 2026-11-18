# Scripts

//Installation de DOCKER:

# 1. Installer les dépendances
sudo apt update
sudo apt install apt-transport-https ca-certificates curl

# 2. Créer le dossier des clés
sudo install -m 0755 -d /etc/apt/keyrings

# 3. Télécharger la clé Docker
sudo curl -fSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

# 4. Donner les permissions de lecture
sudo chmod a+r /etc/apt/keyrings/docker.asc

# 5. Ajouter le dépôt Docker
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 6. Actualiser les dépôts
sudo apt update

# 7. Installer Docker Engine
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 8. Activer le service
sudo systemctl enable --now docker

# 9. Vérifier le service
sudo systemctl status docker

# 10. Tester Docker
sudo docker pull hello-world
sudo docker run hello-world

°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°
//INSTALLATION DU DEPOT GIT D'INTEGRATION de WAZUH IN DOCKER

https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html

$$Error 500 APRES changement du mot de passe admin: solution vider les coockies
$$probleme LVM d'espace partitionner: reetendre la VM sur l'espace de stockage alouer depuis vmware(lsblk)
$$les commandes de resolution:
sudo journalctl --vacuum-size=50M
sudo apt-get clean
docker system prune -f
sudo growpart /dev/sda 3
sudo pvresize /dev/sda3
sudo lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
cd ~/wazuh-docker/single-node/
docker compose down
docker compose up -d

°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°
//PASSER A LA MODIFICATION DU MOTS DE PASSES ADMIN
https://documentation.wazuh.com/current/deployment-options/docker/changing-default-password.html

°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°°
//INTEGRATION DES AGENTS ET UTILISATEURS WAZUH
 
Nous commencerons par créer les utilisateur soc analyste qui seront affecter a un role readonly du wazuh(API) et de l'indexeur RBAC.Avec un URL d'enrollement pour pouvoir superviser l'etat de securité de leur propre machine.
NB: ses utilisateur sont fictif et inutilisable.

Reseau wifi: SOC_TEAM   Pass: 012345678
IP WAZUH: 10.126.240.110
Identify: soc-analyst-01
Pass: G3r3mi@002
url d'enrollement de l'Agent_Soc1:

Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.7-1.msi -OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='10.126.240.110' WAZUH_AGENT_GROUP='Windows' WAZUH_AGENT_NAME='Agent_Soc1' 

NET START Wazuh

Reseau wifi: SOC_TEAM   Pass: 12345678
IP WAZUH: 10.126.240.110
Identify: soc-analyst-02 
Pass: R@ch3l3@002

url d'enrollement de l'Agent_Soc2:

Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.7-1.msi -OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='10.126.240.110' WAZUH_AGENT_GROUP='Windows' WAZUH_AGENT_NAME='Agent_Soc2' 

NET START Wazuh


//CREATION D'UNE REGLE DE DETECTION DE COMMANDE POWERSHELL

*docker cp ./local_rules.xml single-node-wazuh.manager-1:/var/ossec/etc/rules/local_rules.xml
pour créer des regle isoler du fichier de configuration de regle par defaut de wazuh (eviter de casser apres une mise a jour).

*Installer sysmon et sysmonconfig.xml' Outil de reccuperation d'evenement Windows '

//RECCUPERATION D'ESPACE DISK 
*Ubuntu:
sudo apt clean
sudo apt autoremove --purge
sudo journalctl --vacuum-time=2d
sudo truncate -s 0 /var/log/suricata/eve.json
sudo truncate -s 0 /var/log/ufw.log

*Server wazuh:
sudo apt clean
sudo apt autoremove --purge
sudo journalctl --vacuum-time=2d
df -h



NB:
.la seconde erreur rencontrer était issus d'un mauvaise indexation des utilisateur(*).
.La troixieme erreur était issus d'une mauvaise configuration du fichier de configuration netplan.
.La quatrieme erreur de API:La cause initiale est identifiée : la règle personnalisée 255000 dans local_rules.xml provoquait une erreur de syntaxe sur win.eventdata.image, et les journaux signalaient également un problème de permissions sur ce fichier.
.Apres la creation d'une regle , il faut s'assurer que wazuh accepte cette regle avant de la deployer:
docker compose exec -u 0 wazuh.manager /var/ossec/bin/wazuh-analysisd -t

*Referer vous au capture d'ecran pour plus de precision































copie-coller:
sudo apt update
sudo apt install -y open-vm-tools open-vm-tools-desktop  (xclip)
avec activation de guest isolation

install ssh:
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh




