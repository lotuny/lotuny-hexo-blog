---
title: Azure Data Explorer (Kusto)
date: 2022-02-21 21:19:00
categories: Azure
tags:
toc: true
---

> _**Azure Data Explorer** is a fully managed, high-performance, big data analytics platform that makes it easy to analyze high volumes of data in near real time._

<!--more-->

## How-to Guides

### Purge

Purge is designed to be used to protect personal data for obligations under the GDPR. It is not designed to support frequent delete requests or deletion of massive quantities of data, and may have a siginificant performance impact on the service. For other scenarios, retention policy is more suitable.

**Prerequisite**: Enable data purge on your cluster through Azure Portal (Settings/Configurations -> Enable purge) and save.

**Purge syntax**:
```
// Connect to the Data Management service
#connect "https://ingest-[YourClusterName].[region].kusto.windows.net" 

// To purge table records
.purge table [TableName] records in database [DatabaseName] with (noregrets='true') <| [Predicate]

// To purge materialized view records
.purge materialized-view [MaterializedViewName] records in database [DatabaseName] with (noregrets='true') <| [Predicate]
```

**Track status**:
```
.show purges <OperationId>
.show purges [in database <DatabaseName>]
.show purges from '<StartDate>' [in database <DatabaseName>]
.show purges from '<StartDate>' to '<EndDate>' [in database <DatabaseName>]
```

## Kusto Query Language (KQL)

### Tabular operations

#### join operator

```Table1 | join kind=JoinFlavor (Table 2) on CommonColumn, $left.Col1 == $right.Col2```

Join flavors: innerunique (default), inner, leftouter, rightouter, fullouter, leftanti, rightanti, leftsemi, rightsemi

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