---
layout: post
title: "Download TES data: an example for download data by wget in batch"
date: 2022-10-03
description: How to download data in batch using wget given username and password
share: true
tags:
 - Web
 - Big data
---

[TES daily temperature observation data can be found here](https://cmr.earthdata.nasa.gov/search/concepts/C1451577780-LARC.html)

After choosing the data to be downloaded, "EARTHDATA" will generate a list with all links of files. One can download that data link file in .txt format and then download the data given your username and password as follows:

```powershell
wget -i TES_level3_TEMP_dailydownload.txt -P /glade/vertical_temp/TES/ --user <user> --password <password>
```

Last update: 2022/10/03
