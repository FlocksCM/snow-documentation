---
title: Custom Roles
summary: "This section explains how to develop new custome sNow! doamin roles."
last_updated: July 3, 2016
sidebar: mydoc_sidebar
permalink: mydoc_custom_roles.html
folder: mydoc
---

Custom hooks
Path
Template base
Copier les notes que hi ha dins del template
Custom first boot hooks
Explicar perquè 
En quin moment s’executa


## Create a new role
sNow! roles are shell scripts which make them easy to develop and to understand. Keep in mind the following tips and tricks which will help you  develop new roles:

* Use environment variables defined in snow.conf, and extend them if you need new variables to work with
* When you generate a new configuration file, remember to copy the file in the deployed system and also ```/sNow/snow-confispace/system_files```. If there is a file in this path, avoid overwriting it and use it to setup your new system. This will help you to setup continuous integration into your system.
* Place comments inside complex sections of the code in order to help people to understand what you are doing
* Use chroot ${prefix} to run commands inside the new deployed system
* Use installDebianPackage ${prefix} to install packages
* Use variables inside the templates that easy to recognise and replace ```__NAME_OF_VARIABLE__```
* Use sed with pipe symbols rather than slash symbols. This will help you to replace UNIX paths.
* In this directory (```/sNow/snow-configspace/system_files```) you will find the configuration files used in the different roles. Some of them are common for many roles. Some of them are common to all of them. This is a list of files used for the preinstalled roles.

```
[all]
$SNOW_CONF/system_files/etc/dnsmasq.conf
$SNOW_CONF/system_files/etc/ganglia/gmond_snow.conf
$SNOW_CONF/system_files/etc/hosts
$SNOW_CONF/system_files/etc/issue
$SNOW_CONF/system_files/etc/issue.net
$SNOW_CONF/system_files/etc/motd
$SNOW_CONF/system_files/etc/ntp.conf
$SNOW_CONF/system_files/etc/profile.d
$SNOW_CONF/system_files/etc/repos
$SNOW_CONF/system_files/etc/resolv.conf
$SNOW_CONF/system_files/etc/rsyslog.d/50-default.conf
$SNOW_CONF/system_files/etc/ssh
$SNOW_CONF/system_files/etc/sssd/sssd.conf

[snow]
$SNOW_CONF/system_files/etc/active-domains.conf
$SNOW_CONF/system_files/etc/domains.conf
$SNOW_CONF/system_files/etc/exports.d/snow.exports
$SNOW_CONF/system_files/etc/snow.conf

[proxy]
$SNOW_CONF/system_files/etc/squid3
$SNOW_CONF/system_files/etc/ntp_server.conf

[ldap-master]
$SNOW_CONF/system_files/etc/ldap

[slurmctld-master]
$SNOW_CONF/system_files/etc/munge
$SNOW_CONF/system_files/etc/slurm
$SNOW_CONF/system_files/etc/slurm.env

[monitor]
$SNOW_CONF/system_files/etc/ganglia/gmetad.conf
$SNOW_CONF/system_files/etc/ganglia-webfrontend

[syslog]
$SNOW_CONF/system_files/etc/rsyslog.conf

[login]
$SNOW_CONF/system_files/etc/munge
$SNOW_CONF/system_files/etc/slurm
$SNOW_CONF/system_files/etc/slurm.env

[cfs]

[slurmdbd]
$SNOW_CONF/system_files/etc/munge
$SNOW_CONF/system_files/etc/slurm
$SNOW_CONF/system_files/etc/slurm.env

[proxy]
$SNOW_CONF/system_files/etc/exim4

[deploy]
$SNOW_CONF/boot
```
