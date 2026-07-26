---
_$_import: websql, abby
---

::: {.learning} 
Learning Objectives

::: {.objectives} 
>:
>
>1. Use the SELECT statement to retrieve one or more columns of data from a table.
>2. Use the keyword DISTINCT to show only unique rows in a database query.
>3. Add comments to a SQL statement.
:::
:::

## The SELECT Statement

Structured query language (SQL) is used to retrieve and manipulate data stored in database tables. SQL uses a set of reserved, plain-English words, called keywords, to form sentence-like commands. This lesson describes how to use SELECT statements to retrieve one or more columns of data from a table.

::: {.term}
> **Keyword** — A reserved word that is part of the SQL language.
:::

Because SQL statements are sentence-like, they are meant to be easy to create and to understand. For example, the statement `SELECT name FROM museum` is a valid SQL statement, and it is fairly easy to understand: it retrieves all of the values from the `name` column stored on the `museum` table.

The keywords in this statement are `SELECT` and `FROM`. Because keywords are part of the SQL language, they are reserved — meaning they cannot be used to name tables or columns in a database. You would not see a column named `select` or a table named `from` in a database.

::: {.note}
> **Capitalizing SQL Statements.** SQL statements are not case sensitive. The words in a statement can be uppercase, lowercase, or mixed case. `SELECT`, `select`, and `SeLeCt` are all treated the same when they are executed. When we refer to a keyword in this set of lessons, we will always render it in upper case to differentiate it as a keyword, even though it does not need to be capitalized for the statement to execute correctly.
:::

::: {.caution}
> **Table and Column Names and Case.** Although SQL itself is not case sensitive, the names of tables, columns, and values may be. PostgreSQL folds unquoted identifiers to lowercase, so a table created as `Museum` is actually stored — and must be referenced — as `museum`. If you quote an identifier when you create it (`"Museum"`), PostgreSQL preserves its case, and you must then quote it exactly the same way, including case, every time you refer to it.
:::

There are six main keywords in SQL used to retrieve data from a database. Working together, these keywords let you precisely define the data you want to retrieve.

These keywords are:

1. `SELECT` — used to list the columns you would like to retrieve
2. `FROM` — used to specify the table or tables you want to get the data from
3. `WHERE` — used to create conditions or filters on the data that will be retrieved
4. `GROUP BY` — used to categorize the data that is retrieved from the database
5. `HAVING` — used to create more complex conditions or filters on the data that will be retrieved
6. `ORDER BY` — used to sort the data that is retrieved

There are two important rules for using these keywords to build a SELECT statement. First, you must provide at least two pieces of information: the columns you would like to retrieve and the table or tables from which you want to retrieve the data. In other words, every SELECT statement must have both a `SELECT` clause and a `FROM` clause.

Second, the order of the keywords is important. Every SELECT statement follows the order listed above: `SELECT` is always first and `FROM` is always second. The remaining keywords, if used, appear in the same order every time. If a keyword is not used, it is simply omitted from the statement. This lesson uses only `SELECT` and `FROM`; the other keywords are covered in later lessons.

