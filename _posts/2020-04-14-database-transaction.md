---
date: 2020-04-14 00:52:35
layout: post
title: "Database Transaction"
subtitle:
description: CSC3170 Chapter 13 Database Transaction
image: /assets/img/background/wk1rnn7.jpg
optimized_image:
category: course
tags:
    - CSC3170
    - course note
    - database
    - transaction
author: Guochao Xie
paginate: false
math: true
---

**Transaction: a unit of program execution that accesses and possibly updates various data items.**

Main issues to deal with in transaction is : _failures_ and _concurrent execution_. 2 **requirements** for transaction are **atomicity requirement**,  **durability requirement**, **consistency requirement**, and **isolation requirement**.

- **Atomicity requirement**: partially executed transaction are not reflected in the database.
- **Durability requirement**: persist commited transaction.
- **Consistency requirement**:
  - _Explicit_ integrity constraints like primary key and foreign keys.
  - _Implicit_ integrity constraints like the sum.
  - _Temporarily_ inconsistent during transaction execution.
  - After transaction, the database must be _consistent_
- **Isolation requirement**: Can be achieved by running transactions _serially_.

# ACID

- **Atomicity**: Either all operations of the transaction are properly reflected in the database or none are.
- **Consistency**: Execution of a transaction in isolation preserves the _consistency_ of the database.
- **Isolation**: Each transaction must be _unaware) of other concurrently executing transactions. Hide _intermediate transaction results_ from others. That is, for $T_i$ and $T_j$, it _appears_ to $T_i$ that either $T_i$ finishes $\rightarrow$ $T_j$ starts or $T_j$ finishes $\rightarrow T_i$ starts.
- **Durability**: Persist transactions.

# Transaction State

- **Active**: initial
- **Partially committed**: execute the final statement.
- **Failed**: Cannot execute.
- **Aborted**: After rolling back the transaction and the database _resotores_ to its state _prior_ to the start of the transaction. 2 Options:
  - _Restart_.
  - _Kill_.
- **Committed**: after successful completion.

# Concurrent Executions

The advantages include: _increased processor and disk utilization_ that leads to better transaction _throughput_ and _reduced average response time_.

We use **concurrency control schemes** as the mechanisms to achieve isolation. That is to control the order of them.

## Schedule

**A sequence of instructions that specify the chronological (按时间前后) order to execute transactions**. It should include _all_ instructions and _preserve_ the _orders_.

## Serializability

Assume each transaction preserves database _consistency_. Thus, serial execution preserves consistency.

A schedule is serializable if it is _equivalent to_ a serial schedule: _conflict serializability_ and _view serializability_.

### Conflict

Instructions $I_i \& I_j$ conflict if and only if there exists $Q$ accessed by both $I_i \& I_j$ and at least one _wrote_ $Q$. A conflict between $I_i, I_j$ forces a temporal _order_ between them.

**Conflict equivalent** means $S$ can be transformed into $S'$ by a series of _swaps_ of _non-conflicting instructions_. $S$ is _conflict serializable_ if it is **conflict equivalent** to a _serial schedule_.

### Test for Serializability

We use **Procedence graph** (direct graph where *vertices* are transactions). $T_i \rightarrow T_j$ if $T_i$ accessed the data on which the conflict arose _earlier_.

**Serializable** if only if the precedence graph is _acyclic_. Then we can use **topological sorting** to obtain the serializability order.

## Recoverable Schedules

**Transaction failures on concurrent transactions.** $T_j$ reads data previously written by $T_i$ and $T_i$ commits **before** $T_j$ commits. Then, if $T_i$ has failure and must abort, $T_j$ should be recoverable to avoid _inconsistency_.

## Cascading Rollbacks

**A single failure** leads to a series of transaction rollbacks.

### Cascadeless Schedules

No **cascading rollbacks** will occur. Every cascadeless schedule is *recoverable*.

For $T_j$ reads a data previously written by $T_i$, the **commit** of $T_i$ appears **before** the read of $T_j$. 

## Concurrency Control

**Ensure that all possible schedules** are

- either **conflict** or view **serializable** and
- **recoverable** and perferably **cascadeless**

The **goal** is to develop concurrency control _protocols_ to assure _serializability_. Tradeoff between the amount of concurrency and overhead.

## Weak Levels of Consistency

**Allow schedules that are not serializable**. E.g. read only transactions need not be serializable.

# Transaction Definition in SQL

_Begin_ _implicitly_ and _ends_ by **COMMIT** or **ROLLBACK**.

