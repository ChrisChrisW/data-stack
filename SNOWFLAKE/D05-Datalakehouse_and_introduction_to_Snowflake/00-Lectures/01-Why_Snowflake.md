# Why Snowflake ?

## What you will learn in this course? 🧐🧐
* Chit chat on history of DataPlatform and rise of Snowflake


Welcome to this course on Snowflake

The goal of this course is to get you a good understanding of the role of Snowflake, as well as a good overview of its capacity.

But before this we will have a short introduction here on Snowflake...

❄️

❄️❄️

❄️❄️❄️

Snowflake,Snowflake,Snowflake…. why Snowflake ?

In the data platform world, Snowflake is the tool known for having the most advanced features, for the moment it doesn’t have the largest market share, but it feels like all the other tools are either trying to compete with it or to partner with it… so why? why is Snowflake currently so dominant?

In order to understand why Snowflake is so dominant, we need to go travel in the wayback machine, to understand how the world was in the dark age of data, and why we have all run so quickly to Snowflake.

So let’s jump on this journey to see why is it that Snowflake is now dominating the data world.

## The Dark Age of Data

In order to understand what the world was like before tools like Snowflake or even Redshift, we need to think about the fact that there was really no cloud - at least not in the way we understand it today.

Before if a finance team, or any team, operational or otherwise, wanted to report on some sort of KPI or do any form of analysis, and if that all required some sort of Data Warehouse, they would need to go to IT and figure out: 

Were there any servers available that could have space where they could put some form of Data Warehouse? 
More than likely if there wasn't space they'd have to go to procurement and procure a whole new server just for their Data Warehouse,  all of which could likely take months as procurement might have their own process, and then you have to talk to some sort of a supplier of servers and just on and on...
A simple request as spinning up a Data Warehouse for reporting could take months and that would just be to get the physical server! 

This was the before times: this was before you could just click a button and spin up a Data Warehouse instantaneously. 

In addition, data was starting to grow at a very rapid rate.

Meaning you had a much harder time predicting how much space you actually required for your reporting, and more than likely your systems couldn't actually process the data fast enough in order to provide analytics quickly.

That is where a lot of cloud as well as more distributed systems started to jump into play.

## The Cloud OLAP Age of Data - or almost dark age of data…

In 2013, Amazon Redshift was disrupting the market : it was taking what was once calculated somewhere in the range of like 19 to 25 thousand dollars per terabyte of data warehousing solution and turning it into less than a thousand.

You no longer have to wait for procurement to get you servers and in addition if you need to scale up your processing Redshift was an easier option to.

This made Redshift a very popular option, but there were a few clunky things about it: a lot of the data engineers of this time just put up with complexity because they were arguably better than the current on-prem solutions but it did make their lives harder.
For example if you wanted to do some sort of merge or basically what they call now upsearch statement, unlike in other Data Warehouses or database systems where you could just do this with a clause, that actually allowed you to merge, you would instead need three different tables, a piece of gum, a rubber band, and several other random components just to do something like a basic slowly changing dimension - and that's just one of many idiosyncrasies that Redshift had.

The other options at the time weren't arguably that much better : bigquery decided to make their own SQL (and eventually had to kind of go back on that whole decision) and hadoop require you to write code in java other than SQL - at least at the time (hive and presto were not made at this point).

So everything was kind of clunky in terms of either using something like cloud-based (whether it was bigquery or Redshift) or if you wanted to use something like hadoop you'd have to like spin it up yourself and manage it - eventually solutions like hortonworks came out and so you could kind of do hadoop as a managed service but overall a lot of the options that we had weren't exactly what we needed.

Data engineers put up with it: 
* they spent a ton of time learning about the different types of nodes that hadoop has
* they spent a ton of time figuring out how to set up keys on amazon Redshift 

Data engineers had to relearn a ton of things that we had been doing for a long time in classic data warehousing.

And then one day, a light shine from heaven, and that light was named Snowflake.

## Winter is coming - the Snowlake age of Data

Now the thing about snowflake is it did a lot of things really well when it came onto the market, and the first thing being its virtual Data Warehouse where it separated storage and compute. 

You no longer had to pre-allocate the exact size of storage and how it was gonna be correlated with the amount of compute that you're gonna have : these things could be separate instances where you could store a small amount of data or a big amount of data and you could run on what they call small or an extra large very easily with just a click of a drop down.

All of which meant you didn't have to resize your Data Warehouse and you were just going to pay for exactly what you used, and you wouldn't have to spend time migrating in case of resizing.

In addition you could do things like merge statements and other things that are very classic in Data Warehouses so people who are classic Data Warehouse developers didn't have to feel like they were figuring out some new thing for the 10th time.

Snowflake opened up the world of data to companies that were significantly smaller : in the old days it might take a lot of time a lot of expertise to develop something like a Data Warehouse - especially with tools like Redshift or bigquery which have to be relearned by a lot of data professionals and would be very expensive to develop, both from a people standpoint as well as from a tool standpoint. 

Snowflake built a cloud Data Warehouse that was really easy to spin up, and that as long as you kind of understood standard sql allows you to build on it without having you spending an arm and a leg to do it both in terms of people cost as well as in terms of tool cost. 

Recently Snowflake costs have definitely gone up but the technology is still something to be known as it stands now for the Rolls Roice of Data Lakehouse and is always a reference in modern data stack.

## Current Days of Data

Snowflake came into a world that was primed and frustrated by the current options, Redshift required a lot of finagling
hadoop on its own was a little bit frustrating: you required like four services to keep it running and an expensive team of experts just to manage your various clusters.

When snowflake came out and it was really almost as simple as just click and you have a fully functional Data Warehouse people were all for it.

Of course now we are entering a new era.

The previous area was more of the move to the cloud Data Warehouse and that's what snowflake sold itself as.

But even Snowflake now is currently selling itself as a cloud data platform because it realizes that storing the data is just step one.

Their recent $800 million acquisition of [Streamlit](https://streamlit.io/) continues to voice the fact that they are trying to push into a totally new strategy where they're not just the tool that stores your data but they're the tool that you're going to rely on for your analytics and how you're going to actually operationalize the data.

There's tons of tools that are being stacked and built on top of snowflake, things like reverse etl, tools like high touch and census, all whose focus is turning all this data into valuable operational insights.
Snowflake needs to be the cloud data platform that you rely on for operationalizing your analytics.

And that's what we are going to see now!

## Resources 📚📚

* [Streamlit • The fastest way to build and share data apps](https://streamlit.io/)
* [Snowflake acquires Streamlit for $800M to help customers build data-based apps | TechCrunch](https://techcrunch.com/2022/03/02/snowflake-acquires-streamlit-for-800m-to-help-customers-build-data-based-apps/?guccounter=1&guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8&guce_referrer_sig=AQAAAKV3W6mWDebJBU_4P_qsj9BnMlKtjXpuRJfuS8ueOgrVTEUSGnAu-lTPnN5TDJqoPUhWrbvBNDf7wReHJv4SKt4nsYKI_ygfHeHzrbDzDepOQH_noEOfvJcZOyMwjHgBq1O5dTLHNyvPovzMdbJJIiQXLZfSQKOc85172lh4qHhl)
* [OLAP vs. OLTP | Snowflake](https://www.snowflake.com/guides/olap-vs-oltp#:~:text=An%20OLAP%20system%20is%20designed,transactional%20data%20involving%20multiple%20users.)
