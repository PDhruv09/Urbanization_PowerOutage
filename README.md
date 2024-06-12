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

### Exploratory Data Analysis
#### Univariate Analysis
In this section, I conducted a univariate analysis to explore individual variables related to power outages in the dataset. The focus was on understanding the distribution of urban population percentages and the number of power outages across different states.

Population Percentage of Urban Areas for Every State
First, I examined the distribution of the population percentage of urban areas for each state. This analysis helps in understanding how urbanized each state is, which can be a crucial factor in analyzing power outage patterns.

<iframe src="assets/plot1.html" width="800" height="600" frameborder="0" ></iframe> 

This bar plot shows the urban population percentage for each state. From the plot, we can observe the variability in urbanization levels across states, with some states having a high percentage of urban areas, while others have much lower percentages. This information sets the stage for further analysis of how urbanization impacts power outages.

Distribution of the Number of Power Outages for Every State
Next, I analyzed the distribution of the number of power outages for each state. This provides insight into which states experience more frequent outages, which can be crucial for understanding the overall power grid reliability in different regions.

<iframe src="assets/plot2.html" width="800" height="600" frameborder="0" ></iframe>

This bar plot displays the number of power outages for each state. The plot highlights which states have a higher frequency of outages. Understanding the distribution of outages helps in identifying patterns and regions that might require more attention and resources to improve power grid stability.

<!-- ##### Conclusion
Through this univariate analysis, I gained a better understanding of the distribution of urban population percentages and the frequency of power outages across different states. The analysis revealed significant variability in urbanization levels and outage frequencies, providing a foundational understanding for more complex analyses in the subsequent sections. This information is critical for identifying trends and factors that may influence power outages, guiding future investigations and potential interventions to improve grid reliability. -->


#### Bivariate Analysis
In this section, I examined the relationship between urban population percentage and two metrics related to power outages: the number of outages and the average outage duration. My goal was to understand how urbanization levels might influence both the frequency and severity of power outages across different states.

**Relationship between Urban Population Percentage and Number of Outages**
First, I explored the relationship between the urban population percentage of each state and the number of power outages that occurred. My hypothesis was that states with higher urbanization levels might experience more frequent outages due to higher population densities and infrastructure demands.

<iframe src="assets/plot3.html" width="800" height="600" frameborder="0" ></iframe>

This plot shows each state's urban population percentage on the x-axis and the number of outages on the y-axis. While there is some variability, we can observe a general trend where states with higher urban populations tend to experience more power outages.

**Relationship between Urban Population Percentage and Average Outage Duration**
Next, I analyzed the relationship between the urban population percentage and the average duration of power outages. My hypothesis here was that urbanized areas, due to better infrastructure and more efficient response systems, might have shorter outage durations compared to less urbanized areas.

<iframe src="assets/plot4.html" width="800" height="600" frameborder="0" ></iframe>

This plot displays the urban population percentage on the x-axis and the average outage duration on the y-axis. The results indicate that there is no clear correlation between urbanization levels and outage duration. States with both high and low urban populations show a wide range of average outage durations, suggesting that factors other than urbanization might play a more significant role in determining outage duration.

<!-- ##### Conclusion
Through these bivariate analyses, I found that while there is a tendency for more urbanized states to experience a higher number of outages, urban population percentage does not seem to significantly affect the average duration of these outages. This indicates that urbanization might impact the frequency of power outages more than their severity. Further investigation into other variables and their interactions is needed to develop a comprehensive understanding of the factors influencing power outage patterns. -->


#### Grouping and Aggregates

In this section, I performed grouping and aggregation to understand how power outages vary across different states and urban population percentages. The goal was to investigate the relationship between urbanization and power outage characteristics.

**Grouping by State and Urban Population Percentage Bin**

To facilitate the analysis, I binned the urban population percentages into five categories: 0-20%, 20-40%, 40-60%, 60-80%, and 80-100%. This allowed me to group states based on their urbanization levels and perform aggregate calculations within these groups.

**Number of Outages per State and Urban Population Percentage Bin**

Next, I counted the number of outages for each state within each urban population percentage bin. This allowed me to see the distribution of power outages across different levels of urbanization:

POPPCT_URBAN_BIN|	0-20%|	20-40%|	40-60%|	60-80%|	80-100%|
U.S._STATE      |					
Alabama         |	    0|	     0|	     6|	     0|	      0|
Alaska          |	    0|	     0|	     0|	     1|	      0|
Arizona         |	    0|	     0|	     0|	     0|	     28|
Arkansas        |	    0|	     0|	    25|	     0|	      0|
California      |	    0|	     0|	     0|	     0|	    210|
Colorado        |	    0|	     0|	     0|	     0|	     15|
Connecticut     |	    0|	     0|	     0|	     0|	     18|

**Average Outage Duration per State and Urban Population Percentage Bin**

In addition to the number of outages, I also calculated the average outage duration for each state within each urban population percentage bin. This analysis helped in understanding if the average duration of outages varies with urbanization levels:
POPPCT_URBAN_BIN|	  20-40%|	     40-60%|	  60-80%|	    80-100%|
U.S._STATE      |-----------|--------------|------------|--------------|				
Alabama         |	0.000000|	1152.800000|	0.000000|	   0.000000|
Arizona         |	0.000000|	   0.000000|	0.000000|	4552.920000|
Arkansas        |	0.000000|	1514.360000|	0.000000|	   0.000000|
California      |	0.000000|	   0.000000|	0.000000|	1666.338384|
Colorado        |	0.000000|	   0.000000|	0.000000|	 901.071429|
Connecticut     |	0.000000|	   0.000000|	0.000000|	1278.833333|

<!-- ##### Conclusion

By grouping and aggregating the data, I was able to gain insights into how power outages are distributed across different states and urbanization levels. The analysis revealed that states with higher urban population percentages tend to experience a greater number of outages. However, the average outage duration does not show a clear pattern across urbanization bins, indicating that other factors might be influencing the duration of power outages.

These findings provide a deeper understanding of the relationship between urbanization and power outage characteristics, which can help in identifying areas that need more attention for improving grid reliability and resilience. -->
