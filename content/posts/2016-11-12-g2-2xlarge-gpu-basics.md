---
title: "g2.2xlarge GPU basics"
date: 2016-11-12T12:00:00
draft: false
tags: ["machine learning", "gpu"]
---

A Graphics Processing Unit is a specialized processor usually sitting on a special card with its own memory. It does not have interrupts or virtual memory.

For example EC2's g2.2xlarge machine has the following graphics card.

> 00:03.0 VGA compatible controller: nVidia Corporation GK104GL [GRID K520] (rev a1)

[GRID K520](https://www.techpowerup.com/gpudb/2312/grid-k520) belongs to Nvidia's Tesla card series and has GRID support (which allows GPU sharing). It plugs into a PCI slot on the motherboard. Each such card has 2 GK104GL GPUs, two 4GB RAM units, and 2x1536 CUDA cores. I will explain in detail below.

The RAM is special - GDDR5. GDDR can request and receive data in the same memory clock cycle. It also has a much wider memory bus than the normal DDR RAM. Consequently the memory bandwidth of the GPU is 160 GBps which is at least 3 times that available to the E5–2670 CPU on the same machine.

The CPU clock is 2.5GHz while the GPU clock is 800MHz. So each CUDA core is at least a third slower than each cpu hyperthread. Basically each CUDA core is slower but there are many many more of them, ala Agent Smith.

The GPU has 2.5 TFLOPS single precision floating point performance.

The GPU has a 8 SMX - streaming multiprocessors - each having 192 CUDA cores. The 192 cores are arranged in 6 warps of 32 cores each. A warp is a group of cores all running a single instruction. A core is pretty simple - contains FPU+IPU+logic unit+branch unit. Overall 1536 CUDA threads can be running simultaneously on a GPU.

### References
* http://docs.nvidia.com/cuda/cuda-c-best-practices-guide/
* http://stackoverflow.com/questions/12331225/cuda-threads-smx-sp-and-blocks-how-do-they-work
* https://www.techpowerup.com/gpudb/2312/grid-k520
