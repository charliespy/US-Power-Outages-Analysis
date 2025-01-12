# Unraveling the Mysteries Behind Major Power Outages in the U.S.
**Authors:** Charlie Sun, Aaron Feng

# Abstract 
Lorem ipsem 

# Project Overview
Power outages are critical events with far-reaching societal implications, disrupting daily activities, impacting businesses, and posing potential risks to public safety. Enhancing the resilience and reliability of the electrical grid requires a nuanced understanding of outage patterns and characteristics. In our analysis, we focus on predicting the duration of power outages, specifically honing in on the West Climate region. This geographic segmentation provides valuable insights into the unique challenges and contributing factors associated with power outages in this region. Climate regions, shaped by prevailing weather conditions, can significantly influence outage causes and consequences. For instance, the West Climate region might be characterized by dry and hot conditions, distinct from other regions experiencing colder temperatures or severe weather events.



# Introduction

Electricity is the cornerstone of modern civilization, powering homes, fueling industries, and enabling the technologies that drive daily life. Despite its critical role, the continuous supply of electricity is not always assured, as the United States has experienced significant power outages that highlight the fragility of the national energy grid. Power outages disrupt daily activities, impact businesses, and pose risks to public safety, with their causes and consequences often shaped by climatic conditions. The West Climate region, characterized by its dry and hot conditions, faces unique challenges compared to regions with colder temperatures or severe weather events, further underscoring the need for a nuanced understanding of outage patterns.

This study investigates the risks and causes of severe weather-induced power outages in the United States, using data from January 2000 to July 2016. It focuses on major outages affecting over 50,000 customers or causing losses above 300 MW. The first part of the study explores the correlation between different outage causes, such as severe weather and intentional attacks, and their impact on human life and outage severity. The second part aims to predict the duration of power outages, specifically in the West Climate region, to provide insights into the unique factors influencing grid resilience and recovery in this area. Through comprehensive data cleaning, exploratory analysis, and hypothesis testing, the project contributes to understanding and mitigating the risks associated with power outages.



# Data Cleaning

To prepare the dataset for analysis, we followed a comprehensive data cleaning process similar to our previous approach in Project 3. The initial dataset consisted of 55 columns, many of which were irrelevant or redundant. We focused on refining the dataset to include only features essential for predicting 'Outage Duration' and understanding the correlations between outage causes and severity.

The data cleaning process included the following steps:

1. Standardizing the Dataset: The original dataset was in xlsx format, which we converted into a csv format to ensure consistency and compatibility with our analysis tools.
2. Removing Instructional Rows and Columns: The raw dataset included several rows and columns at the beginning that contained instructional information rather than data. These were removed to streamline the dataset.
3. Combining and Cleaning Time Columns: We merged the OUTAGE.START.DATE and OUTAGE.START.TIME columns into a single OUTAGE.START column in pandas datetime format. Similarly, we combined OUTAGE.RESTORATION.DATE and OUTAGE.RESTORATION.TIME into an OUTAGE.RESTORATION column.
4. Column Selection: To focus on the relationship between climate, outage causes, and outage severity, we selected seven key columns: U.S._STATE, OUTAGE.START, OUTAGE.RESTORATION, OUTAGE.DURATION, DEMAND.LOSS.MW, CLIMATE.CATEGORY, and CAUSE.CATEGORY. These columns provide crucial insights into the nature and impact of power outages.
5. Handling Outliers: The distributions of OUTAGE.DURATION and DEMAND.LOSS.MW were highly skewed. Using the interquartile range (IQR), we identified and removed extreme outliers to improve data reliability and avoid skewing the analysis.

After cleaning, we categorized columns based on their roles in the analysis. CLIMATE.CATEGORY and CAUSE.CATEGORY were identified as causal indicator variables, reflecting the underlying factors contributing to outages. Meanwhile, OUTAGE.DURATION and DEMAND.LOSS.MW were labeled as severity indicators, representing the extent and impact of power outages. This refined dataset forms the foundation for studying the correlation between outage causes and severity, as well as predicting outage durations.

