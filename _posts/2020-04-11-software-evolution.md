---
date: 2020-04-11 08:51:43
layout: post
title: "Software Evolution"
subtitle:
description: CSC4001 Chapter 10 Software Evoolution
image: /assets/img/background/big_fc8a845754a3da53890168f967141708643d0296.jpg
optimized_image:
category: course
tags:
    - course note
    - CSC4001
    - software engineering
    - system evolution
author: Guochao Xie
paginate: false
math: true
---

- [Evolution Process](#evolution-process)
  - [Life Cycle](#life-cycle)
  - [Evolution Processes](#evolution-processes)
  - [Change Implementation](#change-implementation)
  - [Urgent Change Request](#urgent-change-request)
  - [Agile methods and evolution](#agile-methods-and-evolution)
  - [Handover problem](#handover-problem)
- [Legacy Systems](#legacy-systems)
  - [Legacy system layers](#legacy-system-layers)
  - [Legacy system replacement and change](#legacy-system-replacement-and-change)
  - [Legacy system management](#legacy-system-management)
  - [Business value assessment](#business-value-assessment)
    - [Issues in business value assesement](#issues-in-business-value-assesement)
    - [System quality assessment](#system-quality-assessment)
      - [Business process assessment](#business-process-assessment)
      - [Environment assessment](#environment-assessment)
      - [Application assessment](#application-assessment)
    - [System measurement](#system-measurement)
- [Software maintenance](#software-maintenance)
  - [Types of maintenance](#types-of-maintenance)
  - [Maintenance costs](#maintenance-costs)
    - [Maintenance prediction](#maintenance-prediction)
    - [Complexity metrics](#complexity-metrics)
    - [Process metrics](#process-metrics)
    - [Software reengineering](#software-reengineering)
    - [Refactoring](#refactoring)
    - [Comparing refactoring and reengineering](#comparing-refactoring-and-reengineering)
- [Bad codes](#bad-codes)

# Evolution Process

To maintain the _value_ of business assets, organizations must update the software system. And the majority of the software budget in large companies is devoted to _changing and evolving_ existing software.

## Life Cycle

The lifecycle of a software: 

- *development*
- *evolution*
  - **In operational use and evolving as new requirements are proposed and implemented**
- *servicing*
  - **Software remains useful but we make changes to keep it operational**, like bug fixes. **NO** new functionality is added.

- *retirement*.
  - **Phase-out**: **NO further changes are made to it**.

## Evolution Processes

Software evolution processes depend on

- The type of software
- The development processes
- The skills and experience of users

We use _proposals_ to _drive_ the system evolution, which should be linked with the affected components to *estimate* the cost and impact of the change.

![Software Evolution Process](/assets/img/contents/software-evolution-process.jpg)

## Change Implementation

**Iteration of the development process**: revisions are designed, implemented, and tested. The first stage of change implementation: _program understanding_. During program understanding, we have to understand the _structure_, the _functionality_ and how the proposed change _affect_ the program. 

## Urgent Change Request

**May have to be implemented without going through all stages of software engineering process**. 

- _Serious system fault_ to be repaired.
- _System's environment_ has unexpected effects.
- _Rapid business changes_.

Change requests $\rightarrow$ Analyze source code $\rightarrow$ Modify source code $\rightarrow$ Deliver modified system

## Agile methods and evolution

**Agile are based on incremental development.** Evolution is a continuation of the development process based on _frequent system releases_. **Automated regression testing** is valuable when changese are made. We can express changes as _additional user stories_.

## Handover problem

**Evolution team is unfamiliar with agile methods and prefer a plan-based approach**. They may expect detailed documnetation.

Or **plan-based -> agile**: start from scratch developing automated tests and teh code may not have been refactored and simplified.

# Legacy Systems

**Older systems that rely on old languages and techonology.** May depend on older hardware.

![Elements of a legacy system](/assets/img/contents/legacy-system.jpg)

## Legacy system layers

Business processes $\rightarrow$ application software $\rightarrow$ platform and infrastructure software $\rightarrow$ hardware

## Legacy system replacement and change

Replacement: **Risky and expensive**:

- Lack of complete system _specification_
- Tight _integration_ of system and business processes
- _Undocumented business rules_ embedded in the legacy system
- New software development may be _late_ / _overe budge_

Change:

- No _consistent_ programming style
- Use of _obsolete_ programming languages with few people available
- Inadequate system _documentation_
- System structure _degradation_
- Program optimizations make them _hard to understand_
- _Data errors, duplication, inconsistency_

## Legacy system management

Based on _quality and business value_ to choose a strategy:

- _Scrap the system completely_ and modify business processes so that it is no longer required
- Continue _maintaining_ the system
- Transform the system by _re-engineering_ to improve its _maintainability
- _Replace_ teh system with a new system

## Business value assessment

Take into account:

- System _end-users_
- Business _customers_
- Line managers
- IT managers
- Senior managers

### Issues in business value assesement

- The _use_ frequency of the system.
- The _business processes_ (efficiency)
- System _dependability_
- System _outputs_: If the business depends on system outputs $\Rightarrow$ high business value

### System quality assessment

- **Business process**: How well does the business process support the current _goals_ of the business?
- **Environment**: _Effective_ and _expensive_.
- **Application**: The _quality_ of the application software system

#### Business process assessment

- Defined process model to follow?
- Different part use different processes for the same function?
- How the process has been adapted?
- Relationships with other business processes?
- Effectively supported by the legacy application software?

#### Environment assessment

- Supplier stability
- Failure rate
- Age
- Performance
- Support requirements
- Maintenance costs
- Interoperability (互用性)

#### Application assessment

- Understandability
- Documentation
- Data
- Performance
- Programming language
- Configuration management
- Test data
- Personnel skills

### System measurement

Collect quantitative data:

- \# system change requests: quality
- \# different user interfaces: consistency
- Volume fo data: consistency and errors
- Cleaning up old data

# Software maintenance

**Modify a program after it has been put into use**. For changing _custom software_ (_generic software products_ are said to _evolve_).

## Types of maintenance

- **Fault repairs**: fix bugs / vulnerabilities and correct deficiencies meeting its requirements
- **Environmental adaptation**: adapt software to a different operating environment. change a system to operate in a different environment from its initial implementation.
- **Functionality addition and modification**: new requirements

## Maintenance costs

Usually, it is _greater than_ development costs. A new team has to understand the programs and maintenance work is unpopular. The structure degreades and the programs become harder to change as programs age.

It is affected by both _technical_ and _non-technical_ factors. It increases if maintenance corrupts the software structure that makes further maintenance more difficult.

### Maintenance prediction

**Assessing which parts may cause problems and have high maintenance costs**. 
- **Change acceptance**: maintainability of the affected components.
- Implementing **changes** *degrades* the system and reduces its *maintainability*.
  - *Tightly coupled* systems require changes whenever the environment is changed. Factors include:
    - **Number** and **complexity** of system **interfaces**
    - **Number** of **inherently** volatile system requirements
    - **Business processes**
- **Costs** depend on the number of changes and _maintainability_.

### Complexity metrics

**Complexity** $\Rightarrow$ **maintainability**. Most is spent on a relatively *small* number of components. It depends on
- Control structures
- Data structures
- Object, method and module size

### Process metrics

**Process metrics** $\Rightarrow$ **maintainability**:
- **Number of requests** for corrective maintenance
- **Avg time** required for impact *analysis*
- **Avg time** taken to _implement_ a change request
- **Number** of _outstanding_ change requests.

### Software reengineering

**Restructuring or rewriting** part or all of a legacy system without changing *functionality*. Applicable for frequent maintenance. It may add effort to make them _easier to maintain_.

Pros:
- **Reduced risk**
- **Reduced cost**

![Reengineering Process](/assets/img/contents/reengineering-process.jpg)

Cost factors:
- *Quality* of the software to be reengineered
- *Tool support* available
- *Extent* of *data conversion*
- *Availability* of *expert staff*: Old systems may have some problems.

### Refactoring

**Process of making improvements to a program to slow down degradation through change**. *Preventative maintenance*: reduces the problems of future change. 

It modifies a program to improve its *structure*, reduce its *complexity* or make it easier to *understand*.

No _functionality_ to add. Concentrate on program improvement.

### Comparing refactoring and reengineering

**Reengineering**: _maintenance costs_ are increasing. Use _automated tools_ to process and reengineer a legacy syssem to create a _new and more maintainable_ system.

**Refactoring**: _continuous process_ of improvement through **development and evolution process**. Avoid _structure_ and _code degradation_ that increases the _costs_ and _difficulties_ of maintaining a system.

# Bad codes

- Duplicate code
- Long method
- Switch statements (use polymorphism)
- Data clumping: re-occur the same group of data items. Replaced with an object.
- Speculative generality: can be removed.
