# Automatisation d'une infrastructure réseau avec Ansible

Projet Ansible : configuration automatisée de 3 équipements Cisco IOS simulés dans GNS3.

## Architecture du labo

- Contrôleur : VM Ubuntu (VMware) avec Ansible, IP 192.168.56.10
- R1 (192.168.56.11) et R2 (192.168.56.12) : routeurs Cisco 7200 (IOS 12.4)
- SW1 (192.168.56.21) : switch Cisco IOU L2
- Connexion : SSH (network_cli), utilisateur `cisco`

## Contenu

| Fichier | Tâche |
|---|---|
| 01_ping.yml | Connectivité |
| 02_facts.yml | Facts (version OS, numéro de série) -> reports/versions_serie.txt |
| 03_backup.yml | Sauvegarde des running-config -> backups/<horodatage>/ |
| 04_ntp.yml | NTP (variable globale ntp_servers) |
| 05_users_ssh.yml | Utilisateur admin (mot de passe haché) et SSH |
| 06_vlans.yml | VLANs en boucle (groupe switches) |
| 07_interfaces.yml | Adresses IP depuis host_vars/R1.yml et R2.yml |
| 08_banner.yml | Bannière MOTD via templates/motd.j2 |
| 09_snmp_audit.yml | Audit SNMP (échec ou correction) |
| 10_rapport.yml | Rapport final -> reports/rapport.md |
| site.yml | Exécute tout dans l'ordre |

## Utilisation

    pip install ansible paramiko
    ansible-galaxy collection install cisco.ios ansible.netcommon ansible.utils
    ansible-playbook site.yml

Audit SNMP en mode correction :

    ansible-playbook 09_snmp_audit.yml -e snmp_remediate=true

## Remarques

- Les mots de passe sont en clair dans l'inventaire : c'est un laboratoire. L'utilisateur admin `netadmin` est créé avec un mot de passe haché (type 5).
- Les IOS anciens imposent un paramiko compatible SHA-1 (version 3.4.1).
- Dossiers `backups/` et `reports/` : exemples de sorties produites par les playbooks.
