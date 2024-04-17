# Hands on Snowflake - part 2

Welcome to this course which purpose is to give you a nearly complete overview of Snowflake as a DataLakeHouse by using it - you can be independant on all its content

This is the second part, where we are covering the analytics capacity of snowflake

We are using the dataset from the first part, so don't forget to load the data as presented in the first part before starting this course

Alright !

## What you'll learn in this course 🧐🧐

* How to perform analytical queries on data in Snowflake, including joins between tables.
* How to load semi-structured data.
* How to clone objects.
* How to undo user errors using Time Travel.

Let's go!

### Create a New Warehouse for Data Analytics

Going back to the lab story, let's assume the Citi Bike team wants to eliminate resource contention between their data loading/ETL workloads and the analytical end users using BI tools to query Snowflake.

As mentioned earlier, Snowflake can easily do this by assigning different, appropriately-sized warehouses to various workloads.
Since Citi Bike already has a warehouse for data loading, let's create a new warehouse for the end users running analytics.

We will use this warehouse to perform analytics in the next section.

Navigate to the **Admin** > **Warehouses** tab, click **+ Warehouse**, and name the new warehouse `` and set the size to *Large*.

If you are using Snowflake Enterprise Edition (or higher) and **Multi-cluster Warehouses** is enabled, you will see additional settings:

* Make sure **Max Clusters** is set to *1*.
* Leave all the other settings at their defaults.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/970f2ab7dc954538.png)

Click the **Create Warehouse** button to create the warehouse.

## Working with Queries, the Results Cache, & Cloning

In the previous exercises, we loaded data into two tables using Snowflake's COPY bulk loader command and the COMPUTE_WH virtual warehouse.

Now we are going to take on the role of the analytics users at Citi Bike who need to query data in those tables using the worksheet and the second warehouse ANALYTICS_WH.

:::note Real World Roles and Querying

Within a real company, analytics users would likely have a different role than SYSADMIN.

To keep the lab simple, we are going to stay with the SYSADMIN role for this section.

Additionally, querying would typically be done with a business intelligence product like Tableau, Looker, PowerBI, etc.

For more advanced analytics, data science tools like Datarobot, Dataiku, AWS Sagemaker or many others can query Snowflake.

Any technology that leverages JDBC/ODBC, Spark, Python, or any of the other supported programmatic interfaces can run analytics on the data in Snowflake.

To keep this lab simple, all queries are being executed via the Snowflake worksheet.

:::

### Execute Some Queries

Go to the *CITIBIKE_ZERO_TO_SNOWFLAKE* worksheet and change the warehouse to use the new warehouse you created in the last section.
Your worksheet context should be the following:

Role: *SYSADMIN* Warehouse: *ANALYTICS_WH (L)* Database: *CITIBIKE* Schema = *PUBLIC*

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/9635d6afa094fff2.png)

Run the following query to see a sample of the *trips* data:

```sql
select * from trips limit 20;
```

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/af740efabf9cd961.png)

Now, let's look at some basic hourly statistics on Citi Bike usage.
Run the query below in the worksheet.
For each hour, it shows the number of trips, average trip duration, and average trip distance.

```sql
select date_trunc('hour', starttime) as "date",
count(*) as "num trips",
avg(tripduration)/60 as "avg duration (mins)",
avg(haversine(start_station_latitude, start_station_longitude, end_station_latitude, end_station_longitude)) as "avg distance (km)"
from trips
group by 1 order by 1;
```

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/57b57e5d9b3f0d00.png)

### Use the Result Cache
Snowflake has a result cache that holds the results of every query executed in the past 24 hours.

These are available across warehouses, so query results returned to one user are available to any other user on the system who executes the same query, provided the underlying data has not changed.

Not only do these repeated queries return extremely fast, but they also use no compute credits.

Let's see the result cache in action by running the exact same query again.

```sql
select date_trunc('hour', starttime) as "date",
count(*) as "num trips",
avg(tripduration)/60 as "avg duration (mins)",
avg(haversine(start_station_latitude, start_station_longitude, end_station_latitude, end_station_longitude)) as "avg distance (km)"
from trips
group by 1 order by 1;
```

