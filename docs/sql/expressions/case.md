---
layout: docu
title: Case Statement
railroad: expressions/case.js
---
<div id="rrdiagram"></div>

The `CASE` statement performs a switch based on a condition. The basic form is identical to the ternary condition used in many programming languages (`CASE WHEN cond THEN a ELSE b END` is equivalent to `cond ? a : b`). With a single condition this can be expressed with `IF(cond, a, b)`.
```sql
CREATE OR REPLACE TABLE INTEGERS AS SELECT UNNEST([1, 2, 3]) AS i;
SELECT i, CASE WHEN i>2 THEN 1 ELSE 0 END AS test FROM integers;
-- 1, 2, 3
-- 0, 0, 1

-- this is equivalent to:
SELECT i, IF(i > 2, 1, 0) AS test FROM integers;
-- 1, 2, 3
-- 0, 0, 1
```

The `WHEN cond THEN expr` part of the `CASE` statement can be chained, whenever any of the conditions returns true for a single tuple, the corresponding expression is evaluated and returned.

```sql
CREATE OR REPLACE TABLE INTEGERS AS SELECT UNNEST([1, 2, 3]) AS i;
SELECT i, CASE WHEN i=1 THEN 10 WHEN i=2 THEN 20 ELSE 0 END AS test FROM integers;
-- 1, 2, 3
-- 10, 20, 0
```

The `ELSE` part of the `CASE` statement is optional. If no else statement is provided and none of the conditions match, the `CASE` statement will return `NULL`.

```sql
CREATE OR REPLACE TABLE INTEGERS AS SELECT UNNEST([1, 2, 3]) AS i;
SELECT i, CASE WHEN i=1 THEN 10 END AS test FROM integers;
-- 1, 2, 3
-- 10, NULL, NULL
```

After the `CASE` but before the `WHEN` an individual expression can also be provided. When this is done, the `CASE` statement is essentially transformed into a switch statement.

```sql
CREATE OR REPLACE TABLE INTEGERS AS SELECT UNNEST([1, 2, 3]) AS i;
SELECT i, CASE i WHEN 1 THEN 10 WHEN 2 THEN 20 WHEN 3 THEN 30 END AS test FROM integers;
-- 1, 2, 3
-- 10, 20, 30

-- this is equivalent to:
SELECT i, CASE WHEN i=1 THEN 10 WHEN i=2 THEN 20 WHEN i=3 THEN 30 END AS test FROM integers;
```
