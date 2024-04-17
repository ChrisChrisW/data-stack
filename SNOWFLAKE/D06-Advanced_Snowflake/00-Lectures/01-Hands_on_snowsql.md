# Hands on SnowSQL (CLI Client)


## What you'll learn in this course 🧐🧐

* What is SnowSQL
* SnowSQL setup
* Uploading data using SnowSQL
* Querying data using SnowSQL
* Managing and deleting data using SnowSQL

Let's go !

## SnowSql

SnowSQL is the command line client for connecting to Snowflake to execute SQL queries and perform all DDL and DML operations, including loading data into and unloading data out of database tables.

SnowSQL (snowsql executable) can be run as an interactive shell or in batch mode through stdin or using the -f option.

SnowSQL is an example of an application developed using the Snowflake Connector for Python.

However, the connector is not a prerequisite for installing SnowSQL. 
All required software for installing SnowSQL is bundled in the installers.

Snowflake provides platform-specific versions of SnowSQL for download for the following platforms:

- Linux : CentOS 7, 8 - Red Hat Enterprise Linux (RHEL) 7, 8 - Ubuntu 16.04, 18.04, 20.04 or later
- macOS 
- Microsoft Windows : Microsoft Windows 8 or later - Microsoft Windows Server 2012, 2016, 2019

## let's install SnowSql

SnowSQL can be downloaded and installed on Linux, Windows, or Mac. 
In this example, we'll download the installer for macOS via the AWS endpoint. 
If you're using a different OS or prefer other methods, check out all the ways to get SnowSQL [here](https://docs.snowflake.com/en/user-guide/snowsql-install-config#installing-snowsql-on-microsoft-windows-using-the-installer).

```bash
curl -O https://sfc-repo.snowflakecomputing.com/snowsql/bootstrap/​<bootstrap-version>/darwin_x86_4/snowsql-<snowsql-version>-darwin_x86_64.pkg
```
Specify ​bootstrap and snowsql version​ number within the cURL command.

The example below is a cURL command to the AWS endpoint for a macOS to download the bootstrap version 1.2 and SnowSQL version 1.2.9.

```bash
curl -O https://sfc-repo.snowflakecomputing.com/snowsql/bootstrap/1.2/darwin_x86_64/snowsql-1.2.9-darwin_x86_64.pkg​
```

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/abb3e71ca1569a4f.png)

### Install SnowSQL Locally

- Double-click the installer file and walk through the wizard prompts.
- Confirm the install was successful by checking the version *$ snowsql -v*.
- Reboot your machine and check again if necessary.

After completing these steps, you'll be ready to use SnowSQL to make a database in the next section.

## Sign in From the Terminal

```bash
snowsql -a <account-name> -u <username>
```

for me it's

```bash
snowsql -a mz45511.eu-west-3.aws -u laurentmorelli
```


The -a flag represents the Snowflake account, and the -u represents the username.

## Create a Database and Schema

```bash
create or replace database sf_tuts;
```

The command ​create or replace database​ makes a new database and auto-creates the schema ‘public.' 
It'll also make the new database active for your current session.

To check which database is in use for your current session, execute:

```
select current_database(),
current_schema();
```

## Generate a Table
```
create or replace table emp_basic (
  first_name string ,
  last_name string ,
  email string ,
  streetaddress string ,
  city string ,
  start_date date
  );
```
Running ​create or replace table​ will build a new table based on the parameters specified. 

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/dda90feea56c46d7.png)

## Make a Virtual Warehouse
```
create or replace warehouse sf_tuts_wh with
  warehouse_size='X-SMALL'
  auto_suspend = 180
  auto_resume = true
  initially_suspended=true;
```
After creation, this virtual warehouse will be active for your current session and begin running once the computing resources are needed.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/930e5dd2da922ec0.png)

With the database objects ready, you'll employ SnowSQL to move the sample data onto the *emp_basic* table.

## Stage file with PUT 

Let's now download some test data and upload it directly in our new table

Download files from [here](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/snowsqlfiles.zip)

Let's load everything with a PUT command in SnowSql

Linux
```bash
put file:///tmp/employees0*.csv @<database-name>.<schema-name>.%<table-name>;
```

