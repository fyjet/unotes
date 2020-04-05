---
layout: post
title: API Freebox python
date: "2020-04-05 16:30"
categories:
  - network
comments: true
---
https://framagit.org/sun/pyfbx

# Installation
pip3 install pyfbx

# Accès
Créer le token
```bash
pyfbx -a appli -u http://mafreebox.freebox.fr
```
copier le token dans le fichier /etc/pyfbx/token.txt
Ajouter une deuxième ligne contenant le nom de l'application

Dans l'interface freeboxos, donner tous les droits au token correspondant

# Requetage
## Lecture
```bash
pyfbx -t f:/etc/pyfbx/token.txt -u http://mafreebox.freebox.fr -c 'Wifi.Get_the_current_WiFi_global_configuration()["enabled"]'
```

## Ecriture
```bash
pyfbx -t f:/etc/pyfbx/token.txt -u http://mafreebox.freebox.fr -c 'Wifi.
```
Update_the_WiFi_global_configuration(post_data={"enabled":True})'

# Configuration de zabbix
Dans /etc/zabbix/scripts, créer le script freebox.sh avec les paramètres maxrate ou wifi