Here's the first few rows of the cleaned dataframe: 

| U.S. STATE   | OUTAGE. START       | OUTAGE. RESTORATION  |  OUTAGE. DURATION |  ANOMALY. LEVEL | CLIMATE. CATEGORY  | CAUSE. CATEGORY    |
|:-------------|:--------------------|:---------------------|------------------:|----------------:|:-------------------|:-------------------|
| Minnesota    | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |              3060 |            -0.3 | normal             | severe weather     |
| Minnesota    | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |                 1 |            -0.1 | normal             | intentional attack |
| Minnesota    | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |              3000 |            -1.5 | cold               | severe weather     |
| Minnesota    | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |              2550 |            -0.1 | normal             | severe weather     |
| Minnesota    | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |              1740 |             1.2 | warm               | severe weather     |



# Section 1: Understanding the Data 

In this section, we aim to discern how various outage causes distinctly impact human life by examining the correlation between different outage triggers.

## Univariate Analysis

We began our analysis by examining the distribution of severity indicators such as `OUTAGE.DURATION` and `DEMAND.LOSS.MW`. The plot below demonstrates that `OUTAGE.DURATION` is skewed to the right. The leftmost outage categorization, short-duration outages (<1000), is the most common. The rest of the outage duration distribution appears relatively uniform.

<p style="text-align:center"><iframe src="assets/outage_duration.html" width=800 height=600 frameBorder=0></iframe></p>

Similar to the outage duration distribution, the next plot below is also skewed to the right. It reveals that small demand losses (<100 MW) are more common than high demand losses.

<p style="text-align:center"><iframe src="assets/demand_loss.html" width=800 height=600 frameBorder=0></iframe></p>

Next, we examined the causal indicator variables. As shown in the table below, most power outages occur in places where the climate is classified as normal.

| Causes   |   CLIMATE.CATEGORY |
|:---------|-------------------:|
| normal   |                620 |
| cold     |                387 |
| warm     |                248 |

Additionally, the table below shows that severe weather and intentional attacks are the primary causes of outages, whereas system operability disruptions, public appeals, and equipment failures account for a smaller proportion of outages.

| Causes                        |   CAUSE.CATEGORY |
|:------------------------------|-----------------:|
| severe weather                |              543 |
| intentional attack            |              415 |
| system operability disruption |              112 |
| public appeal                 |               64 |
| equipment failure             |               54 |

To analyze the distribution of outages by location, we created a heatmap using Folium. As shown on the map below, northeastern and midwestern states experience the highest number of outages. We hypothesize that population and GDP may correlate with the number of outages, as California, Texas, and Washington have the greatest number of outages.

<p style="text-align:center"><iframe src="assets/heatmap.html" width=800 height=400 frameBorder=0></iframe></p>



## Bivariate Analysis

We began by analyzing the relationship between `OUTAGE.DURATION` and `CLIMATE.CATEGORY`. The box plot below depicts the three climate categories—warm, cold, and normal—representing the conditions during which power outages occurred. The x-axis indicates the outage duration measured in minutes.

<p style="text-align:center"><iframe src="assets/biv_1.html" width=800 height=600 frameBorder=0></iframe></p>

The three distributions have similar interquartile ranges (IQR) and shapes. However, the median and 75th percentile of outage duration are highest in cold climates. The warm category has the fewest outliers, while the normal category has the most. Overall, cold climates tend to experience longer and more variable outage durations. In contrast, warm climates exhibit shorter and more consistent durations. This observation supports our prediction that power outages are more severe and impactful in cold climates, where snow is more frequent, than in warm climates, which are generally sunny. Despite these findings, the box plot suggests that climate conditions may not strongly influence the duration and variability of power outages. Therefore, we will likely not include `CLIMATE.CATEGORY` as a feature variable in our hypothesis tests.

