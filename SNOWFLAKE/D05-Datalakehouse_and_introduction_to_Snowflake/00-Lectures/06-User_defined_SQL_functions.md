# User-Defined Functions (UDFs)

## What you'll learn in this course 🧐🧐

With user-defined functions (UDFs), you can extend the system to perform operations that are not available through the built-in, system-defined functions provided by Snowflake.

This is particularly usefull to work with non-structured data - as you can first load non structured data as blob in tables and extract a schema with some Python or Java code.

In this course, you will learn:

* What is an UDF
* Scalar VS Tabular UDFs
* Write SQL UDF according to convention

Let's jump into it!

## Scalar vs Tabular UDFs

UDFs may be *scalar* or *tabular*.

A *scalar* function returns one output row for each input row. 

The returned row consists of a single column/value.

A *tabular* function, also called a table function, returns zero, one, or multiple rows for each input row. 

A tabular UDF is defined by specifying a return clause that contains the TABLE keyword and specifies the names and data types of the columns in the table results. 

Tabular UDFs are often called UDTFs (user-defined table functions) or table UDFs.

## Supported Programming Languages for Creating UDFs

Snowflake supports the following programming languages:
   * SQL
   * Java
   * JavaScript
   * Python

Many factors might affect which programming language you use to write a UDF. Factors might include:

   * Whether you already have code in a particular language. For example, if you already have a .jar file that contains a method that does the work you need, then you might prefer Java as your programming language.
   * The capabilities of the language.
   * Whether a language has libraries that can help you do the processing that you need to do.

In this course we will only review the SQL UDF - feel free to check [Snowflake Documentation](https://docs.snowflake.com/en/sql-reference/udf-overview) if you are curious about Java, Javascript or Python's implementations.

## SQL UDF

A SQL UDF evaluates an arbitrary SQL expression and returns the result of the expression.

The expression can be either a general expression or a query expression:

This UDF uses the general expression pi() * radius * radius:

```sql
CREATE FUNCTION area_of_circle(radius FLOAT)
  RETURNS FLOAT
  AS
  $$
    pi() * radius * radius
  $$
  ;
```

This UDF uses a query expression:

```sql
CREATE FUNCTION profit()
  RETURNS NUMERIC(11, 2)
  AS
  $$
    SELECT SUM((retail_price - wholesale_price) * number_sold)
        FROM purchases
  $$
  ;
```  
For non-tabular UDFs, the query expression must be guaranteed to return at most one row, containing one column.

The expression defining a UDF can refer to the input arguments of the function, as shown in the examples above.

The expression defining a UDF can refer to database objects such as tables, views, and sequences, as shown in the examples above.

However, the following restrictions apply:

The UDF owner must have appropriate privileges on any database objects that the UDF accesses.

Database objects cannot be referred to using dynamic SQL.

For example, the following statement fails because IDENTIFIER(table_name_parameter) is not allowed:

```sql
CREATE OR REPLACE FUNCTION profit2(table_name_parameter VARCHAR)
  RETURNS NUMERIC(11, 2)
  AS
  $$
    SELECT SUM((retail_price - wholesale_price) * number_sold)
        FROM IDENTIFIER(table_name_parameter)
  $$
  ;
```

The expression defining a UDF should not contain a semicolon as a statement terminator.

For example, note that the SELECT in the query expression UDF above is not terminated with a semicolon.

A SQL UDF’s defining expression can refer to other user-defined functions.

However, it cannot refer recursively to itself, either directly or indirectly (i.e. through another function calling back to it).

## Naming Conventions for UDFs

UDFs are database objects, meaning that they are created in a specified database and schema. 

As such, they have a fully-qualified name defined by their namespace, in the form of *db.schema.function_name*. For example:

```sql
SELECT temporary_db_qualified_names_test.temporary_schema_1.udf_pi();
```

When called without their fully-qualified name, UDFs are resolved according to the database and schema in use for the session.

This is in contrast to the built-in, system-defined functions provided by Snowflake, which have no namespace and, therefore, can be called from anywhere.

To avoid conflicts when calling functions, Snowflake does not allow creating UDFs with the same name as any of the system-defined functions.

Let's practice a bit to get sure we understand everything!


## Resources 📚📚

* Learn more about Snowflake's UDF thanks to [Snowflake Documentation](https://docs.snowflake.com/en/sql-reference/udf-overview)
