# D-ploiement-de-Zabbix-conteneuris-pour-le-monitoring-d-un-parc-hybride-Linux-Windows-sous-Cloud-AWS
Déploiement de Zabbix conteneurisé pour le monitoring d'un parc hybride (Linux & Windows) sous Cloud AWS

📝 Description du projet
Ce projet consiste en la mise en place d'une architecture de supervision centralisée et évolutive au sein du Cloud AWS. L'objectif principal était de garantir une visibilité totale sur la santé et les performances d'un parc informatique hétérogène, composé d'instances Linux et Windows Server.

🏗️ Architecture Technique
.Fournisseur Cloud : AWS (Amazon Web Services).
.Réseau : VPC unique avec un sous-réseau privé 10.0.1.0/24.
.Serveur Zabbix : Instance Ubuntu exécutant Zabbix Server sous Docker.
.Clients monitorés :
.Instance Linux (Ubuntu) - Agent Zabbix.
.Instance Windows Server - Agent Zabbix.

🔒 Configuration de la Sécurité (AWS Security Groups)
Pour permettre la communication entre le serveur et les agents, les règles entrantes suivantes ont été configurées :

TCP 10050 - 10051 : Flux Zabbix Agent (Communication serveur/agents).

TCP 80 / 443 : Accès à l'interface Web Zabbix.

TCP 22 & 3389 : Administration à distance (SSH & RDP).

🚀 Installation et Configuration
1. Serveur Zabbix (Docker)
Déploiement du serveur sur l'instance Linux via Docker Compose pour une gestion simplifiée des conteneurs (Zabbix Server, Web Interface, et Database).

2. Agent Windows (EC2)
L'agent a été installé directement sur l'instance Windows 10.0.1.79. Pour résoudre les problèmes de connectivité initiaux ("Timed out"), une règle spécifique a été ajoutée au pare-feu interne de Windows via PowerShell :

PowerShell

# Commande utilisée pour autoriser le trafic Zabbix sur l'instance Windows
New-NetFirewallRule -DisplayName "Zabbix_Agent" -Direction Inbound -LocalPort 10050 -Protocol TCP -Action Allow
Restart-Service "Zabbix Agent"
📊 Résultats du Monitoring
Le tableau de bord final confirme la réussite du déploiement avec une disponibilité totale du parc :

Status : Les 3 hôtes sont marqués comme "Available" (Icônes ZBX au vert).

Télémétrie : Remontée active des données de CPU, RAM et état des services système.

📸 Captures d'écran du projet
Configuration des Security Groups (AWS)
(Insère ici ton image_d0cb0c.png)

Configuration de l'Agent sur Windows Server
(Insère ici ton image_d13f72.jpg)

Dashboard Final de Supervision
(Insère ici ton image_d1b42f.jpg)
