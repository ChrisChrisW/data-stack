# Hands on Snowflake - part 3

Welcome to this course which purpose is to give you a nearly complete overview of Snowflake as a DataLakeHouse by using it - you can be independant on all its content

This is the third part, where we are covering the roles, privileges, sharing data and accessing public data

We are using the dataset from the first part, so don't forget to load the data as presented in the first part before starting this course

Alright !

## What you'll learn in this course 🧐🧐

* How to create roles and users, and grant them privileges.
* How to securely and easily share data with other accounts.
* How to consume datasets in the Snowflake Data Marketplace.

Let's go!

## Working with Roles, Account Admin, & Account Usage

In this section, we will explore aspects of Snowflake's access control security model, such as creating a role and granting it specific permissions.
We will also explore other usage of the *ACCOUNTADMIN* (Account Administrator) role, which was briefly introduced earlier in the lab.

Continuing with the lab story, let's assume a junior DBA has joined Citi Bike and we want to create a new role for them with less privileges than the system-defined, default role of SYSADMIN.

:::note Role-Based Access Control 

Snowflake offers very powerful and granular access control that dictates the objects and functionality a user can access, as well as the level of access they have.

For more details, check out the Snowflake documentation [here](https://docs.snowflake.net/manuals/user-guide/security-access-control.html).
:::

### Create a New Role and Add a User

In the *CITIBIKE_ZERO_TO_SNOWFLAKE* worksheet, switch to the ACCOUNTADMIN role to create a new role.
ACCOUNTADMIN encapsulates the SYSADMIN and SECURITYADMIN system-defined roles.
It is the top-level role in the account and should be granted only to a limited number of users.

In the *CITIBIKE_ZERO_TO_SNOWFLAKE* worksheet, run the following command:
```sql
use role accountadmin;
```
Notice that, in the top right of the worksheet, the context has changed to ACCOUNTADMIN:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/1ce33af499221e4e.png)

Before a role can be used for access control, at least one user must be assigned to it.
So let's create a new role named *JUNIOR_DBA* and assign it to your Snowflake user.

To complete this task, you need to know your username, which is the name you used to log in to the UI.

Use the following commands to create the role and assign it to you.

Before you run the GRANT ROLE command, replace YOUR_USERNAME_GOES_HERE with your username:
```sql 
create role junior_dba;

grant role junior_dba to user YOUR_USERNAME_GOES_HERE;
```

:::note 

If you try to perform this operation while in a role such as SYSADMIN, it would fail due to insufficient privileges.

By default (and design), the SYSADMIN role cannot create new roles or users.
:::

Change your worksheet context to the new *JUNIOR_DBA* role:
```sql
use role junior_dba;
```
In the top right of the worksheet, notice that the context has changed to reflect the *JUNIOR_DBA* role.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/575ebcf012a2eb20.png)

Also, the warehouse is not selected because the newly created role does not have usage privileges on any warehouse.


Let's fix it by switching back to *ADMIN* role and grant usage privileges to *COMPUTE_WH* warehouse.

```sql
use role accountadmin;

grant usage on warehouse compute_wh to role junior_dba;
```

Switch back to the *JUNIOR_DBA* role.
You should be able to use *COMPUTE_WH* now.

```sql
use role junior_dba;

use warehouse compute_wh;
```

Finally, you can notice that in the database object browser panel on the left, the *CITIBIKE* and *WEATHER* databases no longer appear.
This is because the *JUNIOR_DBA* role does not have privileges to access them.

Switch back to the *ACCOUNTADMIN* role and grant the *JUNIOR_DBA* the *USAGE* privilege required to view and use the *CITIBIKE* and *WEATHER* databases:

```sql
use role accountadmin;

grant usage on database citibike to role junior_dba;

grant usage on database weather to role junior_dba;
```

Switch to the *JUNIOR_DBA* role:

```sql
use role junior_dba;
```

Notice that the *CITIBIKE* and *WEATHER* databases now appear in the database object browser panel on the left.

If they don't appear, try clicking ...

in the panel, then clicking **Refresh**.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/ca8ad7b1ccd3299e.png)

### View the Account Administrator UI

Let's change our access control role back to *ACCOUNTADMIN* to see other areas of the UI accessible only to this role.
However, to perform this task, use the UI instead of the worksheet.

