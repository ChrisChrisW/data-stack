# Hands on Snowflake - part 1

Welcome to this course which purpose is to give you a nearly complete overview of Snowflake as a DataLakeHouse by using it - you can be independant on all its content

This is the first part, where we are covering the life of snowflakes's asset, such as databases, tables and data.

We will use the same dataset in the three parts, so don't forget to load the data as presented here even if you just want to dig the second or third part.

Alright !

## What you'll learn in this course 🧐🧐

* How to create stages, databases, tables, views, and virtual warehouses.
* How to load structured data.

Let's go!

## The Snowflake ​User Interface & Lab Story

### If not done : signing up for a free trial on Snowflake ❄️

You can start by signing up for a free trial on Snowflake ❄️:

1 .
Sign up for a free trial by following this [link](https://signup.snowflake.com/) and completing the sign-up form.

2 .
Select the Enterprise edition, choose a cloud provider and region, and agree to the terms of service.

You should consider organizational questions when choosing a cloud provider for a full implementation.
For more information, see [Introduction to Cloud Platforms](https://docs.snowflake.com/en/user-guide/intro-cloud-platforms.html) in the Snowflake docs.

For the purposes of this setup, all cloud providers and regions will work so choose whichever you’d like - I go aws.

Click GET STARTED.

3 .
After submitting the sign-up form, you should receive an email asking you to activate your account.

    Click the link in the email and a new tab will open up where you’ll create your username and password.

    Complete the form and click Get started.


### Logging into the Snowflake User Interface (UI)
Open a browser window and enter the URL of your Snowflake 30-day trial environment that was sent with your registration email.

You should see the following login dialog​.
Enter the username and password that you specified during the registration:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/94b9a152c373c99e.png)

### Navigating the Snowflake UI
Let's get you acquainted with Snowflake! This section covers the basic components of the user interface.
We will move from top to bottom on the left-hand side margin.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/ce13b4af78a66c6b.png)

**Worksheet**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/ba0e07fe7e6ab958.png)

The **​Worksheets​** tab provides an interface for submitting SQL queries, performing DDL and DML operations, and viewing results as your queries or operations complete.
A new worksheet is created by clicking **+ Worksheet** on the top right.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/8fdb16c05fd8c436.png)

The top left corner contains the following:

* **Home icon**: Use this to get back to the main console/close the worksheet.
* **Worksheet_name** drop-down: The default name is the timestamp when the worksheet was created.
Click the timestamp to edit the worksheet name.
The drop-down also displays additional actions you can perform for the worksheet.
* **Manage filters** button: Custom filters are special keywords that resolve as a subquery or list of values.

The top right corner contains the following:

* **Context** box: This lets Snowflake know which role and warehouse to use during this session.
It can be changed via the UI or SQL commands.
* **Share** button: Open the sharing menu to share to other users or copy the link to the worksheet.
* **Play/Run** button: Run the SQL statement where the cursor currently is or multiple selected statements.

The middle pane contains the following:

* Drop-down at the top for setting the database/schema/object context for the worksheet.
* General working area where you enter and execute queries and other SQL statements.

The middle-left panel contains the following:

* **Worksheets** tab: Use this tab to quickly select and jump between different worksheets
* **Databases** tab: Use this tab to view all of the database objects available to the current role
* **Search** bar: database objects browser which enables you to explore all databases, schemas, tables, and views accessible by the role currently in use for the worksheet.

The bottom pane displays the results of queries and other operations.
Also includes 4 options (**Object**, **Query**, **Result**, **Chart**) that open/close their respective panels on the UI.

**Chart** opens a visualization panel for the returned results.

We'll come back to this a litlle bit later...

The various panes on this page can be resized by adjusting their sliders.
If you need more room in the worksheet, collapse the database objects browser in the left panel.
Many of the screenshots in this guide keep this panel closed.


:::note Worksheets vs the UI 

Most of the actions in this course are executed using pre-written SQL within this worksheet to save time.
These tasks can also be done via the UI, but would require navigating back-and-forth between multiple UI tabs.
:::

**Dashboards**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/21d6de710923c81b.png)

The **Dashboards** tab allows you to create flexible displays of one or more charts (in the form of tiles, which can be rearranged).
Tiles and widgets are produced by executing SQL queries that return results in a worksheet.

Dashboards work at a variety of sizes with minimal configuration.

**Databases**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/1efb94fd8e624f9.png)

Under **Data**, the **Databases​** tab shows information about the databases you have created or have permission to access.

You can create, clone, drop, or transfer ownership of databases, as well as load data in the UI.

Notice that a database already exists in your environment.

