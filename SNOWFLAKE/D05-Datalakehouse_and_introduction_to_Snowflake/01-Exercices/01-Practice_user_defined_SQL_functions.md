# User-Defined Functions (UDFs) - Exercice


## Login to Snowflake

Login to Snowflake and pick a account which has a role with the permission to create a database.

Optionnal : Switch the account role from the default *SYSADMIN* to *ACCOUNTADMIN*.

With your new account created and the role configured, you're ready to begin creating database objects.

## Create a new Database

* Create a new database named *udf_db*

* Create a new schema named *udf_schema_public*

* Import data in a new table  
```
create or replace table udf_db.udf_schema_public.sales 
  as
    (select * from snowflake_sample_data.tpcds_sf10tcl.store_sales sample block (1)  limit 10000);
```
The Results should display a status message of Database UDF_DB successfully created .

## Scalar UDF

* create a scalar udf named udf_max() which returns the max of SS_LIST_PRICE from you new table

* call this udf

## Tablar UDF

* create a tablar udf that is named get_market_basket, takes in argument a number named input_item_sk which shall match the column ss_item_sk of sales and display ss_item_sk and the count of distinct ss_ticket_number associated

* call this udf


## Cleanup

Before we wrap-up, drop the practice database you created in this guide. This will remove the database and all of the tables and functions that you created.

Drop the database: *udf_db*.
```
drop database if exists udf_db;
```

Verify the database is entirely gone by checking the **Results** for *UDF_DB successfully dropped*.