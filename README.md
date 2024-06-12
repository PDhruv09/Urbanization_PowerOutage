# Does Urbanization effect Power Outage ?
**By:- Dhruv Patel**

Project for Dsc 80 at UCSD

## Introduction
In this project, I examined a data set of major power outages in the U.S. from January 2000 to July 2016. These major outages are defined by the Department of Energy to have impacted at least 50,000 customers, or causing an unplanned energy demand loss of at least 300 MegaWatts.This dataset was accessed from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure, at https://engineering.purdue.edu/LASCI/research-data/outages.

The dataset includes information about the major outages, as well as geographical, climatic, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages.

First, I will clean the data set and conduct exploratory data analysis in order to gain a basic understanding of the information provided. I will then analyze the missingness mechanisms and dependency of the data set.

Finally, I will explore my research question, How does the urbanization level of a state influence the number and duration of power outages? I will try to build a model that predicts coreraltion between the state's urbanization level and the number and duration of power outage.

The original raw DataFrame contains 1534 rows, corresponding to 1534 outages, and 57 columns. However, I will only focus on a few of these columns for the sake of my analysis.

| column                | Description                                                                                                                                  |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| U.S._STATE            | State the outage occurred in                                                                                                                 |
| YEAR                  | Year an outage occurred                                                                                                                      |
| MONTH                 | Month an outage occurred                                                                                                                     |
| OUTAGE.START          | Day of the year and Time of the day when the outage event started                                                                            |
| OUTAGE.RESTORATION    | Day of the year and Time of the day when power was restored                                                                                  |
| OUTAGE.DURATION       | Duration of outage events (in minutes)                                                                                                       |
| PCT_LAND              | Percentage of land area in the U.S. state as compared to the overall land area in the continental U.S. (in %)                                |
| PCT_WATER_TOT         | Percentage of water area in the U.S. state as compared to the overall water area in the continental U.S. (in %)                              |
| PCT_WATER_INLAND      | Percentage of inland water area in the U.S. state as compared to the overall inland water area in the continental U.S. (in %)                |
| POPPCT_URBAN          | Percentage of the total population of the U.S. state represented by the urban population (in %)                                              |
| POPDEN_URBAN          | Population density of the urban areas (persons per square mile)                                                                              |
| POPDEN_UC             | Population density of the urban clusters (persons per square mile)                                                                           |
| POPDEN_RURAL          | Population density of the rural areas (persons per square mile)                                                                              |
| RES.CUSTOMERS         | Annual number of customers served in the residential electricity sector of the U.S. state                                                    |
| COM.CUSTOMERS         | Annual number of customers served in the commercial electricity sector of the U.S. state                                                     |
| IND.CUSTOMERS         | Annual number of customers served in the industrial electricity sector of the U.S. state                                                     |
| TOTAL.CUSTOMERS       | Annual number of total customers served in the U.S. state                                                                                    |

## Data Cleaning and Exploratory Data Analysis
The first step is to clean the data to make sure it is suitable for effective analysis.

### Cleaning Data
1. I start by dropping irrelevant columns and only keeping the features that I am interested in for analysis. These are:'U.S._STATE', 'YEAR', 'MONTH', 'OUTAGE.START', 'OUTAGE.RESTORATION', 'OUTAGE.DURATION', 'PCT_LAND', 'PCT_WATER_TOT', 'PCT_WATER_INLAND','POPPCT_URBAN', 'POPDEN_URBAN', 'POPDEN_UC', 'POPDEN_RURAL', 'RES.CUSTOMERS', 'COM.CUSTOMERS', 'IND.CUSTOMERS', 'TOTAL.CUSTOMERS'.

2. Next, I combine the OUTAGE.START.DATE and OUTAGE.START.TIME columns into one Timestamp object in an OUTAGE.START column. I do the same for OUTAGE.RESTORATION.DATE and OUTAGE.RESTORATION.TIME. I then dropped the old columns since all the relevant information is in OUTAGE.START and OUTAGE.RESTORATON.

3. Next, I check my outcomes of OUTAGE.DURATION, MONTH, POPPCT_URBAN, POPDEN_URBAN for values of  NaN, which are likely indicative of missing values. Since major outages wouldn't have a duration of NaN minutes, or the urban poppulation percentage/urban population dencity bing NaN.

The first few rows of this cleaned DataFrame are shown below, with a portion of columns selected.
| U.S._STATE   |   YEAR |   MONTH | OUTAGE.START        | OUTAGE.RESTORATION   |   OUTAGE.DURATION |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND |   POPPCT_URBAN |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |
|:-------------|-------:|--------:|:--------------------|:---------------------|------------------:|-----------:|----------------:|-------------------:|---------------:|---------------:|------------:|---------------:|----------------:|----------------:|----------------:|------------------:|
| Minnesota    |   2011 |       7 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |              3060 |    91.5927 |         8.40733 |            5.47874 |          73.27 |           2279 |      1700.5 |           18.2 |         2308736 |          276286 |           10673 |           2595696 |
| Minnesota    |   2014 |       5 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |                 1 |    91.5927 |         8.40733 |            5.47874 |          73.27 |           2279 |      1700.5 |           18.2 |         2345860 |          284978 |            9898 |           2640737 |
| Minnesota    |   2010 |      10 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |              3000 |    91.5927 |         8.40733 |            5.47874 |          73.27 |           2279 |      1700.5 |           18.2 |         2300291 |          276463 |           10150 |           2586905 |
| Minnesota    |   2012 |       6 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |              2550 |    91.5927 |         8.40733 |            5.47874 |          73.27 |           2279 |      1700.5 |           18.2 |         2317336 |          278466 |           11010 |           2606813 |
| Minnesota    |   2015 |       7 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |              1740 |    91.5927 |         8.40733 |            5.47874 |          73.27 |           2279 |      1700.5 |           18.2 |         2374674 |  