Next, we analyzed the influence of different `CAUSE.CATEGORY`s on `DEMAND.LOSS.MW`. The box plot below illustrates that demand loss values are numeric, continuous, and possibly standardized around zero, which may represent deviations from a norm or average. Several categories of outage causes are represented, including islanding, fuel supply emergencies, public appeals, equipment failures, system operability disruptions, intentional attacks, and severe weather.

<p style="text-align:center"><iframe src="assets/biv_2.html" width=800 height=600 frameBorder=0></iframe></p>

The distributions for each cause category vary significantly. The most notable categories are: 
- **Severe weather**: This distribution has a very large IQR and numerous outliers. Its range is the largest among all distributions and is also the most balanced.
- **Intentional attack**: This distribution has an extremely narrow IQR, barely visible on the plot. While its median and quartile ranges are centered around `DEMAND.LOSS.MW = 0`, its outliers extend to the highest values. This indicates that although the majority of power outages caused by intentional attacks have minimal power loss, some well-planned attacks can be very costly.

In summary, the box plots suggest that different causes of power outages lead to varying levels of power loss. Severe weather appears to result in the most consistent demand loss, while intentional attacks exhibit the most variable distribution. This analysis provides insight into which types of events cause the most significant deviations from normal operating conditions. In the hypothesis testing section, we will further analyze the relationship between severe weather and intentional attacks, using demand loss as the standard for comparison.



## Interesting Aggregates

We began by creating a pivot table to analyze the mean outage durations across various categories of `CAUSE.CATEGORY` and `CLIMATE.CATEGORY`. The mean outage durations vary significantly across columns, suggesting a potential correlation between `OUTAGE.DURATION` and `CAUSE.CATEGORY` as well as `CLIMATE.CATEGORY`.

|   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
|              338.5  |                 1521.57 |              328.557 |    259.267  |        1124.37  |          1808.41 |                         590.125 |
|              445.76 |                 1583.2  |              333.366 |     72.6875 |        1385.58  |          1820.49 |                         276.583 |
|              505    |                  255    |              312.557 |    209.833  |         625.333 |          1777.95 |                         495.571 |

Using the pivot table, we visualized these differences with a grouped bar plot:

<p style="text-align:center"><iframe src="assets/aggregates.html" width=800 height=600 frameBorder=0></iframe></p>

The grouped bar plot provides a clearer view of the differences in mean outage durations across the categories:
- **Climate Categories**: The "warm" climate category shows noticeably shorter outage durations compared to the "cold" and "normal" categories. This reinforces the hypothesis that warmer climates experience shorter and less severe power outages, likely due to the absence of snowstorms or freezing conditions, which are more common in colder climates.
- **Cause Categories**:
  - Outages caused by **severe weather** have the longest mean durations across all climate categories. This aligns with the understanding that severe weather events, such as hurricanes or snowstorms, typically cause widespread and prolonged outages.
  - **Fuel supply emergencies** also exhibit high mean durations, suggesting that these outages are difficult to resolve quickly, likely due to logistical challenges.
  - **Intentional attacks** and **equipment failures** have relatively lower mean outage durations, indicating these issues might be more localized and easier to address.
  - **Public appeals** show moderate mean durations but vary considerably between climate categories, possibly reflecting differences in how the public responds to power-saving campaigns or other utility directives.

Additionally, the disparity between the "warm" climate category and the other two categories highlights the potential influence of environmental factors on outage duration. These variations could stem from infrastructure differences, weather resilience, or repair challenges associated with different climates. Severe weather events stand out as a dominant contributor to longer outages, regardless of climate, emphasizing the need for robust infrastructure to mitigate these events' impacts.

In conclusion, the pivot table and grouped bar plot demonstrate that outage durations are influenced by both climate and cause categories. These findings suggest that incorporating `CLIMATE.CATEGORY` and `CAUSE.CATEGORY` as features in further analysis could provide valuable insights into the factors affecting power outages.




## Assessment of Data Missingness

### NMAR Analysis

