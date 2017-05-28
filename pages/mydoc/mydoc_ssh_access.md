---
title: SSH access with sNow! user or HPCNow! user
permalink: mydoc_ssh_access.html
keywords: ssh access
sidebar: mydoc_sidebar
folder: mydoc
---

{% include tip.html content="For a better terminal emulator on Windows, use [Git Bash](https://git-for-windows.github.io/). Git Bash gives you Linux-like control on Windows." %}

During the installation process two users have been created:
The sNow! user (snow) is the admin user which will be able to install scientific applications and to escalate to root privileges.  
The HPCNow! user (hpcnow) can also escalate to root privileges and is meant to be used for remote administration and support. 

By default, the only way to access the system as the sNow! or HPCNow! user is by using the SSH key located in the user’s home directory. Note that at this point, those users do not have a password. If you want to enable password access, you will need to setup a password first.

## Access as sNow! user with the SSH key
You will need to transfer a copy of the snow user SSH key to your desktop computer. You can copy this file to a pendrive or transfer the file from the sNow! server through SCP or any other file transfer mechanism.

## Allow access for professional remote administration and support
You will need to provide the HPCNow! user name and the SSH key or password to the remote administration engineer.

{% include links.html %}