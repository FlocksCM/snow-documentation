---
title: Selecting domains
tags: [domains]
keywords: domains, roles, vms, containers
last_updated: July 3, 2016
summary: "You can implement advanced conditional logic that includes if statements, or statements, unless, and more. This conditional logic facilitates single sourcing scenarios in which you're outputting the same content for different audiences."
sidebar: mydoc_sidebar
permalink: mydoc_selecting_domains.html
folder: mydoc
---

Note: In this documentation we are referring to the independent systems running different sNow services as "domains" or "virtual machines" or "containers". 

Each domain has one or more roles. Each role defines a service or a subset of related services. The roles are scripts which automate the process of deploying a new domain and also configure them based on the parameters available in the main sNow! configuration file (snow.conf). The following command line, provides a short description for each available role.

```
snow list roles
```
The active-domains.conf file provides a list of sNow! domains and the associated roles which define the services provided by each domain. You can modify this list at your convenience. For instance, you may want to only a deploy system for installing bare metal nodes or you may want a full system with batch queue manager, monitoring, centralized syslog, ldap user authentication etc.

The first column of the ```active-domains.conf``` file contains the hostname of the domain, the second column contains the role or list or roles associated with the domain. Each domain can have one or more roles. In the case of multiple roles, use a comma separated list with no spaces.

If a domain has multiple roles, each role script will be executed in the order defined in domains.conf. This file will be automatically generated with the "snow init" command, as described in Section 6.4. The current available roles are located in ```/sNow/snow-tools/etc/roles.d```. This document provides information of all the available roles.

The roles are responsible for populating most of the files located in ```$SNOW_CONF``` but there is no mechanism to syncronize those files. Whatever is modified in the  ```$SNOW_CONF```, inside a domain or inside a compute node, is not going to be syncronized across the cluster. In order to do that, you should consider using a configuration manager like Ansible, CFEngine or Puppet. Given the complexity involved, configuration managers are outside the scope of sNow!

To continue with the sNow installation:
Copy the example file as your working file

```
cp -p /sNow/snow-tools/etc/active-domains.conf-example /sNow/snow-tools/etc/active-domains.conf
```
The following lines represent a typical example of an active-domains.conf configuration file. Add or delete lines as required by your installation needs.

```
hostname    roles
-----------------------------
snow01      snow
ldap01      ldap-master
slurm01     slurmctld-master
monitor01   monitor
syslog01    syslog
login01     login
slurmdb01   slurmdbd
proxy01     proxy
deploy01    deploy
```
{% include links.html %}