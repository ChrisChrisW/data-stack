# Sources

## What you will learn in this course? 🧐🧐
* What are sources
* Why do we need to have them in a separated configuration
* How dbt handles it


Let's discuss the idea of sources and what it will bring in your analytic workflow 🧐

In your analytics Engineering workflow you will always going to start with raw data, from tables that are somehow brought into your warehouse or data platform through whatever means that are necessary.

So those raw tables we’ll just call them **sources** for now.

And let’s consider this : in our model so far we’ve been referring to thoses sources using a direct string like *raw.stripe.payment* - that works but consider what happens if we reconfigure our EL tool to instead put that in a different schema or change the name slightly to follow our style guide. Then wherever you have referenced those raw tables in your model you now have to swap that out manually and that could be super tedious and time consuming while you could be doing something more valuable.

So this is where Sources comes into play : sources in dbt allow you to document those raw tables that have been broaden by your EL tool and put those in a yaml file where you reference the database their in, their schema and their name - you can even put an alias that stands for the source in the yaml file.

Then in your model instead of writing something like *raw.stipe.payment* you can use the source function. So we might pass something like *{{source(‘stripe’,’payments’)}}* and that is creating a direct reference to that yaml file that we’ve created earlier.

And when we run dbt compile or we compile our code, dbt is going to replace the source function with the direct table reference.

So when someone working in the EL part of the pipeline changes something slightly, you can go on the yaml file and tweak it really quick and everything is back up and running.

The other benefit of sources is that they will now show up in you lineage. Before configuring sources you will have only blue node in your dag, but with sources you now have green node that show you the full orchestration of data transformation from raw data all the way to those final tables.


## Sources in a nutshell
- Sources represent the raw data that is loaded into the data warehouse.
- We *can* reference tables in our models with an explicit table name *(raw.jaffle_shop.customers)*.
- However, setting up Sources in dbt and referring to them with the source function enables a few important tools.
    - Multiple tables from a single source can be configured in one place.
    - Sources are easily identified as green nodes in the Lineage Graph.
    - You can use dbt source freshness to check the freshness of raw tables.

## Configuring sources

- Sources are configured in YML files in the models directory.
- The following code block configures the table raw.jaffle_shop.customers and raw.jaffle_shop.orders:

```
version: 2

sources:
  - name: jaffle_shop
    database: raw
    schema: jaffle_shop
    tables:
      - name: customers
      - name: orders
```

## Source function

- The ref function is used to build dependencies between models.
- Similarly, the source function is used to build the dependency of one model to a source.
- Given the source configuration above, the snippet *{{ source('jaffle_shop','customers') }}* in a model file will compile to *raw.jaffle_shop.customers*.
- The Lineage Graph will represent the sources in green.

![pic](http://www.welcome.paprika.tech/dbtpic//DAG_sources.webp)

## Source freshness
- Freshness thresholds can be set in the YML file where sources are configured. For each table, the keys loaded_at_field and freshness must be configured.
```
version: 2

sources:
  - name: jaffle_shop
    database: raw
    schema: jaffle_shop
    tables:
      - name: orders
        loaded_at_field: _etl_loaded_at
        freshness:
          warn_after: {count: 12, period: hour}
          error_after: {count: 24, period: hour}
```
- A threshold can be configured for giving a warning and an error with the keys warn_after and error_after.
- The freshness of sources can then be determined with the command dbt source freshness.



## Resources 📚📚
* [How to use sources in dbt](https://theinformationlab.nl/en/2022/04/08/hoe-gebruik-je-sources-in-dbt/)
* [Everything on sources](https://docs.getdbt.com/docs/build/sources)
