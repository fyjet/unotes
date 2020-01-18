---
layout: post
title: PlatformIO
date: "2020-01-12 14:55"
categories:
  - programming
comments: true
---

# Change project directory

Project default location is **~/Documents/PlatformIO/Project**, see [PlatfomIO documentation](http://docs.platformio.org/en/latest/userguide/cmd_settings.html#projects-dir)

```
user@computer$ platformio settings get
Name                       Current value [Default]                                  Description
-------------------------  -------------------------------------------------------  --------------------------------------------------------
auto_update_libraries      No                                              Automatically update libraries (Yes/No)
auto_update_platforms      No                                              Automatically update platforms (Yes/No)
check_libraries_interval   7                                               Check for the library updates interval (days)
check_platformio_interval  3                                               Check for the new PlatformIO interval (days)
check_platforms_interval   7                                               Check for the platform updates interval (days)
enable_cache               Yes                                             Enable caching for API requests and Library Manager
enable_telemetry           Yes                                             Telemetry service <http://bit.ly/pio-telemetry> (Yes/No)
force_verbose              No                                              Force verbose output when processing environments
projects_dir               /home/user/Documents/PlatformIO/Projects        Default location for PlatformIO projects (PIO Home) [/home/user/Documents/PlatformIO/Projects]
strict_ssl                 No                                              Strict SSL for PlatformIO Services
```

Move PlatformIO dorectory to /home/user

```
user@computer$ platformio settings set projects_dir /home/user/PlatformIO/Projects
```
