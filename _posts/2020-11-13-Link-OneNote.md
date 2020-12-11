---
layout: post
title: "Sync a Random Directory to OneDrive"
date: 2020-11-13
description: How to link and sync a random directory outside OneDrive to OneDrive
share: true
tags:
 - Windows
---

Say we want to link directory ***D:\blog*** to OneDrive so that the files inside the directory can be synced and auto-backup with  OneDrive. 

Entering CMD:
```powershell
mklink /d "C:\Users\Administrator\OneDrive - OSU\blog" "D:\blog"
```

mklink /d 给目录创建符号链接，简称符号链接、软链接；
删除虚拟的链接目录，并不会删除远程文件夹真实文件，注意千万不能用del，del会删除远程的真实文件。需用下面的方式删除虚拟连接：
```powershell
rmdir C:\Users\Administrator\OneDrive - OSU\blog
```

Last updata: 11/23/2020