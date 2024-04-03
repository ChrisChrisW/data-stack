# Practice : Documenting your project

Using the resources in this module, complete the following in your project 

## Write documentation
- Add documentation to the file models/staging/jaffle_shop/stg_jaffle_shop.yml.
- Add a description for your stg_customers model and the column customer_id.
- Add a description for your stg_orders model and the column order_id.
## Create a reference to a doc block
- Create a doc block for your stg_orders model to document the status column.
- Reference this doc block in the description of status in stg_orders.

**models/staging/jaffle_shop/stg_jaffle_shop.yml**
```
version: 2

models:
  - name: stg_customers
    description: Staged customer data from our jaffle shop app.
    columns: 
      - name: customer_id
        description: The primary key for customers.
        tests:
          - unique
          - not_null

  - name: stg_orders
    description: Staged order data from our jaffle shop app.
    columns: 
      - name: order_id
        description: Primary key for orders.
        tests:
          - unique
          - not_null
      - name: status
        description: "{{ doc('order_status') }}"
        tests:
          - accepted_values:
              values:
                - completed
                - shipped
                - returned
                - placed
                - return_pending
      - name: customer_id
        description: Foreign key to stg_customers.customer_id.
        tests:
          - relationships:
              to: ref('stg_customers')
              field: customer_id
```
**models/staging/jaffle_shop/src_jaffle_shop.yml**
```
version: 2

sources:
  - name: jaffle_shop
    description: A clone of a Postgres application database.
    database: raw
    schema: jaffle_shop
    tables:
      - name: customers
        description: Raw customers data.
        columns:
          - name: id
            description: Primary key for customers.
            tests:
              - unique
              - not_null

      - name: orders
        description: Raw orders data.
        columns:
          - name: id
            description: Primary key for orders.
            tests:
              - unique
              - not_null
        loaded_at_field: _etl_loaded_at
        freshness:
          warn_after: {count: 12, period: hour}
          error_after: {count: 24, period: hour}
```
**models/staging/jaffle_shop/jaffle_shop.md**
```
{% docs order_status %}
	
One of the following values: 

| status         | definition                                       |
|----------------|--------------------------------------------------|
| placed         | Order placed, not yet shipped                    |
| shipped        | Order has been shipped, not yet been delivered   |
| completed      | Order has been received by customers             |
| return pending | Customer indicated they want to return this item |
| returned       | Item has been returned                           |

{% enddocs %}
```
## Generate and view documentation
- Generate the documentation by running dbt docs generate.
- View the documentation that you wrote for the stg_orders model.
- View the Lineage Graph for your project.

## Going further
- Add documentation to the other columns in stg_customers and stg_orders.
- Add documentation to the stg_payments model.
- Create a doc block for another place in your project and generate this in your documentation.