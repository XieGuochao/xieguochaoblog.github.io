---
date: 2020-04-10 13:39:32
layout: post
title: "Design and Implementation"
subtitle:
description: CSC4001 Chapter 8 Design and Implementation
image: /assets/img/background/407944.png
optimized_image:
category: course
tags:
    - course note
    - CSC4001
    - software engineering
    - software design
    - software implementation
author: Guochao Xie
paginate: false
math: true
---

- [Build or Buy](#build-or-buy)
- [Object-oriented design using UML](#object-oriented-design-using-uml)
  - [Process Stages](#process-stages)
  - [System Context and Interactions](#system-context-and-interactions)
  - [Architectural Design](#architectural-design)
  - [Object Class Identification](#object-class-identification)
  - [Design Models](#design-models)
    - [Subsystem Model](#subsystem-model)
    - [Sequence Model](#sequence-model)
    - [State Diagrams](#state-diagrams)
    - [Interface Specification](#interface-specification)
- [Design Patterns](#design-patterns)
  - [Pattern Elements](#pattern-elements)
    - [Observer Pattern](#observer-pattern)
  - [Design Patterns](#design-patterns-1)
- [Implementation Issues](#implementation-issues)
  - [Reuse](#reuse)
  - [Configuration Management](#configuration-management)
  - [Host-target development](#host-target-development)
- [Open Source Development](#open-source-development)
  - [Open Source Licensing](#open-source-licensing)

Inter-leaved design and implementation:

1. Deisgn: _creative activity_ to identify components and relationships based on requirements.
2. Implementation: the process of _realizing_ the design as a program.

# Build or Buy

Buy **off-the-shelf systems (COTS)** that can be _adapted_ and _tailored_ to the users' requirements. Concern how to use the configuration features to deliver the requirements.

# Object-oriented design using UML

## Process Stages

- Define the _context and modes of use_ of the system
- Design the system _architecture_
- _Identify_ the principal system _objects_
- _Develop_ design _models_
- Specify _object interfaces_

## System Context and Interactions

Relationship between the software and external environment: decide the _functionality_ and structure the _communication_ with its environment. Understand the _context_: establish the _boundaries_ of the system.

- **System context model**: _structural model_ of **other** systems in the environment.
- **Interaction model**: _dynamic model_ shows how the system _intereacts_ with its environment.

## Architectural Design

**Interactions** $\Rightarrow$ designing system **architecture**. Identify major components, then organize the components using an _architectural pattern_. 

## Object Class Identification

- **Grammatical approach** based on natural language description.
- Base in **application domain**
- **Behavioral approach** based on behaviors.
- **Scenario-based analysis**: identify _objects_, _attributes_, and _methods_.

## Design Models

- **Structural Models**: static structure.
- **Dynamic Models**: interactions.

### Subsystem Model

**Logically related groups** of objects. Using **packages** in UML. 

### Sequence Model

**Sequence of object interactions**:

- **Objects**: horizontally across the *top*
- **Time**: vertically
- **Interactions**: labelled arrows.
- **Thin rectangle**: the time when the object is the controlling obejct.

![Sequence diagram describing data collection](/assets/img/contents/sequence-diagram-data-collection.jpg)

### State Diagrams

**Response to different service requests and the state transitions**. **High-level** models or **run-time** behavior. 

### Interface Specification

Objects and other components can be designed in parallel. Avoid the *interface representation* but hide this in the object itself. Have several interfaces as the *viewpoints* on the methods.

# Design Patterns

**Reuse abstract knowledge about a problem and its solution**. They are ways to describe _best practices, good designs, and acpture experience_ in a way that it is possible for others to **reuse** this experience.

## Pattern Elements

- Name (Identifier)
- Problem description.
- Solution description: _template_.
- Consequences: _results_ and _trade-offs_.

### Observer Pattern

- Name: Observer
- Description: When the objects state changes, all displays are _automatically_ _notified and updated_ to reflect the change.
- Problem description: More than one display format for the state information is required and the object does not need to maintain the state information.
- Solution description: 2 _abstract objects_: **Subject** and **objserver** and 2 _concrete obejcts_: **ConcreteSubject** and **ConcreteObejct**.
- Consequences: The **subject** only knows the _abstract_ **Observer** and does not know details of the concrete class. Minimal *coupling* between these objects.

![Observer pattern](/assets/img/contents/observer-pattern.jpg)

## Design Patterns

- **Observer pattern**: Tell several objects that the _state_ of some other object has _changed_. ([Link](https://www.tutorialspoint.com/design_pattern/observer_pattern.htm))
- **Facade pattern**: Tidy up the _interfaces_ to a number of related objects that have often been developed _incrementally_. ([Link](https://www.tutorialspoint.com/design_pattern/facade_pattern.htm))
- **Iterator pattern**: Provide a standard way of _accessing_ the elements in a collection, irrespective of how that collection is implemented. ([Link](https://www.tutorialspoint.com/design_pattern/iterator_pattern.htm))
- **Decorator pattern**: Allow for the possibility of _extending_ the functionality of an existing class at run-time. ([Link](https://www.tutorialspoint.com/design_pattern/decorator_pattern.htm))

# Implementation Issues

- **Reuse**: reusing existing components or systems.
- **Configuration Management**: keep track of different versions of each component.
- **Host-target development**: Develop on the *host* system and execute on the *target* system.

## Reuse

Reuse levels include:

- **Abstraction**: knowledge of successful abstractions.
- **Object**: directly reuse objects from a library.
- **Component**: Collections of objects and object classes.
- **System**: entire application systems.

Reuse costs include the time _looking for_ software to reuse, _buying_ the reusable software, _adapting and configuring_, and _integrating_ reusable software elements.

## Configuration Management

**General process of managing a changing software system**. The aim is to support system _integration process_ so that all developers can _access_ the project code and documents in a _controlled_ way, find out what _changes_ have been made, and _compile_ and link components to create a new system.

- **Version management**: keep track of different versions and coordinate development by several programmmers.
- **System integration**: define what versions of components are used to create each version of system. Build a system automatically by compiling and linking the required components.
- **Problem tracking**: _report_ bugs and problems and see _who_ is working on these problems and when they are _fixed_.

## Host-target development

Development platform tools:
- An *integrated compiler and syntax-directed editing system*: create, edit and compile code.
- A language _debugging_ system.
- _Graphical editing tools_.
- _Testing codes_.
- _Project support tools_.

**IDE**(intergrated development environments): supports different aspects of software development within some common framework and user interface.

**High availability** systems: components to be deployed on more than one platform.

**High level of communications traffic** between components: deploy them on the same platform to reduces the _delay_ of message passing.

# Open Source Development

**Publish source code** and invite **volunteers** to participate in the development process. Root: Free Software Foundation ([www.fsf.org](www.fsf.org)). For open source business, the business model is reliant on **selling support** for that product. Involving community allows _cheaper_ and _faster_ development and will create a community of users.

## Open Source Licensing

A fundamental principle of open-source development is that: source code are _freely available_ but it does not mean anyone can do as they wish with that code. _Developer of the code_ owns the code. 

- **GPL (GNU General Public License)**: you must also make the software open source.
- **LGPL (GNU Lesser General Public License)**: you can link to open source without having to publish the source of these components.
- **BSD (Berkeley Standard Distribution)**: **No** republish any changes or modifications made to open source code. You can include the code in *proprietary systems* that are sold.

