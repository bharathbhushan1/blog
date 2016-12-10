---
title: "Spark memory management"
date: 2016-12-10T10:00:00
draft: false
tags: ["apache spark", "big data"]
---

MemoryManager is the place where I started looking at spark’s memory management architecture. This class handles the split of the overall memory into four dynamic portions — onHeapStorage, onHeapExecution, offHeapStorage, offHeapExecution. Execution memory is transient (shuffle, join, aggregates, sorting) and storage memory is longer lived (cache, broadcast variables). MemoryManager depends on MemoryStore to actually allocate and free memory.

There are two kinds of MemoryManagers — StaticMemoryManager and UnifiedMemoryManager. Unified allows execution and storage to borrow memory from each other while Static does not allow this.

TaskMemoryManager uses the MemoryManager to manage memory for a task. TaskMemoryManager also has a list of MemoryConsumers that support spilling. Also it does page based allocations.

