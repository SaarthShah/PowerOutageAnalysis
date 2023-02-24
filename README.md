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

|    |   OBS |   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND | OUTAGE.START        | OUTAGE.RESTORATION   |   DAY_OF_WEEK |   TIME_OF_DAY |\n|---:|------:|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|:--------------------|:---------------------|--------------:|--------------:|\n|  0 |     1 |   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 |   6.56252e+06 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |             4 |         61200 |\n|  1 |     2 |   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 |   5.28423e+06 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |             6 |         67080 |\n|  2 |     3 |   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 | 1.46729e+06 | 1.80168e+06 | 1.9513e+06  |   5.22212e+06 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |             1 |         72000 |\n|  3 |     4 |   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 |   5.78706e+06 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |             1 |         16200 |\n|  4 |     5 |   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 |   5.97034e+06 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |             5 |          7200 |

## Assessment of Missingness

### NMAR Analysis
The OUTAGE.RESTORATION column, which is an aggregation of the OUTAGE.RESTORATION.DATE and OUTAGE.RESTORATION.TIME columns from the original dataset, is potentially NMAR, or not missing at random. The OUTAGE.RESTORATION column contains information about when the power was restored and the outage, resolved. It is possible that this column has missing values because the record-keeper for the power outage data may have been sick or on leave on the day of the restoration. This could make the column NMAR and not MCAR because the OUTAGE.RESTORATION column would not be random and, additionally, also be dependent on an external variable, i.e., the presence of the record-keeper for the power outages. If we had data on the presence of the record-keeper on the day of restoration, then we would be able to conclude the variable OUTAGE.RESTORATION as MAR (Missing at random).

### Missingness Dependency

#### Why TVD for checking Missingness?
TVD (Total Variation Distance) is a test statistic that is used to compare categorical distributions of a specific variable. For missingness, when we split our data into two sets based on whether data in a certain column is missing, we look at the categorical distributions of the other columns to see if there is any significant difference. For instance, below, we explore the missingness of CUSTOMERS.AFFECTED in relation to the columns CLIMATE.CATEGORY and U.S._STATES. Both these columns are used to classify data and, hence, are categorical. For this reason, we used TVD as our test statistic in our missingness analysis.

#### Identifying a column with potentially MAR data
Our column for CUSTOMERS.AFFECTED seems to be missing some values. However, this column does not seem to be missing values due to Design (MD) and contains both extremely large and small values (0 to 3241437 people). In this section, we tested whether the missingness of customers affected depends on another column or not.

####  CLIMATE.CATEGORY column
First let's test if the missingess of the CUSTOMERS.AFFECTED value depends on the climate of the place where the power outage was recorded. For this, we will first draw a simple plot to check if there is a visual difference between the null and non-null distribution values of CLIMATE.CATEGORY
[PLOT HERE]

In this chart, the distribution between the null and non-null values seem to be fairly similar. We can further investigate by conducting a permuation test to check if this difference in distribution was purely due to chance or if the CLIMATE.CATEGORY has a correlation with the missingnes of CUSTOMERS.AFFECTED.

**Permutation Test Results:**

Observed TVD = 0.03
P-value = 0.592
Signficance level (alpha) = 5%
[PLOT HERE]

Our p-value of 0.592 is much bigger than our significance interval of 5%, therefore we do not have enough evidence to reject the null hypothesis. Based on this we cannot conclude that the missingness of the CUSTOMERS.AFFECTED values depends on the CLIMATE.CATEGORY column.

Therefore we cannot say that missingness of the CUSTOMERS.AFFECTED values is Missing at Random due to its correlation with the CLIMATE.CATEGORY column.

#### U.S._STATES column
Now, let's test if the missingess of the CUSTOMERS.AFFECTED value depends on the US State where the power outage was recorded. For this, we will first draw a simple plot to check if there is a visual difference between the null and non-null distribution values of U.S._STATE.
[PLOT HERE]

There seems to be a significant difference in values in the null and non-null distributions of the CUSTOMERS.AFFECTED values. We can further investigate by conducting a permuation test to check if this difference in distribution was purely due to chance or if the CLIMATE.CATEGORY has a correlation with the missingnes of U.S._STATES.

**Permutation Test Results:**

Observed TVD = 0.37
P-value = 0.0
Signficance level (alpha) = 5%
[PLOT HERE]

As the p-value is 0 which is less than our 5% confidence level, we have sufficient evidence to reject the null hypothesis and state that the missingness of the CUSTOMERS AFFECTED column is <b>MAR</b> by The U.S._STATE column.

## Hypothesis Testing