::: {.note}
> **The Order the Clauses Are Executed.** You might be curious to know that the clauses of a SELECT statement are executed in a different order than they are written. The order of execution is `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `SELECT`, then `ORDER BY`. Later lessons explain why this matters.
:::

## Creating Queries

Writing a SELECT query to fill a data need involves at least two skills. First, you need to logically structure a solution that retrieves the needed data. With a plan in place, the second skill is implementing that solution using the syntax — the keywords and rules for using them — of SQL. Getting both the logic and the syntax right is essential to writing an effective query.

Incorrect queries produce either a logic error or a syntax error. A logic error happens when the logic of the query isn't right; a query with a logic error still runs, but it returns the wrong data. A syntax error happens when SQL isn't used correctly; a query with a syntax error returns an error message instead of a result. In many cases a syntax error is easier to spot than a logic error, because a logic error doesn't announce itself — it quietly returns some data, just not the right data.

To help you learn both the logic and the syntax of SQL, the examples in this book follow a simple process for building a query:

1. **Plan** what data you need to retrieve.
2. **Write** a SQL statement that implements the plan.
3. **Examine** the proposed statement, revising it if needed.
4. **Try** the statement by running it against the database.

Planning a query means answering four questions:

- **Columns** — which columns do you need to see in the result?
- **Tables** — which table or tables hold that data?
- **Rows** — do you need every row, or only rows that meet some condition?
- **Order** — does the result need to be sorted in a particular way?

This lesson only deals with the first two questions — columns and tables — since it covers only the `SELECT` and `FROM` clauses. Later lessons on filtering and sorting data put the rows and order questions to use.

Throughout this book you'll see examples that follow this process: a short description of a data need, a chat with an AI assistant used to help translate that need into SQL syntax, and a query box where you can run the resulting statement and check its results against the plan.

This lesson uses example SQL statements that query the museum database (see Figure 1). The museum database has five tables:

1. `artist` — information about the artists whose work is exhibited in the museums in the database
2. `work` — information about the individual works of art exhibited in the museums in the database
3. `subject` — information about the topic or focus of the works of art in the database
4. `museum` — information about the museums in the database
5. `museum_hours` — information about the days and times the museums are open

![A diagram of the museum database. The museum database has the following tables: artist, subject, work, museum, and museum_hours.](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh7hX_BsoPaH4zbcUkf25Qf4QBazzQDsi4XqDY6Y9Tef5ODWacEiCNh2gTyXnk5egU7xQljugE1OxqtSZi3tGO8QixxI9zh-GY00O1uwPshuEYqw-r7pFrAEvmBDQu4OxklYro62CgO_RiDA_KCabOk4E0s34yC-_o_mUYKe_V-XqMqi1Et7P_nLHguBEU/s16000/canvas-trans.png)

*Figure 1: The Museum Database Diagram*

The examples in this lesson show how to write SQL statements that retrieve one or more columns from a table in the museum database, along with a few basic ways to limit and de-duplicate the rows that come back.

## Retrieving a Single Column

Perhaps the most basic SELECT statement returns a single column from a database table. A statement like this needs both a `SELECT` clause, to define the column you wish to display, and a `FROM` clause, to specify the table the data comes from.

For example, say you want to see the names of the museums in the database. The plan is straightforward: retrieve the `name` column from the `museum` table.

You can use a chat with an AI assistant to create the SELECT statement that will do this:

::: {.ai-conversation prompt="Write a SQL statement to list the names of museums."}
> To list the names of museums from a database, you would need to know the specific table and column where the museum names are stored. Assuming there is a table called "Museums" with a column named "Name," you can use the following SQL statement:
>
> ```sql
> SELECT Name
> FROM Museums;
> ```
>
> This query selects the "Name" column from the "Museums" table and retrieves all the museum names. Replace "Museums" with the actual table name in your database to execute the query successfully.
:::

Because the AI assistant doesn't know anything about this database, it has to guess at the table and column names. Notice that it guesses the column name correctly — the name of each museum is stored in the `name` column — but it doesn't guess the table name correctly. The museum information is actually stored on the `museum` table, not `Museums`. You can send the assistant a follow-up message with the correct table name:

::: {.ai-conversation prompt="Try again. The table is named museum."}
> Apologies for the mistake. Here's the revised SQL statement to list the names of museums from a table named "museum":
>
> ```sql
> SELECT Name
> FROM museum;
> ```
>
> This query selects the "Name" column from the "museum" table and retrieves all the museum names.
:::

Now the table name matches what is actually in the database. The SELECT clause is set to retrieve a single column, `name`, and the FROM clause specifies that the rows come from the `museum` table. A SELECT statement without a filter — discussed in a later lesson — returns all of the rows in a table. This query also does not specify any sort order, which is also discussed in a later lesson.

**Try the SQL statement.**

```sql {.websql .data-art}
SELECT name
FROM   museum;
```

::: {.note}
> This is one of the book's interactive query boxes (tied to the `artist`/`work`/`museum` schema diagram, with a "Play" button and results panel) — shown here as a plain code block until that component is built on this platform.
:::

::: {.tip}
> **Terminating Statements.** You can execute more than one SQL statement at a time; multiple statements need to be separated by semicolons. PostgreSQL does not require a semicolon at the end of a single statement executed alone, but there's no harm in adding one, and it's a good habit for when you start running several statements together.
:::

All extra white space within a SQL statement is ignored when the statement is processed. A statement can be written on one long line or broken up over many lines, so all three of these statements are treated exactly the same:

```sql {.websql}
SELECT name FROM museum;

