
# ELT & DBT : Modern Data Stack for Analytics Engineering

## What you will learn in this course? 🧐🧐

Welcome to this course on Analytics Engineering powered by dbt, introducing the evergrowing role of Analytics Engineering in data team. 

The goal of this course is to get you a good understanding of the challenges of Analytic's adoption and integration in any organization, and solutions brought by modern approach like dbt.

We will discuss in this introduction of a few topics :

- Traditional Data Teams
- ETL vs ELT
- Analytics Engineer
- Which tool could support today's Analytics need ?
- dbt : A framework for Analytics workflow

Let's go !🔥

## Traditional Data Teams 🧑‍🤝‍🧑

Traditionnal data teams have typically two roles : Data Analysts & Data Engineers

The data Engineers are in charge of building infrastructure that the data is hosted on, usually databases, and also for managing the ETL process, Extract Transform Load for making sure the data is where it needs to be : in tables so that the analysts can query it.

The skillset for a typical data engineer includes : SQL / Python / Java / Orchestration tool

The data analyst on the other hand tends to work a little bit closer to the business, decision makers, finance or marketing and typically query the tables that the data engineers has built to server dashboard or different kind of reports

Their skillset usually includes a lot of excel, sql, sometimes some dashboarding tools as well like power bi, tableau, datastudio.

But when you think about these two roles in an organisation, their is generally a gap between the two of them : the data analysts knows what’s needed to be built so that decision makers get actionable insights and the data engineers has the skills to build that, put it in production, have those tables refreshed. 

Because there is some gap between the two there is an opportunity to increase efficiency in the process.


## ETL and ELT

If you ever tried to calculate a kpi you may have done something like that : extract the data from some a source, maybe from an attached csv or excel file from an email, manipulate that data to calculate your kpi and then serve that data to some colleague so they can make decision based on your analysis.
This has in a nutshell its own ETL process : you are extracting the data by downloading it, you are manipulating the data - thus transforming it, and then you are loading the data in a final Excel file, giving it back to someone who can use it in a new state.

The more traditional ETL process is handled by data engineer, where the data can be extracted from various sources, like databases or apis or files, then transformed on some structured data (which could be presented in an excel file) and then loaded into a database like system in a table where data analyst will be able to query that data.

This usually requires additional tools on top of SQL, python, java, and other programming languages to make this happen. And then in addition to that once your table is built, the process of refreshing its data needs to be automated - which requires additional tooling - and tools like airflow have come to help on the automation part of the process.

But then there has been some recent addition of cloud-based data warehouse - and because they are based in the cloud organization can just purchase them and scale them up as needed. They don’t need to build these things on premise, sot they just have them when they need them.

Then the term data warehouse has emerged and it means slightly different things in different organizations but in a nutshell the data warehouse is the combination of a database and a supercomputer that can run code on that data base.

This has been a complete game changer for the analytic workflow because you could just get raw data, get that in your data warehouse and then transform it from there. This has changed the term ETL to ELT : Extract Load Transform.

For data teams what it means is that we can just focus on the E and the L : let’s extract data from some sources and load it into our data warehouse : just get it there and then when it’s there we can just transform it into whatever shape we need so that the analyst can query it later.

This is interesting because most of the calculation is now done on the data warehouse which specs are scalable : cloud base data warehousing willow allow you to elasticly  
- scale your computing power
- scale your storage
- reduce the transfer time need to load the data

With this new approach emerges a new role : the data analytics engineer which will shape how our data team will now work together.

## Analytics Engineer

The traditional data team is made of data analysts and data engineers, and what really drove this process is the ETL framework, where the data engineers are extracting and transforming the data to load it in DataWharehouse. 
But the rise of more powerfull DataWharehouse such as Snowflake ❄️, or Big Query has enabled this new paradigm of ELT, where the data is extracted and loaded by the data engineers and then taken care of the analyst engineer to serve the data analyst.

So the analyst engineer is solely focused on taking that raw data and transforming it into transformed data so that the analyst can take it from there and serve whatever needs the business has. They really are in charge of the T in ELT.

This then frees up the dataengineer to focus on the extraction from sources and loads it in the datawharehouse - the EL in the ELT. They can also focus on more macro level things like maintaining infrastructure.

