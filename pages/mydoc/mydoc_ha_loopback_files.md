---
title: High Availability based on Loopback images on Shared File System
sidebar: mydoc_sidebar
tags: [ha, high_availability]
permalink: mydoc_ha_loopback_files.html
folder: mydoc
---

The following notes descrive how to setup a High Availability (HA) cluster based on loopback images for domains (Para-virtualised xen VMs) and a BeeGFS shared file system provided by other servers.

IMG SCHEMA TBD

## Enabling cluster file system re-export from sNow! nodes
If this is a new installation, you can jump to the next section.

In the following file you can define which file systems are going to be re-exported. sNow! relies NFS for deploying the compute nodes, so /sNow and /home are expected to be shared through NFS.

Content of /etc/exports.d/snow.exports should be similar to:

```
/sNow            192.168.8.0/255.255.0.0(rw,async,fsid=0,crossmnt,no_subtree_check,no_root_squash)
/home            192.168.8.0/255.255.0.0(rw,sync,fsid=1,crossmnt,no_subtree_check,no_root_squash)
```

Then you can transfer the same file to the other sNow! nodes:

```
scp -pr /etc/exports.d/snow.exports snow02:/etc/exports.d/
```

## Enabling the /sNow path to the compute nodes
You can setup a simple simbolic link to the path where /sNow folder is located in the BeeGFS, for example: /beegfs/sNow

```
ln -s /beegfs/sNow /sNow
```
<!--
Otherwise, you can setup a mount bind 
MOUNT_NFS[1]="/scratch/sNow                     /sNow          none   x-systemd.requires=/scratch/sNow,defaults,bind   0 0"
-->

## Enable Debian Backports

Include Debian Backports in the sources.list:

```
echo "deb http://ftp.debian.org/debian jessie-backports main" >> /etc/apt/sources.list
```
Do the same in the other sNow! nodes

## Install the required software packages

```
aptitude update
aptitude install -t jessie-backports libqb0 fence-agents pacemaker corosync pacemaker-cli-utils crmsh drbd-utils -y
```
Do the same in the other sNow! nodes

## Disable auto start of corosync and pacemaker
In order to avoid a death match situation, it's highly recommended to disable corosync and pacemaker to be started at boot time. To start the service, you only need to start pacemaker.
```
systemctl disable corosync
systemctl disable pacemaker
```

Do the same in the other sNow! nodes

## Setting up Corosync

### Generate keys

```
corosync-keygen
Corosync Cluster Engine Authentication key generator.
Gathering 1024 bits for key from /dev/random.
Press keys on your keyboard to generate entropy.
```

In order to accelerate this process, you can install and execute haveged.

```
aptitude install haveged
```

### Setup the right permissions

```
chmod 400 /etc/corosync/authkey
```

### Transfer the key to the other nodes

```
scp -p /etc/corosync/authkey snow02:/etc/corosync/authkey
```

### Configuring corosync 
The content of /etc/corosync/corosync.conf should be something similar to the following example. Note that this cluster only has two nodes (snow01 and snow02).

```
# egrep -v "^$|#" /etc/corosync/corosync.conf
totem {
        version: 2
        cluster_name: snow
        token: 3000
        token_retransmits_before_loss_const: 10
        clear_node_high_bit: yes
        interface {
                ringnumber: 0
                bindnetaddr: 192.168.8.0
                mcastport: 5405
                ttl: 1
        }
}
logging {
        fileline: off
        to_stderr: no
        to_logfile: no
        to_syslog: yes
        syslog_facility: daemon
        debug: off
        timestamp: on
        logger_subsys {
                subsys: QUORUM
                debug: off
        }
}
quorum {
        provider: corosync_votequorum
        expected_votes: 2
        two_node: 1
        wait_for_all: 1
}
nodelist {
    node {
        ring0_addr: snow01
        nodeid: 1
    }
    node {
        ring0_addr: snow02
        nodeid: 2
    }
}
```

## Xen configuration
Review if /etc/default/xendomains has an empty value for XENDOMAINS_SAVE variable or if it's commented. Otherwise, comment this variable.

### Test if Xen allows live migration between sNow! nodes

Execute the following commands in order to certify that the para-virtual machines can be migrated across the sNow! nodes:

From snow01 you can execute the following commands:
```
snow boot deploy01
xl migrate deploy01 snow02
```

If it works as expected, you should see a output message similar to the following example:
```
[4287] snow01:~ $ xl migrate deploy01 snow02
migration target: Ready to receive domain.
Saving to migration stream new xl format (info 0x0/0x0/799)
Loading new save file <incoming migration stream> (new xl fmt info 0x0/0x0/799)
 Savefile contains xl domain config
xc: progress: Reloading memory pages: 26624/524288    5%
xc: progress: Reloading memory pages: 53248/524288   10%
xc: progress: Reloading memory pages: 78848/524288   15%
xc: progress: Reloading memory pages: 105472/524288   20%
xc: progress: Reloading memory pages: 131072/524288   25%
xc: progress: Reloading memory pages: 157696/524288   30%
xc: progress: Reloading memory pages: 184320/524288   35%
xc: progress: Reloading memory pages: 209920/524288   40%
xc: progress: Reloading memory pages: 236544/524288   45%
xc: progress: Reloading memory pages: 262144/524288   50%
xc: progress: Reloading memory pages: 288768/524288   55%
xc: progress: Reloading memory pages: 315392/524288   60%
xc: progress: Reloading memory pages: 340992/524288   65%
xc: progress: Reloading memory pages: 367616/524288   70%
xc: progress: Reloading memory pages: 393216/524288   75%
xc: progress: Reloading memory pages: 419840/524288   80%
xc: progress: Reloading memory pages: 446464/524288   85%
xc: progress: Reloading memory pages: 472064/524288   90%
xc: progress: Reloading memory pages: 498688/524288   95%
xc: progress: Reloading memory pages: 524482/524288  100%
migration target: Transfer complete, requesting permission to start domain.
migration sender: Target has acknowledged transfer.
migration sender: Giving target permission to start.
migration target: Got permission, starting domain.
migration target: Domain started successsfully.
migration sender: Target reports successful startup.
Migration successful.
```

