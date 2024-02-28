# operators

## arithmetic operators

operators sorted from highest to lowest precedence

- `-` : unary minus
- `*` `/` `DIV` `%` `MOD` : multiplication, division, modulus
- `+` `-` : addition, subtraction

operators with same precedence are evaluated from left to right

✨ **syntax reference**

```sql
SELECT 10 DIV 5 + 2
```

## comparison operators

basic comparison operators. strings are compared lexicographically

- `=` : equal
- `<>` or `!=` : not equal
- `>` : greater than
- `<` : less than
- `>=` :  greater than or equal
- `<=` : less than or equal

✨ **syntax reference**

```sql
SELECT * FROM table_name WHERE column_name > 'text'
```

## other operators

- `IS NULL` : equal to NULL
- `LIKE` : string follow pattern
    - `_` : one character
    - `%` : zero to many characters
- `BETWEEN ... AND ...` : endpoint inclusive range
- `IN` : group membership

✨ **syntax reference**

```sql
SELECT * FROM table_name WHERE column_name IS [NOT] NULL
SELECT * FROM table_name WHERE column_name [NOT] LIKE '%sometext'
SELECT * FROM table_name
WHERE column_name [NOT] BETWEEN 'some-start-date' AND 'some-end-date'
SELECT * FROM table_name WHERE column_name [NOT] IN (1, 4, 8)
```

## logical operators

operators sorted from highest to lowest precedence

- `NOT` : condition must not be satisfied
- `AND` : all conditions must be satisfied
- `OR` : at least one condition must be satisfied

✨ **syntax reference**

```sql
SELECT * FROM table_name 
WHERE NOT (column_name % 4 = 0 OR column_name % 10 = 0)
```