SELECT name
FROM   museum;

SELECT
    name
FROM
    museum;
```

Most people who write a lot of SQL find that breaking statements up over multiple lines makes them easier to read and debug.

## Retrieving Multiple Columns

A SELECT statement used to retrieve more than one column from a table is very similar to one that retrieves a single column. The only difference is that each column is listed after the `SELECT` keyword, separated by a comma.

::: {.tip}
> **Be Careful with Commas.** When selecting multiple columns, put a comma between each column name, but not after the last one. A trailing comma at the end of the column list will result in a syntax error.
:::

Say you want to see the name, city, and country of each museum in the database. The plan: retrieve the `name`, `city`, and `country` columns from the `museum` table, with all rows and no particular order.

You can use a chat with an AI assistant to generate a SQL statement to retrieve this data:

::: {.ai-conversation prompt="Write a SQL statement to list the names, cities, and countries of museums."}
> Certainly! Here's an example of a SQL statement to list the names, cities, and countries of museums:
>
> ```sql
> SELECT name, city, country
> FROM museums;
> ```
>
> In this example, it is assumed that there is a table named "museums" in the database, which contains columns named "name", "city", and "country" to store the respective information for each museum. You may need to adjust the table and column names based on your specific database schema.
:::

Once again, the assistant assumes the table is named `museums`. You can prompt it for the correct name:

::: {.ai-conversation prompt="Try again. The table is named museum."}
> Apologies for the mistake. Here's the corrected SQL statement to list the names, cities, and countries of museums from a table named "museum":
>
> ```sql
> SELECT name, city, country
> FROM museum;
> ```
>
> This statement will retrieve the columns "name", "city", and "country" from the "museum" table and display the respective information for each museum.
:::

The query is ready to try. The `name`, `city`, and `country` columns are included in the result because they are listed in the SELECT clause, separated by commas. The FROM clause ensures the data comes from the `museum` table, and all rows of the table are returned in no particular order.

**Try the SQL statement.**

```sql {.websql}
SELECT name, city, country
FROM   museum;
```

::: {.note}
> **Ordering Columns.** Columns are displayed in the query result in the order they are listed after the SELECT keyword. You can change the order columns appear in the result simply by changing their order in the SELECT statement.
:::

## Retrieving All Columns

There are times you'll want to retrieve every column from a database table. You could list every column name in the SELECT statement, but there's a shortcut: the asterisk (`*`) is a wildcard character that stands in for all of the columns on a table.

Say you want a quick look at everything stored on the `museum` table. The following SQL statement retrieves all of the columns from the `museum` table:

```sql {.websql}
SELECT *
FROM   museum;
```

Using `*` in the SELECT clause returns every column on the table specified in the FROM clause — in this case, `museum`. All rows are returned, and the columns generally appear in the order they are defined on the table.

::: {.tip}
> **Using Wildcards to Explore Data.** It can be helpful to explore the data in a database before you write queries against it, especially if you don't have a schema diagram handy. Using the wildcard character to retrieve every column on a table is a quick way to see what's actually stored there.
:::

It is usually best practice to specify the exact columns you want rather than use the wildcard. Retrieving columns you don't need can slow down the retrieval of data from a database, especially on large tables.

## Removing Duplicate Rows

All of the queries in this lesson so far have returned every row from the source table. There are times you don't want every row, because the rows retrieved by a query often repeat. For example, say you want to list all of the countries represented in the museum database.

The SQL statement that retrieves the countries with museums is:

```sql {.websql}
SELECT country
FROM museum;
```

Although this statement isn't quite right yet, it's instructive to see what it produces.

Notice that this statement returns 57 rows, even though there are only 16 different countries on the `museum` table. Many of the countries appear in the results more than once. The result would be more useful if each country were listed only once.

You can edit the query to show only the unique values for `country` by adding the keyword `DISTINCT` directly after `SELECT`:

```sql {.websql}
SELECT DISTINCT country
FROM museum;
```

With `DISTINCT` in the SELECT clause, only the unique countries are displayed.

Adding `DISTINCT` reduces the result set from 57 rows to 16 — one for each unique country.

AI assistants can generally figure out on their own when `DISTINCT` is needed. Consider a chat that asks an assistant to construct a query to retrieve the countries with a museum:

::: {.ai-conversation prompt="Write a SQL statement to list the countries on the museum table."}
> To list the countries in the "museum" table, assuming there is a column named "country" in the table that stores the country names, you can use the following SQL statement:
>
> ```sql
> SELECT DISTINCT country
> FROM museum;
> ```
>
> This query selects the distinct values from the "country" column of the "museum" table. The DISTINCT keyword ensures that each country name is returned only once, even if there are multiple museums in the same country.
:::

Notice that the assistant not only includes `DISTINCT` in the SQL statement, but also explains why `DISTINCT` is appropriate here.

`DISTINCT` applies to every column in the SELECT clause, not just the first one. For example, retrieving both `country` and `state` returns more than 16 rows, because a country with more than one state appears once for every state on the table — it's the *combination* of country and state that is unique, not the country alone.

```sql {.websql}
SELECT DISTINCT country, state
FROM   museum;