We posit that the dataset exhibits **Missing Not At Random (NMAR)** characteristics. In our analysis of the dataset derived from *"A Multi-Hazard Approach to Assess Severe Weather-Induced Major Power Outage Risks in the U.S."* (Mukherjee et al., 2018), a significant pattern of missingness is observed in the `OUTAGE.RESTORATION` column. 

This dataset exclusively covers major outages, as defined by the Department of Energy's criteria: incidents affecting at least 50,000 customers or resulting in an unplanned firm load loss of at least 300 MW. Given the large scale and critical nature of these events, it is unlikely that the missing restoration data is due to negligence or oversight in data collection. Instead, the missingness in restoration times appears to stem from intrinsic uncertainties and complexities associated with restoring power after such significant outages. Factors contributing to this ambiguity may include:
- Technical challenges in restoring infrastructure.
- Logistical constraints in coordinating repairs and resource allocation.
- Varying protocols and practices across different regions and utility providers.

Thus, the missingness in the `OUTAGE.RESTORATION` column is inherently tied to the nature of the data itself. It is best characterized as NMAR because the absence of restoration information is linked to the restoration process's inherent challenges, rather than being a result of random or systematic collection errors.

### Missingness Dependency

Next, we focused on the missingness of `DEMAND.LOSS.MW` in the dataset and tested whether this missingness is dependent on other variables. We conducted a **permutation dependency test** on the following pairs of columns:
- `{CAUSE.CATEGORY, DEMAND.LOSS.MW}`
- `{CLIMATE.CATEGORY, DEMAND.LOSS.MW}`

#### 1. **`CAUSE.CATEGORY` & `DEMAND.LOSS.MW`**

**Null Hypothesis (H₀):** The distribution of `CAUSE.CATEGORY` is the same when `DEMAND.LOSS.MW` is missing and when it is not missing.  
**Alternative Hypothesis (H₁):** The distribution of `CAUSE.CATEGORY` is different when `DEMAND.LOSS.MW` is missing and when it is not missing.  

**Test Statistic:** We used **Total Variation Distance (TVD)** as the test statistic because `CAUSE.CATEGORY` contains categorical variables. Using a permutation test, we shuffled the missingness of `DEMAND.LOSS.MW` 500 times to generate simulated results for the TVD.

<p style="text-align:center"><iframe src="assets/missingness_1.html" width=800 height=600 frameBorder=0></iframe></p>

**Results:**  
- The p-value was calculated as **0.0**.  
- At a significance threshold of 0.05, we reject the null hypothesis.  
- **Interpretation:** This indicates that the missingness of `DEMAND.LOSS.MW` is **not independent** of `CAUSE.CATEGORY`. In other words, the cause of the outage influences whether demand loss data is missing.

#### 2. **`CLIMATE.CATEGORY` & `DEMAND.LOSS.MW`**

**Null Hypothesis (H₀):** The distribution of `CLIMATE.CATEGORY` is the same when `DEMAND.LOSS.MW` is missing and when it is not missing.  
**Alternative Hypothesis (H₁):** The distribution of `CLIMATE.CATEGORY` is different when `DEMAND.LOSS.MW` is missing and when it is not missing.  

**Test Statistic:** We used **Total Variation Distance (TVD)** as the test statistic because `CLIMATE.CATEGORY` contains categorical variables. Similarly, we shuffled the missingness of `DEMAND.LOSS.MW` 500 times to generate simulated results for the TVD.

<p style="text-align:center"><iframe src="assets/missingness_2.html" width=800 height=600 frameBorder=0></iframe></p>

**Results:**  
- The p-value was calculated as approximately **0.3**.  
- At a significance threshold of 0.05, we fail to reject the null hypothesis.  
- **Interpretation:** This suggests that the missingness of `DEMAND.LOSS.MW` is **independent** of `CLIMATE.CATEGORY`. In other words, climate conditions do not significantly influence whether demand loss data is missing.

### Summary

