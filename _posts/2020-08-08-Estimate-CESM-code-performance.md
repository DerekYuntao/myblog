---
layout: post
title: "Estimate CESM Code Performace: Parallell processes"
date: 2020-08-09
description: How to eastimate CESM code performance
share: true
tags:
 - CESM
 - Linux
 - Tech-accumulate
---

## 1. CESM Timing, Performance and Load Balancing Data
Reference: <https://www2.cisl.ucar.edu/user-support/determining-computational-resource-needs>

### 1.1 PE(processer or MPI tasks) Layouts
Each component is associated with a unique **MPI communicator**, the **driver** runs on the union of all processors and controls the sequencing and hardware partitioning. The component processor layout is via three settings: the number of MPI tasks, the number of OpenMP threads per task, and the root MPI processor number from the global set. The layout of components on processors has no impact on the science. Changing processor layouts does not change intrinsic coupling lags or coupling sequencing. 

Reference: <https://csegweb.cgd.ucar.edu/timing/cgi-bin/timings.cgi>
This information is constantly subject to change due to changes in the model or machine hardware and software.
```powershell
Machine Compset Resolution	Compiler	mpilib	Total PEs	Cost pe-hrs/yr	ThruPut yrs/day	
cheyenne	B1850	 f19_g17	   Intel	   mpt	   12636	    2094.42	      48.27	
cheyenne	B1850	 f19_g17	   Intel	   mpt	   5472	      1410.69	      31.24	
cheyenne	B1850	 f09_g17	   Intel	   mpt	   12960	    3543.22	      29.26	
cheyenne	B1850	 f19_g17	   Intel	   mpt	   1368	      1233.17	      26.62	

Component Total PEs	Root PE	Tasks x Threads
cam	      2700	   0	      900 X 3
cice     	1620	   360	   540 X 3
cism	      108	   0	      36 X 3
clm	      1080	   0	      360 X 3
cpl	      2700	   0	      900 X 3
mosart	   1080  	0	      360 X 3
pop	      1440	   900	   480 X 3
sesp       	3      	0	      1 X 3
ww        	72	      1380   	24 X 3
```

### 1.2 Example 1
In one of my CESM run, the PE layout is as follows. This can be read and modified from  *env_mach_pes.xml*. It cannot be modified after "./cesm_setup" has been invoked without first invoking "cesm_setup -clean".
```powershell
 NTASKS_ATM=468,NTHRDS_ATM=2,ROOTPE_ATM=0
 NTASKS_CPL=468,NTHRDS_CPL=2,ROOTPE_CPL=0
 NTASKS_ICE=324,NTHRDS_ICE=1,ROOTPE_ICE=144
 NTASKS_LND=144,NTHRDS_LND=1,ROOTPE_LND=0
 NTASKS_ROF=144,NTHRDS_ROF=1,ROOTPE_ROF=0
 NTASKS_OCN=180,NTHRDS_OCN=2,ROOTPE_OCN=468
```
**Explaination** 
***NTASKS*** are the total number of MPI tasks/processes. NTASKS must be greater or equal to 1 even for inactive (stub) components.                             
***NTHRDS*** are the number of OpenMP threads per MPI task. If NTHRDS is set to 1, this generally means threading parallelization will be off for that component. The total number of hardware processors allocated to a component is NTASKS *
NTHRDS.                
***ROOTPE*** is the global MPI task associated with the root task of that component, not the hardware processors counts.
***PSTRID*** is the stride of MPI tasks across the global set of pes (for now this is set to 1)                           
***NINST*** is the **number of instances** of the component (will be spread evenly across NTASKS)

NTASKS and ROOTPE are relatively independent of NTHRDS and they determine the layout of mpi processors between components. NTHRDS is used to determine how those mpi tasks should be placed across the machine. 
In this instance, **the ocean component would run on 180 hardware processors** with 180 MPI tasks using 2 thread per task starting from global MPI task 468. 
**Note that** if all components have identical NTASKS, NTHRDS, and ROOTPE set, all components will run sequentially on the same hardware processors. 

