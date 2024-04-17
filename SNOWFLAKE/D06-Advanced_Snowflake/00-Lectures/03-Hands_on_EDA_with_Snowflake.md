# Hands on fast EDA using Snowflake & Deepnote

## What you'll learn in this course 🧐🧐

* How to perform EDA from a Snowflake DataPlatform
* How to connect Deepnote & Snowflake
* Performing an EDA on Deepnote

## EDA on Datalakehouse

Exploratory data analysis (EDA) is used by data scientists to analyze and investigate data sets and summarize their main characteristics, often employing data visualization methods.

It helps determine how best to manipulate data sources to get the answers you need, making it easier for data scientists to discover patterns, spot anomalies, test a hypothesis, or check assumptions.

EDA is primarily used to see what data can reveal beyond the formal modeling or hypothesis testing task and provides a better understanding of data set variables and the relationships between them.

EDA are mostly performed over data from Datawarehouse or Datalake (when data is unstructured yet).

As Snowflake can endoss the role of a datalakehouse, it will be the source of data for your EDA if your organization is using Snowflake.

Therefore you can use the Pyhon connector that we have seen yesterday to extract data from Snowflake in notebooks and run the usual EDA techniques using numpy, pandas, and python's classical visualization libraries.

But you might realize that most of the work you are doing when performing EDA is repetitive for each data sets you are investigating - and as all datasets in Snowflake gets a descriptive schema, some automation can be performed here to reduce the "time to insight" for teams as they explore their data.

Let's think about it, we could make an EDA pipeline based on any Dataframe and then update a SQL query in order to build the Dataframe sourcing our EDA. 

Wouldn't it be marvelous ?

Some tools are positionned on this approach, and we will dig in *Deepnote* to get a better understanding of how this is done.

Let's go ! 😘 🎉

## First let's load some data to be analyzed

- download the file weather.parquet from the src folder

let's use Snowsql to load the data

- let's connect
```bash
snowsql -a <account-name> -u <username>
```

- create a database, a table and a warehouse

```bash
create or replace database weather;
use schema weather.public;

create or replace table public.weather (
    INDEX INTEGER default null,
    DATE varchar default null,
    NAME varchar default null,
    TEMP FLOAT default null,
    WIND FLOAT default null
  );


create or replace warehouse mywarehouse with
  warehouse_size='X-SMALL'
  auto_suspend = 120
  auto_resume = true
  initially_suspended=true;

use warehouse mywarehouse;
```

- create a file format for our file 

```bash
CREATE OR REPLACE FILE FORMAT weather_parquet_format TYPE = parquet;
```

- create a stage object

```bash
CREATE OR REPLACE TEMPORARY STAGE weather_stage FILE_FORMAT = weather_parquet_format;
```


- stage the file 
```bash
PUT file:////Users/laurentmorelli/Downloads/weather.parquet @weather_stage;
```

- feed the table
```
copy into public.weather
 from (select $1:INDEX::integer,
              $1:DATE::varchar,
              $1:NAME::varchar,
              $1:TEMP::float,
              $1:WIND::float
      from @weather_stage/weather.parquet);
```

- check the data
```
select * from public.weather limit 1;
```

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/checkweather.png)

Well done ! 😘 🎉

Now let's go to deepnote !

## Let's create Deepnote account & connect to snowflake

- go to [Deepnote](https://deepnote.com/sign-up) and sign up

- pick Education as a student when asked

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/educationasastudent.png)

- click I'll do this later

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/later.png)

- use *jedha-weather* as a workspace name 

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/jedhaweather.png)

- click invite team membres later if asked

- Pick Snowflake as the used datasource and click *Try Deepnote with my own data*

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/snowflakedatasource.png)

- When Ask - edit Snowflake integration settings to get you connected

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/3bee7ca79b3615ba.png)

- Create a new project in deepnote and connect it to your jedha weather cnx

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/connecttojedhadb.png)

- Once you are connected, you will be able to browse your schema *weather.public* directly from Deepnote and query your tables with SQL blocks 

Congrat's !!!

