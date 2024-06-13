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

## Assessment of Missingness
### NMAR Analysis

In my dataset, one of the columns that is likely NMAR (Not Missing At Random) is OUTAGE.DURATION. The missingness in this column might be due to the data collection process where some outages were not fully recorded in terms of duration. This missing data could be because certain companies or states did not report the duration of outages accurately.

To further investigate if OUTAGE.DURATION is MAR (Missing At Random), additional data could be collected about the reporting entities for each outage. By analyzing the dependency of missingness on the specific reporting entity, we could determine if the missingness is related to these entities.

**Missingness Dependency**
To test the dependency of missingness in OUTAGE.DURATION, I performed permutation tests against two columns: POPPCT_URBAN (urban population percentage) and RES.CUSTOMERS (number of residential customers). The hypothesis testing framework for these tests is as follows:

Urban Population Percentage
Null Hypothesis: The mean missingness of OUTAGE.DURATION is independent of POPPCT_URBAN.

Alternate Hypothesis: The mean missingness of OUTAGE.DURATION depends on POPPCT_URBAN.

<iframe src="assets/plot5_1.html" width="800" height="600" frameborder="0"></iframe>

In the permutation test, I found an observed statistic of mean missingness, which was compared against the distribution of mean missingness obtained through permutations. The resulting histogram shows the distribution of the mean missingness, with a red line indicating the observed statistic. The calculated p-value indicates whether the observed mean missingness is significantly different from the permuted values.

Residential Customers
Null Hypothesis: The mean missingness of OUTAGE.DURATION is independent of RES.CUSTOMERS.

Alternate Hypothesis: The mean missingness of OUTAGE.DURATION depends on RES.CUSTOMERS.

<iframe src="assets/plot5_2.html" width="800" height="600" frameborder="0"></iframe>

Similarly, I performed the permutation test for RES.CUSTOMERS and plotted the distribution of the mean missingness with a red line indicating the observed statistic. The p-value from this test helps in understanding if the missingness of OUTAGE.DURATION is dependent on the number of residential customers.

From these tests, I can conclude the nature of the missingness of OUTAGE.DURATION and its dependency on other columns in the dataset.

## Hypothesis Testing

I will be testing whether the duration of outages is significantly different for states with high urbanization levels compared to those with low urbanization levels. The relevant columns for this test are `OUTAGE.DURATION` and `POPPCT_URBAN`. I will use the median value of `POPPCT_URBAN` to categorize states into high and low urbanization levels.

**Null Hypothesis**: On average, the duration of outages for states with high urbanization levels is the same as the duration of outages for states with low urbanization levels.

**Alternate Hypothesis**: On average, the duration of outages for states with high urbanization levels is different from the duration of outages for states with low urbanization levels.

**Test Statistic**: Difference in means. Specifically, mean outage duration for high urbanization - mean outage duration for low urbanization.

To test this hypothesis, I performed a permutation test with 10,000 simulations to generate an empirical distribution of the test statistic under the null hypothesis. The results are shown below.

### Permutation Test Results

<iframe src="assets/plot6.html" width="800" height="600" frameborder="0"></iframe>

The observed difference in mean duration of outages between high and low urbanization levels is plotted against the empirical distribution of differences obtained from the permutation tests. The red line indicates the observed difference.

### P-value Interpretation

The p-value obtained from the permutation test is 0.308. With a standard significance level of 0.05, we fail to reject the null hypothesis. This suggests that there is no statistically significant difference in the mean duration of outages between states with high and low urbanization levels.

**Conclusion:** Based on the p-value of 0.308, we conclude that there is no significant difference in the duration of power outages between states with high and low urbanization levels.

## Framing a Prediction Problem
My model will try to predict the effect of a power outage depending on the urbanization level. This will be a binary classification because we are only focusing on duration of power outage and urbanization level.

The metric I am using the evaluate my model is the Mean Absolute Error (MAE), because we are predtiing the outage duration and than comparing it with the actual value of the duration. In which case the MAE will tell us how far, on average, your predictions deviate from the actual outage durations. Lower MAE values indicate better model performance, as they reflect smaller prediction errors.

At the time of prediction, we would know the 'POPPCT_URBAN', 'POPDEN_URBAN', 'RES.CUSTOMERS', 'COM.CUSTOMERS', 'IND.CUSTOMERS', 'TOTAL.CUSTOMERS', 'MONTH','Urban_Density_Ratio', and 'Customer_Diversity_Index'. This information will allow us to predict the outage duration closer to the actual outage duration.

## Baseline Model
My baseline model predicts the duration of power outages (OUTAGE.DURATION) based on urbanization-related features (POPPCT_URBAN and POPDEN_URBAN). Here's how the model is implemented and evaluated:

Features Used
1. **POPPCT_URBAN:** Percentage of urban population in the state.
2. **POPDEN_URBAN:** Urban population density in the state.

### Target
OUTAGE.DURATION: Duration of the power outage in hours.

### Model Performance
**Mean Absolute Error (MAE):** The model's MAE on the test set is approximately 3237.25 hours.
### Conclusion
This baseline model provides an initial estimation of power outage durations based on urbanization-related features. Further model enhancements can be explored by incorporating additional relevant features or employing more advanced regression techniques to improve prediction accuracy.

