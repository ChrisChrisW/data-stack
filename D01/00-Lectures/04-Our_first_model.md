# Our first dbt Model

## What are Models ? 🧐

In analytics the process of modeling is the process of shaping the data from raw data through to your final transformed data. 

Typically Data Engineers are responsible for building tables that represent the source data, and on top of that building tables and views that transform the data step by step.

Eventually we build the final table that BI tools will be able to query across your organization.

In dbt, models are just sql select statements - each of these represent one modular piece of logic that will slowly take your raw data to build it into the final transform model.

These models are stored in dbt in the models folder in sql files.

When built, each model will have a one to one relationship with a table or view in the data warehouse - there are some materialization where it’s not the case but will cover that later so we can keep this assumption for the moment.

One of the advantage of dbt is that you don’t need to create the ddl or dml specific to a table or a view, you can just specify at the top of you sql file how you want the model to be represented.

This helps you focusing on writing the business logic and dbt will take care of the rest.

## Building our first Model in dbt

Let's create a file **dim_customers.sql** in our model folder with the following sql logic

```
with customers as (
    select
        id as customer_id,
        first_name,
        last_name
    from raw.jaffle_shop.customers

),

orders as (
    select
        id as order_id,
        user_id as customer_id,
        order_date,
        status
    from raw.jaffle_shop.orders

),

customer_orders as (
    select
        customer_id,
        min(order_date) as first_order_date,
        max(order_date) as most_recent_order_date,
        count(order_id) as number_of_orders
    from orders
    group by 1
),


final as (
    select
        customers.customer_id,
        customers.first_name,
        customers.last_name,
        customer_orders.first_order_date,
        customer_orders.most_recent_order_date,
        coalesce(customer_orders.number_of_orders, 0) as number_of_orders
    from customers
    left join customer_orders using (customer_id)
)

select * from final
```

Let's create the underlying structure in our datawarehouse by running
```
dbt run --select dim_customers
```

- Let's take some time to check the output and see the structure created within Snowflake

- As we can see, a view has been created, which means that the query will be computed every time a user access it.

- Let's change the materialization by adding the following to the beginning of the file
```
{{ config (
    materialized="table"
)}}
```
- Let's run again dbt run and check the impact in Snowflake..

Congratulations! You have successfully build your first model !

## Introduction to modularity

In order to reuse and simplify our code, we will break our previous model into three models and call them from one another

Let's create three new files with the following names and content

**stg_customers.sql**

```
with customers as (
    select 
        id as customer_id,
        first_name,
        last_name
    from raw.jaffle_shop.customers
)

select * from customers
```

**stg_orders.sql**
```
with orders as (
    
    select
        id as order_id,
        user_id as customer_id,
        order_date,
        status

    from raw.jaffle_shop.orders
)

select * from orders
```
**dim_customers.sql**
```
with customers as (
    select * from {{ ref('stg_customers')}}
),

orders as (
    select * from {{ ref('stg_orders') }}
),

customer_orders as (
    select
        customer_id,
        min(order_date) as first_order_date,
        max(order_date) as most_recent_order_date,
        count(order_id) as number_of_orders
    from orders
    group by 1
),


final as (
    select
        customers.customer_id,
        customers.first_name,
        customers.last_name,
        customer_orders.first_order_date,
        customer_orders.most_recent_order_date,
        coalesce(customer_orders.number_of_orders, 0) as number_of_orders
    from customers
    left join customer_orders using (customer_id)
)

select * from final
```

Let's run *dbt run* to have everything updated

That looks so much better !  👏👏👏 🎉

Before we carry on let's discuss the main concepts we are starting to cover