Then for the analyst they can work more closely with the analytical engineer to deliver these final tables that can be queried with a BI tool in a much faster way so that they can get what they need when they need it and get those to the right people. 

![pic](http://www.welcome.paprika.tech/dbtpic//dataengineers.png)

So ultimately the team is composed of Data Engineer, Analytics Engineer and Data Analysts.

Keep in mind that theses roles are quite new, and therefore the lines between these roles are a bit blurry - people don’t need to be slotted into these job descriptions, and in fact if you are a new data team you might need to empower these three roles - but when the team grows people will tend to endoss fewer roles - it allows easier team scalability.

## Which tool could support today's Analytics need?

### Issues in today's Analytics workflow from the ETL paradigm

So now that we have a good view of the role of the Analytic Engineer & Data Analysts, let's focus on the technical requirements behind analytics tools.

The center of gravity in mature analytics organizations has shifted away from proprietary, end-to-end tools towards more composable solutions made up of:

- data integration scripts and/or tools,
- high-performance analytic databases,
- SQL, R, and/or Python, and
- visualization tools.

This change has unlocked significant possibility, but analytics teams still face challenges in consistently delivering high-quality, low-latency analytics.

In data team powering an ETL, it's faster for a Data Analyst to produce complex query than to have Data Engineers adapt the Datawarehouse, so the Data Analyst are producing a significant amount of business code which is nerver shared, monitored or organized - as it is generally stored in the BI tool (Power Bi , Tableau, etc...). This brings a lot of issues in the organization :
- Analysts operate in isolation. 
- Knowledge is siloted.
- Analyst keeps writting what a colleague had already written. 
- It's hard to grasp the nuances of datasets that we’re less familiar with. 
- Analyst often differ in their calculations of a shared metric.

These issues are related to a workflow problem, but analytics don’t have to be this way. 
In fact, the playbook for solving these problems already exists — on the software engineering teams. 
The same techniques that software engineering teams use to collaborate on the rapid creation of quality applications can apply to analytics, whith tooling enabling 
- collaboration around analytic's code
- consideration of analytic's code as a valuable asset
- workflow automation around analytic's code execution

Let's dig in these needs

### Analytics is collaborative​

Analytics team’s techniques and workflow should have the following collaboration features:

- **Version Control​** 👉 Analytic code — whether it’s Python, SQL, Java, or anything else — should be version controlled. Analysis changes as data and businesses evolve, and it’s important to know who changed what, when.
- **Quality Assurance​** 👉 Bad data can lead to bad analyses, and bad analyses can lead to bad decisions. Any code that generates data or analysis should be reviewed and tested.
- **Documentation​** 📄

    Your analysis is a software application, and, like every other software application, people are going to have questions about how to use it. Even though it might seem simple, in reality the “Revenue” line you’re showing could mean dozens of things. 
    Your code should come packaged with a basic description of how it should be interpreted, and your team should be able to add to that documentation as additional questions arise.
- **Modularity​** 🔲

    If you build a series of analyses about your company’s revenue, and your colleague does as well, you should use the same input data. 
    Copy-paste is not a good approach here — if the definition of the underlying set changes, it will need to be updated everywhere it was used. 
    Instead, think of the schema of a data set as its public interface. 
    We shall then create tables, views, or other data sets that expose a consistent schema and can be modified if business logic changes.


### Analytic code is an asset​

The code, processes, and tooling required to produce that analysis are core organizational investments. 
A mature analytics organization’s workflow should have the following characteristics so as to protect and grow that investment:

- **Environments​**

    Analytics requires multiple environments. 
    Analysts need the freedom to work without impacting users, while users need service level guarantees so that they can trust the data they rely on to do their jobs.
- **Service level guarantees​** ✅

    Analytics teams should stand behind the accuracy of all analysis that has been promoted to production. 
    Errors should be treated with the same level of urgency as bugs in a production product. Any code being retired from production should go through a deprecation process.
- **Design for maintainability​** ✅

    Most of the cost involved in software development is in the maintenance phase. 
    Because of this, software engineers write code with an eye towards maintainability. 
    Analytic code, however, is often fragile, as changes in underlying data break most analytic code in ways that are hard to predict and to fix.
    Analytic code should still be written with an eye towards maintainability. 
    Future changes to the schema and data should be anticipated and code should be written to minimize the corresponding impact.

### Analytics workflows require automated tools​

Frequently, much of an analytic workflow is manual. 
Piping data from source to destination, from stage to stage, can eat up a majority of an analyst’s time. 
Software engineers build extensive tooling to support the manual portions of their jobs. 
In order to implement the analytics workflows we are suggesting, similar tools will be required.

Here’s one example of an automated workflow:
- models and analysis are downloaded from multiple source control repositories,
- code is configured for the given environment,
- code is tested, and
- code is deployed.

Workflows like this should be built to be executed with a single command.

## dbt : A framework for Analytics workflow

dbt is a transformation workflow that helps you get more work done while producing higher quality results. 
You can use dbt to modularize and centralize your analytics code, while also providing your data team with guardrails typically found in software engineering workflows. 
Collaborate on data models, version them, and test and document your queries before safely deploying them to production, with monitoring and visibility.

dbt compiles and runs your analytics code against your data platform, enabling you and your team to collaborate on a single source of truth for metrics, insights, and business definitions. 
This single source of truth, combined with the ability to define tests for your data, reduces errors when logic changes, and alerts you when issues arise.

### dbt optimizes analytics workflow​

- Avoid writing boilerplate [DML](https://docs.getdbt.com/terms/dml) and [DDL](https://docs.getdbt.com/terms/ddl) by managing transactions, dropping tables, and managing schema changes. 
    Write business logic with just a SQL select statement, or a Python DataFrame, that returns the dataset you need, and dbt takes care of [materialization](https://docs.getdbt.com/terms/materialization)

- Build up reusable, or modular, data models that can be referenced in subsequent work instead of starting at the raw data with every analysis.
    Dramatically reduce the time your queries take to run: Leverage metadata to find long-running models that you want to optimize and use [incremental models] which dbt makes easy to configure and use.

- Write [DRY](https://docs.getdbt.com/terms/dry)er code by leveraging macros, hooks, and package management.

### dbt provide more reliable analysis

- No longer copy and paste SQL, which can lead to errors when logic changes. 
    Instead, build reusable data models that get pulled into subsequent models and analysis. Change a model once and that change will propagate to all its dependencies.

- Publish the canonical version of a particular data model, encapsulating all complex business logic. All analysis on top of this model will incorporate the same business logic without needing to reimplement it.
- Use mature source control processes like branching, pull requests, and code reviews.
- Write data quality tests quickly and easily on the underlying data. Many analytic errors are caused by edge cases in the data: testing helps analysts find and handle those edge cases.

### dbt is available via different products

You can access dbt using dbt Core (open source, available in docker) or via dbt Cloud. 
dbt Cloud is built around dbt Core, but it also provides:
- Web-based UI so it’s more accessible
- Hosted environment so it’s faster to get up and running
- Differentiated features, such as metadata, in-app job scheduler, observability, integrations with other tools, integrated development environment (IDE), and more.

We will be using dbt Cloud in this course, but the concepts can easily be extended to dbt Core.

Before we jump into action, I'd like to discuss two technological assets which are working closely with dbt : the data platform and version control tools. 

### Data Platform & dbt

dbt connects to and runs SQL against a database, warehouse, lake, or query engine. 
We group all of these SQL-speaking things into one bucket called data platforms. 
dbt is designed to handle the transformation layer of the ‘extract-load-transform’ framework for data platforms. 

In this course, we will be using Snowflake ❄️ as our data warehouse, but keep in mind that dbt can run various data platform, including

- AlloyDB
- Azure Synapse
- BigQuery
- Databricks
- Dremio
- Postgres
- Redshift
- Snowflake ❄️
- Spark
- Starburst & Trino

### Version control & dbt

dbt also enables developers to leverage a version control system to manage their code base by using git. 

In the setup & exercices you will be able to use whichever git hoster you'd rather : github, gitlab, etc...

Alright let's set up our env ! 😘 🎉

---


## Resources 📚📚

* [everything on dbt](https://docs.getdbt.com/)
