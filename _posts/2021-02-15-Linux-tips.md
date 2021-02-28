---
layout: post
title: "Linux Tips"
date: 2021-02-15
description: Useful tips for Linux 
share: true
tags:
 - Linux
---

## Upload Big Data to a Linux Server
**rsync**: a fast, versatile, remote (and local) file-copying tool

e.g transfer directory *amwg* to a Linux server *directory diagpkg_data*

    rsync -avrP --rsh=ssh amwg guest02@<IP address>:diagpkg_data

**explaination**
-a, --archive  archive mode
This is equivalent to -rlptgoD. It is a quick way of saying you want recursion and want to preserve almost everything (with -H being a notable omission). The only exception to the above equivalence is when --files-from is specified, in which case -r is not implied.
-v, --verbose  increase verbosity
-r, --recursive  recurse into directories
-P  same as --partial --progress
Its purpose is to make it much easier to specify these two options for a long transfer that may be interrupted.

 Reference:
 <https://linux.die.net/man/1/rsync>
 <http://www.ruanyifeng.com/blog/2020/08/rsync.html>

## Create recursive (multi-level) directory 
    mkdir -p /a/aa/aaa/aaaa

## Create multiple directories
    mkdir a1 a2 a3 
    
## Find strings in Vim
Reference:
<https://harttle.land/2016/08/08/vim-search-in-file.html>

```powershell
    /{string}    
# case-insesitive
    /{string}\c
```
**set case sensitive/insensitive mode for Vim**
    set ignorecase
    set smartcase     