Windows
```
put file://c:\temp\employees0*.csv @sf_tuts.public.%emp_basic;
```

- *file* specifies the local file path of the files to be staged. File paths are OS-specific.
- *@..%* is the specific database, schema, and table the staged files are headed.
- The *@* sign before the database and schema name *@sf_tuts.public* indicates that the files are being uploaded to an internal stage, rather than an external stage. The % sign before the table name %emp_basic indicates that the internal stage being used is the stage for the table. For more details about stages, see Staging Data Files from a Local File System.

```
put file:///tmp/employees0*.csv @sf_tuts.public.%emp_basic;
```

Here is a PUT call to stage the sample employee CSV files from a macOS file:///tmp/ folder onto the emp_basic table within the sf_tuts database.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/ce56069ad9a9bf77.png)

## LIST Staged Files
```
list @<database-name>.<schema-name>.%<table-name>;
```
To check your staged files, run the list command.

```
list @sf_tuts.public.%emp_basic;
```
The example command above is to output the staged files for the emp_basic table. Learn more LIST syntax ​here​.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/4ed42bd3b64a7169.png)

## COPY INTO​ Your Table

```
copy into emp_basic
  from @%emp_basic
  file_format = (type = csv field_optionally_enclosed_by='"')
  pattern = '.*employees0[1-5].csv.gz'
  on_error = 'skip_file';
```

After getting the files staged, the data is copied into the emp_basic table. 
This DML command also auto-resumes the virtual warehouse made earlier.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/457c26b14bf0db50.png)

The output indicates if the data was successfully copied and records any errors.


## Query Data

With your data in the cloud, you need to know how to query it. 
We'll go over a few calls that will put your data on speed-dial.

- The select command followed by the wildcard * returns all rows and columns in .

```
select * from emp_basic;
```

Here is an example command to *select* everything on the *emp_basic* table.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/f34cdf802f91b44b.png)

Sifting through everything on your table may not be the best use of your time. Getting specific results are simple, with a few functions and some query syntax.

- WHERE​ is an additional clause you can add to your select query.

```
select * from emp_basic where first_name = 'Ron';
```

This query returns a list of employees by the first_name of ‘Ron' from the *emp_basic* table.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/b295a8589ff63ae6.png)

- LIKE​ function supports wildcard % and _.

```
select email from emp_basic where email like '%.au';
```

The like function checks all emails in the emp_basic table for au and returns a record.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/44c619e803c4427f.png)

Snowflake supports many [​functions​](https://docs.snowflake.com/en/sql-reference-functions.html), ​[operators​](https://docs.snowflake.com/en/sql-reference/operators.html), and [​commands​](https://docs.snowflake.com/en/sql-reference-commands.html). However, if you need more specific tasks performed, consider setting up an ​[external function​​](https://docs.snowflake.com/en/sql-reference/external-functions-introduction.html).

## Manage and Delete Data

Often data isn't static. We'll review a few common ways to maintain your cloud database.

If HR updated the CSV file after hiring another employee, downloading, staging, and copying the whole CSV would be tedious. Instead, simply insert the new employee information into the targeted table.

### Insert Data

- ​INSERT​ will update a table with additional values.

```
insert into emp_basic values
  ('Clementine','Adamou','cadamou@sf_tuts.com','10510 Sachs Road','Klenak','2017-9-22') ,
  ('Marlowe','De Anesy','madamouc@sf_tuts.co.uk','36768 Northfield Plaza','Fangshan','2017-1-26');
```

### Drop Objects

In the command displayed, insert is used to add two new employees to the emp_basic table.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/be91ef9c2a92e5bd.png)

- DROP​ objects no longer in use.

```
drop database if exists sf_tuts;

drop warehouse if exists sf_tuts_wh;
```

After practicing the basics covered here, you'll no longer need the sf-tuts database and warehouse. To remove them altogether, use the drop command.

- Close Your Connection with *!exit* or *!disconnect*

For security reasons, it's best not to leave your terminal connection open unnecessarily. 
Once you're ready to close your SnowSQL connection, simply enter *!exit*.


## Resources 📚📚

* Learn more about Snowflake's SnowSql thanks to [Snowflake Documentation](https://docs.snowflake.com/en/user-guide/snowsql)
