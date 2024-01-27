## data types
1. integer : negative, zero and positive integers

    | data type | value range | storage consumed |
    | --- | --- | --- |
    | INT | broad | lower |
    | BIGINT | broader | higher |

    use `BIGINT` for exceptionally large numbers and data accuracy

2. decimals : negative, zero and positive decimals

    | data type | decimal places | storage consumed |
    | --- | --- | --- |
    | FLOAT | up to 7  | fixed |
    | DOUBLE | up to 15 | fixed but higher |
    | DECIMAL | configurable  | depends on precision |
        
    use `FLOAT` when rounding errors are acceptable
        
3. string : text data, must be enclosed with single quotes

    | data type | maximum length | storage consumed |
    | --- | --- | --- |
    | CHAR | lowest | fixed |
    | VARCHAR | medium | depends on length |
    | TEXT | highest | depends on length |

    use `CHAR` when speed is more important than space <br/>
    use `VARCHAR` for flexible metadata overhead
        
4. date and time : must be enclosed with single quotes

    | data type | information | storage consumed | value range |
    | --- | --- | --- | --- |
    | TIME | hour minute second | smallest | fixed |
    | DATE | year month date | small | broad |
    | DATETIME | date + time | medium | broad |
    | TIMESTAMP | date + time + timezone | large | narrow |
        
    use `TIMESTAMP` for references across time zones  <br/>
    timezone is defined by offset +hh:mm or -hh:mm after date and time <br/>
	timestamp without offset are inferred to have same timezone as server
        
5. boolean : no direct support from my SQL. `TINYINT` is used under the hood `TRUE = 1` and `FALSE = 0`

        
## constraints
ensure accuracy, completeness and quality of data
- column level constraints : apply to single column in table
  - `NOT NULL` : prevent NULL values
  
    ```sql
    column_name data_type NOT NULL
    ```
  - `DEFAULT` : provide default value for insertion
    
    ```sql
    column_name data_type DEFAULT default_value
    ```
  - `AUTO_INCREMENT` : generate sequence for numeric column
    
    ```sql
    column_name data_type AUTO_INCREMENT
    ```
  - `COMMENT` : add descriptive text for documentation
    
    ```sql
    column_name data_type COMMENT ‘comment string’
    ```


- table level : can involve multiple columns in table and are namable
  - `CHECK` : validate data with custom logic
  
    ```sql
    [CONSTRAINT [constraint_name]] CHECK (condition)
    ```
  - `UNIQUE` : enforce uniqueness
    
    ```sql
    [CONSTRAINT [constraint_name]] UNIQUE (column1, ...)
    ```
  - `PRIMARY KEY` : uniquely identifies rows to prevent duplicate data
    
    ```sql
    [CONSTRAINT [constraint_name]] PRIMARY KEY (column1, ...)
    ```
  - `FOREIGN KEY` : prevent reference to non-existent data  
    
    ```sql
    [CONSTRAINT [constraint_name]] FOREIGN KEY (column1, ...)
	REFERENCES another_table (column1, ...)
	[ON UPDATE reference_option]
	[ON DELETE reference_option]
    ```

### reference options
`FOREIGN KEY` reference options when parent record is updated or deleted
- `CASCADE` : update. or delete child record according to parent
- `NO ACTION` or `RESTRICT` : restrict operation (default)
- `SET NULL` : set foreign key column as null for child record


## data definition language
useful in managing (CRUD) objects e.g. database, table, column

### display
- list databases on server host

    ```sql
    SHOW DATABASES
    ```
- list tables in database
  
    ```sql
    SHOW TABLES FROM database_name
    ```
- display table structure e.g. column names, data types, constraints etc.

    ```sql
    DESCRIBE database_name.table_name
    ```
  - switch database : eliminate prefixing table name with database name

      ```sql
      USE database_name
      ```
### create
- create database : database name must be unique across server

    ```sql
    CREATE DATABASE [IF NOT EXISTS] database_name
    ```
- create empty table

    ```sql
    CREATE TABLE [IF NOT EXISTS] table_name (
        column_name data_type column_constraints ... ,
        table_constraints ...
    )
    ```