### 1.3 Example 2: ROOTPE_OCN
<entry id="NTASKS_ATM" value="16" />
<entry id="NTHRDS_ATM" value="4" />
<entry id="ROOTPE_ATM" value="0" />
<entry id="NTASKS_OCN" value="64" />
<entry id="NTHRDS_OCN" value="1" />
<entry id="ROOTPE_OCN" value="48" />
the atmosphere and ocean are running concurrently, each on 64 processors with the atmosphere running on MPI tasks 0-15 and the ocean running on MPI tasks 16-79. The first 16 tasks are each threaded 4 ways for the atmosphere. **The batch submission script ($CASE.run) should automatically request 128 hardware processors**, and **the first 16 MPI tasks will be laid out on the first 64 hardware processors with a stride of 4**. The next 64 MPI tasks will be laid out on the second set of 64 hardware processors.
*ROOTPE_OCN = 48* means a total of 176 processors (128+48) would have been requested and the atmosphere would have been laid out on the first 64 hardware processors in 16x4 fashion, and the ocean model would have been laid out on hardware processors 113-176. Hardware processors 65-112 would have been allocated but completely idle(空闲).

**A very important examples given the model timing performance based on different settings of PE layouts**:
[BASICS: How do I change processor counts and component layouts on processors?](http://www.cesm.ucar.edu/models/cesm1.2/cesm/doc/usersguide/x1927.html)

## Estimating Cheyenne core-hours
The computational cost of CESM experiments typically is expressed in core-hours (also known as processor element-hours or "pe-hrs") per simulated year. The core-hours used for a job (a benchmark run) are calculated by **multiplying the number of processor cores used by the wall-clock duration in hours**. Cheyenne core-hour calculations should assume that all jobs will run in the regular queue and that they are charged for use of all 36 cores on each node (Exclusive nodes).

Jobs that run in the exclusive-use queues are charged for use of the all of the cores on each node by this formula:
**wall-clock hours × nodes used × cores per node × queue factor**

**important: how to check the model cost?**
Entering ${case}/timing, and check the *cesm_timing* file of a job.

```powershell
Overall Metrics:
   Model Cost:             703.17   pe-hrs/simulated_year
   Model Throughput:        44.23   simulated_years/day
```

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

## Appendix
Reference: 
进程、线程、服务和任务的区别以及多线程与超线程的概念 <https://www.cnblogs.com/Lxk0825/p/9797867.html>
从硬件角度理解进程与线程 <https://www.jianshu.com/p/f79b877c9611?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation>
MPI Tutorial <https://mpitutorial.com/tutorials/>

process，是指运行中的应用程序，**一旦程序被载入到内存中并准备执行，它就是一个进程，每一个进程都有自己独立的内存空间**。进程是表示资源分配的的基本概念，又是调度运行的基本单位，是系统中的并发执行的单位。一个应用程序可以同时启动多个进程。例如每次执行JDK的java.exe程序，就启动了一个独立的Java虚拟机进程，该进程的任务是解析并执行Java程序代码。

thread，是操作系统能够进行运算的最小单位。线程被包含在进程之中，是行程中的实际运行单位。一条线程是指进程中的一个单一顺序的控制流，一个进程中可以并行多个线程，每条线程并行执行不同的任务。当进程内的多个线程同时运行时，这种运行方式称为并发运行。许多服务器程序，如数据库服务器和Web服务器，都支持并发运行，这些服务器能同时响应来自不同客户的请求。

进程和线程的主要区别在于：每个进程都需要操作系统为其分配独立的内存地址空间，而同一进程中的所有线程在同一块地址空间中工作，这些线程可以共享同一块内存和系统资源，比如共享一个对象或者共享已经打开的一个文件。

task，是指由软件完成的一个活动。任务是比较抽象的概念，是一个一般性的术语，**一个任务既可以是一个进程，也可以是一个线程**。简而言之，它指的是一系列共同达到某一目的的操作。例如，读取数据并将数据放入内存中。这个任务可以作为一个进程来实现，也可以作为一个线程（或作为一个中断任务）来实现。**由此看来CESM中，task指的是一个process.**

CPU顺序运行
按照冯诺依曼架构系统设计，程序应该按照一定的顺序串行执行。CPU就是通过PC指针不断累加，实现程序的串行执行。正常情况下PC指针的值每个cycle会自动加4，由于每个指令占4个字节，那么CPU在执行完当前指令后，就会自动执行下一个指令。
正常情况下从给系统加电， PC指针就不断累加，CPU按照串行顺序一致执行下去。有两个情况PC的值会发生“突变”：（1）程序主动跳转，比如汇编goto指令，c语言的longjmp函数等。本质上就是主动去修改PC的值，直接跳转到需要执行的指令，而不是位置临近的“下一条指令”。（2）系统发生中断，比如CPU的某一条引脚的电瓶为高表示发生了“突变”，系统比如对这个中断做出反应。中断发出者除了发送高电平到CPU相关引脚，还需要提供一个中断向量号，通知CPU发生了什么类型的中断。CPU根据中断向量号，找到相应的中断处理程序并处理中断。

multithreading 多线程
实现多线程并发执行的核心思想是：只要保存下当前程序执行现场，包括PC值、各寄存器值，那么不管CPU现在执行其他什么程序，都能恢复到上次程序执行的现场，并按照之前的执行顺序继续执行。需要做到单核CPU并发，需要涉及到几点：（1）线程执行状态的保存，简单理解就是CPU中各寄存器值的保存，比如保存PC值我们可以知道这个线程执行到哪个指令了。（2）多线程之间的切换，保存下当前线程的状态，恢复之前暂停的线程的状态并继续执行。（3）需要有一个调度者决定恢复执行哪个线程，可以根据线程的优先级，也可以随机选择。
加入当前CPU已经在执行某一个线程，如何能够定期切换到调度程序scheduler呢？这时候就需要硬件的支持了，通过硬件的定时器与中断功能，需要实现一个定时中断程序。比如每隔1ms触发一个硬件中断，强迫CPU回到调度程序中去。中断的本质，就是打破PC寄存器累加的节奏，直接让CPU去处理中断相应程序，而不是一直按照既定顺序一致串行执行下去。

同步：发送方发出数据后，等接收方发回响应以后才发下一个数据包的通讯方式；
异步：发送方发出数据后，不等接收方发回响应，接着发送下个数据包的通讯方式
多线程: 假设现在只有一个CPU，运行两个线程，那么这两个线程其实不是并行运行的，在微观的CPU内部里面，还是串行处理的。CPU通过线程中断，让某一个线程挂起来，然后切换到另一个线程，执行一会儿，再切换回来，使得宏观上看上去好像两个线程同时运行一样。线程切换肯定会带来一定的性能牺牲，于是我们可以增加CPU，这样就真正的做到微观上面的线程并行了。
假设我们现在运行的任何程序都是纯异步的，也就是说CPU没有存在任何的等待时间，此时的线程数如果大于物理CPU个数的话将会得不偿失，因为线程切换回带来一定的性能开销。
实际CPU存在等待时间，比如一个硬盘，有四个CPU来访问，这时候因为多个CPU访问，所以需要等待硬盘响应处理。那如果此时CPU只有一个线程呢？这个线程等待的时候， CPU就空闲了。这就是多线程存在的意义了，在大多数存在同步等待情况的程序中，线程数如果大于物理CPU个数的时候，是可以充分利用CPU，提高性能的。但无限制的加大线程数会带来线程切换的开销，所以一个服务程序的最优线程数需要根据具体情况来具体评估论了。

Last update: 08/09/2020