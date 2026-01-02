# Deploiement de Zabbix pour le monitoring d un parc hybride sur AWS

## Description du projet
Ce projet consiste en la mise en oeuvre d une architecture de supervision centralisee utilisant Zabbix 7.0. L objectif est de surveiller en temps reel les performances et la disponibilite d instances Linux et Windows hebergees sur Amazon EC2.

<img width="1536" height="1024" alt="Architecture_generale" src="https://github.com/user-attachments/assets/a94182b2-258d-43b6-8d9f-9b9c031c7612" />
## Architecture de l infrastructure
* Fournisseur Cloud : Amazon Web Services (AWS)
* Reseau : Deploiement dans un VPC avec un adressage prive en 10.0.1.0/24
* Serveur de Monitoring : Zabbix Server installe via Docker sur une instance Ubuntu
* Hotes supervises :
    * Client_Linux_Zabbix : Instance Ubuntu avec Agent Zabbix
    * Client_Windows_Zabbix : Instance Windows Server avec Agent Zabbix (10.0.1.79)

## Configuration de la securite (AWS Security Groups)
Les flux reseau sont autorises via les Security Groups AWS pour les ports suivants :
* TCP 10050 : Communication entrante vers les agents Zabbix
* TCP 80/443 : Acces a l interface Web Zabbix
* TCP 22 / 3389 : Flux d administration SSH et RDP

## Mise en oeuvre technique

### 1. Configuration du Client Linux
L agent a ete deploye sur l instance Ubuntu selon les etapes suivantes :
* Installation du paquet zabbix-agent via le gestionnaire de paquets
* Modification du fichier /etc/zabbix/zabbix_agentd.conf pour renseigner l IP du serveur Zappix
* Activation et demarrage du service zabbix-agent

### 2. Configuration du Client Windows
L agent a ete deploye sur l instance Windows Server avec les etapes suivantes :
* Installation de l agent Zabbix via l installeur MSI
* Configuration du Hostname (Client_Windows_Zabbix) et de l IP du serveur Zappix

### Acces a l interface web de Zabbix via http://ip_Ec2_Zabbix:80

<img width="959" height="539" alt="interface_Zabbix_Web" src="https://github.com/user-attachments/assets/a88828e0-e101-4a80-b407-2aeaa9047bb3" />

## Interface Web Zabbix
Voici le tableau de bord montrant la disponibilite des hotes (Serveur Zabbix, Client Linux et Client Windows). Le statut vert "ZBX" confirme que la communication est operationnelle :

<img width="959" height="539" alt="Activation_machine_windows_zabbix_web" src="https://github.com/user-attachments/assets/08a68cf3-1a78-4268-8ad5-a71e3ecca34d" />

