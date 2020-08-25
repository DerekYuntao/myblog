---
layout: post
title: "Explaination on Cheyenne large allocation"
date: 2020-08-25
description: Compostion of Cheyenne resource allocation
share: true
tags:
 - Big data
---

**Cheyenne** (SGI ICE XA Cluster) 
Cheyenne is a 5.34-petaflops SGI ICE XA Cluster, featuring 145,152 Intel Xeon processor cores and 313 TB of total memory in 4,032 dual-socket nodes. Cheyenne is mainly used for running large-scale computing. Requests for more than 400,000 core-hours are considered large requests. 

**Casper**
Casper is a heterogeneous cluster of specialized data analysis and visualization resources and large-memory, multi-GPU nodes. Twenty Supermicro SuperWorkstation nodes will be used for data analysis and visualization jobs. Each node has 36 cores and 384 GB of memory. Eight of the 20 nodes also feature an NVIDIA GPU. Four additional nodes feature large-memory, dense GPU configurations to support explorations in machine learning (ML) and deep learning (DL) in atmospheric and related sciences.
standard allocation is 10,000.00 Core-hours.
 
**Campaign Storage**
The NCAR Campaign Storage file system is available to the NCAR labs and to university researchers as archival space for storing data on publication and NSF award timescales. As a disk-based storage system, it is preferred over the tape-based High Performance Storage System (HPSS). Campaign Storage has a five-year retention policy. Usually post-processed data are stored here.

**GLADE Project Space**
The Globally Accessible Data Environment is centralized disk storage that is accessible from the NCAR's production HPC and analysis clusters. Usually large-scale model output is placed here.

Log in to CIS with two-factor authentication:
[https://guide.duo.com/append-mode]

Log in to help center:
[https://helpdesk.ucar.edu/plugins/servlet/desk/portal/3]

Log in to resource allocation system:
[https://xras-submit.ucar.edu/login]

Reference:
https://www2.cisl.ucar.edu/resources/computational-systems/cheyenne

Last updata: 08/25/2020