# coursework
Bootcamp Coursework

# ETL Project Report

Our project started out of curiosity about the type of data that might be interesting to look at with respect to Olympic games. One question that drove us was whether there might exist a deterministic relationship between a nation's medal counts and its economic status. 
In looking through available data sources, we decided to design and build a SQL database in PostgreSQL that would enable analysts to study this and other questions about the Olympics. 
The Extract, Transform and Load (ETL) methodolgoy guided our process, which is decribed below:

## Data source Identification & Extraction Process 

We began the process by searching for data sources. We first identified 3 csv files from Kaggle related to historical Olympics medals won: 
Olympic Sport and Medals, 1896-2014 https://www.kaggle.com/the-guardian/olympic-games. This data set uses 3 CSV files: Dictionary, Summer and Winter.

To help our production analysts study the relationship between economic status and medal counts, we then found a data source for historical Per Capita GDP: Our World in Data 1950 - 2017 https://ourworldindata.org/grapher/real-gdp-per-capita-pwt. This data set contains 1 CSV file.
We imported the csv files using Python and Jupyter lab. We scanned the the data sets and identified a 3 character code called country code that thought could be used to help us in the transformation part of the process. 

## Transformation & Cleanup

After the imports we initially looked for opportunities to clean it and transform it into a more focused, structured set of data that could be efficiently loaded into a relational database. 
We looked for null values in the data sets and deleted rows representing years that were not common to both data sets. 
We added columns representing "summer" and "winter", dropped unnecessary columns.  
We appended all historical olympic games data into one larger dataframe and we subtotaled the data, grouping it by games session, and by country in order to see medal counts.
Upon doing this, we found that some countries who had won medals in the past, (eg. Yugoslavia), were not represented in the Olympic dictionary or in the GDP data set. We then renamed multiple columns from brevity and to ensure that when loading into our database in the next step, we would not encounter problems resulting from mixed cases. 

## Loading into a Production database

After the above transformation steps, we used sqlalchemy and Pandas' dataframe.to_sql method to loaded the data into a relational database in PostgrSQL. 
We built the database in PostgreSQL because of the relative uniformity of the data that we were loading and due to the relative simplicity of the tasks we envision our SQL analysts performing on the database.

The new Olypics_db database has 3 tables, each corresponding to a Pandas dataframe from our work in jupyter lab: 1) country_master, 2 olympic_medal and 3) gdp_history. 

## Possible work to be performed by analysts

Using the database we created with the above described ETL process, analysts could answer questions like:

What countries have won the most medals, and in what games?
What countries have the highest GDP Per Capita, and in what years?
When we normalize for Per Capita GDP, what countries have won the most Olympic medals, and when?
They might have a null hyptothesis that there is no relationship between a nation's medal counts and its economic status.
