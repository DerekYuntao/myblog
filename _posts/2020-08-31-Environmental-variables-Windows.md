---
layout: post
title: "Add a Environmental Variable Using CMD in Windows"
date: 2020-08-25
description: How to add a new environmental variable using CMD in Windows
share: true
tags:
 - Windows
---
Step1: Enter CMD
Step2: input *path* and press Enter to see a list of environmental variables
Step3: input the following command 

```powershell
setx path "%path%;D:\Anaconda3"
```
Step4: Restart CMD and input *path* and check the list.

Last update: 08/31/2020