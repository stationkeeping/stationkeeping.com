---
title: "SQL Basics"
---

### Select

#### Basic Selection

SQL *select* queries return a *Results Set*.

Select `foo` field from `Example` table

```
SELECT foo FROM Examples;
```

Select `foo` and `bar` fields from `Example` table

```
SELECT foo, bar FROM Examples;
```

Select all columns in the `Example` table:

```
SELECT * FROM Examples;
```

##### Distinct

Select only distinct (unique) values from column `foo` from the `Example` table:

```
SELECT DISTINCT foo FROM Examples;
```

##### Where

Select all records from the `Example` table that have a value of `bar` in their `foo` column.

```
SELECT * FROM Examples WHERE foo=bar; 
```

##### Operators

Other than basic comparators, (`=`, `<=` etc), SQL supports the following operators:

Select a value that falls between two values (inclusively).
For example, select all records from `Examples` where the value of the `foo` column is between 0 and 5 inclusive:

```
SELECT * FROM Examples WHERE foo BETWEEN 0 AND 5;
```

Select a value that matches a give pattern. `%` is a wildcard character.
For example, select all records from `Examples` where the value of the `foo` column starts with *a* and ends with *s*:

```
SELECT * FROM Examples WHERE foo LIKE a%s;
```

Like can be negated using `NOT`:

```
SELECT * FROM Examples WHERE foo NOT LIKE a%s;
```

Select a value that is one of given set of values.
For example, select all records from `Examples` where the value of the `foo` column is *dog*, *cat* or *iguana*:

```
SELECT * FROM Examples WHERE foo IN ("dog", "cat", "iguana")
```

##### Logical Operators

`AND` allows a select based on multiple matches:
For example, select all records `Examples` where the value of the `foo` column is *dog* and the value of the `bar` column is *potato*:

```
SELECT * FROM Examples WHERE foo="dog" AND bar="Potato"
```

`OR` allows a select based on alternate matches.
For example, select all records `Examples` where the value of the `foo` column is *dog* or the value of the `bar` column is  *potato*:

```
SELECT * FROM Examples WHERE foo="dog" OR bar="potato"
```

#### Sorting

Results sets are ordered using `ORDER BY`.

By default, `ORDER_BY` sorts in ascending order:

```
SELECT * FROM Examples ORDER BY foo;
```

This can be reversed using `DESC`:

```
SELECT * FROM Examples ORDER BY foo DESC;
```

Results can be sorted by multiple columns:

```
SELECT * FROM Examples ORDER BY foo, bar;
```

### Insert

New records can be inserted using `INSERT INTO`.

Either by passing the values alone (in which case order is crucial):

```
INSERT INTO Examples VALUES ("cat", "potato");
```

Or by passing column names as well:

```
INSERT INTO Examples ("bar", "foo")  VALUES ("potato", "cat");
```

### Update

Existing records can be updated using `UPDATE`.

```
UPDATE Examples SET (foo="cat", bar="potato") WHERE foo="dog";
```

Without the `WHERE`, all records will be updated:

```
UPDATE Examples SET (foo="cat");
```

### Delete

Existing records can be deleted using `DELETE`.

```
DELETE FROM Examples WHERE foo=cat;
```

Without the `WHERE`, all records will be deleted:

```
DELETE FROM Examples;
```

