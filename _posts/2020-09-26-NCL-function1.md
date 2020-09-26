---
layout: post
title: "NCL time2calendar function"
date: 2020-09-26
description: A self-defined NCL function to convert numerical time to calendar
share: true
tags:
 - NCL
---

Function time2calendar is written in a single NCL script.
```powershell
load "$NCARG_ROOT/lib/ncarg/nclscripts/csm/gsn_code.ncl"
load "$NCARG_ROOT/lib/ncarg/nclscripts/csm/gsn_csm.ncl"
load "$NCARG_ROOT/lib/ncarg/nclscripts/csm/contributed.ncl"
function time2calendar(time_in) 
begin
   month_abbr = (/"","Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep", \
                       "Oct","Nov","Dec"/)
    time = time_in
    time@units = time_in@units
    utc_date = cd_calendar(time, 0) ;convert to UTC
    year = tointeger(utc_date(:,0))
    month = tointeger(utc_date(:,1))
    day = tointeger(utc_date(:,2))
    hour = tointeger(utc_date(:,3))
    minute = tointeger(utc_date(:,4))
    date_str = sprinti("%0.2iZ ", hour) + sprinti("%0.2i ", day) + \
    month_abbr(month) + " " + sprinti("%0.4i", year)
    return(date_str)
end    
```

In a script, we can call this function as follows
```powershell
load "time2calendar.ncl"  
begin
...
date=time2calendar(u_obs&time)
print(date1)
...
end
```

Last update: 09/26/2020