# Does Urbanization effect Power Outage ?
## **By:- Dhruv Patel**

Project for Dsc 80 at UCSD

## Introduction
Welcome to my project on major power outages in the United States from January 2000 to July 2016. This dataset, sourced from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure, provides detailed insights into outages that affected at least 50,000 customers or caused an unplanned energy demand loss of at least 300 MegaWatts.

Why Care About This Dataset?

Understanding the factors influencing power outages is crucial for policymakers, utilities, and the public alike. Major outages impact millions of Americans each year, disrupting daily life, commerce, and public safety. By exploring this dataset, we can uncover patterns that inform better infrastructure planning, disaster preparedness, and resilience strategies.

Dataset Overview

The dataset comprises 1,534 records detailing major power outages across the United States. It includes a wide array of information, such as geographical, climatic, land-use characteristics, electricity consumption patterns, and economic data for the affected states.

Key Columns for Analysis

For this project, I will focus on several key columns:

U.S. STATE: The state where the outage occurred.
YEAR: The year in which the outage occurred.
MONTH: The month in which the outage occurred.
OUTAGE.START: Date and time when the outage began.
OUTAGE.RESTORATION: Date and time when power was restored.
OUTAGE.DURATION: Duration of the outage in minutes.
PCT_LAND: Percentage of land area in the state compared to the continental U.S.
PCT_WATER_TOT: Percentage of water area in the state compared to the continental U.S.
PCT_WATER_INLAND: Percentage of inland water area in the state compared to the continental U.S.
POPPCT_URBAN: Percentage of the state's population living in urban areas.
POPDEN_URBAN: Population density in urban areas (persons per square mile).
POPDEN_UC: Population density in urban clusters (persons per square mile).
POPDEN_RURAL: Population density in rural areas (persons per square mile).
RES.CUSTOMERS: Annual number of customers served in the residential electricity sector.
COM.CUSTOMERS: Annual number of customers served in the commercial electricity sector.
IND.CUSTOMERS: Annual number of customers served in the industrial electricity sector.
TOTAL.CUSTOMERS: Total annual number of customers served.
These columns will enable me to investigate how the urbanization level of a state influences the frequency and duration of power outages.

Research Question

This project aims to answer the following question: How does the urbanization level of a state influence the number and duration of power outages? By analyzing this relationship, I aim to develop a model that predicts the correlation between a state's urbanization level and its susceptibility to power outages.                                                                                    |

Data Cleaning and Exploratory Data Analysis
Cleaning Data
To ensure effective analysis, the data underwent several cleaning steps:

Column Selection: Irrelevant columns were dropped, retaining key features for analysis: 'U.S._STATE', 'YEAR', 'MONTH', 'OUTAGE.START', 'OUTAGE.RESTORATION', 'OUTAGE.DURATION', 'PCT_LAND', 'PCT_WATER_TOT', 'PCT_WATER_INLAND','POPPCT_URBAN', 'POPDEN_URBAN', 'POPDEN_UC', 'POPDEN_RURAL', 'RES.CUSTOMERS', 'COM.CUSTOMERS', 'IND.CUSTOMERS', 'TOTAL.CUSTOMERS'.

Timestamp Integration: Combined 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' into 'OUTAGE.START' and similarly for 'OUTAGE.RESTORATION.DATE' and 'OUTAGE.RESTORATION.TIME'. Old columns were dropped as redundant.

Handling Missing Values: Checked and handled NaN values in critical columns ('OUTAGE.DURATION', 'MONTH', 'POPPCT_URBAN', 'POPDEN_URBAN'). NaNs were deemed unusual for outage duration and urban metrics, thus treated as missing data.

Below is a snapshot of the cleaned DataFrame:

