---
title: Check List
tags: [formatting]
keywords: check list
last_updated: July 3, 2016
summary: "This section provides a check list which help to process the installation of an HPC cluster"
sidebar: mydoc_sidebar
permalink: mydoc_check_list.html
folder: mydoc
---

# Checklist before installation
This section provides a reaonable list of parameters that need to known and actions to be performed before to initiate the installation.

## Network layout
- [ ] Network configuration
- [ ] Public IPs and network configuration for those IPs (DHPC or static)
- [ ] DMZ IPs and network configuration for those IPs (DHPC or static)
- [ ] Defined networks and subnetworks. Example:

|                              | network       |
|------------------------------|---------------|
| Administration               | 10.0.0.0/16   |
| Management (IPMI, BMC, iLO)  | 10.1.0.0/16   |
| Low latency fabric           | 10.2.0.0/16   |
| DMZ (optional)               | 172.16.0.0/16 |
 
## Site Services (optional)

|                              | value         |
|------------------------------|---------------|
| DNS1                         |               |
| DNS2                         |               |
| NTP1                         |               |
| NTP2                         |               |

## Auth 
- [ ] (optional) LDAP/AD external server
  - [ ] IP or IPs if using LDAP mirror
  - [ ] domain
  - [ ] LDAP branch 
  - [ ] CA certificate (if required)
  - [ ] TLS certificate (if required)
- [ ] sNow! managed LDAP 

## Proxy server
- [ ] (optional) site proxy server to deliver SMTP, FTP, HTTP(S) and NTP.
  - [ ] client configuration for SMTP clients
  - [ ] (optional) email account to send alerts, job status, etc.
  - [ ] client configuration for FTP and HTTP(S) proxy clients
  - [ ] client configuration for NTP clients
- [ ] sNow! managed proxy server
  - [ ] (optional) email account to send alerts, job status, etc.

## Scientific Applications and Development Tools
- [ ] LMOD with HNSC or flat schema
- [ ] Easybuild
- [ ] Licenses of propietary software (i.e. intel parallel studio cluster edition, ANSYS Fluent, Abaqus, Gaussian, etc.)
- [ ] Toolchains to be installed (foss, intel, etc.)
- [ ] List of scientific applications (and releases) to be installed

## Monitoring tools
- [ ] sNow! managed ganglia
- [ ] sNow! managed icinga
- [ ] Site managed monitoring tools
  - [ ] Client configuration of ganglia
  - [ ] Client configuration of icinga/nagios/etc.

## Cluster File System and Shared File System
- [ ] sNow! managed NFS server
- [ ] site managed NFS server
- [ ] site managed BeeGFS
- [ ] site managed Lustre
- [ ] site managed GPFS (IBM Spectrum Scale)

## Events
- [ ] site managed central syslog server
- [ ] sNow! managed RSyslog
- [ ] ElasticSearch integration
- [ ] Kibana integration

## Workload manager
- [ ] Slurm
- [ ] Torque + Maui
