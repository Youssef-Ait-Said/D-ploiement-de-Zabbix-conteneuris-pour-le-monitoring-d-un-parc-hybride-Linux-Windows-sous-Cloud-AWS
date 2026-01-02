# Déploiement de Zabbix conteneurisé pour le monitoring d'un parc hybride (Linux & Windows) sous Cloud AWS

## 📝 Description du projet
Ce projet consiste en la mise en œuvre d'une architecture de supervision centralisée utilisant **Zabbix 7.0**. L'objectif est de monitorer en temps réel les performances et la disponibilité d'un parc informatique hybride (Linux & Windows) hébergé sur des instances **Amazon EC2**.

## 🏗️ Architecture de l'Infrastructure
* **Fournisseur Cloud :** Amazon Web Services (AWS).
* **Réseau :** Déploiement au sein d'un VPC avec un adressage privé en `10.0.1.0/24`.
* **Serveur de Monitoring :** Zabbix Server déployé via Docker sur une instance Linux.
* **Nœuds supervisés :**
    * **Serveur Zabbix :** Auto-monitoring du conteneur Linux.
    * **Client_Linux_Zabbix :** Instance Ubuntu avec Agent Zabbix.
    * **Client_Windows_Zabbix :** Instance Windows Server (IP : `10.0.1.79`) avec Agent Zabbix.

## 🔒 Configuration de la Sécurité (AWS Security Groups)
Les flux réseau ont été sécurisés via des Security Groups AWS pour autoriser uniquement les ports nécessaires :
* **TCP 10050 :** Trafic entrant pour les agents Zabbix (Zabbix Agent Listen Port).
* **TCP 80 :** Accès HTTP à l'interface Dashboard de Zabbix.
* **TCP 22 & 3389 :** Accès de gestion à distance SSH (Linux) et RDP (Windows).

## 🚀 Mise en œuvre technique

### Configuration de l'Agent Windows
Pour permettre la remontée des données depuis l'instance Windows EC2, les étapes suivantes ont été réalisées :
1. Installation de l'Agent Zabbix sur l'instance `10.0.1.79`.
2. Configuration du pare-feu Windows Defender pour autoriser le port 10050.
3. Résolution des erreurs de service via PowerShell pour assurer le statut **Running** de l'agent.

```powershell
# Commande d'ouverture du port dans le pare-feu interne
New-NetFirewallRule -DisplayName "Zabbix_Agent" -Direction Inbound -LocalPort 10050 -Protocol TCP -Action Allow
