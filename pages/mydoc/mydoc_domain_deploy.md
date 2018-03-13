---
title: Domains Deployment
keywords: deployment, domains
last_updated: March 20, 2016
summary: "This section explains how to deploy and boot sNow! domains"
sidebar: mydoc_sidebar
permalink: mydoc_domain_deploy.html
folder: mydoc
---

Some domains have internal dependencies with others. At the time of writing, sNow! is not able to resolve these dependencies, but it will in a future release. The following commands are ordered in such a way that it can resolve the dependencies.

Each domain usually takes between one and two minutes to be deployed and booted, although this will depend on your system's performance. The aim is to be able to deploy the compute nodes and cluster infrastructure within 1-2 hours.

If you want to see what is happening during the deployment process, you can open a new shell and review the output of the log file in real time, using the following command (this is also valid during any interaction with the snow command):

```
tail -f /sNow/log/snow.log
```

In order to deploy the default domains, run the following commands:

```
snow deploy deploy01
snow deploy ldap01
snow deploy syslog01
snow deploy proxy01
snow deploy slurmdb01
snow deploy slurm01
snow deploy monitor01
snow deploy login01
```

Once you have deployed all domains, you can boot them by running the following commands:

```
# snow boot domains
[*] Domains are UP  OK
```
In order to ensure that all domains are up and running, execute the following command:

```
# snow list domains
Domain                HW status   OS status                                 Roles
slurmdb01             on          up 2 hours, 53 minutes                    slurmdbd
monitor01             on          up 3 hours, 1 minute                      monitor
slurm01               on          up 2 hours, 46 minutes                    slurmctld-master
deploy01              on          up 3 hours, 16 minutes                    deploy
syslog01              on          up 3 hours, 11 minutes                    syslog
ldap01                on          up 3 hours, 23 minutes                    ldap-master
```

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
