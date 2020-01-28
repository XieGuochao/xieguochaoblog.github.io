---
date: 2020-01-28 02:26:08
layout: post
title: "Raft Overview"
subtitle:
description: An overview of the Raft Algorithm
image: /assets/img/contents/annie-solo.png
optimized_image:
category: notes
tags:
  - Raft
  - system
  - distributed system
  - consensus
author: Guochao Xie
paginate: false
---

## Raft

Raft, a consensus algorithm designed to be easy, is equivalent to [Paxos](/paxos-overview/) in fault-tolerance and performance. However, it is decomposed into **independent** **subproblems** and points all major components needed for practical systems.

## Consensus Problem

Consensus is one component of the [CAP Theorem](/cap-theorem/). It is a fundamental problem in _fault-tolerant_ distributed systems. Consensus means once the entire system reach a decision on a value, the decision is final. Typical consensus algorithms run when any **majority** of servers is available.

For replicated state machines, each server has a state machine and a log. The state machine is what we want to achieve fault-tolerant and it appears to clients like a single, reliable state machine.

For each state machine, it takes input commands from its log. A consensus algorithm's goal is to have the entire system agrees on the commands in the servers' logs, that is the same series of commands used to produce the same series of results and states.

## Excellent Demo

An excellent demo: [http://thesecretlivesofdata.com/raft/](http://thesecretlivesofdata.com/raft/)

<!-- <iframe src="http://thesecretlivesofdata.com/raft/"/> -->


## Node Properties

3 States:

- _Follower_
- _Candidate_
- _Leader_

## Procedure

1. Start from _Follower_
2. If no heart beat, _Follower_ => _Candidate_
3. _Candidate_ requests votes from other nodes.
4. If receiving a mojority of votes, _Candidate_ => _Leader_.
5. _Leader_ communicates with Client.

### Log Replication

Write request => write log response => commit request => commit response

### Leader Election

#### Election Timeout

The amount of time a _Follower_ waits until becoming a _candidate_. Randomized from 150ms to 300ms.

After election timeout, _Follower_ => _candidate_. When receiving a vote request, _Follower_s will send a vote response and reset their own election timeouts.

Another case we use election timeout is during the election. 

When a _candidate_ sends a vote request, 

1. if it receives a heartbeat from a _leader_, then _candidate_ => _Follower_;
2. if it receives a majority of votes, then candidatne => _leader_;
3. if **election timeout** elapses without receving heartbeat, **re-election**.

### Partition Consensus

During partition, the consensus of the entire system is still maintained because the minority will never have responses from a majority and thus it cannot commit anything. The majority group can still function.

After the partition, the majority group will have a **higher term** (term of election), so that the _leader_ of the minority group will recognize the new _leader_ and reduce itself to be a _Follower_.


References
===

1. The Raft Consensus Algorithm. [https://raft.github.io/](https://raft.github.io/)
2. In Search of an Understandable Consensus Algorithm (Extended Version). Ongaro D., Ousterhout J. [https://raft.github.io/raft.pdf](https://raft.github.io/raft.pdf)
3. The Secret Lives of Data. [http://thesecretlivesofdata.com/raft/](http://thesecretlivesofdata.com/raft/)