In the **Query Details** pane on the right, note that the second query runs significantly faster because the results have been cached.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/f4f345c789dd5be5.png)

### Execute Another Query

Next, let's run the following query to see which months are the busiest:

```sql
select
monthname(starttime) as "month",
count(*) as "num trips"
from trips
group by 1 order by 2 desc;
```

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/770de9bb669575ab.png)


### Clone a Table

Snowflake allows you to create clones, also known as "zero-copy clones" of tables, schemas, and databases in seconds.

When a clone is created, Snowflake takes a snapshot of data present in the source object and makes it available to the cloned object.

The cloned object is writable and independent of the clone source.

Therefore, changes made to either the source object or the clone object are not included in the other.

A popular use case for zero-copy cloning is to clone a production environment for use by Development & Testing teams to test and experiment without adversely impacting the production environment and eliminating the need to set up and manage two separate environments.

:::note Zero-Copy Cloning

A massive benefit of zero-copy cloning is that the underlying data is not copied.

Only the metadata and pointers to the underlying data change.

Hence, clones are "zero-copy" and storage requirements are not doubled when the data is cloned.

Most data warehouses cannot do this, but for Snowflake it is easy!
:::

Run the following command in the worksheet to create a development (dev) table clone of the trips table:

```
create table trips_dev clone trips;
```

* Click the three dots (...) in the left pane and select Refresh.

* Expand the object tree under the CITIBIKE database and verify that you see a new table named trips_dev.

* Your Development team now can do whatever they want with this table, including updating or deleting it, without impacting the trips table or any other object.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/adae9f4ec4cec092.png)

## Working with Semi-Structured Data, Views, & Joins

Going back to the lab's example, the Citi Bike analytics team wants to determine how weather impacts ride counts.

To do this, in this section, we will:

* Load weather data in semi-structured JSON format held in a public S3 bucket.
* Create a view and query the JSON data using SQL dot notation.
* Run a query that joins the JSON data to the previously loaded TRIPS data.
* Analyze the weather and ride count data to determine their relationship.

The JSON data consists of weather information provided by MeteoStat detailing the historical conditions of New York City from 2016-07-05 to 2019-06-25.

It is also staged on AWS S3 where the data consists of 75k rows, 36 objects, and 1.1MB compressed.

If viewed in a text editor, the raw JSON in the GZ files looks like:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/c025f1200b524e26.png)

:::note SEMI-STRUCTURED DATA 

Snowflake can easily load and query semi-structured data such as JSON, Parquet, or Avro without transformation.

This is a key Snowflake feature because an increasing amount of business-relevant data being generated today is semi-structured, and many traditional data warehouses cannot easily load and query such data.

Snowflake makes it easy!*

:::

### Create a New Database and Table for the Data
First, in the worksheet, let's create a database named WEATHER to use for storing the semi-structured JSON data.

```sql
create database weather;
```

Execute the following USE commands to set the worksheet context appropriately:

```sql
use role sysadmin;

use warehouse compute_wh;

use database weather;

use schema public;
```

*Executing Multiple Commands : Remember that you need to execute each command individually.
However, you can execute them in sequence together by selecting all of the commands and then clicking the Play/Run button (or using the keyboard shortcut).*

Next, let's create a table named *JSON_WEATHER_DATA* to use for loading the JSON data.
In the worksheet, execute the following CREATE TABLE command:

```sql
create table json_weather_data (v variant);
```

Note that Snowflake has a special column data type called *VARIANT* that allows storing the entire JSON object as a single row and eventually query the object directly.

*Semi-Structured Data Magic : The **VARIANT** data type allows Snowflake to ingest semi-structured data without having to predefine the schema.*

In the results pane at the bottom of the worksheet, verify that your table, *JSON_WEATHER_DATA*, was created:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/d8810aacbaa1cbcf.png)

### Create Another External Stage

In the *CITIBIKE_ZERO_TO_SNOWFLAKE* worksheet, use the following command to create a stage that points to the bucket where the semi-structured JSON data is stored on AWS S3:

