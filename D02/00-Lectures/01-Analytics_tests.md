# Testing your analytics


## What you will learn in this course? 🧐🧐
* What is analytic testing
* How dbt handles it

Testing in analytic engineering is about verifying assumptions that you have on your data.

It’s critical that your assumptions are true so that whatever decisions are being made on those final models can be trusted, and people have the confidence that the data they are looking at is in fact liable.

You are likely already doing this in your data career - it might be exporting a csv, opening a file in excel and creating a check column looking that values are correct or present in vlookups, or checking that all your records are well unique and matching.

That thinking, that testing mindset is crucial to making sure that you can trust your data. However it’s particularly difficult to scale across a project - you can’t always download a csv and run a check manually - it’s just not gonna work when you have hundreds or thousands of models. So that’s where testing comes in.

In dbt, dbt tests are run with the command dbt test, and that will run all your tests in your project against the latest materializations of you models.

You can that in development while you write code in order to make sure that the transformation you are working on does what you expect and the underlined data fits your assumptions at the time of development.

That gives you as an analytical engineer the confidence to merge your changes into your main or master branch and run that on production.

And once you are running that code in production the test configuration of your project isn’t the same codebase so you can continue to run those tests every day or every week whenever the models are refreshed so that you have the confidence that the models feeding dashboard can be trusted for your team.


Now let’s discuss some specific things on testing in analytics : there are two main classification of tests : **singular** and **generic**.

Singular tests as the name implies are very specific, applied to one maybe two models and assert something very specific about the logic in a particular model. This is not something that you want to copy paste across multiple models.

Other classification of tests are the generic tests, these are the highly scalable tests. Rather than writing the logic, you are writing a couple lines of yaml code testing a particular model. There are four type of generic tests that you can consider : unique, not null, accepted value and relationship. 
Don’t hesitate to consider using unique or not null on your primary or joining keys to get sure that you are not causing any break down stream.


## Testing in a nutshell

- **Testing** is used in software engineering to make sure that the code does what we expect it to.
- In Analytics Engineering, testing allows us to make sure that the SQL transformations we write produce a model that meets our assertions.
- In dbt, tests are written as select statements. These select statements are run against your materialized models to ensure they meet your assertions.

## Tests in dbt

- In dbt, there are two types of tests - generic tests and singular tests:
    - **Generic tests** are written in YAML and return the number of records that do not meet your assertions. These are run on specific columns in a model.
    - **Singular tests** are specific queries that you run against your models. These are run on the entire model.
- dbt ships with four built in tests: unique, not null, accepted values, relationships.
    - **Unique tests** 👉 to see if every value in a column is unique
    - **Not_null tests** 👉 to see if every value in a column is not null
    - **Accepted_values tests** 👉 to make sure every value in a column is equal to a value in a provided list
    - **Relationships tests** 👉 to ensure that every value in a column exists in a column in another model
- Generic tests are configured in a YAML file, whereas singular tests are stored as select statements in the tests folder.
- Tests can be run against your current project using a range of commands:
    - *dbt test* runs all tests in the dbt project
    - *dbt test --select test_type:generic*
    - *dbt test --select test_type:singular*
    - *dbt test --select one_specific_model*

- In development, dbt Cloud will provide a visual for your test results. Each test produces a log that you can view to investigate the test results further.
![pic](http://www.welcome.paprika.tech/dbtpic//testing-dev.webp)

In production, dbt Cloud can be scheduled to run dbt test. The ‘Run History’ tab provides a similar interface for viewing the test results.
![pic](http://www.welcome.paprika.tech/dbtpic//testing-prod.webp)


## Resources 📚📚
* [Générer de la documentation avec Data Build Tool (DBT)](https://www.effidic.fr/generer-documentation-data-build-tool-dbt/)
* [Building Sustainable dbt Project Documentation](https://blog.montrealanalytics.com/building-sustainable-dbt-project-documentation-8def88ca67c3)
* [Everything on tests](https://docs.getdbt.com/docs/build/tests)