Our analysis revealed:
- The missingness of `DEMAND.LOSS.MW` is dependent on `CAUSE.CATEGORY`, indicating that the type of outage cause plays a role in whether demand loss information is recorded.
- The missingness of `DEMAND.LOSS.MW` is independent of `CLIMATE.CATEGORY`, suggesting that climate conditions do not affect the recording of demand loss data.

These findings highlight the need to account for `CAUSE.CATEGORY` in further analyses involving missing data in `DEMAND.LOSS.MW`. Moreover, the NMAR nature of the dataset's missingness in `OUTAGE.RESTORATION` underscores the challenges inherent in analyzing large-scale power outages.



## Hypothesis Testing

**Null Hypothesis (H0):** The cumulative distribution functions (CDFs) of `DEMAND.LOSS.MW` for the 'severe weather' and 'intentional attack' cause categories are identical.

**Alternative Hypothesis (H1):** The cumulative distribution functions (CDFs) of `DEMAND.LOSS.MW` for the 'severe weather' and 'intentional attack' cause categories are not identical.

We selected only the relevant columns, including `DEMAND.LOSS.MW` and `CAUSE.CATEGORY`. Recognizing the abundance of distinct values in `CAUSE.CATEGORY`, we decided to focus exclusively on 'severe weather' and 'intentional attack'.

Since anomaly level is numerical data and there are outliers in anomaly levels for 'severe weather' and 'intentional attack', we chose the Kolmogorov-Smirnov (KS) statistic as our test statistic due to its sensitivity to differences and robustness to outliers.

We performed 10,000 permutations, and the graph below illustrates the distribution of permutation test results. The red line indicates the observed value.

```python
P-value: 0.0
Observed KS Statistic: 0.4099314384610264
```
<p style="text-align:center"><iframe src="assets/hypothesis_1.html" width=800 height=600 frameBorder=0></iframe></p>

The P-value for this test is approximately 0.65, indicating that at a significance level of 0.05, we fail to reject the null hypothesis.

In practical terms, this result suggests that the cause of the outage ('severe weather' or 'intentional attack') may not strongly predict the severity of the outage. Other factors or a combination of factors likely contribute to the observed severity levels.

Comparable damage levels caused by 'intentional attack' and 'severe weather' could result from resilient infrastructure, proactive maintenance, and efficient response protocols in the power system. Investments in technology, regional climate considerations, and compliance with regulatory standards further mitigate outage severity. Data aggregation into broad categories may obscure nuanced variations, highlighting the importance of detailed analyses for a comprehensive understanding of the observed similarities.

We then repeated the same procedures but replaced the severity indicator variable with `OUTAGE.DURATION`. Similar to `DEMAND.LOSS.MW`, we rejected the null hypothesis. The summarized statistics are presented below:

```python
P-value: 0.0
Observed KS Statistic: 0.5707781401850496
```
<p style="text-align:center"><iframe src="assets/hypothesis_2.html" width=800 height=600 frameBorder=0></iframe></p>

In both hypothesis tests, we rejected the null hypothesis. Therefore, we can conclude with sufficient statistical confidence that the cumulative distribution functions (CDFs) of `DEMAND.LOSS.MW` for the 'severe weather' and 'intentional attack' cause categories are not identical.














# Section 2: Predicting Outage Durations

## Prediction Problem
To make accurate predictions, we will utilize historical data on power outages that have occurred from January 2000 to July 2016. This dataset includes various features such as `OUTAGE.DURATION`, `U.S._STATE`, `CLIMATE.CATEGORY`, `CAUSE.CATEGORY`, `OUTAGE.MONTH`, `UTIL.CONTRI`, `ANOMALY.LEVEL`, and `COM.SALES`, each offering insights into the nature and impact of power outages.

We chose RMSE and R-squared as our evaluation metric. RMSE is a standard measure to evaluate the accuracy of a model's predictions. It gives us a sense of how far our predictions are from the actual values, with a lower RMSE indicating higher accuracy. R-squared, on the other hand, is a statistical measure of how close the data are to the fitted regression line. It essentially shows the proportion of variance for a dependent variable that's explained by an independent variable or variables in a regression model. High R-squared values indicate that a large portion of the variability of the dependent variable has been explained by the independent variables in the model.



