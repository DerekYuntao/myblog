---
layout: post
title: "Extract a level of NC data in NCO"
date: 2021-02-15
description: How to extract a level of NC data in NCO
share: true
tags:
 - Linux
 - Big data
---

-d <variable of the level>, <number of the level>: extract data at “n”th level from depth coordinate.

nckf -F d z_t,1 b.e13.Bi1850C5CN.f19_g16.alpha01b.09.pop.h.TEMP.050001-060312.nc  b.e13.Bi1850C5CN.f19_g16.alpha01b.09.pop.h.potenSST5m.050001-060312.nc

Last update: 02/15/2021