## Setup Pacemaker

Iniciate the setup without STONITH. The last section explains how to setup STONITH using a fence device based on IPMI.

```
[4242] snow01:~ $ crm configure
crm(live)configure# property stonith-enabled=no
crm(live)configure# property no-quorum-policy=ignore
crm(live)configure# property default-resource-stickiness=100
crm(live)configure# commit
crm(live)configure# bye
```

### Setup the right permissions and check the cluster health

```
chown -R hacluster:haclient /var/lib/pacemaker
chmod 750 /var/lib/pacemaker
ssh snow02 chown -R hacluster:haclient /var/lib/pacemaker
ssh snow02 chmod 750 /var/lib/pacemaker
crm cluster health | more 
```

### Define the first service in HA

Execute the ```crm configure``` and define the first service in HA as follows:

```
primitive deploy01 ocf:heartbeat:Xen \
 params xmfile="/sNow/snow-tools/etc/domains/deploy01.cfg" \
 op monitor interval="40s" \
 meta target-role="started" allow-migrate="true"
```

Some operations like the live migration requires some extra time. Specially when the VM uses a reasonable amount of memory. 
It's highly recommended to increase the default timeout to avoid cancelling the live migration due a short time limit. 
The following example, setup 120s as default timeout. You can tune this value attending at your VM needs.

```
crm_attribute --type op_defaults --attr-name timeout --attr-value 120s
```

### Test!

This is a good moment to test if the HA works. In order to review that, execute in a new SSH session the command:

```
crm_mon
```

It should report something like this:

```
Stack: corosync
Current DC: snow01 (version 1.1.16-94ff4df) - partition with quorum
Last updated: Fri Jul 28 06:51:02 2017
Last change: Fri Jul 28 06:50:50 2017 by root via crm_resource on snow02

2 nodes configured
1 resource configured

Online: [ snow01 snow02 ]

Active resources:

deploy01        (ocf::heartbeat:Xen):   Started snow01
```

Using the following command, you will force to migrate the service from one node to the other one:

```
crm resource move deploy01 snow01
```

### Define all the HA services

Since this is quite repetitive task, the following script will help you to generate the multiple commands to define the HA services.

```
[4332] snow01:~ $ cat migrate.sh
#!/bin/bash

for i in proxy01 monitor01 nis01 syslog01 maui01 nis02 flexlm01; do
    echo "primitive $i ocf:heartbeat:Xen \\
          params xmfile=\"/sNow/snow-tools/etc/domains/$i.cfg\" \\
          op monitor interval=\"20s\" \\
          meta target-role=\"started\" allow-migrate=\"true\"
         "
done
echo commit
echo bye
```

Finally, copy the output data into ```crm configure``` to setup the rest of HA services.

The expected outcome should be something like this:

```
Stack: corosync
Current DC: snow01 (version 1.1.16-94ff4df) - partition with quorum
Last updated: Fri Jul 28 07:13:00 2017
Last change: Fri Jul 28 07:11:31 2017 by root via cibadmin on snow01

2 nodes configured
8 resources configured

Online: [ snow01 snow02 ]

Active resources:

deploy01        (ocf::heartbeat:Xen):   Started snow01
proxy01 (ocf::heartbeat:Xen):   Started snow01
monitor01       (ocf::heartbeat:Xen):   Started snow02
nis01   (ocf::heartbeat:Xen):   Started snow02
syslog01        (ocf::heartbeat:Xen):   Started snow01
maui01  (ocf::heartbeat:Xen):   Started snow02
nis02   (ocf::heartbeat:Xen):   Started snow01
flexlm01        (ocf::heartbeat:Xen):   Started snow02
```

## Service placement

In order to balance services across the two nodes and also to distribute additional services with native HA (i.e slurm-master slurm-slave) you can use the following instructions to define the preferred hosts. 
Failback is useful to define well balanced services, but if you have an ongoing issue, you could trigger a failback in a “semi-faulty” node.

```
crm(live)# configure
crm(live)configure# location cli-prefer-maui01 maui01 role=Started inf: snow01
crm(live)configure# location cli-prefer-nis01 nis01 role=Started inf: snow01
crm(live)configure# location cli-prefer-proxy01 proxy01 role=Started inf: snow01
crm(live)configure# location cli-prefer-flexlm01 flexlm01 role=Started inf: snow02
crm(live)configure# location cli-prefer-monitor01 monitor01 role=Started inf: snow02
crm(live)configure# location cli-prefer-nis02 nis02 role=Started inf: snow02
crm(live)configure# location cli-prefer-syslog01 syslog01 role=Started inf: snow02
crm(live)configure# commit
crm(live)configure# bye
```
