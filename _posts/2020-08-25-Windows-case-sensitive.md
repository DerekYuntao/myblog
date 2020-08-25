---
layout: post
title: "Case-sensitive Setting for a Directory on WIN10"
date: 2020-08-25
description: How to set a case-sensitive directory on Windows10
share: true
tags:
 - Tech-accumulate
---

We can't enable a case-sensitive disk on Windows but can enable for a directory. 

Running CMD as administrator and type the following command.
```powershell
fsutil.exe file SetCaseSensitiveInfo D:\...\Github enable
```
**Note** that only WIN10 will work and the disk partition must be NTFS.

Disable a case-sensitive directory:
```powershell
fsutil.exe file SetCaseSensitiveInfo D:\...\Github disable
```

Last update: 08/25/2020