However, we will not be using it in this course.

**Private Shared Data**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/ac79b50634875aeb.png)

Also under **Data**, the **Private Shared Data** tab is where data sharing can be configured to easily and securely share Snowflake tables among separate Snowflake accounts or external users, without having to create a copy of the data.

Be patient young apprentice, this will be covered in a few moments...

**Marketplace**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/2ebba15c6973ed6d.png)

The **Marketplace** tab is where any Snowflake customer can browse and consume data sets made available by providers.

There are two types of shared data: Public and Personalized.

Public data is free data sets available for querying instantaneously.

Personalized data requires reaching out to the provider of data for approval of sharing data.

**Query History**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/78319bfb37054a7a.png)

Under **Activity** there are two tabs **Query History** and **Copy History**:

* **Query History** is where previous queries are shown, along with filters that can be used to hone results (user, warehouse, status, query tag, etc.).
View the details of all queries executed in the last 14 days from your Snowflake account.
Click a query ID to drill into it for more information.
* **Copy History** shows the status of copy commands run to ingest data into Snowflake.

**Warehouses**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/e8ae082b9907f027.png)

Under **Admin**, the **​Warehouses​** tab is where you set up and manage compute resources known as virtual warehouses to load or query data in Snowflake.
A warehouse called COMPUTE_WH already exists in your environment.

**Resource Monitors**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/bf1224962773e6c8.png)

Under **Admin**, the **Resource Monitors** tab shows all the resource monitors that have been created to control the number of credits that virtual warehouses consume.

For each resource monitor, it shows the credit quota, type of monitoring, schedule, and actions performed when the virtual warehouse reaches its credit limit.

**Roles**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/4dda97b955d14138.png)

Under **Admin**, the **Roles** sub-tab of the **Users and Roles** tab shows a list of the roles and their hierarchies.
Roles can be created, reorganized, and granted to users in this tab.

The roles can also be displayed in tabular/list format by selecting the Table sub-tab.

**Users**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/46fa56f76ccba504.png)

Also under **Admin** tab, the **Users** sub-tab of the **Users and Roles** tab shows a list of users in the account, default roles, and owner of the users.

For a new account, no records are shown because no additional roles have been created.

Permissions granted through your current role determine the information shown for this tab.

To see all the information available on the tab, switch your role to *ACCOUNTADMIN*.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/f9ca5cc23fcd83cb.png)

Clicking on your username in the top right of the UI allows you to change your password, roles, and preferences.

Snowflake has several system defined roles.

You are currently in the default role of *SYSADMIN* and will stay in this role for the majority of this course.

*SYSADMIN : The SYSADMIN (aka System Administrator) role has privileges to create warehouses, databases, and other objects in an account.

In a real-world environment, you would use different roles for the tasks in this lab, and assign roles to your users.

We will cover more on roles and Snowflake's access control model later and you can find additional information in the Snowflake documentation.*

## Citi Bike Riders at the rescue!

:::note Some Background on the data we will be manupulating

We will be loading and manipulating data in Snwoflake to allow the exploration of all its capacity.

The data is based on the analytics team at Citi Bike, a real, citywide bike sharing system in New York City, USA.

The team wants to run analytics on data from their internal transactional systems to better understand their riders and how to best serve them.
:::

We will first load structured *.csv* data from rider transactions into Snowflake.

Later we will work with open-source, semi-structured JSON weather data to determine if there is any correlation between the number of bike rides and the weather.

Let's go!

## Preparing to Load Data

Let's start by preparing to load the structured Citi Bike rider transaction data into Snowflake.

This section walks you through the steps to:

* Create a database and table.
* Create an external stage.
* Create a file format for the data.

:::note Getting Data into Snowflake

There are many ways to get data into Snowflake from many locations including the COPY command, Snowpipe auto-ingestion, external connectors, or third-party ETL/ELT solutions.
For more information on getting data into Snowflake, see the [Snowflake documentation](https://docs.snowflake.net/manuals/user-guide-data-load.html).
For the purposes of this lab, we use the COPY command and AWS S3 storage to load data manually.
In a real-world scenario, you would more likely use an automated process or ETL solution.
:::

The data we will be using is bike share data provided by Citi Bike NYC.
The data has been exported and pre-staged for you in an Amazon AWS S3 bucket in the US-EAST region.

The data consists of information about trip times, locations, user type, gender, age, etc.
On AWS S3, the data represents 61.5M rows, 377 objects, and 1.9GB compressed.

Below is a snippet from one of the Citi Bike CSV data files:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/a58c8af550412709.png)

