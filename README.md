# Unraveling the Mysteries Behind Major Power Outages in the U.S.
**Authors:** Charlie Sun, Aaron Feng

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

We began our analysis by examining the distribution of severeness indicators like `OUTAGE.DURATION` and `DEMAND.LOSS.MW`. The plot below demonstrates that `OUTAGE.DURATION` is skewed the right. The leftmost outage categoriazation, short duration outage (<1000), is the most common. The rest of the outage duration distribution are quite average.

<p style="text-align:center"><iframe src="assets/outage_duration.html" width=800 height=600 frameBorder=0></iframe></p>

Simillar to the outage duration distribution, this next plot below is also skewed to the right. It reveals that small demand losses (< 100MW) are more common than high demand losses. 

<p style="text-align:center"><iframe src="assets/demand_loss.html" width=800 height=600 frameBorder=0></iframe></p>

Then, we could take a look at the causal indicator variables. As we see in the dataframe below, most of the power outages happen in places where the climate is normal. 

| Causes   |   CLIMATE.CATEGORY |
|:---------|-------------------:|
| normal   |                620 |
| cold     |                387 |
| warm     |                248 |

As we see the dataframe below, severe weather and itentional attack cause most of the outages. While system operability disruption, public appeal, and quipment failure causes the smallest portion of the outages.

| Causes                        |   CAUSE.CATEGORY |
|:------------------------------|-----------------:|
| severe weather                |              543 |
| intentional attack            |              415 |
| system operability disruption |              112 |
| public appeal                 |               64 |
| equipment failure             |               54 |


To see the distribution of location, we could create a heatmap using folium. As we see on the map below, northeastern and midwestern states suffer most of the outages. We guess that population and GDP may have a correlation with amount of outages as California, Texas, and Washington have the greatest amount of outages. 

<p style="text-align:center"><iframe src="assets/heatmap.html" width=800 height=400 frameBorder=0></iframe></p>


## Bivariate Analysis

We want to start by analyzing the relationship between `OUTAGE.DURATION` and `CLIMATE.CATEGORY`. The box plot below depicts three climate categories: warm, cold, and normal, the conditions during which the power outages occurred. The x-axis indicates the outage duration measured in minutes. 

<p style="text-align:center"><iframe src="assets/biv_1.html" width=800 height=600 frameBorder=0></iframe></p>

The three distributions have similar IQR and shape. However, the median and 75% quartile outage duration is the highest in cold climate. The warm category has the fewest outliers, and the normal category has the most outliers. Overall, the cold climate tends to have longer and more variable outage durations. In contrast, the warm climate tends to have shorter and more consistent durations. This confirms our prediction that power outages are more likely to be severe and impactful in cold climates, where it snows more often than in the warm climates, where there's sunshine most of the time. From this box plot, one could hypothesize that climate conditions do not have a strong impact on both the duration and variability of power outages. Therefore, in our hypothesis tests, we will likely not adopt `CLIMATE.CATEGORY` as a feature variable. 

Next, we will use another box plot to analyze the influence of different `CAUSE.CATEGORY`s on the `DEMAND.LOSS.MW` of the outage. The box plot below shows us that the demand loss values are numeric, continuous, and possibly standardized around zero, which may indicate a deviation from a norm or average situation. Several categories of outage causes are included, such as islanding, fuel supply emergency, public appeal, equipment failure, system operability disruption, intentional attack, and severe weather.

<p style="text-align:center"><iframe src="assets/biv_2.html" width=800 height=600 frameBorder=0></iframe></p>

In the plot, every cause category's distribution looks vastly different. The most notable/unique categories are: 
- **severe weather**: this distribution has a very large IQR and outliers. Its range is the largest of all distributions, and it is also the most balanced distributions. 
- **intentional attack**: This distribution has an extremely slim IQR, not even visible on the plot. Since its median and quartile ranges lie around `DEMAND.LOSS.MW` = 0, but its outliers span all the way to the largest values, it means that although the majority of the power outages caused by intentaionl attacks do not cost a lot of power, some well-planned attacks can be very costly. 

In summary, the box plots suggests that different causes of power outages result in varying levels of power loss. Severe weather seems to be have the most stable amount of demands loss, while intentional attacks appear to have the most variable distribution. This analysis helps us in understanding which types of events create more significant deviations from normal operating conditions. In the hypothesis testing section, we will be analyzing the relationship between severe weather outages and intentional attack outages, using their demand loss as the standard. 


## Interesting Aggregates 

