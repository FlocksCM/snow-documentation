---
title: Domains Deployment
keywords: deployment, domains
last_updated: March 20, 2016
summary: "This section explains how to deploy and boot sNow! domains"
sidebar: mydoc_sidebar
permalink: mydoc_domain_power_control.html
folder: mydoc
---

## Boot
Once you have deployed all domains, you can boot them by running the following commands:
```
# snow boot domains
[*] Domains are UP  OK
```
## Automatic Boot (single sNow! server)
If you want your domains to run automatically when the snow01 system is rebooted, just add links to their XEN config files to the /etc/xen/auto directory. You will find the config files on /sNow/snow-tools/etc/domains.
```
# ls -l
lrwxrwxrwx 1 root root   41 nov 13 20:13 deploy01.cfg -> /sNow/snow-tools/etc/domains/deploy01.cfg
lrwxrwxrwx 1 root root   40 nov  9 10:38 login01.cfg -> /sNow/snow-tools/etc/domains/login01.cfg
lrwxrwxrwx 1 root root   42 jul 28 13:11 monitor01.cfg -> /sNow/snow-tools/etc/domains/monitor01.cfg
lrwxrwxrwx 1 root root   40 jul 28 13:11 proxy01.cfg -> /sNow/snow-tools/etc/domains/proxy01.cfg
lrwxrwxrwx 1 root root   40 jul 28 13:11 slurm01.cfg -> /sNow/snow-tools/etc/domains/slurm01.cfg
lrwxrwxrwx 1 root root   42 jul 28 13:11 slurmdb01.cfg -> /sNow/snow-tools/etc/domains/slurmdb01.cfg
lrwxrwxrwx 1 root root   41 jul 28 13:11 syslog01.cfg -> /sNow/snow-tools/etc/domains/syslog01.cfg
```
## Automatic Boot (sNow! server in HA cluster mode)