- create new table from existing one

    ```sql
    CREATE TABLE table_name AS SELECT * FROM another_table
    ```
- create empty table with same schema

    ```sql
    CREATE TABLE table_name LIKE another_table
    ```
### update
- rename table

    ```sql
    ALTER TABLE table_name RENAME TO new_table_name
    ```
- add new column

    ```sql
    ALTER TABLE table_name
    ADD column_name data_type column_constraints ...
    ```
- delete existing column

    ```sql
    ALTER TABLE table_name DROP COLUMN column_name
    ```
- modify column definition

    ```sql
    ALTER TABLE table_name
    MODIFY COLUMN column_name new_datatype column_constraints ...
    ```
- rename existing column

    ```sql
    ALTER TABLE table_name
    RENAME COLUMN column_name TO new_column_name
    ```
- add constraint

    ```sql
    ALTER TABLE table_name
    ADD table_constraint
    ```
- modify constraint

    ```sql
    ALTER CONSTRAINT constraint_name [NOT] ENFORCED
    ```
- drop constraint

    ```sql
    ALTER TABLE table_name
    DROP CONSTRAINT constraint_name
    ```
### delete
- delete database

    ```sql
    DROP DATABASE [IF EXISTS] database_name
    ```
- delete table

    ```sql
    DROP TABLE [IF EXISTS] table_name
    ```
## data manipulation language
useful in managing (CRUD) data inside tables

### query data
- select columns
    ```sql
    SELECT * FROM table_name -- select all columns from table
    ```
    - basic arithmetic operators from highest to lowest precedence by line
        - `-` : unary minus
        - `*` `/` `DIV` `%` `MOD` : multiplication, division, modulus
        - `+` `-` : addition, subtraction
    - arithmetic operators with same precedence are evaluated from left to right

        ```sql
        SELECT 10 DIV 5 + 2 -- division takes the precedence
        ```

    - use `DISTINCT` after select to get unique combination of columns

        ```sql
        SELECT DISTINCT column1, column2 FROM table_name
        ```
    - give selected column an alias

        ```sql
        SELECT column_name AS alias FROM table_name
        ```
- filter data
    - logical operators sorted by precedence
        - `NOT` : condition must not be satisfied
        - `AND` : all conditions must be satisfied
        - `OR` : at least one condition must be satisfied
    - basic comparison operators `=, <> or !=, >, <, >=, <=` : equal, not equal, greater than, less than, greater than or equal, less than or equal

        ```sql
        SELECT * FROM table_name WHERE column_name > 'text' -- filter lexicographically later texts
        ```
    - `IS NULL` : equal to NULL

        ```sql
        SELECT * 
        FROM table_name 
        WHERE column_name IS NOT NULL
        ```
    - `LIKE` : string follow pattern
        - `_` : one character
        - `%` : zero to many characters

            ```sql
            SELECT * 
            FROM table_name 
            WHERE column_name IS NULL OR column_name LIKE '%sometext'
            ```

    - `BETWEEN ... AND ...` : endpoint inclusive range
        ```sql
        SELECT * FROM table_name WHERE column5 BETWEEN '2022-01-01' AND '2022-01-07'
        ```
    - `IN` : group membership
    
        ```sql
        SELECT * FROM table_name WHERE column_name NOT IN (1, 4, 8)
        ```
    
- order data
    - `ASC` : order data in ascending manner (default)
    - `DESC` : order data in descending manner
    - special data types
        - strings are ordered lexicographically
        - date are ordered by sequence in calendar

    <br>

    ```sql
    -- order by column1 and use column2 for ties
    SELECT * from table_name ORDER BY column1 DESC, column2
    ```
    
- limit data
    
    ```sql
    -- show top 5 rows after ordering by column
    SELECT * FROM table_name ORDER BY column_name LIMIT 5
    ```
    
- skip rows
    
    in some RDBMS including MySQL `LIMIT` is mandatory when using `OFFSET`
    
    ```sql
    -- show top 5 rows after ordering and skipping first 3
    SELECT * FROM table_name ORDER BY column_name LIMIT 5 OFFSET 3
    ```
    
