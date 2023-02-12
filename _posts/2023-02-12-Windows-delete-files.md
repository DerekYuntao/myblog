---
layout: post
title: "Compulsively remove files in Windows"
date: 2023-02-12
description: How to delete files compulsively in Windows
share: true
tags:
 - Windows
---

In CMD window, entering the following commands:
```powershell
rd/s/q D:/app   # for directory and all files inside

del/f/s/q D:/app.exe  # for a file
```

Updated: 02/12/2023