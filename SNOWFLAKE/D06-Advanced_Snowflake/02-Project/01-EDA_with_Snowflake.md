# EDA using Snowflake & Deepnote on Aircraft Data


The goal of this project is that you use deepnote to perform EDA on Aircraft Data loaded into snowflake.

## Load data in Snowflake using the web IDE

- First of all, do the following : create a database / schema / small warehouse

- select the schema & warehouse in an active worksheet

- take the content of *src/aircraft_db.sql* and copy / paste it in a snowflake worksheet to load it in database

## Create a new project in Deepnote to retrieve this data

- check that you can connect to this new db and retrieve correctly all data

## Prepare vizualisation to answer the following questions :

- Which aircraft has flown the most?

  When saying "flown the most" we refer to the number of flights and not accumulated distance flown.

  Look at the data in the individual_flights table, count how many individual flights there were per aircraft, and then enhance the result with the name of the aircraft instead of the code. We will provide the number of flights as well the name of the aircraft.

- Which airport has transported the most passengers through it?

  Calculate the number of passengers transported as the number of flights of each aircraft type, times its capacity, all together grouped by airport.

  It is worth mentioning that every individual flight will transport N passengers, but that will double count the number of people because it will be attributed to both airports (departure and destination) per flight, therefore, N would be attributed to airport A and airport B.

- What was the best year for Revenue Passenger-Miles for each airline?

  The fields that track international values have frequent null values. We will consider them as zeros for simplicity but that must be accounted for before doing any kind of analysis because that will bias the results.

  Process
  Calculate the SUM of all RPM (Domestic, International or both at the same time, so, Total), and then select the MAX per airline, which will yield us the year as well. By selecting a specific type of RPM or both we will obtain different answers, therefore all three must be provided.

- What was the best year for growth for each airline?

  We have ASM and RPM, as well the number of flights Domestic and for some flights International, per Airline and Airport. To list some of the possible ways of evaluate growth, we have: Decrease the difference between ASM - RPM, therefore captivating more passengers out of the ones that they had the capacity for. Increase on ASM, therefore increasing the fleet or the size of the planes, or the number of flights. Increase in number of flights alone.

  For simplicity and because it seems to be the one providing the best answer, we will use ASM as our growth indicator, and the more ASM an airline has over time, the more we will say they grew.

  To calculate the growth we will take the AVG(ASM_Domestic) per Airline per Year.

## Do not forget to drop what you have created !

- drop the associated database & warehouse