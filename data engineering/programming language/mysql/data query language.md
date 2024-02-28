# data query language

## clauses

- `SELECT` : used to retrieve specified data fields or columns
- `FROM` : used to specify table to query data from
- `WHERE` : used to filter data based on defined conditions
- `ORDER BY` : used to order data by specified fields
- `GROUP BY` : used to group data by specified fields for aggregation
- `HAVING` : used to filter groups with aggregated values
- `LIMIT` : used to show top `N` rows of data
- `OFFSET` : used to skip top `N` rows of data

## select columns

- use `*` to select all columns
- use `DISTINCT` after select to get unique combination of columns
- optionally give selected column an alias

✨ **syntax reference**

```sql
SELECT * FROM table_name
SELECT DISTINCT column1, column2 FROM table_name
SELECT column_name AS column_alias FROM table_name
```

## filter data

data is filtered based on condition defined with help of operators

[operators](operators%201859058c2daf4c32a3b79f43016e25c6.md) 

## order data

order by multiple columns to break ties

- `ASC` : order data in ascending manner (default)
- `DESC` : order data in descending manner
- `NULL` come first for ascending sort and last for descending sort
- strings are ordered lexicographically while date are ordered by occurrence

✨ **syntax reference**

```sql
SELECT * from table_name ORDER BY column1 DESC, column2
```

## aggregate functions

| function | purpose | null values |
| --- | --- | --- |
| COUNT(*) or COUNT(1) | count rows | included |
| COUNT | count values | excluded |
| COUNT(DISTINCT column_name) | count unique values | excluded |
| SUM | sum values | excluded |
| AVG | take average | excluded |
| MIN | find minimum value | excluded |
| MAX | find maximum value | excluded |
| GROUP_CONCAT | concatenate values as string with separator (optional) | excluded |

`GROUP_CONCAT` result is truncated to the maximum length defined by system variable

✨ **syntax reference**

```sql
SELECT COUNT(column_name) FROM table_name
SELECT COUNT(DISTINCT column1), COUNT(DISTINCT column2) FROM table_name
SELECT SUM(column_name), AVG(column_name) FROM table_name
SELECT MAX(column_name), MIN(column_name) FROM table_name
SELECT column1, GROUP_CONCAT(DISTINCT column2 ORDER BY column3 SEPARATOR '-')
FROM table_name
GROUP BY column1
```

## group by

- group rows to apply aggregate function
- return unique combinations of columns when no aggregation applied

✨ **syntax reference**

```sql
SELECT column1, column2 FROM table_name GROUP BY 1, 2
SELECT column1, SUM(column2) FROM table_name GROUP BY 1
```

## having

- filter groups using aggregated value
- must be used with group by

✨ **syntax reference**

```sql
SELECT column1, AVG(column2) AS average
FROM table_name
GROUP BY 1
HAVING average < 10
```

## limit data

show top `N` rows usually after ordering data

✨ **syntax reference**

```sql
SELECT * FROM table_name ORDER BY column_name LIMIT 5
```

## skip rows

- skip first `N` rows usually after ordering data
- in some RDBMS including MySQL `LIMIT` is mandatory when using `OFFSET`

✨ **syntax reference**

```sql
SELECT * FROM table_name ORDER BY column_name LIMIT 5 OFFSET 3
```

## join

- combine data from within or across tables based on shared relationships
- require from clause and can optionally give tables aliases
- `INNER` : return only matched records from both tables
- `LEFT` : return all left table records and their matches in right table
- `RIGHT` : return all right table records and their matches in left table
- `FULL OUTER` : return deduplicated combination of left join and right join results
- `CROSS` : return cartesian product of rows from both tables

default type of join is `INNER` join only which can reduce no. of rows in result set

✨ **syntax reference**

```sql
SELECT t1.column1, t1.column2, t1.column3, t2.column1
FROM table1 t1 [INNER | LEFT | RIGHT | OUTER] JOIN table2 t2 
ON t1.column2 = t2.column4

SELECT t1.column1, t2.column2, t3.column3 
FROM table1 t1, table2 t2, table3 t3
```

## self join

can be useful for scenarios involving rows from same table e.g.

- hierarchical relationships
- finding cycles or loops
- finding nearest neighbors

✨ **syntax reference**

```sql
SELECT t1.column1, t1.column2, t1.column3, t2.column1
FROM table1 t1 LEFT JOIN table1 t2 ON t1.column2 = t2.column4
```

## subqueries

break down complex problems into smaller and more manageable parts

✨ **uncorrelated subquery**

- inner query runs first and is executed only once
- inner query is not dependent on data from outer query

✨ **correlated subquery**

- inner query is executed once for each row of outer query
- inner query is dependent on data from outer query

✨ **use case**

- temporary table in `FROM` or `JOIN` clause
- selection in `SELECT` clause
- comparison in `WHERE` `HAVING` or `SELECT` clause

✨ **rules**

rules for subquery depends on its purpose

- temporary table : subquery can return any number of rows and columns
- selection : subquery can return only 1 row and 1 column
- comparison

subquery can return any number of rows and columns if used with `IN` or `EXISTS`

- `EXISTS` : check for row existence (regardless of columns being null)

subquery can return any number of rows but 1 column if used with `ANY` `SOME` `ALL`

- `ALL` : check if condition is met for every value in subquery
- `ANY` or `SOME` : check if condition is satisfied at least once

note that `= ANY` and `= SOME` are equivalent to `IN`

✨ **syntax reference**

```sql
SELECT column1, column2, column3
FROM (SELECT * FROM table_name WHERE column1 IS NOT NULL) sub

SELECT column1, (SELECT COUNT(1) FROM table2) AS total_count
FROM table1

SELECT column1, (column1, column2) IN (SELECT column1, column2 FROM table2)
FROM table1

SELECT column1, EXISTS (SELECT column1, column2 FROM table2)
FROM table1

SELECT column1, SUM(column2) AS summation
FROM table1
GROUP BY 1
HAVING summation > ALL (SELECT column2 FROM table2)

SELECT column1, column2
FROM table1
WHERE (column1, column2) = ANY (SELECT column1, column2 FROM table2)
```