## Final Model
My final model predicts the duration of power outages (OUTAGE.DURATION) using a RandomForestRegressor. Here's how the model is implemented and evaluated:

### Features Used
1. POPPCT_URBAN: Percentage of urban population in the state.
2. POPDEN_URBAN: Urban population density in the state.
3. RES.CUSTOMERS: Number of residential customers.
4. COM.CUSTOMERS: Number of commercial customers.
5. IND.CUSTOMERS: Number of industrial customers.
6. TOTAL.CUSTOMERS: Total number of customers served.
7. MONTH: Month of the outage.

### New Features Created
1. Urban_Density_Ratio: Ratio of urban population density to total customers.
2. Customer_Diversity_Index: Standard deviation of residential, commercial, and industrial customers.

### Components Explained
1. StandardScaler: This transformer standardizes numeric features by removing the mean and scaling to unit variance. It ensures that each feature contributes equally to the model fitting process without being biased by the magnitude of the feature values.

2. OneHotEncoder: This encoder converts categorical features (like MONTH in this case) into binary vectors where each category becomes a column with either a 0 or 1. This is necessary because many machine learning algorithms cannot directly handle categorical data and require numerical inputs.

3. ColumnTransformer: This class allows for applying different preprocessing steps to different columns in the dataset. Here, it applies StandardScaler to numeric features and OneHotEncoder to categorical features (MONTH).

4. GridSearchCV: This technique performs an exhaustive search over a specified parameter grid to find the best parameters for a model. It uses cross-validation to evaluate each combination of parameters and selects the one that gives the best performance according to a specified scoring metric (neg_mean_absolute_error in this case).

### Best Model from GridSearchCV
The best model identified by GridSearchCV incorporates:

**RandomForestRegressor with hyperparameters:**
**n_estimators:** 200
**max_depth:** None (allowing the tree to expand until all leaves are pure or contain less than min_samples_split samples)
**min_samples_split:** 10
**min_samples_leaf:** 2
**Preprocessing:** Standard scaling of numeric features and one-hot encoding of categorical feature MONTH.

### Model Performance
**Mean Absolute Error (MAE):** The optimized model's MAE on the test set is approximately 2732.72 hours.

### Improvement in MAE
The improvement in Mean Absolute Error (MAE) from the baseline model to the final model can be attributed to several factors:

Feature Engineering: Introducing new features (Urban_Density_Ratio and Customer_Diversity_Index) that capture additional information about urbanization and customer base diversity.
Optimized Model Selection: Using GridSearchCV to tune hyperparameters of the RandomForestRegressor, resulting in a model that better fits the data and reduces prediction errors.
Enhanced Preprocessing: Applying StandardScaler and OneHotEncoder through ColumnTransformer ensures that the model interprets features correctly and efficiently handles both numeric and categorical data.
Model Complexity: The final model, being a RandomForestRegressor, can capture non-linear relationships between features and target, which might have been missed by the baseline linear regression model.
By incorporating these improvements, the final model achieves a lower MAE, indicating more accurate predictions of power outage durations compared to the baseline model.

### Conclusion
This final model significantly improves prediction accuracy compared to the baseline, achieving a lower MAE. It incorporates a RandomForestRegressor with optimized hyperparameters, leveraging features that capture urbanization, customer diversity, and seasonal factors affecting power outages. Further enhancements could involve additional feature engineering or exploring different ensemble methods to improve predictive performance.

## Fairness Analysis
Groups Defined

Group X: High Urbanization
Group Y: Low Urbanization
We defined the groups based on the median value of POPPCT_URBAN (Percentage of Urban Population). Group X includes test data points where the urban population percentage is greater than or equal to the median, while Group Y includes those where the percentage is less than the median.

Evaluation Metric
Mean Absolute Error (MAE): This metric measures the average magnitude of errors in a set of predictions, without considering their direction. It is the average over the test sample of the absolute differences between prediction and actual observation where all individual differences have equal weight.

Hypotheses
Null Hypothesis (H0): The model's MAE is the same for both high and low urbanization levels. Any observed difference is due to random chance.
Alternative Hypothesis (H1): The model's MAE is different for high and low urbanization levels.

Test Statistic and Significance Level
Test Statistic: The difference in MAE between Group X and Group Y.
Significance Level (α): 0.01

Permutation Test Results
We conducted a permutation test with 1000 trials to determine if the observed difference in MAE between high and low urbanization levels is statistically significant.

Results
MAE for Group X (High Urbanization): 2651.86
MAE for Group Y (Low Urbanization): 2824.05
Observed Difference in MAE: -172.19
P-value: 0.9130

The figure below shows the distribution of the statistic.
<iframe src="assets/plot7.html" width="800" height="600" frameborder="0" ></iframe>

Conclusion
Fail to reject the null hypothesis: The p-value is 0.9130, which is greater than the significance level of 0.01. This indicates that there is no evidence to suggest that the model's performance differs significantly between high and low urbanization levels.
This fairness analysis demonstrates that the model's accuracy, as measured by the Mean Absolute Error, does not significantly differ across different levels of urbanization. This ensures that the model is equitable and performs consistently, regardless of urbanization levels in the test data.