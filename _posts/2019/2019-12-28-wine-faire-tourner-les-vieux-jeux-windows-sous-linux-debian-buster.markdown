---
layout: "post"
title: "Wine, faire tourner les vieux jeux windows sous linux (debian buster)"
date: "2019-12-28 14:30"
categories:
- linux
---
# Préparation de la plateforme
## Installer et configurer wine
Installer wine
Créer un répertoire pour mettre plusieurs prefix (versions de windows)
mkdir /opt/wineprefixes

Créer un prefix pour windows 98
```bash
mkdir /opt/wineprefixes/win95-Genereric
WINEPREFIX=/opt/wineprefixes/win95-Generic winecfg
```
Installer cdemu
cdemu permet de jouer en mode "no-cd" en émulant le lecteur de cdrom. Les images ISO peuvent être montées et vues comme le cd par le jeu, et eviter l'usure des cdrom souvent anciens.

## Télécharger les sources cdemu et compiler
Installer les packages générés
Installer un jeu

Extraire l'iso, le charger dans cdemu et monter le cdrom
Effectuer l'installation comme sous windows

# Lanceur pour le jeu
```bash
#!/bin/bash -x
export WINEPREFIX=/opt/wineprefixes/win95-Generic

cdemu load 0 /opt/wineprefixes/ISO/deltaforce2.iso
sudo mount -t iso9660 /dev/sr1 /media/cdrom0

cd "/opt/wineprefixes/win95-Generic/drive_c/Program Files/NovaLogic/Delta Force 2"
wine Df2.exe

sudo umount /dev/sr1
cdemu unload 0
```
