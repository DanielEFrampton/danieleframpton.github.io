---
layout: default
title: 'SQL Oddities'
date: 2020-01-23
---

# SQL Oddities

![SQL Oddities by David Bowie](https://user-images.githubusercontent.com/40702808/72953963-c05a1180-3d8e-11ea-88e6-323b3f2dbdf6.png)

Think you're hot stuff, huh? You can write a query, eh? Well, try these concepts on for size. SQL is stranger than you think.

These are lessons I learned while completing Turing's Intermediate SQL Exercises [1](https://github.com/turingschool/lesson_plans/blob/master/ruby_03-professional_rails_applications/intermediate_sql.md) and [2](https://gist.github.com/case-eee/5affe7fd452336cef2c88121e8d49f5d). See [my previous guide](https://danieleframpton.github.io/guides/sql_and_postgresql) for introductory SQL concepts. (I recommend listening to David Bowie while reading. And just generally, I suppose.)

## Aggregate Functions

The five SQL aggregate functions (aggregate because they perform a calculation on a gathered-together group of rows on a table) are `AVG`, `MIN`, `MAX`, `SUM`, and `COUNT`. They do pretty much what you'd think. I discovered some interested gotchas, though.

`COUNT` returns the total number of records it finds with any value _except_ for `NULL` values. If you wanted to get around this to find a basic total number of rows on a table, you could count the primary keys (i.e., `count(id)`) or, in the absence of a primary key, give it the wildcard operator `*`:
```sql
SELECT count(*) FROM table`.
```

## Aliases

When using `AS` to provide an alias for a column name, you can use spaces if you surround the name with double quotation marks (but not single). While you can also do this with table name aliases, you shouldn't: it gets complicated real quick trying to refer to those tables elsewhere in the query.

You can also skip `AS` altogether and just give the alias immediately following the column being selected or table, like so:
```sql
SELECT bg.name board_game_name, p.name publisher_name FROM board_games bg INNER JOIN publishers p ON ...
```

## Outer Joins

While outer joins are conceptually more complex than inner joins, using them in a query is not. You simply replace `INNER JOIN` with `LEFT OUTER JOIN` or `RIGHT OUTER JOIN`. You have to be aware that the resulting data from the query will include a lot of `NULL` values wherever there would have been data from the other side of the join, however.

For example, let's assume the following two tables:

`board_games`

| id | name                 | min_players | max_players | publisher_id |
|----|----------------------|-------------|-------------|--------------|
| 1  | "Carcassone"         | 2           | 8           | 1            |
| 2  | "Quest for El Dorado | 2           | 4           | 2            |
| 3  | "Dominion"           | 2           | 4           | 1            |
| 4  | "Chess"              | 2           | 2           |              |
| 5  | "Go"                 | 2           | 2           |              |

`publishers`

| id | name           |
|----|----------------|
| 1  | "Rio Grande"   |
| 2  | "Ravensburger" |

An inner join between the two tables would exclude Chess and Go from the resulting dataset because they don't have a `publisher_id` that can match an `id` on the publishers table. So this query:

```sql
SELECT * FROM board_games INNER JOIN publishers ON board_games.publisher_id=publishers.id;
```

Would produce this result set (which I fell down a rabbit hole confirming is the correct term for the data that results from a SQL query):

```
 id | name                 | min_players | max_players | publisher_id | id | name           
----+----------------------+-------------+-------------+--------------+----+----------------
 1  | "Carcassone"         | 2           | 8           | 1            | 1  | "Rio Grande"   
 2  | "Quest for El Dorado | 2           | 4           | 2            | 2  | "Ravensburger"
 3  | "Dominion"           | 2           | 4           | 1            | 1  | "Rio Grande"   
```

On the other hand, if we change that query to call for a left outer join:

```sql
SELECT * FROM board_games LEFT OUTER JOIN publishers ON board_games.publisher_id=publishers.id;
```

We would get this result set, patchy with NULL values like some kind of eldritch Swiss Cheese:

```
 id | name                 | min_players | max_players | publisher_id | id | name           
----+----------------------+-------------+-------------+--------------+----+----------------
 1  | "Carcassone"         | 2           | 8           | 1            | 1  | "Rio Grande"   
 2  | "Quest for El Dorado | 2           | 4           | 2            | 2  | "Ravensburger"
 3  | "Dominion"           | 2           | 4           | 1            | 1  | "Rio Grande"   
 4  | "Chess"              | 2           | 2           |              |    |
 5  | "Go"                 | 2           | 2           |              |    |
```

A right outer join would look very similar, but with the gaps on the board_games side. If we added some publishers which had no associated games on `board_games` and changed `LEFT` to `RIGHT` in the above query, we might get this:

```
 id | name                 | min_players | max_players | publisher_id | id | name           
----+----------------------+-------------+-------------+--------------+----+----------------
 1  | "Carcassone"         | 2           | 8           | 1            | 1  | "Rio Grande"   
 2  | "Quest for El Dorado | 2           | 4           | 2            | 2  | "Ravensburger"
 3  | "Dominion"           | 2           | 4           | 1            | 1  | "Rio Grande"   
    |                      |             |             |              | 3  | "Asmodee"
    |                      |             |             |              | 4  | "Wizards of the Coast"
```

## Subqueries

Yo dawg. I heard you liked queries, so I learned how to write queries within queries. (Or should I have gone for an Inception pun? Ah well.)

Subqueries can be used for many purposes, one of which is to use the result set of one query as the condition for another. For example, if you wanted to find all the board games with above-average max_player amounts, you could use a subquery to find the average and place that subquery within the `WHERE` clause. Sub-queries go inside of parentheses and are _not_ terminated by a semi-colon (that comes at the end of the overall query). E.g,
```sql
SELECT *
  FROM board_games
 WHERE max_players > (SELECT avg(max_players)
                        FROM board_games);
```

(As an aside, see [this SQL style guide](https://www.sqlstyle.guide/) for the indentation style used above. There does not appear to be a widely-held consensus on SQL style, but Simon Holywell's reasoning seems sound.)

As in a programming language governed by a [call stack structure](https://backend.turing.io/module1/lessons/call_stack), or (if you prefer maths) like the way you perform calculations inside parentheses first, the subquery (or "inner query") is evaluated _before_ the outer query. The result set of the subquery (specifically its return _value_) takes the place of the subquery and then the outer query runs. Using the above example, it would look like this after running the subquery:

```sql
SELECT *
  FROM board_games
 WHERE max_players > 4;
```
Then the outer query would run with this new information in place, and return this result set:
```
 id | name                 | min_players | max_players | publisher_id
----+----------------------+-------------+-------------+--------------
 1  | "Carcassone"         | 2           | 8           | 1            
 ```

In addition to comparison operators (=, >, <, >=, <=, and !=), SQL has several other powerful operators that can be used in conjunction with subqueries, most of which can handle a subquery that returns zero or more values (whereas the comparison operators expect exactly one). They include `EXISTS` and `NOT EXISTS`, which need no preceding value and simply check whether any records are returned or not; `ALL` and `ANY` which are used in conjunction with a comparison operator (like `max_players > ALL (subquery)`) to compare a value against a set of values; and `IN` and `NOT IN` which check to see if a given value exists in the subquery's result set or not. See [this excellent article](https://www.sqltutorial.org/sql-subquery/) for full explanations of each.

(Do not, however, see [this somewhat confusing w3schools article](https://www.w3resource.com/sql/subqueries/understanding-sql-subqueries.php) which features [this informative illustration of how subqueries function](https://www.w3resource.com/w3r_images/sql-subqueries.gif) which takes my award for least helpful illustration of 2019.)

Subqueries can contain subqueries. I'm pretty sure if you do this too many times it will rip a hole in the fabric of space-time, but results may vary. How many levels down you can go appears to depend on what SQL platform you're using; [Oracle can handle 255 levels of nested subqueries](https://www.tutorialspoint.com/sql_certificate/subqueries_to_solve_queries.htm), while [pitiful SQL Server can only handle 32](https://www.sqlservertutorial.net/sql-server-basics/sql-server-subquery/). I mean, come on.

## Inserting Values

Everyone other than me probably learned this first, but I discovered I knew next to nothing about inserting data into a table from the SQL console. (Probably because, immediately after learning it, I only used ActiveRecord for the CUD in CRUD for the next six weeks! Thanks, Rails.) In any case, if you're going to insert more than a single row of data at a time, this is what it might look like:
```sql
INSERT INTO board_games (name, min_players, max_players, publisher_id)
VALUES ('Dungeons & Dragons', 2, NULL, 4),
('RoboRally', 2, 8, 4),
('San Juan', 2, 2, 2);
```
As you can see, `INSERT INTO` is followed by the name of the table you're inserting data into, then within parentheses the columns you wish to provide data for. After `VALUES` you provide comma-separated lists of data in parentheses corresponding to the columns on the first line, one for each row you're inserting.

If you only want to provide data for certain columns and leave the others blank, you can either place `NULL` in that position for a particular row or write a separate `INSERT INTO` statement which leaves that column out of the parenthetical list.

(As an aside, the PostgreSQL console reached from the terminal by `psql` handles multiple-line SQL statements fairly intuitively; hitting return before you have closed the statement with a semi-colon saves what you've entered but does not execute the statement, and at least in iTerm2 pressing the Up arrow when the previous command was multi-line does not cause it to freak out but allows you to edit the whole multi-line command.)

## Creating Tables & Data Types

(See above comment regarding learning things in the wrong order.)

Here's the standard SQL syntax for creating a table:
```sql
CREATE TABLE table_name (column_name_1 data_type column_constraint,
                         column_name 2 ....,
                         table_constraint);
```

The constrains (`column_constraint`, and `table_constraint`) are optional, and there can be multiple constraints simply separated by spaces. Depending on which guide you read, constraints are alternatively referred to as parameters or properties.

This syntax is fairly straightforward, but at the minimum it requires some outside knowledge about the SQL keywords for its different accepted data types (and preferably also the various constraints). By outside knowledge I mean something you have to look up every time you do it. And by you, I mean me.

What complicates this, however, is that the data type keywords available vary depending on _which_ SQL system you're working with. There are [basic SQL data types](https://www.sqltutorial.org/sql-data-types/) but the varying SQL systems build onto those or have different conventions around their usage.

For example, in Turing's back-end development program we primarily use PostgreSQL. In addition to [its own buffet of data types](https://www.postgresqltutorial.com/postgresql-data-types/), PostgreSQL provides a "pseudo-data-type" called `SERIAL` which streamlines the process of making an auto-incrementing integer column (typically to use as a primary key). As [this tutorial explains](https://www.postgresqltutorial.com/postgresql-data-types/), the following PostgreSQL statement:
```sql
CREATE TABLE example_table(id SERIAL);
```

Accomplishes what would take all this to do with basic SQL:
```sql
CREATE SEQUENCE example_table_id_seq;

CREATE TABLE example_table (
    id integer NOT NULL DEFAULT nextval('table_name_id_seq')
);

ALTER SEQUENCE example_table_id_seq
OWNED BY table_name.id;
```

That looks fairly complex, but as that tutorial outlines it's simply creating a `SEQUENCE` object that tracks the most recently used number, associating it with the table, and calling the `nextval()` function on it to get the default id value for new records.

Here's where the fun began in my research. I started off researching `SERIAL` because it's what I was seeing used in the examples I was given in an exercise, but it turned out--as often happens in this sea of tutorials that we call the internet--to be effectively deprecated. (Deprecated means that it is an older piece of code or piece of functionality that has been either removed in newer versions of a language or framework or still works for backwards-compatability reasons but is no longer recommended to be used.)

An id column using `SERIAL` to auto-increment can get messed up if you insert a value into that column because, as [this StackOverflow response](https://stackoverflow.com/questions/55300370/postgresql-serial-vs-identity) explains, "the underlying sequence and the values in the table are not in sync any more" but you're not given a descriptive error message telling you that happened. I saw this occur in my past life as a DBA and it took a solid confusing day to fix.

For this reason, `SERIAL` is not considered SQL-standard compliant, [isn't recommended](https://wiki.postgresql.org/wiki/Don%27t_Do_This#Don.27t_use_serial), and has been effectively replaced by another method since PostgreSQL version 10. This new method uses a "constraint" (or "property") to make a column auto-increment rather than using a pseudo-data-type. In use, it looks like this:
```sql
CREATE TABLE table_name (id INT GENERATED ALWAYS AS IDENTITY);
```

It's much more verbose, but there you have it. [This guide goes into the details](https://www.postgresqltutorial.com/postgresql-identity-column/), and [this guide compares the various implementations of auto-incrementing columns across various SQL systems](https://www.sqltutorial.org/sql-auto-increment/). A quick glance through the latter guide demonstrates that PostgreSQL is far from alone in having a unique way of going about this.

In sum, SQL usage is far from monolithic and varies pretty widely from system to system. When looking up information, be sure to look for resources specific to your SQL database system, whether that's PostgreSQL, SQL Server, MySQL, or what have you. And be sure to look for up-to-date resources, because these platforms are constantly evolving and improving.

## Recommended Resources

In addition to the various guides and tutorials linked above, these were some of the most helpful resources overall that I would recommend either for study or as references:

- [SQL Tutorial](https://www.sqltutorial.org/)
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)
