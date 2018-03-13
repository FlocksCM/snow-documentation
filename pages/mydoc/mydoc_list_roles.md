---
title: List Roles
tags: [domains]
keywords: domains, roles, vms, containers
last_updated: July 3, 2016
sidebar: mydoc_sidebar
permalink: mydoc_list_roles.html
folder: mydoc
---

Note: In this documentation we are referring to the independent systems running different sNow services as "domains" or "virtual machines" or "containers".

Each domain has one or more roles. Each role defines a service or a subset of related services. The roles are scripts which automate the process of deploying a new domain and also configure them based on the parameters available in the main sNow! configuration file (snow.conf). The following command line, provides a short description for each available role.

```
snow list roles
Role Name                     Description
-------------                 -----------
beegfs                        This role installs BeeGFS server
builder                       Minimal OS to compile code and generate debian packages
cfengine                      Installs CFenfine upon the new guest system
cfs                           Allows to setup NFS client and cluster file system clients (experimental)
deploy                        Installs the required to deploy OS and boot OS via PXE and TFTP. It also provides DHCP and DNS.
docker                        Installs Docker Community Edition and Docker compose
gateone                       Installs Gate One, a web based terminal emulator and SSH client
gdm                           Installs GDM with VNC support.
icinga                        Installs standard HPC alert tool: Icinga.
ldap-master                   Installs LDAP master server.
login                         Installs login node with workload manager clients and creates a new SSH instance allocated in 22022/TCP dedicated to end users.
minimal                       Installs a minimal OS.
monitor                       Installs standard HPC monitoring tools : Ganglia and Icinga.
openvpn_as                    Installs OpenVPN Access Server (2 free client connections for testing purposes).
proxy                         Installs proxy server for HTTP(S),FTP and other relay services (NTP, mail).
puppet                        Installs puppet upon the new guest system.
puppet-master                 Installs puppet master server.
slurmctld-master              Installs Slurm mster server and it can setup the system based on the snow.conf
slurmdbd                      Installs MySQL server and SlurmDB server. It can setup the system based on the snow.conf
snow                          Base role responsible to setup all the required clients and generate the configuration files.
snow_reference_template       Template to help sNow! users to develop their own roles quickly.
snow_template                 Role used to generate the basic image system.
swarm-manager                 Installs and setup Docker Swarm to accommodate docker based services.
swarm-worker                  Installs and setup Docker Swarm to accommodate docker based services.
syslog                        Installs a centralised Rsyslog server to consolidate the logs of the whole cluster.
torque-master                 Installs Torque and Maui. It also generates the install packages for the compute nodes.
xdm                           Installs XDM with VNC support.
```

The active-domains.conf file provides a list of sNow! domains and the associated roles which define the services provided by each domain. You can modify this list at your convenience. For instance, you may want to only a deploy system for installing bare metal nodes or you may want a full system with batch queue manager, monitoring, centralised syslog, ldap user authentication etc.

The first column of the ```active-domains.conf``` file contains the hostname of the domain, the second column contains the role or list or roles associated with the domain. Each domain can have one or more roles. In the case of multiple roles, use a comma separated list with no spaces.

If a domain has multiple roles, each role script will be executed in the order defined in domains.conf. This file will be automatically generated with the "snow init" command, as described in Section 6.4. The current available roles are located in ```/sNow/snow-tools/etc/roles.d```. This document provides information of all the available roles.

The roles are responsible for populating most of the files located in ```$SNOW_CONF``` but there is no mechanism to synchronise those files. Whatever is modified in the  ```$SNOW_CONF```, inside a domain or inside a compute node, is not going to be synchronised across the cluster. In order to do that, you should consider using a configuration manager like Ansible, CFEngine or Puppet. Given the complexity involved, configuration managers are outside the scope of sNow!

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
