---
title: Deploy the first compute node
keywords: deploy, compute node
last_updated: July 3, 2016
tags: [deploy]
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_node_deploy.html
folder: mydoc
---

1. Start the deployment of the first compute node (golden node).
```
snow deploy first_compute_node
```
2. Monitor the deployment progression from the compute node console
```
snow console first_compute_node
```
3. Review the deployment logs
Once the deployment is done, you will be able to review the deployment logs. Usually those files are located in the local file system inside the root user home directory:
```
/root/post-install.log
/root/first-boot.log
```
4a. Start the deployment of the rest of the compute nodes.
Example:
```
snow deploy node-[001-010]
```
4b. Or alternatively, (gather a OS image)[mydoc_image_gather.html] and boot the nodes using that image.
Example:
```
snow boot node-[001-900] centos-7.4-custom
```