It is in comma-delimited format with a single header line and double quotes enclosing all string values, including the field headings in the header line.
This will come into play later in this section as we configure the Snowflake table to store this data.

### Create a Database and Table

First, let's create a database called *CITIBIKE* to use for loading the structured data.

Ensure you are using the sysadmin role by selecting **Switch Role** > **SYSADMIN**.

Navigate to the **Databases** tab.
Click **Create**, name the database *CITIBIKE*, then click **CREATE**.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/617e691cd998d830.png)

Now navigate to the **Worksheets** tab.
You should see the worksheet we created earlier.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/12afc32e8196d6b0.png)

We need to set the context appropriately within the worksheet.
In the upper right corner of the worksheet, click the box to the left of the **Share** button to show the context menu.
Here we control the elements you can see and run from each worksheet.
We are using the UI here to set the context.
Later in the lab, we will accomplish the same thing via SQL commands within the worksheet.

Select the following context settings:

Role: *SYSADMIN* Warehouse: *COMPUTE_WH*

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/e1133050d123b025.png)

Next, in the drop-down for the database, select the following context settings:

Database: *CITIBIKE* Schema = *PUBLIC*

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/3dafea3169e84fff.png)

:::note 
Data Definition Language (DDL) operations are free! 
All the DDL operations we have done so far do not require compute resources, so we can create all our objects for free.
:::

To make working in the worksheet easier, let's rename it.
In the top left corner, click the worksheet name, which is the timestamp when the worksheet was created, and change it to *CITIBIKE_ZERO_TO_SNOWFLAKE*.

Next we create a table called *TRIPS* to use for loading the comma-delimited data.
Instead of using the UI, we use the worksheet to run the DDL that creates the table.
Copy the following SQL text into your worksheet:

```
create or replace table trips
(tripduration integer,
starttime timestamp,
stoptime timestamp,
start_station_id integer,
start_station_name string,
start_station_latitude float,
start_station_longitude float,
end_station_id integer,
end_station_name string,
end_station_latitude float,
end_station_longitude float,
bikeid integer,
membership_type string,
usertype string,
birth_year integer,
gender integer);
```

:::note Many Options to Run Commands 
SQL commands can be executed through the UI, via the **Worksheets** tab, using the SnowSQL command line tool, with a SQL editor of your choice via ODBC/JDBC, or through our other connectors (Python, Spark, etc.).

As mentioned earlier, to save time, we are performing most of the operations in this lab via pre-written SQL executed in the worksheet as opposed to using the UI.
:::

Run the query by placing your cursor anywhere in the SQL text and clicking the blue **Play/Run** button in the top right of the worksheet.

Or use the keyboard shortcut [Ctrl]/[Cmd]+[Enter].

Verify that your TRIPS table has been created.
At the bottom of the worksheet you should see a Results section displaying a "Table TRIPS successfully created" message.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/dbed54bac4e63585.png)

Navigate to the **Databases** tab by clicking the **HOME** icon in the upper left corner of the worksheet.

Then click **Data** > **Databases**.

In the list of databases, click *CITIBIKE* > *PUBLIC* > **TABLES** to see your newly created *TRIPS* table.

If you don't see any databases on the left, expand your browser because they may be hidden.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/3b9b222029a49f7a.png)

Click *TRIPS* and the **Columns** tab to see the table structure you just created.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/5201e54b3b7772f4.png)

### Create an External Stage

We are working with structured, comma-delimited data that has already been staged in a public, external S3 bucket.

Before we can use this data, we first need to create a Stage that specifies the location of our external bucket.

:::note 
For this course we are using an AWS-East bucket.

To prevent data egress/transfer costs in the future, you should select a staging location from the same cloud provider and region as your Snowflake account.
:::

From the **Databases** tab, click the *CITIBIKE* database and *PUBLIC* schema.

In the **Stages** tab, click the **Create** button, then **Stage** > **Amazon S3**.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/a0b3421ffacddeec.png)

In the "Create Securable Object" dialog that opens, replace the following values in the SQL statement:

*stage_name* : *citibike_trips*

*url* : *s3://snowflake-workshop-lab/citibike-trips-csv/*

**Note**: Make sure to include the final forward slash (/) at the end of the URL or you will encounter errors later when loading data from the bucket.
Also ensure you have removed ‘credentials = (...)' statement which is not required.

The create stage command should resemble that show above exactly.

:::note 
The S3 bucket for this course is public so you can leave the credentials options in the statement empty.

In a real-world scenario, the bucket used for an external stage would likely require key information.
:::

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/91fa215e0a17b483.png)

