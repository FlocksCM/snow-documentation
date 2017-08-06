---
title: sNow! Command Line Interface Administratrion
tags: [special_layouts]
keywords: CLI
last_updated: July 3, 2016
summary: "Quick summary of sNow! CLI"
sidebar: mydoc_sidebar
permalink: mydoc_snow_cli.html
folder: mydoc
---

The snow command is the most important administration command you will use to manage your cluster. It allows you to do the initial configuration, deploy the domains and compute nodes, boot or power cycle them when needed, open the console, PXE install the compute nodes, etc. This command interacts with the cluster in two ways:
When focused on a domain or VM, it is a wrapper which runs the appropriate XEN commands to control the domain.
When focused on compute nodes it uses the IPMI interface to control the compute nodes.

The following sections will explain in detail how to interact with the snow command.
```
snow [function] <domain|node> <option>
```
## Man pages
There is a manpage covering the usage of the snow command. We encourage you to read it as it might have information not available in this guide or, at least, organized in a different way. The available man pages are :
* snow
* snow.conf
* active-domains.conf
* domains.conf

## Version and help

```version```: shows the version of sNow!

```help```: prints the standard help message