First, click the **Home** icon in the top left corner of the worksheet.

Then, in the top left corner of the UI, click your name to display the user preferences menu.

In the menu, go to **Switch Role** and select *ACCOUNTADMIN*.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/214666277b661dea.png)

:::note Roles in User Preference vs Worksheet 

Why did we use the user preference menu to change the role instead of the worksheet?

The UI session and each worksheet have their own separate roles.

The UI session role controls the elements you can see and access in the UI, whereas the worksheet role controls only the objects and actions you can access within the role.

:::

Notice that once you switch the UI session to the *ACCOUNTADMIN* role, new tabs are available under **Admin**.

**Usage**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/8d884fa261e50a8a.png)

The *Usage* tab shows the following, each with their own page:

* **Organization**: Credit usage across all the accounts in your organization.
* **Consumption**: Credits consumed by the virtual warehouses in the current account.
* **Storage**: Average amount of data stored in all databases, internal stages, and Snowflake Failsafe in the current account for the past month.
* **Transfers**: Average amount of data transferred out of the region (for the current account) into other regions for the past month.

The filters in the top right corner of each page can be used to break down the usage/consumption/etc.
visualizations by different measures.

**Security**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/7120e6f745df5b1a.png)

The **Security** tab contains network policies created for the Snowflake account.

New network policies can be created by selecting **"+ Network Policy"** at the top right hand side of the page.

**Billing**

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/f0aa505b50d2f777.png)

The **Billing** tab contains the payment method for the account:

* If you are a Snowflake contract customer, the tab shows the name associated with your contract information.
* If you are an on-demand Snowflake customer, the tab shows the credit card used to pay month-to-month, if one has been entered.
If no credit card is on file, you can add one to continue using Snowflake when your trial ends.

For the next section, stay in the ACCOUNTADMIN role for the UI session.

## Sharing Data Securely & the Data Marketplace

Snowflake enables data access between accounts through the secure data sharing features.

Shares are created by data providers and imported by data consumers, either through their own Snowflake account or a provisioned Snowflake Reader account.

The consumer can be an external entity or a different internal business unit that is required to have its own unique Snowflake account.

With secure data sharing:
* There is only one copy of the data that lives in the data provider's account.
* Shared data is always live, real-time, and immediately available to consumers.
* Providers can establish revocable, fine-grained access to shares.
* Data sharing is simple and safe, especially compared to older data sharing methods, which were often manual and insecure, such as transferring large *.csv* files across the internet.

:::note
To share data across regions or cloud platforms, you must set up replication.

This is outside the scope of this lab, but more information is available [here](https://www.snowflake.com/trending/what-is-data-replication)
:::

Snowflake uses secure data sharing to provide account usage data and sample data sets to all Snowflake accounts.

In this capacity, Snowflake acts as the data provider of the data and all other accounts.

Secure data sharing also powers the Snowflake Data Marketplace, which is available to all Snowflake customers and allows you to discover and access third-party datasets from numerous data providers and SaaS vendors.
Again, in this data sharing model, the data doesn't leave the provider's account and you can use the datasets without any transformation.
Let's take a quick look on that...


### View Existing Shares
In the home page, navigate to **Data > Databases**.
In the list of databases, look at the **SOURCE** column.

You should see two databases with *Local* in the column.

These are the two databases we created previously in this session.
The other database, *SNOWFLAKE*, shows Share in the column, indicating it's shared from a provider.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/9cca7e7a65947747.png)

### Create an Outbound Share
Let's go back to the Citi Bike story and assume we are the Account Administrator for Snowflake at Citi Bike.

We have a trusted partner who wants to analyze the data in our *TRIPS* database on a near real-time basis.

This partner also has their own Snowflake account in the same region as our account.

So let's use secure data sharing to allow them to access this information.


Navigate to **Data > Private Sharing**, then at the top of the tab click **Shared by My Account**.

Click the **Share** button in the top right corner and select **Create a Direct Share**:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/cd89c2f5d7184298.png)

Click **+ Select Data** and navigate to the *CITIBIKE* database and *PUBLIC* schema.
Select the 2 tables we created in the schema and click the **Done** button:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/11826cb562ca6ac2.png)

The default name of the share is a generic name with a random numeric value appended.

