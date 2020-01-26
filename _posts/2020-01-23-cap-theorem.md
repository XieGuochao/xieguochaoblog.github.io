---
date: 2020-01-23 12:16:40
layout: post
title: "CAP Theorem"
subtitle:
description: The relative knowledge about CAP Theorem
image:
optimized_image:
category: notes
tags:
  - system
  - distributed system
  - CAP
author: Guochao Xie
paginate: false
---

# CAP Theorem

**CAP** stands for Consistency, Availablility, and Partition Tolerance. As stated in the CAP Theorem, a distributed database system can only have 2 of the 3: Consistency, Availability, and Partition Tolerance.

![](https://robertgreiner.com/content/images/2019/09/CAP-overview-2.png)

Demonstration of CAP Theorem, source https://robertgreiner.com/content/images/2019/09/CAP-overview-2.png

**Consistency** means that any part of the overall system should provide **precisely** the correct data whenever it is requested.

On the other hand, **availability** requires the overall system is always running; that is, whenever a request arrives, the system should promptly (immediately) provide an answer.

For the **Partition Tolerance** condition, even when some amount of the network fail, the system can still function with sufficient data record replications across the networks.

Implied from the **CAP Theorem** is that, when the network goes down, the system must give up either consistency or availability.

# Examples

**CA** examples:

- Relational Databases:
  - SQL Server
  - MySQL
  - Postgre
- Document-oriented Tools:
  - ElasticSearch

**CP** examples:

- MongoDB
- Redis
- AppFabric Caching
- MemcacheDB

**AP** examples:

- DynamoDB
- CouchDB
- Cassandra

# Proof

We can use proof of contradiction to have a brief proof. Assume the system is CAP with 2 servers G1 and G2. 

![](/assets/img/contents/cap21.svg)

First of all, we partition G1 and G2 and set their initial value to be v0. 

![](/assets/img/contents/cap22.svg)

![](/assets/img/contents/cap23.svg)

![](/assets/img/contents/cap24.svg)

Then, we write v1 to G1; however, G2 has no way to notice it. If we now read from G2, the response is v0 not v1. Contradiction!

![](/assets/img/contents/cap25.svg)

![](/assets/img/contents/cap26.svg)

(The source of the illustration: [https://mwhittaker.github.io/blog/an_illustrated_proof_of_the_cap_theorem/]https://mwhittaker.github.io/blog/an_illustrated_proof_of_the_cap_theorem/)

# Thoughts

In the paper _A certain freedom: Thoughts on the CAP Theorem_[1], Eric Brewer considers using **commutative operations**,

  _which make it easy to restore consistency after a partition heals_

However, some non-commutative exceptions may happen in practice; thus, we need to have "delayed exceptions" as "compensation" to consider commutative and simplify eventual consistency during a partition.

---

# References

1. A Certain Freedom: Thoughts on the CAP Theorem. Brewer E. 2010. (PODC' 10) [https://dl.acm.org/doi/abs/10.1145/1835698.1835701](https://dl.acm.org/doi/abs/10.1145/1835698.1835701)
2. There is no free lunch with distributed data. [http://disi.unitn.it/~montreso/ds/papers/NoFreeLunchDS.pdf](http://disi.unitn.it/~montreso/ds/papers/NoFreeLunchDS.pdf)
3. CAP Theorem: Explained [https://robertgreiner.com/cap-theorem-explained/](https://robertgreiner.com/cap-theorem-explained/)
4. An illustrated proof of the CAP Theorem. [https://mwhittaker.github.io/blog/an_illustrated_proof_of_the_cap_theorem/](https://mwhittaker.github.io/blog/an_illustrated_proof_of_the_cap_theorem/)