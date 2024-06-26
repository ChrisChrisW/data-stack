# Practice : Analytics modeling on our first model

Let's complete the following in your dbt project 💪

## Quick Project Polishing
- In your dbt_project.yml file, change the name of your project from my_new_project to jaffle_shop (line 5 AND 35)

## Staging Models
- Create a *staging/jaffle_shop* directory in your models folder.
- Create a *stg_customers.sql* model for *raw.jaffle_shop.customers*
```
select
    id as customer_id,
    first_name,
    last_name

from raw.jaffle_shop.customers
```
- Create a stg_orders.sql model for raw.jaffle_shop.orders

## Mart Models

- Create a marts/core directory in your models folder.
- Create a dim_customers.sql model
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
## Configure your materializations
- In your dbt_project.yml file, configure the staging directory to be materialized as views.
```
models:
  jaffle_shop:
    staging:
      +materialized: view
```
- In your dbt_project.yml file, configure the marts directory to be materialized as tables.
```
models:
  jaffle_shop:
  ...
    marts:
      +materialized: table
```
## Building a fct_orders Model

- Use a statement tab or Snowflake to inspect *raw.stripe.payment*
- Create a *stg_payments.sql* model in *models/staging/stripe*
- Create a *fct_orders.sql* (not *stg_orders*) model with the following fields.  Place this in the marts/core directory.
    - order_id
    - customer_id
    - amount (hint: this has to come from payments)

## Refactor your dim_customers Model
- Add a new field called lifetime_value to the dim_customers model:
    - lifetime_value: the total amount a customer has spent at jaffle_shop
    - Hint: The sum of lifetime_value is $1,672