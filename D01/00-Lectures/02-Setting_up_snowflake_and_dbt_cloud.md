
# Set up and connect Snowflake ❄️ to dbt cloud

## Introduction​
Using dbt implies using a data platform - in this course we will be using Snowflake.
So first let's get started with setting up a Snowflake DB ❄️ !

This guide will teach you on how to to set up Snowflake and connect it to dbt Cloud by walking you through:

- Setting up a Snowflake trial account
- Loading training data into your Snowflake account
- Creating a dbt Cloud account through the account flow
- Connecting dbt Cloud and Snowflake ❄️
- Setting up the dbt Cloud IDE, querying data, and doing your first dbt run


## Prerequisites​

The only prerequisites for this guide are to have access to an email account for signing up for Snowflake and dbt Cloud.

## Setting up​

You can start by signing up for a free trial on Snowflake ❄️:

1 . Sign up for a free trial by following this [link](https://signup.snowflake.com/) and completing the sign-up form.

2 . Select the Enterprise edition, choose a cloud provider and region, and agree to the terms of service.

You should consider organizational questions when choosing a cloud provider for a full implementation. For more information, see [Introduction to Cloud Platforms](https://docs.snowflake.com/en/user-guide/intro-cloud-platforms.html) in the Snowflake docs. 
For the purposes of this setup, all cloud providers and regions will work so choose whichever you’d like - I go aws.

Click GET STARTED.

![pic](http://www.welcome.paprika.tech/dbtpic//snowflakeedition.png)

3 . After submitting the sign-up form, you should receive an email asking you to activate your account. 
    Click the link in the email and a new tab will open up where you’ll create your username and password. 
    Complete the form and click Get started.

![pic](http://www.welcome.paprika.tech/dbtpic//welcometosnowflake.png)


Congrats! Your workspace is ready for some data !

![pic](http://www.welcome.paprika.tech/dbtpic//snowflakeui.png)

## Loading data

Now we’re ready for some sample data. 
The data used here is stored as CSV files in a public S3 bucket and the following steps will guide you through how to prepare your Snowflake account for that data and upload it.

If using the new Snowflake UI, create a new worksheet by clicking the "+ Worksheet" button in the upper right hand corner of the screen.

![pic](http://www.welcome.paprika.tech/dbtpic//newworksheet.png)
Run the following commands to create a new virtual warehouse, two new databases (one for raw data, the other for future dbt development), and two new schemas (one for jaffle_shop data, the other for 'stripe' data)

Feel free to copy/paste from below:

```
create warehouse transforming;

create database raw;

create database analytics;

create schema raw.jaffle_shop;

create schema raw.stripe;
```

![pic](http://www.welcome.paprika.tech/dbtpic//createwarehouse.png)

Our next step will focus on creating three raw tables in the raw database and jaffle_shop and stripe schemas. Execute the tabbed code snippets below to create the customers, orders, and payment table and load the respective data.

Let's start with the customers
```
​​create table raw.jaffle_shop.customers
 ( id integer,
  first_name varchar,
  last_name varchar
);

copy into raw.jaffle_shop.customers (id, first_name, last_name)
from 's3://dbt-tutorial-public/jaffle_shop_customers.csv'
file_format = (
  type = 'CSV'
  field_delimiter = ','
  skip_header = 1
  );
```


Now the orders
```
create table raw.jaffle_shop.orders
( id integer,
  user_id integer,
  order_date date,
  status varchar,
  _etl_loaded_at timestamp default current_timestamp
);

copy into raw.jaffle_shop.orders (id, user_id, order_date, status)
from 's3://dbt-tutorial-public/jaffle_shop_orders.csv'
file_format = (
  type = 'CSV'
  field_delimiter = ','
  skip_header = 1
  );
```


And now the payments
```
create table raw.stripe.payment 
( id integer,
  orderid integer,
  paymentmethod varchar,
  status varchar,
  amount integer,
  created date,
  _batched_at timestamp default current_timestamp
);

copy into raw.stripe.payment (id, orderid, paymentmethod, status, amount, created)
from 's3://dbt-tutorial-public/stripe_payments.csv'
file_format = (
  type = 'CSV'
  field_delimiter = ','
  skip_header = 1
  );
```



Great! Your data is loaded and ready to go. 
Just to make sure, run the following commands to query your data and confirm that you see an output for each one.
```
select * from raw.jaffle_shop.customers;

select * from raw.jaffle_shop.orders;

select * from raw.stripe.payment;
```


Now we’re ready to set up dbt Cloud!


## Connecting to dbt Cloud​

### Create a dbt Cloud account​
Let's start this section by creating a dbt Cloud account if you haven't already.
1. Navigate to [dbt Cloud](https://cloud.getdbt.com/).
2. If you don't have a dbt Cloud account, create a new one, and verify your account via email.
3. If you already have a dbt Cloud account, you can create a new project from your existing account:
    - Click the gear icon in the top-right, then click Projects.
    - Click + New Project.
4. You've arrived at the "Setup a New Project" page.
5. Type "Analytics" in the dbt Project Name field. You will be able to rename this project later.
6. Click Continue.

### Connect dbt Cloud to Snowflake​

Now let's formally set up the connection between dbt Cloud and Snowflake.
1. Choose Snowflake ❄️ to setup your connection.

![pic](http://www.welcome.paprika.tech/dbtpic//dbt_cloud_setup_snowflake_connection_start.png)

2. For the name, write Snowflake or another simple title.
3. Enter the following information under Snowflake settings.
    - **Account**: Find your account by using the Snowflake trial account URL and removing snowflakecomputing.com. 

        The order of your account information will vary by Snowflake version. 
        For example, Snowflake's Classic console URL might look like: oq65696.west-us-2.azure.snowflakecomputing.com. 
        The AppUI or Snowsight URL might look more like: snowflakecomputing.com/west-us-2.azure/oq65696. 
        In both examples, your account will be: oq65696.west-us-2.azure. 
        For more information, see ["Account Identifiers"](https://docs.snowflake.com/en/user-guide/admin-account-identifier.html) in the Snowflake documentation.

        ✅ db5261993 or db5261993.east-us-2.azure

        ❌ db5261993.eu-central-1.snowflakecomputing.com


    - **Role**: Leave blank for now. 
        
        You can update this to a default Snowflake role in the future.
    - **Database**: analytics. 
        
        This tells dbt to create new models in the analytics database.
    - **Warehouse**: transforming. 
        
        This tells dbt to use the transforming warehouse we created earlier.

![pic](http://www.welcome.paprika.tech/dbtpic//dbt_cloud_snowflake_account_settings.png)

4. Enter the following information under Development credentials.
    - **Username**: The username you created for Snowflake. 
        
        Note: The username is not your email address and is usually your first and last name together in one word.
    - **Password**: The password you set when creating your Snowflake account
    - **Schema**: You’ll notice that the schema name has been auto created for you. 
        
        By convention, this is dbt_first-initiallast-name. 
        This is the schema connected directly to your development environment, and it's where your models will be built when running dbt within the Cloud IDE.
    - **Target name**: leave as default
    - **Threads**: Leave as 4. 
        
        This is the number of simultaneous connects that dbt Cloud will make to build models concurrently.

![pic](http://www.welcome.paprika.tech/dbtpic//dbt_cloud_snowflake_development_credentials.png)

5. Click Test Connection at the bottom. 

This verifies that dbt Cloud can access your Snowflake account.

If the connection test succeeds, click **Next**. 

If it fails, you may need to check your Snowflake settings and credentials.

## Initialize your repository and start development
When you develop in dbt Cloud, you can leverage Git to version control your code.

To connect to a repository, you can either set up a dbt Cloud-hosted managed repository or directly connect to a supported git provider. 

Managed repositories are a great way to trial dbt without needing to create a new repository. 
In the long run, it's better to connect to a supported git provider to use features like automation and continuous integration.

If you used Partner Connect, you can skip over to initializing your dbt project as the Partner Connect sets you up with a managed repository already. 
If not, you will need to create your repository connection.

### Setting up a managed repository
To set up a managed repository:

1. Under "Add repository from", select **Managed**.
2. Type a name for your repo such as lmorelli-dbt-jedha
3. Click Create. It will take a few seconds for your repository to be created and imported.
4. Once you see the "Successfully imported repository," click Continue.

### Initialize your dbt project
Now that you have a repository configured, you can initialize your project and start development in dbt Cloud:

1. Click Develop from the upper left. 

It might take a few minutes for your project to spin up for the first time as it establishes your git connection, clones your repo, and tests the connection to the warehouse.
2. Above the file tree to the left, click Initialize your project. 

This builds out your folder structure with example models.
3. Make your initial commit by clicking Commit. 

Use the commit message initial commit. 
This creates the first commit to your managed repo and allows you to open a branch where you can add new dbt code.
4. Now you should be able to directly query data from your warehouse and execute dbt run. 

Paste your following warehouse-specific code in the IDE:
```
select * from raw.jaffle_shop.customers
```
In the command line bar at the bottom, type dbt run and click Enter. 

Congratulations!👏 🎉🎉🎉🎉 You have successfully completed the following:

- Set up a new Snowflake instance
- Loaded training data into your Snowflake account
- Connected dbt Cloud and Snowflake