SELECT DISTINCT country
FROM   museum;
```

Both queries return a unique set of values, but the first returns the unique combinations of country and state, while the second returns only the unique list of countries.

## Using Comments

SQL statements are instructions processed by the database management system to interact with the database. There are times when you want to include notes in a SQL statement that shouldn't be processed. These notes are called comments.

::: {.term}
> **Comments** — Notes placed inside a SQL statement that are not processed by the database management system.
:::

Here are a few reasons you might want to leave comments in your SQL statements:

1. The SQL statements you write can become complex. It's often helpful to leave descriptive comments explaining the logic of the statement.
2. You may want to leave a comment at the beginning of a statement describing what it does.
3. When troubleshooting a statement that isn't working as expected, it can help to temporarily comment out part of the statement to see the effect of removing it.

PostgreSQL supports two ways to write a comment. The first is an inline comment, created with two hyphens (`--`). Any text on the same line after the hyphens is not processed. For example, the statement below retrieves the list of museums, ignoring the text "This is a comment" when it's processed:

```sql {.websql}
SELECT name  -- This is a comment
FROM   museum;
```

You can also write comments that span multiple lines. A multiline comment begins with `/*` and ends with `*/`. For example, the text "This is a multiline comment" is not processed in this statement:

```sql {.websql}
SELECT name
/* This is a
multiline comment */
FROM   museum;
```

## Summary

This lesson covered simple SQL statements that retrieve data from a database. It described how to retrieve a single column and multiple columns from a table, and how to use the wildcard character to retrieve every column on a table. It also covered how to use `DISTINCT` to retrieve only unique rows, and how to embed comments in a SQL statement.



## Abby Chapter Embed

```abby {book="availabook-db" section="sql-retrieving-sorting-data-oral-debrief"}
```

# Retrieving Data