Now let's take a look at the contents of the *citibike_trips* stage.
Navigate to the **Worksheets** tab and execute the following SQL statement:
```
list @citibike_trips;
```
In the results in the bottom pane, you should see the list of files in the stage:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/e60ace83025630e9.png)

### Create a File Format
Before we can load the data into Snowflake, we have to create a file format that matches the data structure.

In the worksheet, run the following command to create the file format:
```
--create file format

create or replace file format csv type='csv'
  compression = 'auto' field_delimiter = ',' record_delimiter = '\n'
  skip_header = 0 field_optionally_enclosed_by = '\042' trim_space = false
  error_on_column_count_mismatch = false escape = 'none' escape_unenclosed_field = '\134'
  date_format = 'auto' timestamp_format = 'auto' null_if = ('') comment = 'file format for ingesting data for zero to snowflake';
```
![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/3c52420ff7677f1.png)

Verify that the file format has been created with the correct settings by executing the following command:

```
--verify file format is created

show file formats in database citibike;
```

The file format created should be listed in the result:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/e277ae2e58729906.png)

## Loading Data

In this section, we will use a virtual warehouse and the COPY command to initiate bulk loading of structured data into the Snowflake table we created in the last section.

### Resize and Use a Warehouse for Data Loading

Compute resources are needed for loading data.

Snowflake's compute nodes are called virtual warehouses and they can be dynamically sized up or out according to workload, whether you are loading data, running a query, or performing a DML operation.

Each workload can have its own warehouse so there is no resource contention.

Navigate to the **Warehouses** tab (under **Admin**).

This is where you can view all of your existing warehouses, as well as analyze their usage trends.

Note the **+ Warehouse** option in the upper right corner of the top.

This is where you can quickly add a new warehouse.

However, we want to use the existing warehouse COMPUTE_WH included in the 30-day trial environment.

Click the row of the *COMPUTE_WH* warehouse.

Then click the ... (dot dot dot) in the upper right corner text above it to see the actions you can perform on the warehouse.

We will use this warehouse to load the data from AWS S3.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/5fcf1309cb972674.png)

Click **Edit** to walk through the options of this warehouse and learn some of Snowflake's unique functionality.

:::note
If this account isn't using Snowflake Enterprise Edition (or higher), you will not see the Mode or Clusters options shown in the screenshot below.
The multi-cluster warehouses feature is not used in this lab, but we will discuss it as a key capability of Snowflake.
:::

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/94c5ebc4ce00220c.png)

* The **Size** drop-down is where the capacity of the warehouse is selected.
For larger data loading operations or more compute-intensive queries, a larger warehouse is recommended.
The sizes translate to the underlying compute resources provisioned from the cloud provider (AWS, Azure, or GCP) where your Snowflake account is hosted.
It also determines the number of credits consumed by the warehouse for each full hour it runs.
The larger the size, the more compute resources from the cloud provider are allocated to the warehouse and the more credits it consumes.

For example, the 4X-Large setting consumes 128 credits for each full hour.

This sizing can be changed up or down at any time with a simple click.

* If you are using Snowflake Enterprise Edition (or higher) the **Query Acceleration** option is available.
When it is enabled for a warehouse, it can improve overall warehouse performance by reducing the impact of outlier queries, which are queries that use more resources than the typical query.
Leave this disabled

* If you are using Snowflake Enterprise Edition (or higher) and the **Multi-cluster Warehouse** option is enabled, you will see additional options.

This is where you can set up a warehouse to use multiple clusters of compute resources, up to 10 clusters.

For example, if a 4X-Large multi-cluster warehouse is assigned a maximum cluster size of 10, it can scale out to 10 times the compute resources powering that warehouse...and it can do this in seconds! However, note that this will increase the number of credits consumed by the warehouse to 1280 if all 10 clusters run for a full hour (128 credits/hour x 10 clusters).
Multi-cluster is ideal for concurrency scenarios, such as many business analysts simultaneously running different queries using the same warehouse.
In this use case, the various queries are allocated across multiple clusters to ensure they run quickly.

* Under **Advanced Warehouse Options**, the options allow you to automatically suspend the warehouse when not in use so no credits are needlessly consumed.
There is also an option to automatically resume a suspended warehouse so when a new workload is sent to it, it automatically starts back up.
This functionality enables Snowflake's efficient "pay only for what you use" billing model which allows you to scale your resources when necessary and automatically scale down or turn off when not needed, nearly eliminating idle resources.
Additionally, there is an option to change the Warehouse type from Standard to Snowpark-optimized.
Snowpark-optmized warehouses provide 16x memory per node and are recommended for workloads that have large memory requirements such as ML training use cases using a stored procedure on a single virtual warehouse node.
Leave this type as Standard.


