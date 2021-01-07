---
layout: post
title: "Use Anaconda to Install Python Package Locally"
date: 2021-01-07
description: Ways to install python package to anaconda
share: true
tags:
 - python
---

There are 3 ways to install python packges. 
## Way 1 - Anaconda installation:

    conda install -c conda-forge <pkg_name>

## Way 2 - Pip installation:

    pip install <pkg_name>

## Way 3 - Install locally:
Step1: clone package from Github
Step2: Unzip package to directory `~anaconda3/pkgs/`    
Step3: Entering `~anaconda3/pkgs/` and install pakage by:

    python setup.py install

If we import the packge in python and is tolded that the package is not found, just restart anaconda and try again. 

Last update: 01/07/2021    