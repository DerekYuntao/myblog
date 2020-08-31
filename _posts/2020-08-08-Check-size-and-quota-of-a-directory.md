---
layout: post
title: "Check Directory Size and Quota Size"
date: 2020-08-08
description: How to check directory size and quota size on Linux
share: true
tags:
 - Linux
---

## Check directory size ##
```powershell
du -sh [directory]
du -sm [directory]  #size in unit M
du -sh * [directory]  #check the size of each directory, not file!
```

## Check quota size ##
Check the quota size of the **root path** of a directory:
```powershell
df -h [directory]
# or 
df -hl [directory]
```
output:
```powershell
Filesystem      Size  Used Avail  Use%  Mounted on
fs1             250T  224T   27T  90%   /glade/...
```

Last update: 08/08/2020