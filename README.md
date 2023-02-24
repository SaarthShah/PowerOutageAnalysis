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

Before beginning the data cleaning, we first had to read the data properly by ensuring that pandas does not read these specific columns.

`pd.read_excel('outage.xlsx',skiprows=[0,1,2,3,4,6]).drop(columns=['variables']` is the code that would allow us to drop these specific rows and columns.

Our next step was to clean the dataset in order to make the rows and columns usable for our study. Here are some of the steps we took to properly clean the dataset:

1.  


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

**Results:**

Observed TVD = 0.03
P-value = 0.592
Signficance level (alpha) = 5%
[PLOT HERE]

Our p-value of 0.592 is much bigger than our significance interval of 5%, therefore we do not have enough evidence to reject the null hypothesis. Based on this we cannot conclude that the missingness of the CUSTOMERS.AFFECTED values depends on the CLIMATE.CATEGORY column.

Therefore we cannot say that missingness of the CUSTOMERS.AFFECTED values is Missing at Random due to its correlation with the CLIMATE.CATEGORY column.

## Hypothesis Testing



