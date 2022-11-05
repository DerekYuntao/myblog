---
layout: post
title: "Creat conda environment and install climlab"
date: 2022-11-04
description: How to manage conda environment and install climlab
share: true
tags:
 - python
---

conda create --name climlab-env python=3.9

conda info --env

conda activate climlab-env

conda install -c conda-forge climlab

conda deactivate climlab-env

If jupyter notebook cannot find the new environment, one can do this:
conda install -n climlab-env ipykernel

python -m ipykernel install --name climlab-env

Then restart jupyter notebook and change the kernel

conda install -c conda-forge matplotlib

Last update: 11/04/2022