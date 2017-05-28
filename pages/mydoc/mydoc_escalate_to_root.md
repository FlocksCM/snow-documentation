---
title: Escalate to root user
permalink: mydoc_escalate_to_root.html
keywords: root access escalate
sidebar: mydoc_sidebar
folder: mydoc
---

{% include tip.html content="For a better terminal emulator on Windows, use [Git Bash](https://git-for-windows.github.io/). Git Bash gives you Linux-like control on Windows." %}

Access as sNow! user and execute the following command line:

sudo su -

The root user and sNow! user are able to access as a root, all domains and servers managed by sNow! but only from the sNow! servers. This means that you will be not able to escalate to root privileges from the login node, a compute node or any domain.

{% include links.html %}