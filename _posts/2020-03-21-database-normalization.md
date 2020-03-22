---
date: 2020-03-21 04:29:45
layout: post
title: "Database Normalization"
subtitle:
description: "Review of different database normalization methods, including 1NF, 2NF, 3NF, BCNF, 4NF, 5NF, 6NF"
image: /assets/img/contents/3d_landscape__26_.jpg
optimized_image:
category: course
tags:
  - CSC3170
  - course note
  - database
  - normalization
author: Guochao Xie
paginate: false
math: true
---

In this article, we are going to review the database normalization methods and compare different methods. The corresponding chapters in the textbook is from Chapter 5 to Chapter 7 .

Content of Table:

- [Purpose of Normalization](#purpose-of-normalization)
- [Lossless Decomposition](#lossless-decomposition)
- [Functional Dependencies](#functional-dependencies)
  - [Closure of a Set of Functional Dependencies](#closure-of-a-set-of-functional-dependencies)
  - [Trivial Functional Dependency](#trivial-functional-dependency)
  - [Transitive Functional Dependency](#transitive-functional-dependency)
  - [Full Functional Dependency](#full-functional-dependency)
  - [Test of Lossless Decomposition](#test-of-lossless-decomposition)
  - [Dependency Preservation](#dependency-preservation)
  - [Canonical Cover](#canonical-cover)
- [Keys](#keys)
  - [Candidate Key](#candidate-key)
  - [Primary Key](#primary-key)
  - [Secondary Key](#secondary-key)
  - [Prime Attribute](#prime-attribute)
  - [Non-prime Attribute](#non-prime-attribute)
  - [Extraneous Attribute](#extraneous-attribute)
- [1NF](#1nf)
- [2NF](#2nf)
- [3NF](#3nf)
  - [Algorithm for 3NF Decomposition](#algorithm-for-3nf-decomposition)
- [BCNF](#bcnf)
  - [BCNF and Dependency Preservation](#bcnf-and-dependency-preservation)
- [Multivalued Dependency](#multivalued-dependency)
  - [Restriction of Multivalued Dependencies](#restriction-of-multivalued-dependencies)
  - [Trivial MVD](#trivial-mvd)
- [4NF](#4nf)
- [Temporal Data](#temporal-data)
- [Conclusion](#conclusion)
- [References:](#references)

## Purpose of Normalization

Database normalization aims to reduce redundancy and dependency of data. 

One of the goal for database design is **Atomic**. It means that elements are *indivisible* units.

## Lossless Decomposition

To perform normalization, we will use _lossless_ decompositions. To decompose a relation schema  $R$ into $R_1$ and $R_2$ means that $R = R_1 \cap R_2$. A _lossless_ decomposition should satisfy $\Pi_{R_1}(r) \bowtie \Pi_{R_2}(r) = r$.

## Functional Dependencies

Represented by $\alpha \rightarrow \beta$, where $\alpha \subseteq R, \beta \subseteq R$.

$$(\alpha \rightarrow \beta) \Leftrightarrow (t_1[\alpha] = t_2[\beta] \Rightarrow t_1[\beta] = t_2[\beta]\ \forall t_1, t_2)$$

### Closure of a Set of Functional Dependencies

For a set $F$ of functional dependencies, $A \rightarrow B$ and $B \rightarrow C$ $\Rightarrow A \rightarrow C$. The set of **all** functional dependencies logically implied by $F$ is the **closure** of $F$, denoted by $F^+$.

To compute $F^+$ we repeatedly apply _Armstrong's Axioms_:

1. **Reflexive Rule**: $\beta \subset \alpha \Rightarrow \alpha \rightarrow \beta$.
2. **Augmentation Rule**: $\alpha \rightarrow \beta \Rightarrow \gamma\alpha \rightarrow \gamma\beta$.
3. **Transitivity Rule**: $\alpha \rightarrow \beta \And \beta \rightarrow \gamma \Rightarrow \alpha \rightarrow \gamma$.
4. **Union Rule**: $\alpha \rightarrow \beta \And \alpha \rightarrow \gamma \Rightarrow \alpha \rightarrow \beta\gamma$.
5. **Decomposition Rule**: $\alpha \rightarrow \beta\gamma \Rightarrow \alpha \rightarrow \beta \And \alpha \rightarrow \gamma$.
6. **Pseudotransitivity Rule**: $\alpha \rightarrow \beta \And \gamma \beta \rightarrow \delta \Rightarrow \alpha \gamma \rightarrow \delta$

### Trivial Functional Dependency

$$\beta \subset \alpha \Rightarrow \alpha \rightarrow \beta$$

### Transitive Functional Dependency

$X \rightarrow Z$ can be derived from 2 FDs $X \rightarrow Y$ and $Y \rightarrow Z$, where $Y$ is a nonprime attribute.

For example, _Ssn $\rightarrow$ Dmgr_ssn_ can be derived by _Ssn $\rightarrow$ Dnumber_ and _Dnumber $\rightarrow$ Dmgr_ssn_.

### Full Functional Dependency

A FD $Y \rightarrow Z$ where removal of **any** attribute from $Y \Rightarrow Y \nrightarrow Z$. 

### Test of Lossless Decomposition

$\Pi_{R_1}(r) \bowtie \Pi_{R_2}(r) = r$ is _lossless_ if **one** of the following holds (**sufficient**):

- $R_1 \cap R_2 \rightarrow R_1$
- $R_1 \cap R_2 \rightarrow R_2$

### Dependency Preservation

Preserving the dependency in a schema instead of decompositing it, so that the check of dependency constraints can be done without a Cartisian Product.

For example,

_dept\_advisor(s\_ID, i\_ID, department\_name)_ where

- _i\_ID → dept\_name_
- _s\_ID, dept\_name → i\_ID_

Since [1NF](#1nf), [2NF](#2nf) and [3NF](#3nf) consider only non-prime attributes, the above schema does not violate them. However, _i\_ID → dept\_name_ introduces a repetition for storing the _dept\_name_. If we decompose it into two schemas, we will not include _s\_ID, i\_ID, department\_name_ in the same s chema anymore, which is not __dependency preserving__.

Formally, let $F_i$ be the set of dependencies $F^+$ that include only attributes in $R_i$, then $(F_1 \cup F_2 \cup ... \cup F_n)^+ = F^+$. To test if $\alpha \rightarrow \beta$ is preserved:

- _result_ = $\alpha$
- repeat
  - for each $R_i$ in the decomposition
    - t = (_result_ $\cap R_i$ ) $^+ \cap R_i$
    - _result_ = _result_ $\cup$ t
  - until (_result_ not change)
- Check if _result_ contains all attributes in $\beta$


### Canonical Cover

A **Canonical cover** for $F$ is a set of dependencies $F_c$ such that,

1. $F \implies F_c$
2. $F_c \implies F$
3. No functional dependency in $F_c$ contains an [**extraneous attribute**](#extraneous-attribute).
4. Each left side of functional dependency in $F_c$ is unique, i.e. no 2 dependencies in $F_c$:
   1. $\alpha_1 \rightarrow \beta_1 \And \alpha_2 \rightarrow \beta_2$
   2. $\alpha_1 = \alpha_2$

To compute a **canonical cover** for $F$, we repeatedly remove an extraneous attribute from $F_c$ until $F_c$ not change.

## Keys

### Candidate Key

$K$ is a candidate key for if and only if it is the minimal subset of attributes to determine $R$, that is $\Leftrightarrow$

- $K \rightarrow R$
- no $\alpha \subset K, \alpha \rightarrow R$

### Primary Key

One of the **Candidate Key** of $R$ is chosen as the primary key.

### Secondary Key

All the other **candidate keys** except the **primary key** are secondary keys. It is usually used for _search_ and _index_ purposes.

### Prime Attribute

A member of some **candidate keys**.

### Non-prime Attribute

Not a member of any **candidate keys**.

### Extraneous Attribute

An attribute of a [functional dependency](#functional-dependencies) in $F$ is **extraneous** if we can remove it without changing [$F^+$](#closure-of-a-set-of-functional-dependencies).

Consider $\alpha \rightarrow \beta$ in $F$,
  
- **Remove from the left**:
  1. $A \in \alpha$
  2. $F = (F - \{\alpha \rightarrow \beta\}) \cap ( \{ (\alpha - A) \rightarrow \beta \})$, i.e. removing $A$ does not change $F$.
- **Remove from the right**:
  1. $A \in \beta$
  2. $F = (F - \{\alpha \rightarrow \beta\}) \cap ( \{ \alpha \rightarrow (\beta - A) \})$, i.e. removing $A$ does not change $F$.

Example: 

$F = \{AB \rightarrow CD, A\rightarrow E, E \rightarrow C\}$, we want to check $C$:

1. Compute $(AB)^+$ under $F' = \{AB \rightarrow D, A \rightarrow E, E \rightarrow C\}$.
2. $(AB)^+ = ABCDE$ which includes $CD$.
3. This implies $C$ is **extraneous**.

**Extraneous attribute** is useful for [**Canonical Cover**](#canonical-cover).

## 1NF

The first normal form for a schema requires the domains of all attributes are **atomic**. An example is that, to store ones _telephones_, we should introduce a new table _people\_telephone_ to store the pair _(ID, telephone)_ as one record instead of storing a list of telephones in an element.

Schema _people_:

ID | Name | Telephones
---------|----------|---------
 1 | David | 123
 2 | Jasmine | 234, 345
 3 | Jack | NULL

We should modify it into:

Schema _people_:

ID | Name 
---------|----------
 1 | David 
 2 | Jasmine 
 3 | Jack 

and Schema _people\_telephone_:

ID | Telephone 
---------|----------
 1 | 123 
 2 | 234
 2 | 345

## 2NF

- [1NF](#1nf)
- Every [nonprime attribtues](#non-prime-attribute) $A$ in $R$ is [fully functionally dependent](#full-functional-dependency) on the [primary key](#primary-key).
- (General) Every [nonprime attribtues](#non-prime-attribute) $A$ in $R$ is [fully functionally dependent](#full-functional-dependency) on every [candidate key](#candidate-key).

Example:

_EMP\_PROJ(Emp#, Proj#, Ename, Pname, No\_hours)_

_(Emp#, Proj#)_ is the candidate key, but _Proj#_ $\rightarrow$ _Pname_ and _No\_hours_. 

$$\Rightarrow$$

- _EMP(Emp#, Ename)_
- _PROJ(Proj#, Pname)_
- _EP(Emp#, Proj#, No\_hours)_

## 3NF

- [2NF](#2nf)
- No [nonprime attribute](#non-prime-attribute) _A_ in _R_ is [transitively dependent](#transitive-functional-dependency) on the [primary key](#primary-key).
- (Generally) When a nontrival functional dependency $X \rightarrow A$ holds in _R_, then either
  - _X_ is a super key of _R_
  - or _A - X_ is a prime attribute of _R_
- (Alternative) Every nonprime attribute in _R_ meets both conditions:
  - Fully conditional functionally dependent on every key of _R_.
  - [Non-transitively dependent](#transitive-functional-dependency) on every key of _R_.
- (Alternative) For all $\alpha \rightarrow \beta \ in\ F^+$, at least one of the following holds:
  - $\alpha \rightarrow \beta$ is trivial.
  - $\alpha$ is a superkey of _R_
  - Each attribute _A_ in $\beta - \alpha$ is contained in a candidate key for _R_.

Example:


emp_id	| emp_name |	emp_zip |	emp_state |	emp_city |	emp_district
---|---|---|---|---|---
1001	|	John	|	282005	|	UP	|	Agra	|	Dayal Bagh
1002	|	Ajeet	|	222008	|	TN	|	Chennai	|	M-City
1006	|	Lora	|	282007	|	TN	|	Chennai	|	Urrapakkam
1101	|	Lilly	|	292008	|	UK	|	Pauri	|	Bhagwan
1201	|	Steve	|	222999	|	MP	|	Gwalior	|	Ratan

Here, _{emp\_id}_ is the candidate key; however, we have _emp\_zip $\rightarrow$ emp\_state, emp\_city, and emp\_district_. Therefore, we need to split them into a new table.

_employee table_:

emp_id	|	emp_name	|	emp_zip
---|---|---
1001	|	John	|	282005
1002	|	Ajeet	|	222008
1006	|	Lora	|	282007
1101	|	Lilly	|	292008
1201	|	Steve	|	222999

_employee\_zip table_:

emp_zip	|	emp_state	|	emp_city	|	emp_district
---|---|---|---
282005	|	UP	|	Agra	|	Dayal Bagh
222008	|	TN	|	Chennai	|	M-City
282007	|	TN	|	Chennai	|	Urrapakkam
292008	|	UK	|	Pauri	|	Bhagwan
222999	|	MP	|	Gwalior	|	Ratan

### Algorithm for 3NF Decomposition

1. Compute $F_c$: the [**canonical cover**](#canonical-cover) for $F$
2. i = 0
3. for each FD $\alpha \rightarrow \beta$ in $F_c$ do
   1. if none $R_j, 1 \le j \le i$ contains $\alpha \beta$, then **Create a new relation $R_i$**
      1. i = i + 1
      2. $R_i = \alpha \beta$
4. if none of the schemas $R_j, 1 \le j \le i$ contains a candidate key for $R$, then
   1. i = i + 1
   2. $R_i$ = all candidate key for $R$.
5. Remove redundant relations:
6. repeat
   1. if any schema $R_j$ is contained in another schema $R_k$
      1. $R_j = R_i$
      2. i = i - 1
7. until no more $R_j$ can be deleted
8. return $(R_1, R_2, ..., R_i)$

## BCNF

For all functional dependencies in $F^+$ where $\alpha \rightarrow \beta$, at least one of the following holds:

1. $\alpha \rightarrow \beta$ is [trivial](#trivial-functional-dependency)
2. $\alpha$ is a **superkey** of $R$

A **simplified** test for **BCNF** is to check only the dependencies in $F$ if it causes a violation, that is 

Compute $\alpha^+$ and verify that it includes all attributes of $R$.

For example,

_in\_dep (ID, name, salary, dept\_name, building, budget)_ is not BCNF because _dept\_name → building, budget_ but _dept\_name_ is not a superkey $\Rightarrow$ decomposites it into _instructor_ and _department_.

However, **simplified** test using only $F$ may mislead a non-BCNF as a BCNF where $\alpha$ is not in the same relation $R$. Therefore, we may need $F^+$ to prove **BCNF**.

### BCNF and Dependency Preservation

It is not always possible to achieve both **BCNF** and [**dependency preservation**](#dependency-preservation).

For example,

_dept\_advisor(s\_ID, i\_ID, department\_name)_ where

- _i\_ID → dept\_name_
- _s\_ID, dept\_name → i\_ID_

It is not **BCNF** but any decomposition will violate the **dependency preservation**.

Recall an alternative definition for [**3NF**](#3nf):

(Alternative) For all $\alpha \rightarrow \beta \ in\ F^+$, at least one of the following holds:
- $\alpha \rightarrow \beta$ is trivial.
- $\alpha$ is a superkey of _R_
- Each attribute _A_ in $\beta - \alpha$ is **contained** in a **candidate key** for _R_.

The third condition is a _relaxation_ of **BCNF** for dependency preservation.

For the example

_dept\_advisor(s\_ID, i\_ID, department\_name)_ where

- _i\_ID → dept\_name_
- _s\_ID, dept\_name → i\_ID_

it is not in **BCNF**. However, it is in **3NF**.

Functional dependencies:

- _i\_ID → dept\_name_
- _s\_ID, dept\_name → i\_ID_

There are 2 candidate keys: _{(S\_ID, i\_ID), (s\_ID, department\_name)}_.

- _s\_ID, dept\_name_ is a superkey.
- _i\_ID_ is not a superkey, but _dept\_name_ - _i\_ID_ is _dept\_name_ and it is in the candidate key _(s\_ID, department\_name)_.

The drawback of [**3NF**](#3nf) is that it may contains _repetition_ of information and it may contain _NULL_ values.

## Multivalued Dependency

**Multivalued Dependency** (MVD) is used to **represent** the independence of attributes.

$\alpha \rightarrow \rightarrow \beta$ holds on $R$ if in any legal relation $r(R)$, we have the constraint: if 2 tuples $t_1$ and $t_2$ exist in $r$ such that $t_1[\alpha] = t_2[\alpha]$, then there exist tuples $t_3$ and $t_4$ in $r$ such that:

- $t_1[\alpha] = t_2[\alpha] = t_3[\alpha] = t_4[\alpha]$
- $t_1[\beta] = t_3[\beta] \And t_2[\beta] = t_4[\beta]$
- $t_1[\gamma] = t_4[\gamma] \And t_2[\gamma] = t_3[\gamma]$

In this case, we also have $\alpha \rightarrow \rightarrow \gamma$.

We can also adopt the similar definition to a set of attributes: $R = Y, Z, W$. We say $Y \rightarrow \rightarrow Z \| W$ iff for all possible relations $r(R)$.

$<y_1, z_1, w_1> \in r \And <y_1, z_2, w_2> \in r$

$\Rightarrow\ <y_1, z_1, w_2> \in r \And <y_1, z_2, w_1> \in r$

Note: If $Y \rightarrow Z \Rightarrow Y \rightarrow\rightarrow Z$.

For example, we have a relation _<ID, child\_name, phone\_number>_, and one _ID_ can have multiple _child\_name_ and multiple _phone\_number_, then we will have 

- _ID_ $\rightarrow\rightarrow$ _child\_name_
- _ID_ $\rightarrow\rightarrow$ _phone\_number_


### Restriction of Multivalued Dependencies

The **restriction** of $D$ to $R_i$ is the set $D_i$ consisting of

- All FD in $D^+$ that include only attributes of $R_i$
- All MVD of the form $\alpha \rightarrow\rightarrow (\beta \cap R_i)$ where $\alpha \subset \R_i \And \alpha \rightarrow\rightarrow \beta$ is in $D^+$.

### Trivial MVD

- $\beta \subseteq \alpha$ or
- $\alpha \cup \beta = R$.

## 4NF

$R$ is in **4NF** w.r.t. a set $D$ of functional and multivalued dependencies if 

for all **nontrivial** multivalued dependencies in $D^+$ of the form $\alpha \rightarrow\rightarrow \beta$, where $\alpha \subseteq R \And \beta \subseteq R$, $\alpha$ is a **supeykey**.

For the case of a relation _<ID, child\_name, phone\_number>_, and one _ID_ can have multiple _child\_name_ and multiple _phone\_number_, we will have 

- _ID_ $\rightarrow\rightarrow$ _child\_name_
- _ID_ $\rightarrow\rightarrow$ _phone\_number_

We will need to decomposite it into 2 relations _<ID, child\_name>_ and _<ID, phone\_number>_ in order to satisfy the **4NF**.

## Temporal Data

**Temporal Data** have an association time interval. A **snapshot** is the value of the data at a particular point in time. A **temporal functional dependency** $X\rightarrow Y$ holds on $R$ if $X\rightarrow Y$ holds on all **snapshots** for all legal instances $r(R)$.

## Conclusion

- [1NF](#1nf): All attributes depend on the **key**; *Atomic* attributes.
- [2NF](#2nf): All attributes depend on the **whole key**.
- [3NF](#3nf): All attributes depend on **nothing but the key**.
- (1NF, 2NF, 3NF consider only the *primary key*.)
- [BCNF](#bcnf): Non-trivial $\alpha \rightarrow \beta \Rightarrow \alpha$ is a **superkey**. 
- (BCNF may violate the dependancy preservation.)
- [4NF](#4nf): Consider [multivalued dependency](#multivalued-dependency); try to divide relations with independent attributes.

## References:

1. https://www.guru99.com/database-normalization.html
2. https://beginnersbook.com/2015/05/normalization-in-dbms/
3. https://en.wikipedia.org/wiki/ACID