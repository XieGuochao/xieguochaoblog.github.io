---
date: 2020-04-10 08:39:54
layout: post
title: "System Modeling"
subtitle:
description: Corresponding to CSC4001 Chapter 6 System Modeling
image: /assets/img/background/_3128_x_1760__beautiful_golden_colorado_in_169_-_imgur.jpg
optimized_image:
category: course
tags:
    - course note
    - CSC4001
    - software engineering
    - system modeling
author: Guochao Xie
paginate: false
math: true
---

- [System Modeling](#system-modeling)
  - [System Perspectives](#system-perspectives)
  - [Graphical Models](#graphical-models)
- [Context Model](#context-model)
  - [System Boundary](#system-boundary)
- [Process Model](#process-model)
- [Interaction Model](#interaction-model)
  - [Use Case Model](#use-case-model)
  - [Sequence Diagrams](#sequence-diagrams)
- [Structural Model](#structural-model)
  - [Class Diagram](#class-diagram)
    - [Generalization](#generalization)
- [Behavioral Model](#behavioral-model)
  - [Data-driven Modeling](#data-driven-modeling)
  - [Event-driven Modeling](#event-driven-modeling)
    - [State Machine Model](#state-machine-model)
- [Model-driven Engineering (MDE)](#model-driven-engineering-mde)
  - [Pros and Cons of MDE](#pros-and-cons-of-mde)
  - [Model driven architecture (MDA)](#model-driven-architecture-mda)
    - [Types of Model](#types-of-model)
    - [Agile and MDA](#agile-and-mda)
    - [Adoption of MDA](#adoption-of-mda)
- [Summary](#summary)

# System Modeling

**The process of developing abstract models of a system, with each model presenting a different view or perspective of that system.**

System modeling helps to understand the *functionality* and *communicate* with customers. Models are used during **requirement enineering** to *clarify* what the system does and can be used to discuss its strengths and weaknesses. For new systems, models help to explain the proposed requirements and discuss design proposals.

## System Perspectives

- External: model the context or environment.
- Interaction: model the interactions between a system and environment, or between components of a system.
- Structural: model the organization of a system or the structure of data.
- Behavioral: model the dynamic behavior and how it responds to events.

## Graphical Models

1. Facilitating discussion.
2. Documenting.
3. Description to generate implementation.

# Context Model

**Illustrate the operational context of a system** (what lies outside the system boundaries).

## System Boundary

**What is inside and what is outside the system**. It has a _profound_ (意味深远的) effect on the system requirements. Definining a system boundary is a _political judgement_ because of _pressures_ to develop system boundaries that effect the _influence or workload_ of different parts of an organization.

![The Context of the Mentcare System](/assets/img/contents/context-of-mentcare.jpg)

# Process Model

**How the system is used in broader business processes.**

![Process Model of Involuntary Detention](/assets/img/contents/process-model-of-involuntary-detention.jpg)

# Interaction Model

**Identify user requirements.** It highlights the _communication_ problems that may arise. **Deliver the required system performance and dependability**. 

$\Rightarrow$ **case diagram** and **sequence diagrams**.

## Use Case Model

**Requirements elicitation**. Each use case: a discrete task + _external interaction_ with a system. _Actors_ may be people / other systems.

![Transfer data in the Mentcare System](/assets/img/contents/use-case-mentcare.jpg)

![Use cases in Mentcare System involving "Medical Receptionist"](/assets/img/contents/use-cases-mentcare-medical.jpg)

## Sequence Diagrams

**Interactions between the actors and the objects within a system**. It shows the sequence of _interactions_ that take place during a particular use case (instance). We list the _objects_ and _actors_ on the **top** and use _annotated arrows_ to indicate _interactions_.

![Sequence diagram for view patient information](/assets/img/contents/sequence-diagram-view-patient-information.jpg)

![Sequence Diagram for Transfer Data](/assets/img/contents/sequence-diagram-for-transfer-data.jpg)

# Structural Model

**Organization of a system in terms of the components that make up that system and their relationships**.

## Class Diagram

**Object-oriented system model to show the classes and the associations**. **Object class**: general definition. **Association**: relationships. 

![Class and associations in the MHC-PMS](/assets/img/contents/class-and-association.jpg)

### Generalization

**Place entities in more general classes and learn the characteristics**.

![Generalization hierarchy](/assets/img/contents/generalization-hierarchy.jpg)

# Behavioral Model

**Dynamic behavior**: what happens when a system responds to a stimulus (_data_, _events_).

## Data-driven Modeling

Controlled by the data input to the system. It shows the _sequence_ of actions involved in processing _input data_ and generating an associated _output_. Useful during the _analysis_ of requirements to show _end-to-end_ processing in a system.

![Activity model of the insulin pump's operation](/assets/img/contents/activity-model-insulin-pumps.jpg)

![Order processing](/assets/img/contents/order-processing.jpg)

## Event-driven Modeling

**Respond to external and internal events**: _finite_ number of states and events may cause a _transition_ of states.

### State Machine Model

- Nodes: states.
- Arcs: events.

![State diagram of a microwave oven](/assets/img/contents/state-diagram-microwave-oven.jpg)

# Model-driven Engineering (MDE)

**Models are the principal outputs**: automatically generate hardware / software platform. _Abstraction_ in software engineering.

## Pros and Cons of MDE

- Pros
  - Higher level of _abstraction_
  - Automatically generate code $\Rightarrow$ _cheaper_ to _adapt_ systems to new platforms.
- Cons
  - Models for _abstraction_ and not necessarily right for _implementation_.
  - Costs of developing _translators_ for new platforms.

## Model driven architecture (MDA)

(Precursor of MDE). **Model-focused approach to software design and implementation**. Create different levels of _abstraction_. From a high-level platform independent model, it is possible to generate a working program withou _manual intervention_. 

### Types of Model

- **CIM: Computation independent model**: _domain abstractions_.
- **PIM: Platform independent model**: _operation of the system_ without reference to its _implementation_. Show _static system structure_ and hwo it responds to _external and internal events_.
- **PSM: Platform specific model**: _PIM + platform-specific detail_.

![MDA transformations](/assets/img/contents/mda-transformations.jpg)

### Agile and MDA

**MDA** supports an _iterative approach_ to be used within agile methods. If _transformations_ can be completely _automated_ and a complete program generated from a **PIM**, then MDA could be used in an **agile** development without separate coding.

### Adoption of MDA

- Specialized tool support for transforming models.
- Limited tool _availability_ so we require _tool adaptation and customization_ to their environment.
- Prefer big companies.
- **Models**: software design. **Abstractions** may alter.
- _Platform-independence_ : good for large, long-lifetime systems.

# Summary

- Model: _abstraction_; ignore system details.
- **Context models**: environment with other systems and processes.
- **Use case diagrams** and **sequence diagrams**: interactions between users and systems.
  - **Use case diagrams**: system + external actors.
  - **Sequence diagrams**: system objects.
- **Structural models**: organization and architecture.
- **Class diagrams**: _static_ structure of _classes_ and their _associations_. 
- **Behavioral models**: _dynamic behavior_ (data / event).
- **Activity diagrams**: _processing of data_.
- **State diagrams**: _behavior_ in response to _internal_ or _external_ events.
- **odel-driven engineering**: a set of models that can be automatically _transformed_ to code.