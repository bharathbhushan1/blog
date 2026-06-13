---
title: "Offline Reinforcement Learning"
date: 2026-06-13T10:00:00
draft: false
tags: ["machine learning", "reinforcement learning"]
---

A really nice insight about offline RL algorithms is that off-policy (but online)
algorithms can also be used for offline RL use cases. Structurally they have the same
inputs and same kind of outputs. But there is a fatal flaw.

For out-of-distribution (OOD) actions, the estimated Q value for off-policy algorithms
can never be corrected. These actions are never present in the dataset and can never
really be learnt from. So there is catastrophic overestimation for OOD actions.