We are ready to perform some EDA

## Generalities on DeepNote

### Query Snowflake with Deepnote's SQL blocks

Similar to Python code cells used in Jupyter notebooks, Deepnote also includes native SQL cells which include syntax highlighting and autocomplete based on the Snowflake schema.

Hover your mouse on the border of block, click the ➕ sign, and select your Snowflake SQL block from the list.

Now you can write SQL as you would in a standard SQL editor as shown below.

Notice that in the example below we can already start exploring the weather data via the rich DataFrame display.

This includes being able to use filters, sorting, pagination, as well as examining histograms, ratios of values, and data types.

Importantly, the results set is saved to a Pandas DataFrame (in this case *df*)—meaning that you can continue using Pandas or any other tool in the Python library ecosystem to further explore the data.

```bash
SELECT date, name, wind, temp
FROM WEATHER_SCHEMA.NY_WEATHER
WHERE name in ('BUFFALO NIAGARA INTERNATIONAL','ROCHESTER')
```

### Mix-and-match Python and SQL

SQL queries in Deepnote also support the JinjaSQL templating language. 

This allows users to pass Python variables directly into SQL queries as well as use jinja-style control structures (conditionals and loops) for supplementing standard SQL.

In the example below, weather stations are filtered in the WHERE clause by passing in the python list called station_list rather than having to list them all out manually.

This is just one way to parameterize your SQL queries; we will see later how you can build a UI around your SQL blocks, allowing for interactive data exploration without having to repeat similar blocks of code, over and over again.

```bash
import pandas as pd 
import numpy as np 

station_list=['BUFFALO NIAGARA INTERNATIONAL',
              'ROCHESTER',
              'SYRACUSE HANCOCK INTERNATIONAL',
              'ALBANY INTERNATIONAL AIRPORT',
              'CENTRAL PARK']
```

```bash
SELECT date, name, wind, temp
FROM WEATHER_SCHEMA.NY_WEATHER
WHERE name in {{ station_list | inclause }}
```

### Visualize the results of a query

Deepnote provides no-code data visualization tools for streamlining EDA.

Given the results of the query above, let's build a data visualization to quickly explore the expected weather patterns (again, click the ➕ sign to choose a chart block type).

We should expect temperature to get warmer in the summer months and colder in the winter months.

Let's choose *df* as the DataFrame to visualize by selecting it from the dropdown at the top of the Chart block. 

Select "point chart" and specify how you would like to map columns of your data to the X/Y coordinates on the chart. 

Since we want to see temperature as a function of time, let's select date for the X axis and temp for the Y axis (example below).

Sure enough, we can verify the seasonality in the weather patterns—and all without a single line of code.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/a61b75fec8650512.png)

While the chart above confirms the expected seasonality, we can't see the data broken down by station (i.e., the location/city of the weather sensor).

Since the name column contains this information, we can again use no-code charts to look at the average temp by name to get a sense of how locations/cities differ in terms of temperature.

As shown below, aggregations and other options can be selected by clicking the ellipsis next to the encoding channels (X, Y, and Color). 

Here, we can visualize average temperature by station name by selecting Average from the aggregation options on the X axis (which is set to temp), and setting the Y and Color channels to name.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/3430aba324a37632.gif)

Many chart types and encoding combinations can used to create helpful data visualizations. 

This can be helpful not only in terms of developer speed but also for non-technical team members who may not be as familiar with plotting via code. 

For more complex needs, nothing is stopping us from using Altair, Plotly, or any other Python visualization library in Deepnote... Sky is the limit !



### Drop Objects

Don't forget to drop unused Database

- DROP​ objects no longer in use.

```
drop database if exists weather;

DROP WAREHOUSE IF EXISTS mywarehouse;
```

## Resources 📚📚

* IBM's take on [EDA](https://www.ibm.com/topics/exploratory-data-analysis#:~:text=Exploratory%20data%20analysis%20(EDA)%20is,often%20employing%20data%20visualization%20methods.)
* Deepnote's [documentation](https://deepnote.com/docs)