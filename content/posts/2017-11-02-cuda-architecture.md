---
title: "GPU: CUDA architecture"
date: 2017-11-02T10:00:00
draft: false
tags: ["machine learning", "gpu"]
---

CUDA is Nvidia’s initiative to define a programming model and hardware architecture which aims to allow GPUs to be unified with CPUs for compute. There are various aspects of CUDA and we will try to gain an appreciation for it in this article.

An application which wants to use GPU for some general purpose compute has to define a kernel function (C interface). This kernel function is run on lots of data elements in parallel. Each GPU can run different kernels, possibly from different applications. One kernel submitted to the GPU runs as a grid. Each grid consists of many thread blocks. Each thread block consists of many threads.