- aggregate functions
    - `count(*) or count(1)` : count all rows
    - `count` : count non-null rows
  
        ```sql
        SELECT COUNT(column_name) FROM table_name
        ```
    - `distinct count` : count unique non-null column value

        ```sql
        SELECT COUNT(DISTINCT column1), COUNT(DISTINCT column2) FROM table_name
        ```

    - `sum` : sum non-null rows

        ```sql
        SELECT SUM(column_name) FROM table_name
        ```
    - `avg` : exclude nulls and take average of rows

        ```sql
        SELECT AVG(column_name) FROM table_name
        ```

    - `min` : exclude nulls and find minimum value
    - `max` : exclude nulls and find maximum value

        ```sql
        SELECT MAX(column_name) - MIN(column_name) FROM table_name
        ```
    
- joins
    - `INNER` : only matched records from both tables
    - `LEFT` : all left table records and their matches in right table (if any)
    - `RIGHT` : all right table records and their matches in left table (if any)
    - `FULL OUTER` : deduplicated combination of left join and right join results

        ```sql
        -- adaptable with left, right and full outer joins
        SELECT t1.column1, t1.column2, t1.column3, t2.column1
        FROM table1 t1 -- give table an alias
        INNER JOIN table2 t2
        ON t1.column2 = t2.column4
        ```

    - `CROSS` : cartesian product of rows from both tables

        ```sql
        -- return combinations of columns
        SELECT t1.column1, t2.column2, t3.column3
        FROM table1 t1, table2 t2, table3 t3
        ```

- union
    - concatenate rows from multiple tables
    - unified rows must have same no. of columns and data types
    - `UNION ALL` : return result as is
    
        ```sql
        -- concatenate rows and deduplicate result
        SELECT 'table1' AS source, column_name FROM table1
        UNION
        SELECT 'table2' AS source, column_name FROM table2
        ```
    
- intersect
    - returns common rows from multiple tables
    - intersected rows must have same no. of columns and data types
    - `INTERSECT ALL` : return result as is
    
        ```sql
        -- find common rows and deduplicate result
        SELECT 'table1' AS source, column_name FROM table1
        INTERSECT
        SELECT 'table2' AS source, column_name FROM table2
        ```
    
- except
    - returns rows that only exist in first table
    - rows excepted must have same no. of columns and data types
    - `EXCEPT ALL` : return result as is
    
        ```sql
        -- find row difference and deduplicate result
        SELECT 'table1' AS source, column_name FROM table1
        EXCEPT
        SELECT 'table2' AS source, column_name FROM table2
        ```

- group by
    - return unique combinations of columns

        ```sql
        -- equivalent to select distinct
        SELECT column1, column2
        FROM table_name
        GROUP BY 1, 2 -- shorthand for column1, column2
        ```
    - group rows to apply aggregate function
    
        ```sql
        SELECT column1, SUM(column2)
        FROM table_name
        GROUP BY 1
        ```
    
- having : filters aggregated value
    
    ```sql
    -- filter groups with aggregate value
    SELECT column1, AVG(column2) AS average
    FROM table_name
    GROUP BY 1
    HAVING average < 10
    ```
    
- functions
    - `CONCAT` : join non-null strings
        ```sql
        -- concatenate columns with - delimiter
        SELECT CONCAT(column1, '-', column2) FROM table_name
        ```
    - `ROUND` : round decimals to specified decimal point
        ```sql
        -- round decimal column to specified places 
        SELECT ROUND(column_name, 2) FROM table_name
        ```
    - `COALESCE` : return first non-null value
        ```sql
        -- return specified default if all columns are null
        SELECT COALESCE(column1, column2, column3, 0) FROM table_name
        ```
    - `CASE` : conditional statement, can use everywhere except `FROM` and `LIMIT` clause
        ```sql
        -- determine display value based on condition 
        SELECT CASE
        WHEN column_name < 100 THEN 'low'
        WHEN column_name < 200 THEN 'medium' -- >= 100 but < 200
        ELSE 'high' -- default to NULL if else clause omitted
        FROM table_name
        ```
    - `NULLIF` : return null if both arguments are equal, otherwise return first argument

        ```sql
        -- return null if column value is a dash
        SELECT NULLIF(column_name, '-')
        FROM table_name;
        ```
    
