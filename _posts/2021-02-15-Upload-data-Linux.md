---
layout: post
title: "Upload big data to a Linux server"
date: 2021-02-15
description: How to upload big data to a Linux server using rsync
share: true
tags:
 - Linux
 - Big data
---

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

 Last update: 2021/02/15
