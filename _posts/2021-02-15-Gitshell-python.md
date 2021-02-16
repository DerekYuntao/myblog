---
layout: post
title: "Set python location for Gitshell"
date: 2021-02-15
description: How to set and run python in Gitshell
share: true
tags:
 - Web
 - python
---

When running a python script in Gitshell, here comes the problem:
    bash: /c/Users/dk/AppData/Local/Microsoft/WindowsApps/python3: Permission denied
How to deal with this problem?

**Step1**:
Type "manage app execution aliases" into the Windows search prompt and disable the store versions of Python altogether.
如果是中文系统，在window中左下角搜索"管理应用执行别名"，关闭"应用安装程序"

**Step2**:
Add python location or anaconda to *.bashrc*:
    export PATH="$$PATH:/D:/Anaconda3/Scripts"
    export PATH="$$PATH:/D:/Anaconda3:/D:/Anaconda3/Scripts"

That's it. Oh Windows, you make me trouble!

Reference:
https://github.com/gmacario/spaceappschallenge-2019/issues/6

Last update: 02/15/2021
