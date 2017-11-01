---
title: "GPU: Nvidia architectures"
date: 2017-11-01T10:00:00
draft: false
tags: ["machine learning", "gpu"]
---

GPU microarchitectures: Tesla -> Fermi -> Kepler -> Maxwell -> Pascal -> Volta

* AWS P3 uses Tesla V100 cards — Volta microarchitecture.
* AWS G3 uses Tesla M60 cards — Maxwell microarchitecure.
* AWS G2 uses Tesla GRID K520 cards — Kepler microarchitecture.
* AWS P2 uses Tesla K80 cards (GK210 GPU) — Kepler microarchitecture.
* AWS CG1 uses Tesla M2050 cards — Fermi microarchitecture.
* GCE provides K80 and P100.

A tensorcore is a logic circuit which allows 16x16 matrix multiplication followed by addition as a primitive. This speeds up the inner loops of deep learning programs.

