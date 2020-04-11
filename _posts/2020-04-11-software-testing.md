---
date: 2020-04-11 05:35:45
layout: post
title: "Software Testing"
subtitle:
description: CSC4001 Chapter 9 Software Testing
image: /assets/img/background/paint_strobe_captures__1920x1080_.jpg
optimized_image:
category: course
tags:
    - course note
    - CSC4001
    - software engineering
    - system testing
author: Guochao Xie
paginate: false
math: true
---

# Program Testing

The goal is to show the software meets its _requirements_ : It means at least one test for _every requirement_. For generic products, there should be tests for _all system features_: $\Rightarrow$ validation testing.

And to discover situations of _incorrect behavior_ : $\Rightarrow$ Defect testing.

## Verification and Validation

Verification means we should *conform to its specification*, while validation means we should do what the user *really requires*. They aims to establish *confidence* that the system is fit for purpose.

- Software Purpose: how *critical* the software is to an organization.
- User expectations: Users may have *low expectations*.
- Marketing environment: Market *early* may be more important than finding defects.

## Inspections and Testing

- **Software inspections** (static): analysis of the *static system* representation (tool-based document and code analysis).
  - Exam the source representation for anomalies and defects.
  - *NOT* require execution.
  - Apply to any *representation* of the system.
  - Testing errors may mask other errors $\Rightarrow$ you don't need to concerned with interactions between errors. (+)
  - Inspect _incomplete versions_. (+)
  - To broader quality attributes of a program, such as _compliance with standards_ (遵守标准), _portability_ and _maintainability_. (+)
  - Inspections cannot check _conformance_ with the customer's _real requirements_. (-)
  - Cannot check _non-functional_ characteristics such as performance, usability, etc.. (-)

- **Software testing** (dynamic): exercising and observing product _behaviour_.
  - **Development testing**: discover _bugs_ and _defects_.
  - **Release testing**: a *separate* testing team tests a *complete* version of the system.
  - **User testing**: potential users test in their own environment.

# Development Testing

**By the team developing the system**.

- **Unit testing**: individual program units or object classes. Focus on _functionality_.
- **Component testing**: _integrate_ individual units to create _composite components_. Focus on testing _component interfaces_.
- **System testing**: some or all of the components in the system are _integrated_. Focus on testing _component interactions_.

## Unit Testing

Test in _isolation_. A _defect_ testing process. 

For **Object class testing**: a complete test coverage involves:

1. All _operations_ associated with an object
2. Setting adn interrogating all object _attributes_
3. Exercising the object in all possible _states_

### Automated Testing

Unit testing should be automated by making use of a test automation framework. Unit testing frameworks provide _generic_ test classes that we can extend to create specific test cases. It has 3 components: _setup_ part to initialize the system with teh test case (inputs and expected outputs), _call part_, and _assertion part_ to compare the result.

### Choosing Test Cases

They should reveal defects in the component. One type is _normal operation_ and shoudl work as expected. The other is based on testing experience of _commom problems_ like abnormal inputs.

### Testing Strategies

- **Partition Testing**: groups of inputs that have common characteristics and process in the same way.
  - Different _classes_ where all members are related.
  - Each class is an *equivalence partition or domain* where the program behaves in an equivalent way.
  - Choose test cases from *each partition*.
- **Guideline-based Testing**: reflect previous experience of the kinds of _errors_.
  - Choose inputs that force the system to generate *all error messages*.
  - Design *overflow* inputs.
  - Repeat same input.
  - Force *invalid outputs* to be generated.
  - Force *computation results* to be too large or small.

## Component Testing

Access the functionality of objects through the defined _comopnent interface_. Assume the _unit tests_ on the individual objects have been completed.

### Interface Testing

Interface types:

- **Parameter interfaces**: pass data.
- **Shared memory**
- **Procedural** sub-system encapsulates a set of procedures.
- **Message passing**: sub-systems request services from other sub-systems.
  
### Interface Errors

- **Interface misuse**: wrong parameters.
- **Misunderstanding**: assumptions about the behavior of the called component are incorrect.
- **Timing**: different speeds and out-of0date information is accessed.

### Interface testing guideline

- Design *extreme ends* parameters.
- Test *null pointers*.
- Design tests to cause *failure*.
- *Stress testing* in message passing systems.
- Vary the order of components' activation in *shared memory* systems.
  
## System Testing

*Integrating* components to create a *version* of the system and then testing the intergrated system. The focus is testing the _interactions_ between components. It checks if components are *compatible*, _interact_ correctly and _transfer_ the right data at the right time. Also test the *emergent behaviour* of a system.

### Use-case testing

Test the use case to force interactions to occur. To derive test cases from _sequence diagram_, an input of a request should have _acknowledgement_ and a _report_ should be returned from the request.

# Test-driven Development

**Test-driven development (TDD)**: _interleave_ testing and code development. Write tests before code. Develop code _incrementally_ witha  test for that increment. This is part of **agile methods** but can also be used in **plan-driven development** processes.

1. Identify the increment of required functionality (small).
2. Write a test for this functionality and implement as an automated test.
3. Run the tests. No implementation should fail the test.
4. Implemente functionality and re-run the test.
5. Move on if all tests run successfully.

Pros:

- **Code coverage**: every line has at least one test.
- **Regression testing**: incrementally development.
  -  Check the changes NOT broke *previously* working code.
  -  Cheap with automated testing.
  -  Run successfully before change is committed.
- **Simplified debugging**: obvious to see where teh problem lies.
- **System documentation**: Tests are a form of documentation to describe the code.

# Release Testing

Testing a _release_ of a system that is intended for use outside of the development team. Goal: _convince_ the supplier. It is usually a _black-box_ testing process where tests are only from the system _specification_.

It is a form of **system testing**. A _seperate team_ should be responsible for it. While **system testing** focuses on debugging, **release testing** focuses on checking the requirements.

## Requirements-based testing

**Exam each requirement and develop a test for it**.

## Performance Testing

**Testing the emergent properties of a system, such as performance and reliability**. Tesets should reflect the *profile* of use of the system. Test when the *load* is steadily increased until the system becomes unacceptable. **Stress testing** is a form to deliberately *overload* the system and test its failure behaviour.

# User Testing

**User or customers provide input and advice on system testing**. It is essential because *influences from the user's working environment* have a major effect on the *reliability, performance, usability and robustness* of a system.

- **Alpha testing**: Developer's side.
- **Beta testing**: User's side (release teseting).
- **Acceptance testing**: Customer environment.
    1. Acceptance criteria
    2. Plan testing
    3. Derive tests
    4. Run tests
    5. Negotiate results
    6. Reject / accept system

## Agile methods and acceptance testing

In agile methods, user / customer is _part of the development team_ and is responsible for making decisions on the acceptability of the system. Define tests by the user / customer and _integrate_ them with other tests. **NO** separate acceptance testing process. Problem: choose the *typical* users.