```
create stage nyc_weather
url = 's3://snowflake-workshop-lab/zero-weather-nyc';
```
Now let's take a look at the contents of the nyc_weather stage.
Execute the following LIST command to display the list of files:

```
list @nyc_weather;
```

In the results pane, you should see a list of .gz files from S3:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/f94b7130857937b0.png)

### Load and Verify the Semi-structured Data

In this section, we will use a warehouse to load the data from the S3 bucket into the *JSON_WEATHER_DATA* table we created earlier.

In the *CITIBIKE_ZERO_TO_SNOWFLAKE* worksheet, execute the COPY command below to load the data.

Note that you can specify a *FILE FORMAT* object inline in the command.

In the previous section where we loaded structured data in CSV format, we had to define a file format to support the CSV structure.

Because the JSON data here is well-formed, we are able to simply specify the JSON type and use all the default settings:

```sql
copy into json_weather_data
from @nyc_weather 
    file_format = (type = json , strip_outer_array = true);
```

Verify that each file has a status of LOADED:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/56840eb3550a91e2.png)

Now, let's take a look at the data that was loaded:

```sql
select * from json_weather_data limit 10;
```

Click any of the rows to display the formatted JSON in the right panel:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/396dcc5f249ba6ff.png)

To close the display in the panel and display the query details again, click the X (Close) button that appears when you hover your mouse in the right corner of the panel.

### Create a View and Query Semi-Structured Data

Next, let's look at how Snowflake allows us to create a view and also query the JSON data directly using SQL.

:::note Views & Materialized Views 

A view allows the result of a query to be accessed as if it were a table.

Views can help present data to end users in a cleaner manner, limit what end users can view in a source table, and write more modular SQL.

Snowflake also supports materialized views in which the query results are stored as though the results are a table.

This allows faster access, but requires storage space.

Materialized views can be created and queried if you are using Snowflake Enterprise Edition (or higher).
:::

Run the following command to create a columnar view of the semi-structured JSON weather data so it is easier for analysts to understand and query.

The *72502* value for *station_id* corresponds to Newark Airport, the closest station that has weather conditions for the whole period.

```sql
// create a view that will put structure onto the semi-structured data
create or replace view json_weather_data_view as
select
    v:obsTime::timestamp as observation_time,
    v:station::string as station_id,
    v:name::string as city_name,
    v:country::string as country,
    v:latitude::float as city_lat,
    v:longitude::float as city_lon,
    v:weatherCondition::string as weather_conditions,
    v:coco::int as weather_conditions_code,
    v:temp::float as temp,
    v:prcp::float as rain,
    v:tsun::float as tsun,
    v:wdir::float as wind_dir,
    v:wspd::float as wind_speed,
    v:dwpt::float as dew_point,
    v:rhum::float as relative_humidity,
    v:pres::float as pressure
from
    json_weather_data
where
    station_id = '72502';
``` 

SQL dot notation *v:temp:: is used in this command to pull out values at lower levels within the JSON object hierarchy.
This allows us to treat each field as if it were a column in a relational table.

The new view should appear as *JSON_WEATHER_DATA* under *WEATHER* > *PUBLIC* > **Views** in the object browser on the left.

You may need to expand or refresh the objects browser in order to see it.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/19a8eb22f6f8c146.png)

Verify the view with the following query:

```sql
select * from json_weather_data_view
where date_trunc('month',observation_time) = '2018-01-01'
limit 20;
``` 

Notice the results look just like a regular structured data source.
Your result set may have different observation_time values:


![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/1f418f127b4bf730.png)

### Use a Join Operation to Correlate Against Data Sets

We will now join the JSON weather data to our *CITIBIKE.PUBLIC.TRIPS* data to answer our original question of how weather impacts the number of rides.

Run the query below to join *WEATHER* to *TRIPS* and count the number of trips associated with certain weather conditions:

:::note 
Because we are still in the worksheet, the WEATHER database is still in use.

You must, therefore, fully qualify the reference to the TRIPS table by providing its database and schema name.
:::

