---
layout: "post"
title: "Compiler grafana sur debian 9.5 32 bits "
date: "2018-10-23 14:38"
tags: [grafana,debian]
categories:
- linux
---
# Prérequis
## Installer la dernière version de go
https://golang.org/doc/install
## installer fpm
```bash
apt-get install ruby-dev build-essential
gem install fpm
```

# Compiler grafana
le compte root est utilisé

```bash
go get github.com/grafana/grafana
cd ~/go/src/github.com/grafana/grafana
go run build.org setup
go run build.org build

npm install -g yarn
yarn install --pure-lockfile
yarn watch
go get github.com/Unknown/bra
```

lancer l'instance de dev
```
~/go/bin/bra run
```
connection sur http://localhost:3000, user admin, pass admin

créer le packqge pour debian:
```
go run build.org pkg-deb
cd dist
```
# Installation
```
dpkg -i --force-architecture grafana_5.4.0-1540273471pre1_386.deb
useradd -G grafana grafana
cp conf/defaults.ini /etc/grafana/grafana.ini
```
