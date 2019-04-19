---
title: "Video summary: Adaptive Communication Strategies in Local-Update SGD"
date: 2019-04-19T12:00:00
draft: false
tags: ["machine learning", "SysML"]
---

This is a summary of the video [Adaptive Communication Strategies in Local-Update SGD](https://youtu.be/RTGb-sbK19s) from [SysML 2019](https://www.sysml.cc) conference.

Datasets as well as ML models are becoming larger. So SGD on a single machine is slow. Generally we need to optimise _Error Convergence_ (actual training progress per iteration). But in a distributed setting we also need to optimise _System Throughput_ (iterations per unit of wall-clock time). Ideally we need to focus on both (actually the product of the two, which is training progress per unit of wall-clock time).

<!--more-->

### Strategy 1
Distributed fully-synchronous SGD. This has two problems. The computation time across nodes is variable, even with same mini-batch size. Communication time is significant and, for larger models, is exorbitant.

### Strategy 2
An improvement is Periodic Averaging SGD. Several local parameter updates happen before a global averaging is done. This reduces communication overhead and also smoothens the computation delays of different nodes/batches. But this has problems with convergence.

### Observation
For larger communication delay, the error convergence is very good initially but it settles down at a higher value. For a small communication delay, the error decreases slowly but ultimately settles at a better optimum. Most importantly the best communication period changes with time -- hence Adaptive communication strategy is better.

### Proposal ([AdaComm](http://www.sysml.cc/doc/2019/124.pdf))
Split the overall training into fixed time periods. In each time period use a fixed communication delay (in terms of number of epochs). The initial value is computed via simple grid search. For subsequent values, the insight used is that _ratio of delays is equal to the square root of ratio of training losses_ (for any two given training time periods). So communication delay decreases as training loss decreases.

This idea is very similar to adaptive learning rate strategies. But wall clock time is used instead of number of epochs. Also this tradeoff of communication overhead vs error convergence can be used in other kinds of SGD.

## Appendix: Tangential learning while writing this note.

* Downpour SGD (from Google's [DistBelief](https://ai.google/research/pubs/pub40565), 2012) uses model parallelism, data parallelism, parameter servers, asynchronous updates - to implement fast distributed SGD. The parameter servers don't share updates with each other.

* Softmax is actually soft-arg-max, a smooth approximation to the argmax function which returns the index of the maximum element in a list.