U.S._STATE	YEAR	MONTH	OUTAGE.START	OUTAGE.RESTORATION	OUTAGE.DURATION	PCT_LAND	PCT_WATER_TOT	PCT_WATER_INLAND	POPPCT_URBAN	POPDEN_URBAN	POPDEN_UC	POPDEN_RURAL	RES.CUSTOMERS	COM.CUSTOMERS	IND.CUSTOMERS	TOTAL.CUSTOMERS
Minnesota	2011	7	2011-07-01 17:00:00	2011-07-03 20:00:00	3060	91.5927	8.40733	5.47874	73.27	2279	1700.5	18.2	2308736	276286	10673	2595696
Minnesota	2014	5	2014-05-11 18:38:00	2014-05-11 18:39:00	1	91.5927	8.40733	5.47874	73.27	2279	1700.5	18.2	2345860	284978	9898	2640737
Minnesota	2010	10	2010-10-26 20:00:00	2010-10-28 22:00:00	3000	91.5927	8.40733	5.47874	73.27	2279	1700.5	18.2	2300291	276463	10150	2586905
Minnesota	2012	6	2012-06-19 04:30:00	2012-06-20 23:00:00	2550	91.5927	8.40733	5.47874	73.27	2279	1700.5	18.2	2317336	278466	11010	2606813
Minnesota	2015	7	2015-07-18 02:00:00	2015-07-19 07:00:00	1740	91.5927	8.40733	5.47874	73.27	2279	1700.5	18.2	2374674	...		
Exploratory Data Analysis
Univariate Analysis
Population Percentage of Urban Areas for Every State

I created a bar plot using Plotly to visualize the distribution of urban population percentages across states. This plot helps in understanding the variability in urbanization levels, which is crucial for analyzing power outage patterns.

<iframe src="assets/plot1.html" width="800" height="600" frameborder="0"></iframe>
Bivariate Analysis
Relationship between Urban Population Percentage and Number of Outages

To explore how urbanization influences outage frequency, I plotted the urban population percentage against the number of power outages for each state. The plot indicates a trend where states with higher urban populations tend to experience more outages.

<iframe src="assets/plot2.html" width="800" height="600" frameborder="0"></iframe>
Grouping and Aggregates

I grouped the data by state and binned urban population percentages to analyze the distribution of power outages across different levels of urbanization. This aggregation provides insights into how outage characteristics vary across states with different urbanization levels.

Number of Outages per State and Urban Population Percentage Bin

I counted the number of outages for each state within urban population percentage bins. This analysis reveals the distribution of power outages across various levels of urbanization.

Average Outage Duration per State and Urban Population Percentage Bin

Additionally, I calculated the average outage duration for each state within each urban population percentage bin. This helps in understanding if outage duration varies significantly based on urbanization levels.

Here are what the tables look like

Table for the Population percentage of Urban Area and the State for number of power outages

POPPCT_URBAN_BIN|	0-20%|	20-40%|	40-60%|	60-80%|	80-100%|
U.S._STATE      |					
Alabama         |	    0|	     0|	     6|	     0|	      0|
Alaska          |	    0|	     0|	     0|	     1|	      0|
Arizona         |	    0|	     0|	     0|	     0|	     28|
Arkansas        |	    0|	     0|	    25|	     0|	      0|
California      |	    0|	     0|	     0|	     0|	    210|
Colorado        |	    0|	     0|	     0|	     0|	     15|
Connecticut     |	    0|	     0|	     0|	     0|	     18|

Table for the Population percentage of Urban Area and the State for duration of power outages

POPPCT_URBAN_BIN|	  20-40%|	     40-60%|	  60-80%|	    80-100%|
U.S._STATE      |-----------|--------------|------------|--------------|				
Alabama         |	0.000000|	1152.800000|	0.000000|	   0.000000|
Arizona         |	0.000000|	   0.000000|	0.000000|	4552.920000|
Arkansas        |	0.000000|	1514.360000|	0.000000|	   0.000000|
California      |	0.000000|	   0.000000|	0.000000|	1666.338384|
Colorado        |	0.000000|	   0.000000|	0.000000|	 901.071429|
Connecticut     |	0.000000|	   0.000000|	0.000000|	1278.833333|

Conclusion