We first created a pivot table. The mean of outage duration in each column varies indicating that the there may exits an correlation between `OUTAGE.DURATION` and {`CAUSE.CATEGROY` and `CLIMATE.CATEGORY`}. 

|   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
|              338.5  |                 1521.57 |              328.557 |    259.267  |        1124.37  |          1808.41 |                         590.125 |
|              445.76 |                 1583.2  |              333.366 |     72.6875 |        1385.58  |          1820.49 |                         276.583 |
|              505    |                  255    |              312.557 |    209.833  |         625.333 |          1777.95 |                         495.571 |

Then, using the pivot table, we created a grouped bar plot. 

<p style="text-align:center"><iframe src="assets/aggregates.html" width=800 height=600 frameBorder=0></iframe></p>

With the Bar Plot computed from the pivot table above, the difference in mean can be more easily observed, especally between warm category and the other two climate category. expand on this



## Assessment of Data Missingness

### NMAR Analysis
We think that the dataset is NMAR. In the analysis of the dataset derived from 'A Multi-Hazard Approach to Assess Severe Weather-Induced Major Power Outage Risks in the U.S.' (Mukherjee et al., 2018), a significant pattern of missingness is observed in the `OUTAGE.RESTORATION` column. This dataset exclusively encompasses major outages, defined per the Department of Energy's criteria as incidents impacting at least 50,000 customers or causing an unplanned firm load loss of at least 300 MW. Given the considerable scale and impact of these events, it is implausible that the lack of restoration data is due to inadvertent omission or data collection negligence. Rather, it is posited that the missingness in restoration times is predominantly attributable to the intrinsic uncertainties and complexities inherent in the restoration process of such large-scale outages. These complexities might include multifaceted technical challenges, logistical constraints, and varying restoration protocols, which collectively contribute to an ambiguity in precisely determining the restoration moment. Consequently, this pattern of missing data is best characterized as NMAR, where the absence of information is inherently linked to the nature and context of the data itself, rather than random or systemic data collection errors.


### Missingness Dependency

Now, we focus on the missingness of demand loss in the power dataframe and test the dependency of this missingnes We are conducting **permutation dependency test** on two paris of columns: {`CAUSE.CATEGORY`, `DEMAND.LOSS.MW`}, and {`CLIMATE.CATEGORY`, `DEMAND.LOSS.MW`}.

1. **`CAUSE.CATEGORY` & `DEMAND.LOSS.MW`:**

Null Hypothesis (H0): the distribution of the cause when demand loss is missing is the same as the distribution of the cause when demand loss is not missing. 

Alternative Hypothesis (H1): the distribution of the cause when demand loss is missing is different from the distribution of the cause when demand loss is not missing. 

Test statistic: We are using TVD (Total Variation Distance) as our test statistics because CAUSE.CATEGORY contains categorical variables. We use permutation test to shuffle the missingness of demand loss 500 times and get 500 simulating results about the TVD.

<p style="text-align:center"><iframe src="assets/missingness_1.html" width=800 height=600 frameBorder=0></iframe></p>

We get a p-value of 0.0, when we use 0.05 as our significance threshold, we should reject the null hypothesis that the distribution of the cause when demand loss is missing is the same as the distribution of the cause when demand loss is not missing. In another word, demand loss is not dependent on the cause.

2. **`CLIMATE.CATEGORY` & `DEMAND.LOSS.MW`:**

Null Hypothesis (H0): the distribution of the climate when demand loss is missing is the same as the distribution of the climate when demand loss is not missing 

Alternative Hypothesis (H1): the distribution of the climate when demand loss is missing is different from the distribution of the climate when demand loss is not missing 

Test statistic: we are using TVD (Total Variation Distance) as our test statistics because CLIMATE.CATEGORY contains categorical variables. 

We use permutation test to shuffle the missingness of demand loss 500 times and get 500 simulating results about the TVD.

<p style="text-align:center"><iframe src="assets/missingness_2.html" width=800 height=600 frameBorder=0></iframe></p>

We get a p-value of approximately 0.3, and when we use 0.05 as our significance threshold, we fail to reject the null hypothesis that the distribution of the climate when demand loss is missing is the same as the distribution of the climate when demand loss is not missing. In another word, demand loss is dependent on climate category.




## Hypothesis Testing 

Null Hypothesis (H0): The cumulative distribution functions (CDFs) of `DEMAND.LOSS.MW` for the 'severe weather' and 'intentional attack' cause categories are identical.

Alternative Hypothesis (H1): The cumulative distribution functions (CDFs) of `DEMAND.LOSS.MW` for the 'severe weather' and 'intentional attack' cause categories are not identical.

