---
layout: post
title: "Estimate CESM Code Performace"
date: 2020-08-08
description: How to eastimate CESM code performance and storage
share: true
tags:
 - CESM
 - Tech-accumulate
---

### Estimating Cheyenne core-hours
Cheyenne allocations are made in core-hours. The core-hours used for a job (a benchmark run) are calculated by multiplying the number of processor cores used by the wall-clock duration in hours. Cheyenne core-hour calculations should assume that all jobs will run in the regular queue and that they are charged for use of all 36 cores on each node.

**Check the number of nodes and cores**
```powershell
qstat
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
3545063.chadmin yttp     economy  restT04     59078  36 129    --  08:00 R 02:43
```
**Job ID**
This job carries the identification number "1201" and was submitted from node "cicum81".
**Queue**
Lists the type of queue the jobs are running in. 
**SessID**
Refers to the session id (if the job is running).
**NDS**
Refers to the number of nodes requested by the job.
**TSK**
Refers to the number of cpus (cores) or tasks requested by the job.
**Req’d Memory** 
The amount of memory requested by the job.
**S**
Refers to the jobs current state: 
   E – Job is exiting
   H – Job is held. This may be the result of a manual hold or of a job dependency
   Q – Job is queued.
   R – Job is running.

### Campaign Storage