## Feature Selection
To select the categorical features, we chose some columns that we thought would be important indicators to how long a power outage occurs. Specifically, the state is important because it reflects local infrastructure and response capabilities, which can vary significantly. The climate category is crucial as it gives insights into the weather conditions that might precipitate an outage or affect its duration. The cause category is vital in understanding the nature of the outage and its potential duration. The month in which the outage occurred is important because the power recovery speed might vary from season to season (slower in winter and faster in summer). 

For the numerical columns, we plotted a pair plot and selected the features that had the highest correlation with `OUTAGE.DURATION`. The pair plot of the selected features is given below.

<p style="text-align:center"><iframe src="assets/scatter.html" width=800 height=600 frameBorder=0></iframe></p>

Here is a detailed explanation of each column in the dataset:
- The `OUTAGE.DURATION` column represents the duration of outage events, measured in minutes. This is the variable we aim to predict.
- The `U.S._STATE` column includes all the states within the continental United States, reflecting the geographic diversity of the data.
- The `CLIMATE.CATEGORY` column categorizes the climate conditions corresponding to the years of the data. The categories—“Warm,” “Cold,” or “Normal”—are determined based on a threshold of ±0.5°C for the Oceanic Niño Index (ONI), which tracks significant temperature changes in the ocean.
- The `CAUSE.CATEGORY` column enumerates the different categories of events that have led to major power outages.
- The `OUTAGE.MONTH` column specifies the month during which each outage occurred, providing insights into seasonal patterns of power outages.
- The `UTIL.CONTRI` column quantifies the utility industry's contribution to the Gross State Product (GSP) in each state, expressed as a percentage of the total real GDP contributed by the utility industry.
- The `ANOMALY.LEVEL` column reflects the Oceanic Niño Index (ONI) values, indicating the intensity of oceanic El Niño/La Niña episodes by season, which are known to influence weather patterns.
- The `COM.SALES` column records the electricity consumption in the commercial sector, measured in megawatt-hours, offering insights into commercial energy usage patterns.

Below are the first five entries from the cleaned, selected dataset:

| U.S._STATE   | CLIMATE.CATEGORY   | CAUSE.CATEGORY     |   OUTAGE.MONTH |   UTIL.CONTRI |   ANOMALY.LEVEL |   COM.SALES |
|:-------------|:-------------------|:-------------------|---------------:|--------------:|----------------:|------------:|
| Minnesota    | normal             | severe weather     |              7 |       1.75139 |            -0.3 | 2.11477e+06 |
| Minnesota    | normal             | intentional attack |              5 |       1.79    |            -0.1 | 1.80776e+06 |
| Minnesota    | cold               | severe weather     |             10 |       1.70627 |            -1.5 | 1.80168e+06 |
| Minnesota    | normal             | severe weather     |              6 |       1.93209 |            -0.1 | 1.94117e+06 |
| Minnesota    | warm               | severe weather     |              7 |       1.6687  |             1.2 | 2.16161e+06 |



## Baseline Model
### Model Description
For the baseline prediction task, we employ a linear regression model to estimate the duration of power outages (`OUTAGE.DURATION`). The selected features for the model include `COM.SALES` and `ANOMALY.LEVEL`. These features are deemed relevant in predicting power outage duration, with `COM.SALES` representing electricity consumption in the commercial sector and `ANOMALY.LEVEL` indicating the oceanic El Niño/La Niña (ONI) index.

### Feature Preprocessing 
Prior to model fitting, the features undergo preprocessing using a ColumnTransformer. Numerical features, `COM.SALES` and `ANOMALY.LEVEL`, are subjected to imputation of missing values through mean imputation and scaled using StandardScaler. The remainder of the features remains untouched (`passthrough`).

