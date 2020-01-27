---
date: 2020-01-27 03:30:09
layout: post
title: "Paxos Overview"
subtitle:
description: "An overview of Paxos (Basic version)"
image: /assets/img/EverestStarTrailsJeffDai.jpg
optimized_image:
category: notes
tags:
  - system
  - consensus
  - paxos
author: Guochao Xie
paginate: false
---

# CAP Theorem

CAP Theorem is the basic principle of designing distributed systems. The related knowledge about the CAP Theorem can be found in this note: [CAP Theorem](/cap-theorem)

# Paxos Algorithm

Author: L. Lamport

## Goal: Achieving Consensus in a Distributed System

A consensus algorithm ensures that a **single** one among multiple propose values is chosen. And if a value has been chosen, then processes should be able to **learn** the chosen value.

The goal of the Paxos Algorithm is to **eventually** achieve the consensus of the entire system.

## Assumption: non-Byzantine model

1. Agents:
   1. arbitrary operation speed
   2. may stop and restart
   3. able to remember some information
2. Messages:
   1. Be delivered in arbitrarily long range
   2. Can be duplicated
   3. Can be lost but not corrupted

## Safety Requirements for consensus

1. Only a value that has been **propsoed** may be **chosen**.
2. Only a **single** value is **chosen**
3. A process never **learns** that a value has been **chosen** unless it actually has been.

## 3 Roles For Each Proposal

1. A Proposer
2. Acceptors
3. Learners

## 3 Phases of Choosing a Value

### P1. Prepare

A proposer chooses a new proposal number _n_ and sends a request to each member of some set of acceptors, asking it to respond with:

1. A **promise** never again to accept a proposal numbered less than _n_, and
2. The proposal with the highest number less than n that it **has accepted**, if any.

### P2. Accept

Case 1: If the proposer receives a response to its **prepare** requests (_n_) from a majority of acceptors, then it sends an **accept** to those acceptors for _n_ and value _v_.

Case 2: If an acceptor receives an **accept** for a proposal numbered _n_, it accepts the proposal unless it has already responded to a **prepare** request having a number greater than _n_.

### P3. Learn

The learner will eventually learn the proposed value _v_.

## Paxos Examples

![](/assets/img/contents/paxos3.jpg)

P3.1 is only accepted by S1 before P4.5. Then, S3 sends a promise response to P4.5 and rejects the accept request from P3.1. Finally, the majority of the system accepts P4.5 and the value _y_ instead of _x_.

## Live Lock Example

![](/assets/img/contents/paxos_lock.jpg)

---

# Reference List:

1. Paxos Made Simple - Leslie Lamport. 2001. [https://lamport.azurewebsites.net/pubs/paxos-simple.pdf](https://lamport.azurewebsites.net/pubs/paxos-simple.pdf)
2. Paxos算法详解(知乎). [https://zhuanlan.zhihu.com/p/31780743](https://zhuanlan.zhihu.com/p/31780743)