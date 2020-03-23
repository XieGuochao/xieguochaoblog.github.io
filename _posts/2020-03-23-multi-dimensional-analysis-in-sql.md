---
date: 2020-03-23 11:03:48
layout: post
title: "Multi-Dimensional Analysis in SQL"
subtitle:
description: "Multidimensional Analysis in SQL"
image: /assets/img/contents/big_5cd3a671d08f42342fb72f34863ad9714801e85d.png
optimized_image:
category: course
tags:
    - course note
    - CSC3170
    - sql
author: Guochao Xie
paginate: false
math: true
---

This review is based on Chapter 10 in CSC3170.

# Concepts

- **ROLLUP**: the operation of moving from finer granularity data to coarser granularity
- **Drill-down**: the operation of moving from coarser granularity data to finer granularity.
- **Slicing**: Fixing on a particular value of an attribute.
- **Dicing**: Fixing on more values.

# ROLLUP

```sql
SELECT time, region, dept, sum(profit) AS profit
FROM sales GROUP BY ROLLUP(time, region, dept);
```

This will computes the **union** of 4 groupings of the sales relation:

_{(time, region, dept), (time, region), (time), ()}_

If we have _n_ attributes in the `ROLLUP` list, then we will have _n+1_ groupings corresponding to their orders. It is equivalent to the **union** of _n+1_ `SELECT` linked with `UNION ALL`.

# CUBE

```sql
SELECT time, region, dept, sum(profit) AS profit
FROM sales GROUP BY CUBE(time, region, dept);
```

For `CUBE`, it is the **union** of $2^n$ groupings:

_{(time, region, dept), (time, region), (time, dept), (region, dept), (time), (region), (dept), ()}_

# Partial CUBE

We can perform a **partial CUBE** by adding attributes in the `GROUP BY` but not in the `CUBE`.

```sql
SELECT time, region, dept, sum(profit) AS profit
FROM sales GROUP BY time, CUBE(region, dept);
```

By doing so, we will not have the grouping _{ (region, dept), (region), (dept), () }_ because all groupings will include _time_.

# Top-N Queries

To get **Top-N** queries we use `ROWNUM` (not supported in MySQL) as follows:

```sql
SELECT column_list, ROWNUM FROM
    (SELECT column_list FROM table
        ORDER BY Top_N_column)
    WHERE ROWNUM <= N;
```

$\Rightarrow$ 1, 2, 2, 4, ...

Note that `ROWNUM` starts from 1.


# RANK

`RANK` is the **tied** rank for tied rows. If two tuples tied again, they will have the same `RANK` and same `ROWNUM`.

```sql
SELECT deptno, ename, sal, comm, 
    RANK() OVER (PARTITION BY deptno ORDER BY sal DESC, comm) as rk
    FROM emp;
```

The main **order** is by _deptno_ and then **rank** by _sal_

# DENSE_RANK

Similar to `RANK`, it is the **tied** rank. However, it produces **consecutive** integers starting from 1. Tow tuples with the same `ROWNUM` will not have the same `DENSE_RANK`.

```sql
SELECT deptno, ename, sal, comm, 
    DENSE_RANK() OVER (ORDER BY sal DESC, comm) as rk
    FROM emp;
```

$\Rightarrow$ 1, 2, 2, 3, 4, 5, ...

# CUME_DIST

**Cumulative distribution** computes the relative position of a specified value in a group. It is from 0 to 1 and tied value will have the same `CUME_DIST`.

# NTILE

`NTILE` divides an ordered dataset into a number of _n_ buckets and assign the appropriate **bucket number** to each row. It ranges from 1 to n. The **number of rows** in the buckets can deffer by **at most 1**.

# MOVING_AVERAGE

```sql
SELECT time, price, avg(price) OVER (
    ORDER BY time RANGE INTERVAL '7' DAY PRECEDING
)
FROM PRICE;
```

This can calculate the _average_ for the current and the preceding 7 days (in total 8 days).


```sql
SELECT time, price, sum(PRICE) OVER (
    ORDER BY time ROWS 2 PRECEDING
)
FROM PRICE;
```

This can calculate the _sum_ for the current and the preceding 2 days (in total 3 days).

# CASE

```sql
CASE WHEN <cond1>
        THEN <v1>
    WHEN <cond2>
        THEN <v2>
    ...
    ELSE <vn+1>
END
```

For example,

```sql
SELECT AVG(
    CASE WHEN Emp.Sal > 2000
            THEN Emp.Sal
            ELSE 2000
    END
)
FROM Emp;
```

# HISTOGRAM Using CASE

```sql
SELECT
SUM(CASE WHEN age BETWEEN 70 AND 79 THEN 1
        ELSE 0
    END) as "70-79",
SUM(CASE WHEN age BETWEEN 80 AND 89 THEN 1
        ELSE 0
    END) as "80-89",
SUM(CASE WHEN age BETWEEN 90 AND 99 THEN 1
        ELSE 0
    END) as "90-99",
SUM(CASE WHEN age > 99 THEN 1
        ELSE 0
    END) as "100+"
FROM persons;
```
    

