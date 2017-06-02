---
title: Download Domain Template
tags: [template domain]
keywords: template, domain, domains, roles
last_updated: July 16, 2016
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_download_domain_template.html
folder: mydoc
---

sNow! relies on pre-built domain images which are used as a template in role deployment. This allows accelerating the deployment of. It is good practice to update this image before creating any new service in order to fix potential bugs.

The following command looks for a new update in the public repository, and if a new update is available, it will download the new image. The size of the image is around 250MB, so expect some delay here.

```
snow update template
```