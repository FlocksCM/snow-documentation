---
title: Deploy the first compute node
keywords: deploy, compute node
last_updated: July 3, 2016
tags: [deploy]
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_deploy_the_first_compute_node.html
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