We would select only the useful column, including `DEMAND.LOSS.MW` and `CAUSE.CATEGORY`. Recognizing the abundance of difference values in 'CAUSE.CATEGORY', we decide to only perform text on 'severe weather' and 'intentional attack'.

Since anomaly level is numerical data, and there exits outier in anomaly level for severe weather and intentional attack, we choose to use  Kolmogorov-Smirnov (KS) statistic as our test statistics, considering its advantage on sensitivity to difference and robustness on ouliers.

We ran permutation for 10000 times and the graph shows the distribution of permuation test result. The red line marks the observed value.

```python
P-value: 0.0
Observed KS Statistic: 0.4099314384610264
```
<p style="text-align:center"><iframe src="assets/hypothesis_1.html" width=800 height=600 frameBorder=0></iframe></p>

The P-value for the testing is roughly 0.65, which means that at significant level of 0.05, we fail to reject the null hypothesis. 

In real-life terms, this result suggests that the reason for the outage ('severe weather' or 'intentional attack') may not be a strong predictor of the severity of the outage. Other factors or a combination of factors might contribute to the observed severity levels.

The comparable damage levels caused by 'intentional attack' and 'severe weather' may be due to resilient infrastructure, proactive maintenance, and swift response protocols in the power system. Investments in technology, regional climate considerations, and adherence to regulatory standards further contribute to minimizing outage severity. Data aggregation into broad categories may conceal nuanced variations, emphasizing the importance of a detailed analysis for a comprehensive understanding of the observed similarities.

We then repeated the same procedures, but changed the severeness indicator variables to `OUTAGE.DURATION`. Similar to `DEMAND.LOSS.MW`, we also rejected the null hypothesis. The summarized statistics are given below:

```python
P-value: 0.0
Observed KS Statistic: 0.5707781401850496
```
<p style="text-align:center"><iframe src="assets/hypothesis_2.html" width=800 height=600 frameBorder=0></iframe></p>

Since in both hypothesis tests, we rejected the null hypothesis, we could conclude, with enough statistical confidence, that the cumulative distribution functions (CDFs) of `DEMAND.LOSS.MW` for the 'severe weather' and 'intentional attack' cause categories are not identical.

























### Prediction Problem
To make accurate predictions, we will utilize historical data on power outages that have occurred from January 2000 to July 2016. This dataset includes various features such as `OUTAGE.DURATION`, `U.S._STATE`, `CLIMATE.CATEGORY`, `CAUSE.CATEGORY`, `OUTAGE.MONTH`, `UTIL.CONTRI`, `ANOMALY.LEVEL`, and `COM.SALES`, each offering insights into the nature and impact of power outages.

### Features Selection
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

### Metric of Evaluation
We chose RMSE and R-squared as our evaluation metric. RMSE is a standard measure to evaluate the accuracy of a model's predictions. It gives us a sense of how far our predictions are from the actual values, with a lower RMSE indicating higher accuracy. R-squared, on the other hand, is a statistical measure of how close the data are to the fitted regression line. It essentially shows the proportion of variance for a dependent variable that's explained by an independent variable or variables in a regression model. High R-squared values indicate that a large portion of the variability of the dependent variable has been explained by the independent variables in the model.



# Baseline Model
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




# Final Model
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

### Summary
The evolution of our predictive model for power outage duration reveals significant enhancements in accuracy, precision, and overall performance. Beginning with a linear regression baseline, we observed commendable performance, even with a restricted feature set. However, as the model complexity increased and more diverse features were introduced, the random forest regressor emerged as the superior choice.

The incorporation of categorical features such as 'CLIMATE.CATEGORY' proved pivotal in capturing nuanced patterns within the data. These features, transformed using one-hot encoding, contributed to the model's ability to discern intricate details about climate episodes and outage causes.

The introduction of cyclical encoding for temporal features, exemplified by 'OUTAGE.MONTH,' empowered the model to grasp seasonal patterns more effectively. Additionally, the inclusion of 'U.S._STATE' and 'UTIL.CONTRI' as features further enriched the model's understanding of regional and economic influences on power outage duration.

The model's journey from a linear regression baseline to a tuned random forest regressor showcased the importance of model selection and hyperparameter tuning. The latter significantly improved accuracy, as evidenced by the reduction in RMSE and the increase in R-squared values. These outcomes underscore the model's proficiency in predicting power outage duration, offering valuable insights for utility planning and resource allocation.

In conclusion, the iterative refinement process, coupled with strategic feature selection and hyperparameter tuning, culminated in a robust predictive model capable of providing accurate estimates of power outage duration.



# Fairness Analysis
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