```sql
select 
    weather_conditions as conditions,
    count(*) as num_trips
    from citibike.public.trips
left outer join json_weather_data_view
    on date_trunc('hour', observation_time) = date_trunc('hour', starttime)
where conditions is not null
group by 1 order by 2 desc;
```

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/a155931384ef1a63.png)

The initial goal was to determine if there was any correlation between the number of bike rides and the weather by analyzing both ridership and weather data.
Per the results above we have a clear answer.

As one would imagine, the number of trips is significantly higher when the weather is good!

## Using Time Travel

Snowflake's powerful Time Travel feature enables accessing historical data, as well as the objects storing the data, at any point within a period of time.

The default window is 24 hours and, if you are using Snowflake Enterprise Edition, can be increased up to 90 days.

Most data warehouses cannot offer this functionality, but - you guessed it - Snowflake makes it easy!

Some useful applications include:

* Restoring data-related objects such as tables, schemas, and databases that may have been deleted.
* Duplicating and backing up data from key points in the past.
* Analyzing data usage and manipulation over specified periods of time.

**Drop and Undrop a Table**

First let's see how we can restore data objects that have been accidentally or intentionally deleted.

In the *CITIBIKE_ZERO_TO_SNOWFLAKE* worksheet, run the following *DROP* command to remove the *JSON_WEATHER_DATA* table:
```
drop table json_weather_data;
```
Run a query on the table:
```sql
select * from json_weather_data limit 10;
```
In the results pane at the bottom, you should see an error because the underlying table has been dropped:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/b3e5cb9802b07b8d.png)

Now, restore the table:
```sql
undrop table json_weather_data;
```
The json_weather_data table should be restored.
Verify by running the following query:
```sql
--verify table is undropped

select * from json_weather_data limit 10;
```

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/1c83f7a8b9f40112.png)

Isn't it magic ?

**Roll Back a Table**

Let's roll back the *TRIPS* table in the *CITIBIKE* database to a previous state to fix an unintentional DML error that replaces all the station names in the table with the word "oops".

First, run the following SQL statements to switch your worksheet to the proper context:

```sql
use role sysadmin;

use warehouse compute_wh;

use database citibike;

use schema public;
```

Run the following command to replace all of the station names in the table with the word "oops":

```sql
update trips set start_station_name = 'oops';
```

Now, run a query that returns the top 20 stations by number of rides.
Notice that the station names result contains only one row:

```sql
select
    start_station_name as "station",
    count(*) as "rides"
from trips
    group by 1
    order by 2 desc
limit 20;
```

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/7c217a81e0eaaf86.png)

Normally we would need to scramble and hope we have a backup lying around.

In Snowflake, we can simply run a command to find the query ID of the last *UPDATE* command and store it in a variable named *$QUERY_ID*.

```sql
set query_id =
(select query_id from table(information_schema.query_history_by_session (result_limit=>5))
where query_text like 'update%' order by start_time desc limit 1);
```

Use Time Travel to recreate the table with the correct station names:

```sql
create or replace table trips as
(select * from trips before (statement => $query_id));
```

Run the previous query again to verify that the station names have been restored:

```sql
select
start_station_name as "station",
count(*) as "rides"
from trips
group by 1
order by 2 desc
limit 20;
```

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/9f28f362225b2ee1.png)

Incredible !
Now that we are error proof, let's dig a bit about roles and accounts !



Congratulations on completing this exercise!  👏👏👏 🎉
You've mastered the Snowflake basics and are ready to apply these fundamentals to your own data.


## Resources 📚📚

* Learn more about the [Snowsight](https://docs.snowflake.com/en/user-guide/ui-snowsight.html#using-snowsight) docs.
* Read the [Definitive Guide to Maximizing Your Free Trial](https://www.snowflake.com/test-driving-snowflake-the-definitive-guide-to-maximizing-your-free-trial/) document.
* [HANDS-ON LAB GUIDE FOR VIRTUAL ZERO-TOSNOWFLAKE](https://s3.amazonaws.com/snowflake-workshop-lab/OnlineZTS_LabGuide.pdf)