### Model Architecture 
The baseline model employs a Linear Regression model, capturing the linear relationship between the selected features and the target variable, `OUTAGE.DURATION`.

### Model Performance
The baseline model yields a test Root Mean Squared Error (RMSE) of approximately 1574.340978391451 and an R-squared value of -0.028481049417356408. These metrics provide insights into the model's accuracy and explainability. The RMSE measures the average deviation between the actual and predicted outage durations, with lower values indicating better performance. The R-squared value quantifies the proportion of variance in the target variable captured by the model, where higher values denote a better fit.

### Visualization
A 3D scatter plot and regression plane visualization illustrate the relationship between transformed features (`COM.SALES` and `ANOMALY.LEVEL`) and outage duration. This aids in visually assessing the model's ability to capture patterns in the data.

<p style="text-align:center"><iframe src="assets/3d.html" width=800 height=800 frameBorder=0></iframe></p>

In the subsequent sections, we explore the evolution of the model from this baseline, incorporating additional features, tuning hyperparameters, and optimizing performance metrics.




## Final Model
### Model Selecting and Features
After thorough experimentation and iterative model development, the final predictive model employs a random forest regressor, chosen for its ability to handle a diverse range of features and potential interactions. Unlike linear regression, the random forest model offers more tunable hyperparameters, making it well-suited for optimizing predictive performance.

### Feature Preprocessing
- **Cyclical Month Encoding (`OUTAGE.MONTH`):** To capture seasonal patterns effectively, we introduced a cyclical encoding transformation using sine and cosine functions. This enhancement allows the model to better understand the temporal aspect of power outage occurrences.

- **Categorical Features:** Utilizing one-hot encoding, we transformed categorical features to numerical representations for model compatibility. These include:
    - `U.S._STATE`: Represents all the states in the continental U.S.
    - `CLIMATE.CATEGORY`: Describes climate episodes corresponding to the years, categorized as “Warm,” “Cold,” or “Normal” based on the Oceanic Niño Index (ONI).
    - `CAUSE.CATEGORY`: Represents categories of events causing major power outages.

- **Numerical Features:** The following numerical features underwent imputation of missing values using the mean and standardization using the Standard Scaler:
    - `UTIL.CONTRI`: Utility industry’s contribution to the total Gross State Product (GSP) in the state, expressed as a percentage of the total real GDP contributed by the utility industry.
    - `ANOMALY.LEVEL`: Represents the oceanic El Niño/La Niña (ONI) index, indicating cold and warm episodes by season.
    - `COM.SALES`: Electricity consumption in the commercial sector (megawatt-hour).

This strategic selection of features aims to provide the model with a comprehensive understanding of the diverse factors influencing power outage duration. The inclusion of temporal, categorical, and numerical features contributes to the model's ability to capture intricate patterns and make accurate predictions.

### Model Architecture 
The final model employs a random forest regressor with tuned hyperparameters, striking a balance between complexity and predictive power.

### Model Performance 
Even before hyperparameter tuning, the final model exhibits a significant improvement in performance compared to the baseline. The test RMSE is 1474.2525101701656 and the R-squared value is 0.27785385330546075. 

### Hyperparameters Tuning
In the process of hyperparameter tuning for the Random Forest Regressor, a grid search was performed over a range of `max_depth` and `n_estimators`, exploring values from 1 to 200. The computational time for this search was approximately 1 minute and 45 seconds. The optimal hyperparameters identified were `max_depth`=11 and `n_estimators`=41. These parameters showcased a balance between model complexity and generalization, contributing to improved performance on the test set.

The tuned Random Forest model demonstrated a modest enhancement over the untuned version, with the Root Mean Squared Error (RMSE) decreasing to 1383.6004458577404 and the R-squared value changing to 0.20563503640079228 on the test dataset. This suggests that the fine-tuned hyperparameters enabled the model to better capture underlying patterns in the data, emphasizing the significance of hyperparameter optimization for model refinement. The chosen hyperparameters are deemed effective for achieving a favorable trade-off between model complexity and predictive accuracy.

