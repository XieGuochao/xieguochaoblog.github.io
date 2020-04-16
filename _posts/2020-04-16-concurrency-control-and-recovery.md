---
date: 2020-04-16 01:18:32
layout: post
title: "Concurrency Control and Recovery"
subtitle:
description: CSC3170 Chapter 14 Concurrency Control and Recovery
image: /assets/img/background/wallpaper-1835304.jpg
optimized_image:
category: course
tags:
    - CSC3170
    - course note
    - database
    - transaction
    - lock
author: Guochao Xie
paginate: false
math: true
---

# Lock-Based Protocols

- **Exclusive (X)**: read-write lock.
- **Shared (S)**: read-only lock.
- **Upgrade**: convert **lock-S** to **lock-X**
- **Downgrade**: convert **lock-X** to **lock-S**

**Locking Protocol**: a set of _rules_ followed by all transactions while requesting and releasing locks. It enforces **serializability** by restricting the set of possible _schedules_.


## Two-Phase Locking Protocol

**Ensure conflict-serializable schedules**

1. **Growing Phase**: Obtain locks
2. **Shrinking Phase**: Release locks

**Lock point** is the point where a transaction _acquired_ its _final lock_. The transactions can be **serialized** in the order of their **lock points**.

**Considering recoverability**:

1. **Strict two-phase locking**: hold all **X-locks** till it commits / aborts.
2. **Rigorous two-phase locking**: hold all **locks** till it commits / aborts. (Most)

## Lock Manager

A **separate process**: transactions can send lock and unlock requests as messages. It maintains an in-memory data-structure: **lock table**.

### Lock Table

![Lock Table](/assets/img/contents/lock-table.jpg)

# Graph-Based Protocols

**Partial ordering**: $d_i \rightarrow d_j$ then all transaction accessing both should access $d_i$ before $d_j$. **Tree-protocol** is a simple kind of graph protocol.

## Tree Protocol

- **Exclusive locks** only.
- **Unlock** at any time.
- **Locked and unlocked** by $T_i$ cannot subsequently be _relocked_ by $T_i$.


## Deadlock

**Every transaction in the set is waiting for another transaction in the set**.

**Starvation**: a transaction caonnot complete its task for an *indefinite* period of time while other transactions continue. FOr example, 2 transactions competing for the same _X-lock_ and are _repeatedly rolled back_ due to **deadlocks**.

**Deadlock prevention** protocols: never enter into a deadlock state:

Strategies:
- **Pre-declaration**: require each transaction locks all its data before it begins.
- **Graph-based protocol**: partial ordering of all data items. Lock only in the specified order.

Schemes:
1. **Wait-die**:
  - *non-preemptive*
  - Older wait for younger to release.
  - Younger roll back.
  - May die several times before acquiring a lock.
2. **Wound-wait**:
  - *preemptive*
  - Older wounds (*force rollback*) younger.
  - Younger may wait for older ones.
3. **Timeout-based**:
  - Solve deadlocks by timeout.
  - May have unnecessary roll back.
  - Difficult to determine good timeout interval.
  - Possible starvation.

**1 and 2**: Ensure older transactions have **precedence** over newer ones to avoid startation.

### Deadlock Detection

**Wait-for graph**. Deadlock occurs $\Leftrightarrow$ a cycle.

### Deadlock Recovery

- **Total rollback**: Abort and restart.
- **Partial rollback**: Only as far as necessary to release locks.

Starvation:
- Same transaction is always chosen as a *victim*.
- Solution: *oldest* is never chosen as a *victim*.

## Multiple Granularity

Various sizes and a hierarchy of data granularities. Represented as a tree. Lock the descendant nodes.

- **Fine granularity** (lower): high _concurrency_ and high _locking overhead_.
- **Coarse granularity** (higher): low _concurrency_ and low _locking overhead_.

### Intention Lock Modes

In addition to **S** and **X** locks, there are 3 additinoal lock modes:

- **Intention-shared (IS)**: a _shared lock_ will be requested on some _descendant nodes_.
- **Intention-exclusive (IX)**: an _exclusive lock_ will be requested on some _descendant nodes_.
- **Shared and intention-exclusive (SIX)**: current node is locked in _shared_ mode but an _exclusive lock_ will be requested on some _descendant nodes_.

**Intention locks** allow a higher level node to be locked in *S or X mode* without having to check _all descendent nodes_.

The compatibility matrix for all lock modes:

![Compatibility matrix for all lock modes](/assets/img/contents/compatibility-matrix.jpg)

The following rules to lock a node Q:

1. Compatibility matrix.
2. Lock root first.
3. $T_i$, **S** or **IS**: only if the **parent** is locked by $T_i$ in either **IX** or **IS** mode.
4. $T_i$, **X**, **SIX**, or **IX**: only if the **parent** is locked by $T_i$ in **IX** or **SIX**.
5. $T_i$ can lock only if it has _not previously unlocked_ any node.
6. $T_i$ can unlock only if _none of the children_ are locked by $T_i$.

Acquire: **root-to-leaf**; Release: **leaf-to-root**.

**Lock granularity escalation**: too *many locks* at a particular level.

# Timestamp-Based Protocol

Each $T_i$ is issued a timestamp $TS(T_i)$ when it enters the system: _unique_, _strictly increasing_.

## Timestamp-Ordering Protocol (TSO)

Maintain Q:

- **W-timestamp(Q)**: largest that executed **write(Q)** successfully.
- **R-timestamp(Q)**: largest that executed **read(Q)** successfully.

Rules:

- **Conflicting operations**: timestamp order.
- **Out of order operations**: rollback.

## Thomas' Write Rule

Modified version of **timestamp-ordering protocol** where obsolete **write** operations may be ignored.

When $TS(T_i) <$ W-timestamp(Q), $T_i$ will **ignore** instead of **roll back**.

# Recovery Algorithms

- Actions taken during *normal* transaction processing: *enough information* to recover from failures.
- Actions after a *failure*: recover contents to a state of _atomicity_, _consistency_, and _durability_.

## Failure Classification

- Transaction Failure
  - Logical errors: internal error condition
  - System errors: must terminate an active transaction (deadlock)
- System crash:
  - Fail-stop assumption: non-volatile storage contents: preserved by system crash
- Disk failure:
  - Destruction is assumed to be detectable (checksum)

## Storage Structure

- **Volatile**
- **Nonvolatile**
- **Stable storage**: a mythical form of storage that survives all failures. Approximated by maintaining *multiple copies* on distinct *nonvolatile* media.

## Data Access

Each transaction $T_i$ has its **private work-area** _local copies_ of data.

Read on and write to the **buffer block** in the **private work-area**. **read** for the first time of accessing X. **write** any time.

## Recovery and Atomicity

- **Shadow-copying** and **shadow-paging**: a new copy of data. After update, move the _pointer_ to the new block.
- **Log-Based**: _log records_ on _stable storage_:
  - <$T_i$ start>
  - <$T_i, X, V_1, V_2$> : $V_1$: old, $V_2$: new.
  - <$T_i$ commit>

Logs are _buffered_ in main memory, and when _log force_ we output them to stable storage.

The transaction enters the commit state only when the log record <$T_i$ commit> has been output to stable storage. Before a block buffered in memory output to the _database_, all logs should be output to _stable storage_. $\Rightarrow$ **Write-ahead logging (WAL)**

## Modification

- **Immediate-modification**: uncommitted transaction made to buffer or disk. Update log record before write to database.
- **Deferred-modification**: only at the time of transaction commit.

### Commit Point

**Effect of all the transaction operations on the database have been output to the log.**

**Have committed**: _commit log record_ is output to stable storage.

## Undo and Redo

- **undo**: follow the log to roll back. Write <$T_i$ abort>.
  - If transaction starts but **not** commit / abort.
- **redo**: go forward from the first log record for $T_i$.
  - If transaction contains start **and** commit / abort.

**Repeating history**: $T_i$ is undone earlier and the <$T_i$ abort> was written to the log and then a failure occurs. On the recovery, $T_i$ is redone: redoes all the original actions of $T_i$ including the steps that _restored old values_.

## Checkpoints

1. Output all _log records_ in main memory on stable storage.
2. Output all _modified buffer_ blocks to disk.
3. Write `<Checkpoint L>` on stable storage. L: a list of all transactions active at the time of checkpoint.
4. Stop all updates when doing checkpointing.

We only need to consider the most recent transactoin that started before the checkpoint and all transactions after it.

## Recovery Algorithm

**Logging**:

1. `<T start>`
2. `<T, X, V1, V2>`
3. `<T commit>`

**Transaction rollback**:

1. $T_i$ be the transaction to roll back.
2. Scan log backwards, perform undo by writing $V_1$ to $X_j$, and write a log <$T_i, X_j, V_1$> (**compensation log records**).
3. Stop the scan when see `<T start>` and write `<T abort>`.

**Recovery**:

- **Redo**:
  1. Find last `<Checkpoint L>`
  2. Scan forward from L:
    - Redo operations.
    - For `<T start>`: add to undo-list
    - For `<T commit>` or `<T abort>`: remove from undo-list.
- **Undo**:
  - Scan backward:
    - For undo-list's operations: **rollback**
    - For undo-list's start: write **abort**, remove from undo-list
    - Stop until undo-list is empty.

![Recovery Example](/assets/img/contents/recovery-example.jpg)