:::note Snowflake Compute vs Other Data Warehouses
Many of the virtual warehouse and compute capabilities we just covered, such as the ability to create, scale up, scale out, and auto-suspend/resume virtual warehouses are easy to use in Snowflake and can be done in seconds.

For on-premise data warehouses, these capabilities are much more difficult, if not impossible, as they require significant physical hardware, over-provisioning of hardware for workload spikes, and significant configuration work, as well as additional challenges.

Even other cloud-based data warehouses cannot scale up and out like Snowflake without significantly more configuration work and time.
:::

**Warning - Watch Your Spend!**
During or after this course, you should be careful about performing the following actions without good reason or you may burn through the $400 of free credits more quickly than desired:

* Do not disable auto-suspend.
If auto-suspend is disabled, your warehouses continues to run and consume credits even when not in use.
* Do not use a warehouse size that is excessive given the workload.
The larger the warehouse, the more credits are consumed.


We are going to use this virtual warehouse to load the structured data in the CSV files (stored in the AWS S3 bucket) into Snowflake.

However, we are first going to change the size of the warehouse to increase the compute resources it uses.

After the load, note the time taken and then, in a later step in this section, we will re-do the same load operation with an even larger warehouse, observing its faster load time.

Change the **Size** of this data warehouse from *X-Small* to *Small* then click the **Save Warehouse** button:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/8c4fb3ecad091198.png)

### Load the Data
Now we can run a COPY command to load the data into the *TRIPS* table we created earlier.

Navigate back to the *CITIBIKE_ZERO_TO_SNOWFLAKE* worksheet in the **Worksheets** tab.

Make sure the worksheet context is correctly set:

Role: *SYSADMIN* Warehouse: *COMPUTE_WH* Database: *CITIBIKE* Schema = *PUBLIC*

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/a22dcc20c3929d09.png)

Execute the following statements in the worksheet to load the staged data into the table.
This may take up to 30 seconds.

```
copy into trips from @citibike_trips file_format=csv PATTERN = '.*csv.*' ;
```

In the result pane, you should see the status of each file that was loaded.
Once the load is done, in the Query Details pane on the bottom right, you can scroll through the various statuses, error statistics, and visualizations for the last statement executed:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/6bcc9ac924e135ed.png)

Next, navigate to the **Query History** tab by clicking the **Home** icon and then **Activity** > **Query History**.

Select the query at the top of the list, which should be the COPY INTO statement that was last executed.

Select the **Query Profile** tab and note the steps taken by the query to execute, query details, most expensive nodes, and additional statistics.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/ba7874d9fe5cb2b7.png)

Now let's reload the *TRIPS* table with a larger warehouse to see the impact the additional compute resources have on the loading time.

Go back to the worksheet and use the TRUNCATE TABLE command to clear the table of all data and metadata:
```
truncate table trips;
```
Verify that the table is empty by running the following command:

```
--verify table is clear
select * from trips limit 10;
```

The result should show "Query produced no results".

Change the warehouse size to *large* using the following ALTER WAREHOUSE:

```
--change warehouse size from small to large (4x)
alter warehouse compute_wh set warehouse_size='large';
```

Verify the change using the following SHOW WAREHOUSES:

```
--load data with large warehouse
show warehouses;
```
![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/808e94b8dc375648.png)

The size can also be changed using the UI by clicking on the worksheet context box, then the **Configure** (3-line) icon on the right side of the context box, and changing *Small* to *Large* in the **Size** drop-down:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/a22dcc20c3929d09.png)

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/e92369b011ce703f.png)

Execute the same COPY INTO statement as before to load the same data again:

```
copy into trips from @citibike_trips
file_format=CSV;
```

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/2e1eaaced0a95340.png)

Once the load is done, navigate back to the **Queries** page (**Home** icon > **Activity** > **Query History**).

Compare the times of the two COPY INTO commands.

The load using the *Large* warehouse was significantly faster.


## Resources 📚📚

* Learn more about the [Snowsight](https://docs.snowflake.com/en/user-guide/ui-snowsight.html#using-snowsight) docs.
* Read the [Definitive Guide to Maximizing Your Free Trial](https://www.snowflake.com/test-driving-snowflake-the-definitive-guide-to-maximizing-your-free-trial/) document.
* [HANDS-ON LAB GUIDE FOR VIRTUAL ZERO-TOSNOWFLAKE](https://s3.amazonaws.com/snowflake-workshop-lab/OnlineZTS_LabGuide.pdf)