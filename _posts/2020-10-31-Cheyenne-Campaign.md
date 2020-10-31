---
layout: post
title: "Load data to or download data from Campaign"
date: 2020-10-31
description: How to process or migrate data storage in Campaign 
share: true
tags:
 - Big data
---

## Intro.
**Campaign Storage**
The NCAR Campaign Storage file system is available to the NCAR labs and to university researchers as archival space for storing data on publication and NSF award timescales. As a disk-based storage system, it is preferred over the tape-based High Performance Storage System (HPSS). Campaign Storage has a five-year retention policy. Usually post-processed data are stored here.

Data-access nodes on Campaign facilitate transfers of small files to and from GLADE spaces such as /glade/scratch and /glade/work.

**Casper**
Casper is a heterogeneous cluster of specialized data analysis and visualization resources and large-memory, multi-GPU nodes. Twenty Supermicro SuperWorkstation nodes will be used for data analysis and visualization jobs. Each node has 36 cores and 384 GB of memory. Eight of the 20 nodes also feature an NVIDIA GPU. Four additional nodes feature large-memory, dense GPU configurations to support explorations in machine learning (ML) and deep learning (DL) in atmospheric and related sciences.
standard allocation is 10,000.00 Core-hours.

## Data storage processing or migration
**Check the quota status on Cheyenne:**
```powershell
gladequota
```

**List projects or groups：**
```powershell
id
```
It returns the user id, group id, and project id:
uid=0000(username) gid=0000(ncar) groups=0000(ncar),0000(project name)...

**Check which machine are you located now:**
```powershell
hostname
```
e.g. If you are now at Cheyenne, then the output would be Cheyenne.

**Enter Campaign:**
```powershell
ssh data-access.ucar.edu
# or 
ssh username@data-access.ucar.edu
```
After entering password and authorized can enter the Campaign storage.

**Enter Campaign project:**
```powershell
cd /glade/campaign/univ/...
```

**Migrate data between Campaign and Cheyenne scratch:**
Use the common command cp, mv, and et.al.

**Background migrate with out concerned about log off:**
```powershell
tmux

#return to front:
tmux attach
```

Reference:
https://www2.cisl.ucar.edu/user-support/training/library/migrating-files-from-hpss

Last updata: 10/31/2020