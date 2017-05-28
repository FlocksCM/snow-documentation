---
title: Configuration example for Scenario A
tags: [publishing]
keywords: network
last_updated: July 3, 2016
summary: "scenario A."
sidebar: mydoc_sidebar
permalink: mydoc_scenario_a.html
folder: mydoc
---
The following example shows how to setup the public bridge and the sNow! (private) network bridge.

We will define the following bridges:
* xsnow0 as the private bridge for node deployment and administration. It uses the eth0 interface.
* xpub0  as the public bridge for the house network. It uses the eth1 interface.

1. Additionally we will define:
An alias IP on the xsnow0 bridge to access the IPMI interfaces on the nodes (mandatory if they are not in the same network)

2. Optionally we will define:
An IP on the ib0 Infiniband interface or in the high speed network interface of your choice.

3. In order to enable the required network bridges, follow the next four simple steps:

Edit /etc/network/interfaces by following this example file (please carefully review the file and adapt it to your real network environment)

4. After the network configuration file is edited, reboot the system and check your configuration has been applied with the following commands:

```
ifconfig
ip addr show
```

5. The expected output should be similar to the following, if you used ifconfig on the previous step:

```
eth0  	Link encap:Ethernet  HWaddr 00:1e:67:d6:0e:4e  
      	UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
      	RX packets:521186 errors:0 dropped:0 overruns:0 frame:0
      	TX packets:1575110 errors:0 dropped:0 overruns:0 carrier:0
      	collisions:0 txqueuelen:1000
      	RX bytes:429970630 (410.0 MiB)  TX bytes:836884100 (798.1 MiB)
      	Memory:91920000-9193ffff

eth1  	Link encap:Ethernet  HWaddr 00:1e:67:d6:0e:4f  
      	UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
      	RX packets:7600132 errors:0 dropped:0 overruns:0 frame:0
      	TX packets:2371910 errors:0 dropped:0 overruns:0 carrier:0
      	collisions:0 txqueuelen:1000
      	RX bytes:3662822294 (3.4 GiB)  TX bytes:341538824 (325.7 MiB)
      	Memory:91900000-9191ffff

lo    	Link encap:Local Loopback  
      	inet addr:127.0.0.1  Mask:255.0.0.0
      	inet6 addr: ::1/128 Scope:Host
      	UP LOOPBACK RUNNING  MTU:65536  Metric:1
      	RX packets:324 errors:0 dropped:0 overruns:0 frame:0
      	TX packets:324 errors:0 dropped:0 overruns:0 carrier:0
      	collisions:0 txqueuelen:0
      	RX bytes:38958 (38.0 KiB)  TX bytes:38958 (38.0 KiB)

xpub0 	Link encap:Ethernet  HWaddr 00:1e:67:d6:0e:4f  
      	inet addr:YOUR_LAN_IP_ADDRESS  Bcast:YOUR_LAN_BROADCAST  Mask:YOUR_LAN_NETMASK
      	inet6 addr: fe80::21e:67ff:fed6:e4f/64 Scope:Link
      	UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
      	RX packets:7208440 errors:0 dropped:0 overruns:0 frame:0
      	TX packets:2319933 errors:0 dropped:0 overruns:0 carrier:0
      	collisions:0 txqueuelen:0
      	RX bytes:3536513820 (3.2 GiB)  TX bytes:338107598 (322.4 MiB)

xsnow0	Link encap:Ethernet  HWaddr 00:1e:67:d6:0e:4e  
      	inet addr:192.168.7.1  Bcast:192.168.8.255  Mask:255.255.255.0
      	inet6 addr: fe80::21e:67ff:fed6:e4e/64 Scope:Link
      	UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
      	RX packets:1270216 errors:0 dropped:0 overruns:0 frame:0
      	TX packets:521744 errors:0 dropped:0 overruns:0 carrier:0
      	collisions:0 txqueuelen:0
      	RX bytes:483568143 (461.1 MiB)  TX bytes:666980607 (636.0 MiB)

xsnow0:mgmt Link encap:Ethernet  HWaddr 00:1e:67:d6:0e:4e  
      	inet addr:192.168.100.1  Bcast:192.168.100.255  Mask:255.255.255.0
      	UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
```

You can check the bridges and their associated network interfaces with the following command:
```
brctl show
bridge name    bridge id   	    STP enabled    interfaces
xpub0   	    8000.001e67d60e4f    no   	         eth1
xsnow0   	    8000.001e67d60e4e    no   	         eth0
```
{% include image.html file="killalljekyll.png" caption="iTerm profile settings to kill all Jekyll" %}

{% include links.html %}
