---
title: Configuration example for Scenario C
tags: [formatting]
keywords: dcode samples syntax highlighting
last_updated: July 3, 2016
datatable: true
summary: "You can use fenced code blocks with the language specified after the first set of backtick fences."
sidebar: mydoc_sidebar
permalink: mydoc_scenario_c.html
folder: mydoc
---

The following example shows how to setup the public bridge, the sNow! network bridge and a virtual bridge based on a dummy module. On top of this configuration, there are some iptables rules used to enable NAT communication from the public IP to DMZ services.

* xsnow0 as the private bridge for node deployment administration. It uses the eth0 interface.
* xpub0  as the public bridge for the house network. It uses the eth1 interface.
* xmgmt0 as the management bridge for accessing consoles and the IPMI interfaces on the nodes. It uses the eth2 interface.
* xdmz0  as the DMZ bridge for the house network. It uses the eth3 interface.
* xllf0  as the Low Latency Fabric network bridge used by cluster filesystem and/or MPI. In this example, it uses the eth4 interface which is the virtual interface enabled by using Mellanox eIPoIB. More information in this regard available in section 9 (Advanced Network Setup).

In order to enable the required network bridges, follow the next four simple steps:

Add the following line in /etc/modules

```
dummy numdummies=1
```
Download the configuration file example for /etc/network/interfaces from our website, update the IP addresses and remove the configuration blocks that you do not need.
Edit /etc/network/interfaces by following this [example file](examples/network_interfaces_scenario_c.txt) (please carefully review the file and adapt it to your real network environment)
{% include examples/network_interfaces_scenario_c.txt %}

After the network configuration file is edited, reboot the system and check your configuration has been applied with the following command:

```
ip addr show
```
The expected output should be similar to the following text:

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master xpub0 state UP group default qlen 1000
    link/ether 08:00:27:3c:04:cb brd ff:ff:ff:ff:ff:ff
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master xsnow0 state UP group default qlen 1000
    link/ether 08:00:27:30:38:ac brd ff:ff:ff:ff:ff:ff
4: xpub0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 08:00:27:3c:04:cb brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global xpub0
       valid_lft forever preferred_lft forever
5: xsnow0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 08:00:27:30:38:ac brd ff:ff:ff:ff:ff:ff
    inet 192.168.7.1/24 brd 192.168.7.255 scope global xsnow0
       valid_lft forever preferred_lft forever
```

After that, you need to setup the firewall. sNow! allows you to setup the firewall automatically by taking into account all the services available in domains.conf and dmz_portmap.conf, which contains all the NAT rules associated with each role. 
sNow! uses the Uncomplicated Firewall (ufw) to manage IPTABLES. The following command lines will setup advanced and complex rules for you.

```
snow update firewall
ufw disable
ufw enable
```
You can modify or add new rules in the firewall by working with the standard ufw files or directly with IPTABLES. 
IMPORTANT ADVICE: 
These firewall rules are only required for Scenario C.
The default port for SSH for the sNow! server is 22/TCP. It is recommend that you replace this with a higher port number in order to avoid automatic attacks. If you want to change it, you will need to update the OpenSSH config file (/etc/ssh/sshd_config) and also the firewall (/etc/ufw/applications.d/ufw-snow).
If you are installing remotely, ensure that you have access to the console. Otherwise there is a high risk of losing the SSH connection due to a misconfiguration in the firewall.


For the list of supported languages you can use (similar to `js` for JavaScript), see [Supported languages](https://github.com/jneen/rouge/wiki/list-of-supported-languages-and-lexers).
