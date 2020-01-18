---
layout: post
title: Syncthing on low resources computer
date: "2020-01-12 14:55"
categories:
  - linux
---

On low resources like banapi, some tuning alow syncthing to run without swapping and using all resources

# After syncthing start, folders scan

## Syncthing configuration

In syncthing configuration file, set "setLowPriority" to false, according to [syncthing documentation](https://docs.syncthing.net/users/config.html#set-low-priority)

## Systectl configuration

Use Systemctl to restrict usage to one CPU, and nice to 19.
Edit /etc/systemd/system/syncthing@syncthing.service and add in service section:

```
Environment="GOMAXPROCS=1"
Nice=20
```

# After scan, limit bandwidth

Set upload and download rate to 2 KiB in GUI
