---
title: The SQL is never as good as the original
author: Rohan
date: '2018-07-28'
slug: the-sql-is-never-as-good-as-the-original
categories: []
tags: []
draft: no
---

# Introduction
SQL is a popular way of working with data. You can do a lot of analysis just using SQL, but even having a working knowledge of SQL increases the number of datasets that you can get data from to then analyse in other languages such as R or Python. You can even use SQL within RStudio if you want. The following are a few notes to help future-Rohan when he needs to use SQL. A worked example with a sample of the Hansard data will be included in a future post. Thanks to Monica for the title.

SQL is fairly straightforward if you've used mutate, filter and join in the R tidyverse as the concepts (and sometimes even the verb) are the same. In that case, half the battle is getting used to the terminology, and the other half is getting on top of the order of operations. 

SQL ("see-quell" or "S.Q.L." - both camps seem fairly insistent on their way...) is a way of working with data from relational databases. A relational database is just a collection of at least one table, where a table is a data organized into rows and columns. If there's more than one table in the database, then there should be some column that links them. It feels a bit like HTML/CSS in terms of being halfway between markup and programming. One fun aspect is that line spaces mean nothing: include them or don't, but always end a SQL command in a semicolon;

# Creating a table
Create an empty table of three columns of type: int, text, int:
```{sql, include = TRUE, eval = FALSE}
CREATE TABLE table_name (
  column1 INTEGER,
  column2 TEXT,
  column3 INTEGER
);
```

Add a row of data:
```{sql, include = TRUE, eval = FALSE}
INSERT INTO table_name (column1, column2, column3) 
  VALUES (1234, 'Gough Menzies', 32);
```

Add a column:
```{sql, include = TRUE, eval = FALSE}
ALTER TABLE table_name 
  ADD COLUMN column4 TEXT;
```

# Viewing the data
See one column (similar to R's select):
```{sql, include = TRUE, eval = FALSE}
SELECT column2 
  FROM table_name;
```
See two columns:
```{sql, include = TRUE, eval = FALSE}
SELECT column1, column2
  FROM table_name;
```

See all columns:
```{sql, include = TRUE, eval = FALSE}
SELECT * 
  FROM table_name;
```

See unique rows in a column (similar to R's distinct):
```{sql, include = TRUE, eval = FALSE}
SELECT DISTINCT column2
  FROM table_name;
```

See the rows that match a criteria (similar idea to R's which or filter):
```{sql, include = TRUE, eval = FALSE}
SELECT *
  FROM table_name
    WHERE column3 > 30;
```
All the usual operators are fine here: =, !=, >, <, >=, <=. Just make sure the condition evaluates to true/false.

See the rows that are pretty close to a criteria:
```{sql, include = TRUE, eval = FALSE}
SELECT *
  FROM table_name
    WHERE column2 LIKE  '_ough Menzies';
```
The _ above is a wildcard that matches to any character e.g. 'Cough Menzies' would be matched here, as would 'Gough Menzies'. LIKE is not case-sensitive: 'Gough Menzies' and 'gough menzies' would both match here.

Use % as an anchor to matches pieces:
```{sql, include = TRUE, eval = FALSE}
SELECT *
  FROM table_name
    WHERE column2 LIKE  '%Menzies';
```
That matches anything ending with 'Menzies', so 'Cough Menzies', 'Gough Menzies', 'Sir Menzies' etc, would all be matched here. Use surrounding percentages to match within, e.g. %Menzies% would also match 'Sir Menzies Jr' whereas %Menzies would not.

This is wild: NULL values (!) (True/False/NULL) are possible, not just True/False, but they need to be explicitly matched for:
```{sql, include = TRUE, eval = FALSE}
SELECT *
  FROM table_name
    WHERE column2 IS NOT NULL;
```

This too is wild: There's an underlying ordering for number, date and text fields which allows you to use BETWEEN on all those, not just numeric! The following looks for text that starts with a letter between A and M (not including M) so would match 'Gough Menzies', but not 'Sir Gough Menzies'! 
```{sql, include = TRUE, eval = FALSE}
SELECT *
  FROM table_name
    WHERE column2 BETWEEN 'A' AND 'M';
```
If you look for a numeric (as opposed to text) then it is inclusive!

Combine conditions with AND (both must be true to be returned) or OR (at least one must be true):
```{sql, include = TRUE, eval = FALSE}
SELECT *
  FROM table_name
    WHERE column2 BETWEEN 'A' AND 'M'
    AND column3 = 32;
```

Can order the result:
```{sql, include = TRUE, eval = FALSE}
SELECT *
  FROM table_name
    ORDER BY column3;
```

Ascending is the default, add DESC for alternative:
```{sql, include = TRUE, eval = FALSE}
SELECT *
  FROM table_name
    ORDER BY column3 DESC;
```

Restrict the return to a certain number of values by adding LIMIT at the end:
```{sql, include = TRUE, eval = FALSE}
SELECT *
  FROM table_name
    ORDER BY column3 DESC
    LIMIT 1;
```
(This doesn't work all the time - only certain SQL databases.)

# Modifying data and using logic
Edit a value:
```{sql, include = TRUE, eval = FALSE}
UPDATE table_name
  SET column3 = 33
    WHERE column1 = 1234;
```

Implement if/else logic: 
```{sql, include = TRUE, eval = FALSE}
SELECT *,
  CASE
    WHEN column2 = 'Gough Whitlam' THEN 'Labor'
    WHEN column2 = 'Robert Menzies' THEN 'Liberal'
    ELSE 'Who knows'
  END AS 'Party'
  FROM table_name;
```
This returns a column called 'Party' that looks at the name of the person to return a party.

Delete some rows:
```{sql, include = TRUE, eval = FALSE}
DELETE FROM table_name 
  WHERE column3 IS NULL;
```

Add an alias to a column name (this just shows in the outputs)
```{sql, include = TRUE, eval = FALSE}
SELECT column2 AS 'Names'
  FROM table_name;
```

# Summarising data
We can use COUNT(), SUM(), MAX()/MIN(), AVG() and ROUND() in the place of summarise in R. COUNT() counts the number of rows that are not empty for some column or all using *:
```{sql, include = TRUE, eval = FALSE}
SELECT COUNT(*) 
  FROM table_name;
```

You can SUM(), MAX(), MIN(), and AVG() the values in a column:
```{sql, include = TRUE, eval = FALSE}
SELECT SUM(column1) 
  FROM table_name;
```

ROUND() takes a column and a integer to specify how many decimal places:
```{sql, include = TRUE, eval = FALSE}
SELECT ROUND(column1, 0)
  FROM table_name;
```

SELECT and GROUP BY is similar to group_by() in R:
```{sql, include = TRUE, eval = FALSE}
SELECT column3, COUNT(*)
  FROM table_name
    GROUP BY column3;
```
You can GROUP BY column number instead of name e.g. 1 instead of column3 in the GROUP BY line or 2 instead of COUNT(*) if that was of interest.

HAVING for aggregates, is similar to filter in R or the WHERE for rows from earlier. Use it after GROUP BY and before ORDER BY and LIMIT.

# Combining data
Combine two tables using JOIN or LEFT JOIN:
```{sql, include = TRUE, eval = FALSE}
SELECT *
  FROM table1_name
  JOIN table2_name
    ON table1_name.colum1 = table2_name.column1;
```

UNION is the equivalent of cbind.

Primary key columns uniquely identify rows and are: 1) never NULL; 2) unique; 3) only one column per table. A primary key can be primary in one table and foreign in another. Unique columns have a different value for every row and there can be many in one table. 