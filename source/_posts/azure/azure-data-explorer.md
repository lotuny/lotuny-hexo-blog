---
title: Azure Data Explorer (Kusto)
date: 2022-02-19 16:35:58
categories: Azure
tags:
---

> Azure Data Explorer is a fully managed, high-performance, big data analytics platform that makes it easy to analyze high volumes of data in near real time.

<!--more-->

## How-to Guides

### Purge

## Kusto Query Language (KQL)

### Aggregation Functions

#### hll()

```| summarize hll(column1) by column2```

HyperLogLog is a probabilistic algorithm to estimate the number of distinct values in a set.

The basis of the HyperLogLog algorithm is the observation that the cardinality of a multiset of uniformly distributed random numbers can be estimated by calculating the maximum number of leading zeros in the binary representation of each number in the set. If the maximum number of leading zeros observed is n, an estimate for the number of distinct elements in the set is 2^n.

First, all values should be hashed to uniformly distributed random numbers. To minimise the variance, the numbers can be splitted to numerous subsets. After then, find the maximum number of leading zeros in each subset, use a harmonic mean to combine these estimates, and obtain an estimate of the cardinality of the whole set.

Reference: https://en.wikipedia.org/wiki/HyperLogLog

#### tdigest()

```| summarize tdigest(column1) by column2```

Calculates the intermediate results for percentile across the group, which are represented as a dynamic array. For example, the result ```[[5],[2,3],[1,2]]``` for a specific group means there are 5 groups in total, with one 2 and two 3s in this group.