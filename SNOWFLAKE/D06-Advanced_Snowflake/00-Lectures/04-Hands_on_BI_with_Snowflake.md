# Hands on BI using Snowflake & Looker

On this course, we will connect a BI tool to snowflake : Looker

## What you'll learn in this course 🧐🧐

* How to connect Looker to Snowflake
* Getting some visualisations done on Looker

## Why Snowflake + Looker?

Snowflake brings together all the enterprise’s data and makes it available, easily, to all the users and systems that need to analyze it.

Looker harnesses the full power of the underlying database by providing a unique in-database architecture and flexible modelling layer operate on the data directly in Snowflake. 

Combining both together, the innovative design is especially important when it comes to structured and semi-structured data, such as JSON, Avro, and XML. 

You want to leverage the compute to analyze the data as it is stored. 

Together, Snowflake and Looker natively integrate to understand business data from both traditional and newer machine generated sources, so all of an enterprise’s users can work with all the data and all workloads.

## What can you do with Looker and Snowflake ?

### Easily access to explore and analyze diverse data

* Centralize your data on Snowflake’s elastic infrastructure
* Take advantage of Snowflake’s native support for semi-structured data (JSON, AVRO etc)
* Scale storage and compute power independently and automatically
* Run with any browser. No need to install a client.
* Share and embed content is easy and intuitive.

### Realize interactive performance

* Query all of your data directly in the database
* Explore row-level data for high-resolution insights
* Create a governed data platform with agreed-upon metrics and definitions

### Deliver fast insightful analysis at any scale.

* Empower everyone in your organization with intuitive access to the data they need
* Expect fast answers, no matter how much data you’re querying or how many concurrent users are accessing the data

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/Snowflake-Looker-Data-Sheet.png)

## Let's load some data in Snowflake using the web IDE

- First of all, do the following : create a database / schema 

- select the schema & warehouse in an active worksheet

- take the content of *src/aircraft_db.sql* and copy / paste it in a snowflake worksheet to load it in database


## Let's create a user to allow looker to query this data

Make sure that you run the following queries step by step, replace when needed in the following the database name & schema name by the names you have created

- First of all, let's switch to the admin account to create a role for looker *looker_role*

```sql
-- change role to ACCOUNTADMIN
use role ACCOUNTADMIN;

-- create role for looker
create role if not exists looker_role;
grant role looker_role to role SYSADMIN;
    -- Note that we are not making the looker_role a SYSADMIN,
    -- but rather granting users with the SYSADMIN role to modify the looker_role
```
- let's now create a dedicated user to connect from looker and associated with this role
```sql
-- create a user for looker
create user if not exists looker_user
password = '<enter password here>';
grant role looker_role to user looker_user;
alter user looker_user
set default_role = looker_role
default_warehouse = 'looker_wh';
```
- let's now create a dedicated warehouse for our looker role
```sql
-- change role
use role SYSADMIN;

-- create a warehouse for looker (optional)
create warehouse if not exists looker_wh

-- set the size based on your dataset
warehouse_size = medium
warehouse_type = standard
auto_suspend = 1800
auto_resume = true
initially_suspended = true;
grant all privileges
on warehouse looker_wh
to role looker_role;
```

- now let's grant the appropriate rights for our looker role
```sql
-- change role to ACCOUNTADMIN
use role ACCOUNTADMIN;
-- grant read only database access (repeat for all database/schemas)
grant usage on database <database> to role looker_role;
grant usage on schema <database>.<schema> to role looker_role;

-- rerun the following any time a table is added to the schema
grant select on all tables in schema <database>.<schema> to role looker_role;
-- or
grant select on future tables in schema <database>.<schema> to role looker_role;
--
grant select on all views in schema aircraft.public to role looker_role;
grant select on all future views in schema aircraft.public to role looker_role;

-- create schema for looker to write back to
use database <database>;
create schema if not exists looker_scratch;
use role ACCOUNTADMIN;
grant ownership on schema looker_scratch to role SYSADMIN revoke current grants;
grant all on schema looker_scratch to role looker_role;
```

- let's create a view for the data to be accessed
```sql
create view looker_view as (select 
	individual_flights."Flight_Id" ,
	individual_flights."Airline_Code" ,
	individual_flights."Departure_Airport_Code" ,
	individual_flights."Destination_Airport_Code" ,
	individual_flights."Aircraft_Id" ,
    aircraft."Aircraft_Type",
	aircraft."Mass" ,
	aircraft."Length",
	aircraft."Cost",
	aircraft."Capacity" 
    from individual_flights left outer join aircraft on individual_flights."Aircraft_Id" = aircraft."Aircraft_Id")
```

## Connecting Looker Studio

- First let's go to [https://lookerstudio.google.com/](https://lookerstudio.google.com/)

- on the top left click on create > datasource

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/createdatasource.png)

- search for snowflake

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/createdatasource.png)

- click on connect

- fill your account

- pick LOOKER_ROLE

- pick LOOKER_WAREHOUSE

- Choose the database you created

- Choose the scheme you used for the data

- Enter a query

```sql
select * from looker_view
```
- proceed on the connection


Congrats !!! You can now browse your data in looker studio !

## Resources 📚📚

* [Looker into Snowflake: Using the Looker Data Platform to Query Snowflake](https://community.snowflake.com/s/article/Looker-into-Snowflake-Using-the-Looker-Data-Platform-to-Query-Snowflake)
* [Managing Snowflake Data Warehouse Compute in Looker](https://community.snowflake.com/s/article/Managing-Snowflake-Data-Warehouse-Compute-in-Looker)
* [Snowflake Looker Configuration Guide](https://www.snowflake.com/wp-content/uploads/2017/01/Snowflake-Looker-Configuration-Guide.pdf)
* [Looker into Snowflake: Using the Looker Data Platform to Query Snowflake](https://community.snowflake.com/s/article/Looker-into-Snowflake-Using-the-Looker-Data-Platform-to-Query-Snowflake)
* Deepnote's [documentation](https://deepnote.com/docs)