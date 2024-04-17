# Storing an API result in Snowflake via pandas

In this exercise we will use Python's function to retrive data from APIs, store them in a pandas DataFrame and save them in Snowflake

Let's go

## Before we start

- Open your favorite Python Notebook (Jupyter for ex)
- make sure you have installed the librairie to connect snowflake to pandas

```
pip install --upgrade snowflake-connector-python
pip install "snowflake-connector-python[pandas]"
pip install requests
pip install pandas
```

## About our Dataset

We’ll be working with the [Open Notify](https://open-notify.org/) API, which gives access to data about the international space station. 

It’s a great API for learning because it has a very simple design, and doesn’t require authentication. 

Often there will be multiple APIs available on a particular server.

Each of these APIs are commonly called endpoints.

The first endpoint we’ll use is http://api.open-notify.org/astros.json, which returns data about astronauts currently in space.

Do the following in your python notebook

- import the libraries snowflake-connector-python, requests, pandas
- using requests.get retrieve the response for a get call on http://api.open-notify.org/astros.json
- check that the status code of the response is 200
- retrieve the response content in json format thanks to response.json() in a variable called *data_response*
- isolate in a variable *data_people* the content of the section people of *data_response*
- thanks to the function [from_dict](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.from_dict.html) create a pandas dataframe in a variable *df*
- create a connection to your snowflake account in a variable *conn*

Now we will write the content of our dataframe directly to a new table

- create a dedicated database using the python snowflake connector - store its name in a variable *database*
- use the USE DATABASE command on your db
- create a dedicated schema using the python snowflake connector - store its name in a variable *schema*
- use the USE SCHEMA command on your schema
- store a table name in a variable *table_name* - we do not create the table yet !

- run the following to instert your data - more details on the function write_pandas can be found [here](https://docs.snowflake.com/en/user-guide/python-connector-api#write_pandas)

```python
from snowflake.connector.pandas_tools import write_pandas
success, num_chunks, num_rows, output = write_pandas(
            conn=conn,
            df=df,
            auto_create_table=True, 
            table_name=table_name
        )
```

- print the values of the variables *success* , *num_chunks*, *num_rows* and *output*

Now let's retrieve the content of our table in new dataframe !

- Write a sql query in a variable retrieve all data from your table - store it in a text variable called *sql*
- run the following
```python
df2 = conn.cursor().execute(sql).fetch_pandas_all()
```
- check the content of *df2*
- drop your database using the python snowflake connector
- close your connection to Snowflake 


Congrats you successfully manipulated snowflake within pandas !  👏👏 🎉


## Resources 📚📚

- [Use Pandas Dataframes](https://docs.snowflake.com/en/user-guide/python-connector-pandas.html)
