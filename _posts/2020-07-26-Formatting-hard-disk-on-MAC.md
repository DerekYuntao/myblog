---
layout: post
title: "Case-sensitive Setting for a Hard Disk on MAC"
date: 2020-07-26
description: How to format and set a case-sensitive mobile hard disk on macOS before backing-up data 
share: true
tags:
 - MAC
 - Tech-accumulate
---
An initialization is always required before we use a new hard disk to backup data, which is called formatting or erasing the disk.<span style="color:red;"> One thing we should note is that macOS does not distinguish between upper and lower case in file paths by defualt, while Linux systems are generally case-sensitive</span>. In this case, if we backup data from Linux to hard disks through MAC, two data files with the same name and the only difference in its upper and lower cases will cause a problem. For exmaple, two files with the names of *"H2O_r.nc"* and *"H2O_R.nc"* are actually different. But macOS by default is case-insensitive and can not tell the difference, thus file *"H2O_R.nc"* would overwrite *"H2O_r.nc"*, which may destory our plan to backup the data.<span style="color:red;"> The solution is to set the disk quota as *case-sensitive* when we initialize the disk.

**How to find Disk settings on MAC**
Entering ***Applications-Utilities-Disk Utility*** and click ***Erase*** in *Disk Utility* window. Follow the imagine below to set the format as ***Mac OS Extended (Case-sensitive, Journaled)***

![img1]({{site.baseurl}}/assets/images/2020-07-25-1.jpg)

For hard disks attached in MAC computers (e.g. Macintosh HD), we can set a volumn case-sensitive similarly through ***partition***. [Click here to Check (in Chinese)](https://zhuanlan.zhihu.com/p/35908178)

Last update: 07/26/2020