- subqueries
    
    rules for subquery depends on purpose of subquery
    
    - temporary table in `FROM` or `JOIN` clause
        - subquery can return any number of rows and columns

            ```sql
            SELECT column1, column2, column3
            FROM (SELECT * FROM table_name WHERE column1 IS NOT NULL) sub
            ```
    
    - selection in `SELECT` clause
        - subquery can return only 1 row and 1 column

            ```sql
            SELECT column1, (SELECT COUNT(1) FROM table2) AS total_count
            FROM table1
            ```
    
    - comparison in `WHERE` `HAVING` or `SELECT` clause
        - subquery can return any number of rows and columns if used with `IN` or `EXISTS`
            ```sql
            SELECT column1, (column1, column2) IN (SELECT column1, column2 FROM table2)
            FROM table1
            ```
            - `EXISTS` : check for row existence (regardless of columns being null)

                ```sql
                SELECT column1, EXISTS (SELECT column1, column2 FROM table2)
                FROM table1
                ```
        - subquery can return any number of rows but 1 column if used with `ANY` `SOME` `ALL`

            - `ALL` : check if condition is met for every value in subquery

                ```sql
                -- subquery for comparison with ALL (can be adapted for ANY, SOME)
                SELECT column1, SUM(column2) AS summation
                FROM table1
                GROUP BY 1
                HAVING summation > ALL (SELECT column2 FROM table2 WHERE column2 IS NOT NULL)
                ```
          - `ANY` or `SOME` : check if condition is satisfied at least once
          - note that `= ANY` and `= SOME` are equivalent to `IN`

            ```sql
            -- subquery for comparison with ANY (can be adapted for SOME)
            SELECT column1, column2
            FROM table1
            WHERE (column1, column2) = ANY (
                SELECT column1, column2
                FROM table2
                WHERE column2 IS NOT NULL
            )
            ```
    
### order of execution
1. `FROM` and `JOIN`
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. `SELECT`
6. `DISTINCT`
7. `UNION` `INTERSECT` `EXCEPT`
    - highest to lowest precedence by row
        - `INTERSECT`
        - `UNION` `EXCEPT`
    - keywords with same precedence are executed from top to bottom
8. `ORDER BY`
9. `LIMIT` and `OFFSET`

### append data

```sql
INSERT INTO table_name (column1, column2)
VALUES ('value1', 'value2') -- insert single row 
```
- list of column names can be omitted if data to insert has identical structure with destination
- structure here includes no. of columns and column data type
    
    ```sql
    -- insert multiple rows
    INSERT INTO table_name (column1, column2)
    VALUES
        ('value1', 'value2'),
        ('value3', 'value4')
    ```
- insert data from another table

    ```sql
        INSERT INTO table1
        SELECT * FROM table2
    ```
    
### update data
- use `CASE` to set column value based on conditions
- use `WHERE` to filter rows for update
```sql
UPDATE table_name
SET column1 = 1, column2 = 'value2'
WHERE column3 = 'value3'
```
- use `JOIN` to perform cross-table updates

```sql
-- update data in multiple tables
UPDATE table1 t1 INNER JOIN table2 t2 ON t1.column1 = t2.column1
SET t1.column2 = t2.column2, t1.column3 = 'value1', t2.column3 = 'value2'
```
    
### delete data
    
- use `WHERE` to filter rows for deletion

    ```sql
    -- delete rows after filter
    DELETE FROM table_name
    WHERE column_name = 'value1';
    ```

- use `JOIN` to filter rows for deletion

    ```sql
    -- delete rows from table1 after join and filter
    DELETE table1
    FROM table1 t1 LEFT JOIN table2 t2 ON t1.column1 = t2.column1
    WHERE t2.column1 IS NULL;
    ```
