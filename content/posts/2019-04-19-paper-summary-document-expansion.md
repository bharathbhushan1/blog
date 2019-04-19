---
title: "Paper summary: Document Expansion by Query Prediction"
date: 2019-04-19T10:00:00
draft: false
tags: ["machine learning", "search engine"]
---

This is a short summary of the paper [Document Expansion by Query Prediction](https://arxiv.org/abs/1904.08375) by Rodrigo Nogueira, Wei Yang, Jimmy Lin, and Kyunghyun Cho.

When a search engine wants to find a document, it uses its index. The costly parts of the ranking algorithm are usually implemented as re-rankers which take the top-n search results and then reorder them (or re-rank them).

This paper has a doc2query ML model which takes a document and comes up with queries which are well answered by the document. Then the query is attached to the document as metadata.

If no re-ranking is allowed, then the query helps the search engine deliver effective results at low latency (compared to neural re-rankers).

If re-ranking is allowed, then SoTA is achieved in a couple of document retrieval tasks. 