Through comprehensive data cleaning and exploratory analysis, I gained valuable insights into power outage patterns across different states and urbanization levels. These insights are crucial for identifying trends and factors influencing outage frequency and duration, paving the way for more targeted strategies to enhance grid reliability and resilience.

Assessment of Missingness
NMAR Analysis
In my dataset, one of the columns that is likely NMAR (Not Missing At Random) is OUTAGE.DURATION. The missingness in this column might be due to the data collection process where some outages were not fully recorded in terms of duration. This missing data could be because certain companies or states did not report the duration of outages accurately.

To further investigate if OUTAGE.DURATION is MAR (Missing At Random), additional data could be collected about the reporting entities for each outage. By analyzing the dependency of missingness on the specific reporting entity, we could determine if the missingness is related to these entities.

Missingness Dependency
To test the dependency of missingness in OUTAGE.DURATION, I performed permutation tests against two columns: POPPCT_URBAN (urban population percentage) and RES.CUSTOMERS (number of residential customers). The hypothesis testing framework for these tests is as follows:

Urban Population Percentage

Null Hypothesis: The mean missingness of OUTAGE.DURATION is independent of POPPCT_URBAN.
Alternate Hypothesis: The mean missingness of OUTAGE.DURATION depends on POPPCT_URBAN.

<iframe src="assets/plot5_1.html" width="800" height="600" frameborder="0"></iframe>

In the permutation test, I found an observed statistic of mean missingness, which was compared against the distribution of mean missingness obtained through permutations. The resulting histogram shows the distribution of the mean missingness, with a red line indicating the observed statistic. The calculated p-value indicates whether the observed mean missingness is significantly different from the permuted values.

Years

Null Hypothesis: The mean missingness of OUTAGE.DURATION is independent of Years.
Alternate Hypothesis: The mean missingness of OUTAGE.DURATION depends on Years.

<iframe src="assets/plot5_2.html" width="800" height="600" frameborder="0"></iframe>

Similarly, I performed the permutation test for Years and plotted the distribution of the mean missingness with a red line indicating the observed statistic. The p-value from this test helps in understanding if the missingness of OUTAGE.DURATION is dependent on the number of Year.

From these tests, I can conclude the nature of the missingness of OUTAGE.DURATION and its dependency on other columns in the dataset.

## Hypothesis Testing

I am investigating whether the duration of power outages differs significantly between states with high urbanization levels and states with low urbanization levels. The relevant data columns for this analysis are `OUTAGE.DURATION` and `POPPCT_URBAN`, where `POPPCT_URBAN` represents the percentage of urban population.

**Null Hypothesis:** The average duration of power outages is the same for states with high urbanization levels as it is for states with low urbanization levels.

**Alternative Hypothesis:** The average duration of power outages differs between states with high urbanization levels and states with low urbanization levels.

**Test Statistic:** Difference in means. Specifically, mean outage duration for states with high urbanization minus mean outage duration for states with low urbanization.

**Significance Level:** α = 0.05 (standard significance level)

To test this hypothesis, I performed a permutation test with 10,000 simulations to generate an empirical distribution of the test statistic under the null hypothesis. The results are visualized below.

### Permutation Test Results

<iframe src="assets/plot6.html" width="800" height="600" frameborder="0"></iframe>

The red line represents the observed difference in mean outage duration between high and low urbanization states against the distribution of differences obtained from permutation tests.

### P-value Interpretation

The p-value obtained from the permutation test is 0.308. With a significance level of 0.05, we fail to reject the null hypothesis. This suggests that there is no statistically significant difference in the mean duration of power outages between states with high and low urbanization levels.

**Conclusion:** Based on the p-value of 0.308, we conclude that there is no significant difference in the duration of power outages between states with high and low urbanization levels.

This conclusion supports the cautious interpretation of statistical tests, emphasizing that we cannot definitively prove whether the null hypothesis is true or false.

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

Here is the graph for all five folds of the GridSearchCV

<iframe src="assets/plot8.html" width="800" height="600" frameborder="0" ></iframe>

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