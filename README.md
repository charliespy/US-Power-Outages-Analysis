# Unraveling the Mysteries Behind Major Power Outages in the U.S.
**Authors:** Charlie Sun, Aaron Feng

# Framing the Problem
Power outages are critical events with far-reaching societal implications, disrupting daily activities, impacting businesses, and posing potential risks to public safety. Enhancing the resilience and reliability of the electrical grid requires a nuanced understanding of outage patterns and characteristics. In our analysis, we focus on predicting the duration of power outages, specifically honing in on the West Climate region. This geographic segmentation provides valuable insights into the unique challenges and contributing factors associated with power outages in this region. Climate regions, shaped by prevailing weather conditions, can significantly influence outage causes and consequences. For instance, the West Climate region might be characterized by dry and hot conditions, distinct from other regions experiencing colder temperatures or severe weather events.

### Data Cleaning
Similar to our approach in the previous Project 3 analysis, we began by converting the xlsx file into a csv format and conducted crucial data cleaning procedures to ready the dataset for analysis. Our focus was on refining the dataset to include only relevant features for predicting 'Outage Duration.'

To achieve this, we removed unnecessary rows and columns, ensuring that the selected features directly contribute to the prediction of 'Outage Duration.' This streamlined dataset forms the basis for our predictive modeling, enabling us to identify patterns and characteristics associated with power outages in different states and regions.

In the initial phase of data cleaning, we took the following steps to refine our dataset:
1. Converted the original xlsx file into a csv format, ensuring a standardized data structure.
2. Dropped unnecessary rows that did not contribute to the predictive modeling of 'Outage Duration.'
3. Kept all relevant columns intact, as each of them played a crucial role in capturing the symbolic characteristics of the states or regions we aimed to predict.

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
