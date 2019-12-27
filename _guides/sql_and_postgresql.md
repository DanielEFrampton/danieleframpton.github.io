---
layout: default
title: 'SQL Basics'
---

# SQL Basics

As [part of the coursework](https://backend.turing.io/module2/intermission_work/sql) for the Back-End Engineer program at [Turing School of Software & Design](https://turing.io), I compiled the following reference guide to the basics of SQL and PostreSQL. The information is largely derived from the [JumpstartLab SQL Tutorial](http://tutorials.jumpstartlab.com/topics/sql/fundamental_sql.html) and these [PostgreSQL Exercises](https://pgexercises.com/), which I highly recommend reading and completing for yourself. But if you'd like a more condensed summary, read on!

1. What is a database?

    Databases are a means of storing, fetching, calculating, and sorting data.

1. What is SQL?

    SQL (Structured Query Language) is a programming language with which one can create, modify, and arrange tables in a database, update or remove data on those tables, and retrieve data from the database using complex conditions and run calculations on that data.

1. What is SQLite3?

    SQLite3 is one of many database systems based on SQL, a relatively simple one. It is pre-installed in Mac OSX and stores its data in plaintext files, whereas other systems would store their data in a separate space in the filesystem and only allow access through its tools.

1. What is a Table?

    A table is a database object which has columns, each of which takes a specified data type and typically has one column that is its primary key, which rows of data can be stored on.

1. What is a primary key?

    A primary key is a unique identifier for each row on a table, frequently set to autoincrement so that each new row is automatically assigned the next unused value and no values are reused.

1. What is a foreign key?

    A foreign key is column on a table which allows rows to refer to the primary key on another table, in order to allow for  relationships between tables and their rows.

1. Explanations of the most important SQL commands:

    * insert

      `INSERT` allows for the addition of new rows to a table. Its basic syntax looks like this:

       ``` sql
       INSERT INTO table(column_name1, column_name2) VALUES ('String Value', 0);
       ```

    * select

      `SELECT` opens a query statement used to display data according to the criteria and conditions that follow it. Its basic syntax looks like the following:

      ``` sql
      SELECT fruits.name, sales.created_at
      FROM fruits
      INNER JOIN sales ON fruits.id=sales.fruit_id;
      ```
      The arguments that immediately follow `SELECT` specify which columns to retrieve data from; the argument following `FROM` specifies which table or tables to retrieve that data from, using `INNER JOIN` (or another `JOIN` type) to specify how to relate those tables; and the argument following `ON` specifies which columns should match each other for a specific `JOIN`. Multiple joins can be made to the first table following `FROM`, each with its own `ON` criteria, like this:

      ``` sql
      SELECT customers.name, fruits.name, sales.created_at
      FROM fruits
      INNER JOIN sales ON fruits.id=sales.fruit_id
      INNER JOIN customers ON sales.customer_id=customers.id;
      ```
      Alternatively, `*` can be used as the single argument of `SELECT` to return all the columns in a given `FROM` table.

    * where

      `WHERE` further restricts which rows are returned by a `SELECT` statement or altered by an `UPDATE` or `DELETE` statement by providing additional criteria. Its basic syntax looks like this:

      ``` sql
      UPDATE fruits SET quantity=17 WHERE name='bananas';
      ```
      ``` sql
      DELETE FROM fruits WHERE name='oranges';
      ```
      ``` sql
      SELECT * FROM fruits WHERE name='apples';
      ```
      In addition to comparing specific values, `WHERE` can use SQL methods to perform calculations, such as looking at the length of a string:
      ``` sql
      SELECT * FROM fruits WHERE LENGTH(name)=7;
      ```

    * order by

      `ORDER BY` can be used in conjunction with `SELECT` to order the results of the query by the values in a specific column, either by ascending (the default) or descending order. Its basic syntax looks like this:

      ``` sql
      SELECT * from fruits ORDER BY name DESC;
      ```

    * inner join

        As described above under `SELECT`, `INNER JOIN` is used to specify the relationship between tables when retrieving data from a combination of two or more tables. It is an "inner join" in the sense that it returns only what the two tables have in common, like the internal overlapping space in a Venn diagram. The commonality in question is specified by what follows the `ON` keyword, which specifies which columns are being compared and how. Its basic syntax looks like this:

        ``` sql
        SELECT fruits.name, sales.created_at
        FROM fruits
        INNER JOIN sales ON fruits.id=sales.fruit_id;
        ```
        With this and other kinds of joins, an alias for any table can be given by simply inserting it one space after the table. E.g.,
        ``` sql
        SELECT frt.name, sls.created_at
        FROM fruits frt
        INNER JOIN sales sls ON frt.id=sls.fruit_id;
        ```

1. How can you limit which columns you select from a table?

    By providing specific column names after `SELECT` and before `FROM`, only the data in those columns is retrieve from rows in the table specified by `FROM`. E.g.,

    ``` sql
    SELECT name, membercost FROM cd.facilities
    ```
1. How can you limit which rows you select from a table?

    The `WHERE` keyword can be used to filter which rows are returned from those candidates specified by `SELECT` and `FROM`. It can take one or more conditions by which to filter them. E.g.,
    ``` sql
    SELECT * FROM cd.facilities WHERE membercost > 0
    ```
    Comparisons can be made against dates if the column uses a date-related datatype by referring to dates using the YYYY-MM-DD format as a string. E.g.,
    ``` sql
    SELECT memid, surname, firstname, joindate
    FROM cd.members
    WHERE joindate > '2012-08-31';
    ```
    Multiple `WHERE` conditions are combined using the `AND` and `OR` keywords. E.g.,
    ```sql
    SELECT facid, name, membercost, monthlymaintenance
    FROM cd.facilities
    WHERE membercost > 0 AND membercost < monthlymaintenance / 50;
    ```
    Additionally, the `LIKE` keyword can be used in place of a comparison operator (i.e., =, <, >, >=, <=, etc.) to perform pattern matching. It is provided a string in quotations, wherein `%` can take the place of any number of characters and `_` takes the place of any single character. E.g.,
    ```sql
    SELECT * FROM cd.facilities WHERE name LIKE '%Tennis%';
    ```
    Finally, the `IN` keyword can be used to look for the existence of matching values in a given column or list of values in parentheses. E.g. the following example selects only those rows where its `facid` value exists in the list `(1, 5)`:
    ```sql
    SELECT * FROM cd.facilities WHERE facid IN (1,5);
    ```
    The results of two or more `SELECT` statements can be combined into one output using `UNION` between them, as long as the number of columns and their respective datatypes match. E.g.,
    ```sql
    SELECT surname FROM cd.members UNION SELECT name FROM cd.facilities;
    ```
1. How can you give a selected column a different name in your output?

    Using the `AS` keyword, a column given as an argument for `SELECT` can be renamed. E.g.,
    ```sql
    SELECT memid AS member_id FROM cd.facilities;
    ```
    Additionally, using the `CASE...WHEN...THEN...END` keywords which SQL uses for conditional statements, a new column can be made with conditional values. E.g.,
     ```sql
     SELECT name, CASE WHEN (monthlymaintenance > 100) THEN 'expensive' ELSE 'cheap' END AS cost
     FROM cd.facilities;
     ```
1. How can you sort your output from a SQL statement?

    The `ORDER BY` keyword sorts the retrieved rows by a specified column; the sort direction can optionally be specified after the column name with `ASC` for ascending (the default order) and `DESC` for descending. E.g.,
    ```sql
    SELECT surname FROM cd.members ORDER BY surname;
    ```
    The number of results from a SQL statement can be limitied using the `LIMIT` keyword at the end of the statement followed by an integer specifying the max number of rows to display.
     ```sql
    SELECT surname FROM cd.members ORDER BY surname LIMIT 10;
    ```
    And the results can further by modified by using the `SELECT DISCTINCT` keyword, which does not display additional duplicate rows (that is, rows with identical values across all selected columns) beyond the first.
     ```sql
    SELECT DISTINCT surname FROM cd.members ORDER BY surname LIMIT 10;
    ```
1. What is joining? When do you need to join?

    Joining combines two (or more) tables into one by the use of shared values (i.e., the primary key on one table matches the foreign key on another), resulting in one table with rows that have been combined where those values are shared. Joining is needed when data across different tables needs to be compared or simply retrieved and displayed together where they correlate. The `INNER JOIN` keyword introduces the additional tables beyond the first after its `FROM` keyword, followed by the `ON` keyword which specifies which values should equal one another. E.g.,
    ```sql
    SELECT starttime
    FROM cd.bookings
    INNER JOIN cd.members ON cd.bookings.memid=cd.members.memid
    WHERE cd.members.firstname='David' AND cd.members.surname='Farrell';
    ```
    Outer joins come in the varieties of `LEFT JOIN`, `RIGHT JOIN`, and `FULL JOIN`. `LEFT JOIN` produces all of the rows on the "left" side of the join (i.e., the table introduced by `FROM`) whether or not they match a foreign key on the "right" side of the join. In other words, in a left outer join the rows from the right side are optional rather than mandatory. `RIGHT JOIN` works the same but in the other direction, and `FULL JOIN` produces all rows on both sides and is rarely used. E.g.,
    ```sql
    SELECT mems.firstname AS memfname,
    mems.surname AS memsname,
    refs.firstname AS recfname,
    refs.surname AS recsname
    FROM cd.members mems
    LEFT JOIN cd.members refs
    ON mems.recommendedby = refs.memid
    ORDER BY mems.surname, mems.firstname;
    ```

1. What is an aggregate function?

    An aggregate function takes all the possible results from a group and performs some kind of calculation on them, returning a scalar (i.e., single) value.

1. List three aggregate functions and what they do.

    - `MAX()` takes a group of rows and returns the one that outputs the highest value. E.g., in the following line all the values in the `joindate` column on the `cd.members` table are compared and the highest value is returned:
    ```sql
    SELECT MAX(joindate) AS latest FROM cd.members;
    ```

    - `COUNT()` takes a group of rows and returns the total number of them. `COUNT(*)` returns the total number of rows in a given result set (i.e., after `WHERE` and `JOIN` keywords have filtered it). `COUNT(column_name)` returns the total number of non-null values in that column. `COUNT(DISTINCT column_name)` counts the total number of unique non-null values.

    - `SUM()` takes a column and returns the sum of the values in that column, provided the data type is numeric.

1. What does the `group` statement do?

    The `GROUP BY` keywords batch rows into groups, one for each distinct/unique value in the given column or, if more than one is provided, for each unique combination of those columns. E.g.,
    ```sql
    SELECT department FROM employees GROUP BY department
    ```

1. How does the `group` statement relate to aggregates?

    The `GROUP BY` keywords are particularly suited to allowing aggregates to be executed on groups of rows and then used as column values, so that aggregate values can be displayed alongside the group to which they apply. For example:
    ```sql
    SELECT names.recommendedby, COUNT(recommendedby)
    FROM cd.members names
    WHERE names.recommendedby IS NOT null
    GROUP BY names.recommendedby
    ORDER BY names.recommendedby
    ```

    `GROUP BY` groups rows _after_ any `WHERE` statements, but _before_ any `HAVING` statements, so `HAVING` is useful for filtering which groups are retrieved and displayed by a query and it can take aggregates as arguments while `WHERE` cannot.
