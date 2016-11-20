---
title: "Spark - QueryExecution"
date: 2016-11-20T10:00:00
draft: false
tags: ["big data", "apache spark"]
---

Today I am going to talk about the **QueryExecution** class in Spark. This class takes care of providing fine grained access to different stages of running a SparkSQL query. Basically the parsing, analysis, optimization and physical planning are all accessible via this class. It also handles inserting extra operations like extracting python UDFs, inserting shuffle stages, internal row format conversions etc.

**IncrementalExecution** is a derived class which allows the same query to be run repeatedly in an incremental fashion, with possible state save in between executions. This is used in Structured Streaming. It is also used to allow users to run their own optimization strategies which is very cool.