Edit the default name to a more descriptive value that will help identify the share in the future (eg. *ZERO_TO_SNOWFLAKE_SHARED_DATA* ).

You can also add a comment.

In a real-world scenario, the Citi Bike Account Administrator would next add one or more consumer accounts to the share, but we'll stop here for the purposes of this exercise.

Click the **Create Share** button at the bottom of the dialog:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/9a779766bb0908b6.png)

The dialog closes and the page shows the secure share you created:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/1054c2298f427b81.png)

You can add consumers, add/change the description, and edit the objects in the share at any time.
In the page, click the **<** button next to the share name to return to the **Share with Other Accounts** page:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/7ef3b0f4e0abcfed.png)

Alright ! As you've seen, it only takes seconds to give other accounts access to data in your Snowflake account in a secure manner with no copying or transferring of data required!

Snowflake provides several ways to securely share data without compromising confidentiality.

In addition to tables, you can share secure views, secure UDFs (user-defined functions), and other secure objects.

For more details about using these methods to share data while preventing access to sensitive information, see the [Snowflake documentation](https://docs.snowflake.com/en/user-guide/data-sharing-secure-views.html).


Alright let's now take a small look on the data available in the marketplace.

### Snowflake Data Marketplace

Make sure you're using the *ACCOUNTADMIN* role and, navigate to the **Marketplace**:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/f68187f997fbab13.png)

**Find a listing**

The search box at the top allows you to search for a listings.
The drop-down lists to the right of the search box let you filter data listings by Provider, Business Needs, and Category.

Type *COVID* in the search box, scroll through the results, and select **COVID-19 Epidemiological Data** (provided by Starschema).

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/65eaa2ae94859dda.png)

In the **COVID-19 Epidemiological Data** page, you can learn more about the dataset and see some usage example queries.
When you're ready, click the **Get** button to make this information available within your Snowflake account:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/bef8d41daf4d9db5.png)

Review the information in the dialog and click **Get** again:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/972d3855489076f5.png)

You can now click **Done** or choose to run the sample queries provided by Starschema:

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/d145f0c4d17fd68f.png)

If you chose **Open**, a new worksheet opens in a new browser tab/window:

1. Set your context
2. Select the query you want to run (or place your cursor in the query text).
3. Click the **Run/Play** button (or use the keyboard shortcut).
4. You can view the data results in the bottom pane.
5. When you are done running the sample queries, click the **Home** icon in the upper left corner.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/9cde374afcc7c1c8.png)

Next:

1. Click **Data > Databases**
2. Click the *COVID19_BY_STARSCHEMA_DM* database.
3. You can see details about the schemas, tables, and views that are available to query.

![pic](https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M08-ETL_and_datawarehousing/snowflake/64d8b592cac65f81.png)

That's it! You have now successfully subscribed to the COVID-19 dataset from Starschema, which is updated daily with global COVID data.
Notice we didn't have to create databases, tables, views, or an ETL process.
We simply searched for and accessed shared data from the Snowflake Data Marketplace.
 👏👏👏 🎉

## Resetting Your Snowflake Environment
If you would like to reset your environment by deleting all the objects created as part of this lab, run the SQL statements in a worksheet.

First, ensure you are using the ACCOUNTADMIN role in the worksheet:
```sql
use role accountadmin;
```
Then, run the following SQL commands to drop all the objects we created in the lab:
```sql
drop share if exists zero_to_snowflake_shared_data;
-- If necessary, replace "zero_to_snowflake-shared_data" with the name you used for the share

drop database if exists citibike;

drop database if exists weather;

drop warehouse if exists analytics_wh;

drop role if exists junior_dba;
```

Congratulations on completing this exercise!  👏👏👏 🎉
You've mastered the Snowflake basics and are ready to apply these fundamentals to your own data.


## Resources 📚📚

* Learn more about the [Snowsight](https://docs.snowflake.com/en/user-guide/ui-snowsight.html#using-snowsight) docs.
* Read the [Definitive Guide to Maximizing Your Free Trial](https://www.snowflake.com/test-driving-snowflake-the-definitive-guide-to-maximizing-your-free-trial/) document.
* [HANDS-ON LAB GUIDE FOR VIRTUAL ZERO-TOSNOWFLAKE](https://s3.amazonaws.com/snowflake-workshop-lab/OnlineZTS_LabGuide.pdf)