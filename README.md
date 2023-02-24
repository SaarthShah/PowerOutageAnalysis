## Introduction

In this study we will be exploring the [Power Outage Dataset](https://dsc80.com/project3/power-outages). This dataset contains datapoints for the major power outage data in the continental U.S. from January 2000 to July 2016. Overall this dataset contains 1534 power outage events that happened in the United States.

Some of the relevant columns that the dataset entails contains information about where and when outages happened, how many people were affected, the population breakdown of where this event happened, cause of the power outage, price of electricity, etc. 

> YEAR: Contains the year when this event happened <br><br>
> U.S._STATE: Name of the US State where the Outage Happened <br><br>
> OUTAGE.DURATION: How long did each duration last <br><br>
> CUSTOMERS.AFFECTED: How many people got affected <br><br>
> CAUSE.CATEGORY: The reported reason for the Outage <br><br>
> CLIMATE.CATEGORY: Current climate where the outage was reported <br><br>
> OUTAGE.RESTORATION.DATE: Date when the outage was resolved <br><br>
> OUTAGE.RESTORATION.TIME: Time when the outage was resolved <br><br>
> OUTAGE.START.DATE: Date when the outage was reported <br><br>
> OUTAGE.START.TIME: Time when the outage was reported <br><br>

Our goal while exploring this dataset is to better understand the factors that may be affecting how long these power outages last. As this dataset contains information about a lot of major power outages, it can provide us with a substantial amount of relative insight about what factors influence the likeliness and duration of these outages. These results can then be distributed to the relevant authorities in order to take precautionary steps to prevent futher events from happening alongside shortening the impact and duration. In the ever rising digital world, such insights have the potnetial to significantly impact the quality of life of US citizens by ensuring that they have constant access to power.

For this study, we will focus on answering the following question:

**Do power outages have a specific pattern? Do charactersitics like day of the week, time of the day, regions, electricity price, and outage causes have a correlation with the duration of the power outage**

## Cleaning and EDA

### Data Cleaning

Our data for this study is provided in an excel sheet that has certain useless columns that we can avoid during our analysis.
![Excel Sheet Preview](https://i.imgur.com/79yPXtg.png)

Before beginning the data cleaning, we first had to read the data properly by ensuring that pandas does not read these specific columns using the following code:

`pd.read_excel('outage.xlsx',skiprows=[0,1,2,3,4,6]).drop(columns=['variables']`

Our next step was to clean the dataset in order to make the rows and columns usable for our study. Here are some of the steps we took to properly clean the dataset:

1. The power outage start date and time is given by 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' which are two seperate columns. Aggregating the dates and times into a singular datetime object would allow us to quickly look into when the outages are starting with an exact time, without having to look into two seperate columns.

2. The power outage restoration date and time is given by 'OUTAGE.RESTORATION.DATE' and 'OUTAGE.RESTORATION.TIME' which are two seperate columns. Aggregating the dates and times into a singular datetime object would allow us to quickly look into when the outages where resolved with an exact time, without having to look into two seperate columns.

3. As our study emphasizes on the correlations between the duration of the power outage and other columns, it would make sense for us to drop any datapoints where we do not have a reported power outage duration. Thereby dropping all columns where the duration of the power outage is null.

4. Additionally, we would also be factoring in the time of the day when this power outage occured by converting the time into seconds for us to easily analyze the numerical values.

5. For one of our tests we would be find correlations between day of the week and power outage duration. We can quickly extract these values from the pandas date time object.

While cleaning we also checked how many null values are there for the 'prices' for electricity with the hopes of getting insight into what columns we can drop if we decide to run any tests with the electricity prices.

![Negative and Null Electricity Prices](https://i.ibb.co/cJzYGvF/Screenshot-at-Feb-23-17-24-47.png)

After thoroughly cleaning the dataset, here's a preview of our cleaned dataframe for power outages:



## Assessment of Missingness

### NMAR Analysis
The OUTAGE.RESTORATION column, which is an aggregation of the OUTAGE.RESTORATION.DATE and OUTAGE.RESTORATION.TIME columns from the original dataset, is potentially NMAR, or not missing at random. The OUTAGE.RESTORATION column contains information about when the power was restored and the outage, resolved. It is possible that this column has missing values because the record-keeper for the power outage data may have been sick or on leave on the day of the restoration. This could make the column NMAR and not MCAR because the OUTAGE.RESTORATION column would not be random and, additionally, also be dependent on an external variable, i.e., the presence of the record-keeper for the power outages. If we had data on the presence of the record-keeper on the day of restoration, then we would be able to conclude the variable OUTAGE.RESTORATION as MAR (Missing at random).

## Hypothesis Testing



