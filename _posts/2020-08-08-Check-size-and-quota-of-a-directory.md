---
layout: post
title: "Check Directory Size and Quota Size"
date: 2020-08-08
description: How to check directory size and quota size on Linux
share: true
tags:
 - LINUX
 - Tech-accumulate
---

## Check directory size ##
```powershell
du -sh [directory]
du -sm [directory]  #size in unit M
```

## Check quota size ##
Check the quota size of the **root path** of a directory:
```powershell
df -h [directory]
or 
df -hl [directory]
```

output:
Filesystem      Size  Used Avail Use% Mounted on
fs1             250T  224T   27T  90% /glade/p/...

Last update: 08/08/2020