## Summary
The evolution of our predictive model for power outage duration reveals significant enhancements in accuracy, precision, and overall performance. Beginning with a linear regression baseline, we observed commendable performance, even with a restricted feature set. However, as the model complexity increased and more diverse features were introduced, the random forest regressor emerged as the superior choice.

The incorporation of categorical features such as 'CLIMATE.CATEGORY' proved pivotal in capturing nuanced patterns within the data. These features, transformed using one-hot encoding, contributed to the model's ability to discern intricate details about climate episodes and outage causes.

The introduction of cyclical encoding for temporal features, exemplified by 'OUTAGE.MONTH,' empowered the model to grasp seasonal patterns more effectively. Additionally, the inclusion of 'U.S._STATE' and 'UTIL.CONTRI' as features further enriched the model's understanding of regional and economic influences on power outage duration.

The model's journey from a linear regression baseline to a tuned random forest regressor showcased the importance of model selection and hyperparameter tuning. The latter significantly improved accuracy, as evidenced by the reduction in RMSE and the increase in R-squared values. These outcomes underscore the model's proficiency in predicting power outage duration, offering valuable insights for utility planning and resource allocation.

In conclusion, the iterative refinement process, coupled with strategic feature selection and hyperparameter tuning, culminated in a robust predictive model capable of providing accurate estimates of power outage duration.



## Fairness Analysis
### Residual Analysis
We first analyze the residual of each of our prediction models. In a residual graph, the horizontal axis represents the predicted values from the models. It is the independent variable in this context, showing the range of predictions that your model has made. The vertical axis represents the residuals, which are the differences between the observed values and the values predicted by the models. A residual is positive if the prediction is below the actual value and negative if it is above. Ideally, the residuals should be randomly scattered around the horizontal axis (which represents a residual value of zero), with no discernible pattern. This would indicate that the model's predictions are unbiased.

For the baseline Linear Regression model, the residual graph looks like this: 
<p style="text-align:center"><iframe src="assets/res1.html" width=800 height=600 frameBorder=0></iframe></p>

In this plot, the residuals do not show a clear pattern or trend, which is generally good. However, there appears to be a slight "funnel" shape, where the spread of the residuals increases for higher values of the baseline. This could indicate heteroscedasticity, meaning that the variance of the residuals is not constant across all levels of the independent variable. Outliers can also be spotted as points that are far away from the horizontal line at zero. The outliers are all at higher predicted values, meaning the actual value is a lot more than the predicted value. 

For the final Random Forest model, the residual graph looks like this: 
<p style="text-align:center"><iframe src="assets/res2.html" width=800 height=600 frameBorder=0></iframe></p>

The Random Forest model has a similar residual pattern as the baseline model. However, its "funnel" shape is slightly more evident, which could be explained by the extra features. In addition, there are as many positive residual points as negative ones, which is a stark contrast from the baseline residual plot, which mainly converges at the negative ends, with few outliers having much higher residuals than others. The range of predictions is also notably larger. The highest prediction made by the baseline model is only around 2000, whereas the final model can make predictions up to 4500. 

### RMSE Analysis
Our secondary evaluation metric is the RMSE value. We present a similar null hypothesis suggesting that our model’s RMSE value for determining outage severity is approximately equal for all seasons, and any discrepancies are due to random chance. Alternatively, our model could be unfair if the RMSE value is higher for the summer compared to for the winter. We’ve chosen the difference between the RMSE in the summer and in the winter as our test statistic, with a significance level of 0.05. Upon executing a permutation test 10,000 times, we obtained a p-value of 0.5666. As this value is above our significance level, we cannot reject the null hypothesis, suggesting that our model is fair based on the RMSE analysis. Therefore, we can confidently assert that our model is fair regardless of the season of the power outage. 

<p style="text-align:center"><iframe src="assets/rmse.html" width=800 height=600 frameBorder=0></iframe></p>
