# Practice : Bringing sources to your project

Now that we have a good overview of source's potential, let's strengthen our project ! 💪💪💪

## Configure sources
- Configure a source for the tables raw.jaffle_shop.customers and raw.jaffle_shop.orders in a file called src_jaffle_shop.yml.
**models/staging/jaffle_shop/src_jaffle_shop.yml**


sources:
  - name: jaffle_shop
    database: raw
    schema: jaffle_shop
    tables:
      - name: customers
      - name: orders
```
- Configure a source for the table raw.stripe.payment in a file called `src_stripe.yml`.


## Refactor staging models

- Refactor stg_customers.sql using the source function.

**models/staging/jaffle_shop/stg_customers.sql**


- Refactor stg_orders.sql using the source function.

**models/staging/jaffle_shop/stg_orders.sql**

```
select
    id as order_id,
    user_id as customer_id,
    order_date,
    status
from {{ source('jaffle_shop', 'orders') }}
```
- Refactor stg_payments.sql using the source function.

## Going further
- Configure your Stripe payments data to check for source freshness.
- Run dbt source freshness.

