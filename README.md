# Unraveling the Mysteries Behind Major Power Outages in the U.S.
**Authors:** Charlie Sun, Aaron Feng

## Project Overview

This project investigates the risks and causes of severe weather-induced power outages in the United States, using data from January 2000 to July 2016. It focuses on major outages affecting over 50,000 customers or causing losses above 300 MW. The study aims to understand the correlation between different causes of power outages and their impact on people's lives. The project involves comprehensive data cleaning, exploratory data analysis, assessment of missingness, and hypothesis testing to assess the dependency of outage severity on various factors like climate conditions and causes. Key findings suggest a significant impact of severe weather and intentional attacks on power outage severity.

## Investigating Topic and Introduction

Electricity is the cornerstone of modern civilization, invisibly powering homes, fueling industries, and charging technologies. However, its continuous supply is not always assured. The United States has experienced significant power outages, highlighting the fragility of the national energy grid. Our project explores the risks and causes of severe weather-induced power outages, using a dataset covering incidents from January 2000 to July 2016. These events affected over 50,000 customers or resulted in losses exceeding 300 MW. We aim to discern how various outage causes distinctly impact human life by examining the correlation between different outage triggers.

The dataset includes columns such as `U.S._STATE`, `OUTAGE.START`, `OUTAGE.RESTORATION`, `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, `CLIMATE.CATEGORY`, and `CAUSE.CATEGORY`, each offering insights into the nature and impact of power outages.

Through the study, we hope to answer, "How do power outages that stem from different causes affect people's lives differently?" In order to answer this question, we will investigate the correlation between different causes of power outages and see if their distributions are different. 

Here is a detailed explanation of each column in the dataset:

- The `U.S._STATE` column represents all the states in the continental U.S.

- The `OUTAGE.START` column indicates the time in pandas datetime format when the outage event started (as reported by the corresponding Utility in the region). 

- The `OUTAGE.RESTORATION` column indicates the time in pandas datetime format when power was restored to all the customers (as reported by the corresponding Utility in the region).

- The `OUTAGE.DURATION` column means duration of outage events (in minutes).

- The `DEMAND.LOSS.MW` column represents the mount of peak demand lost during an outage event in Megawatts. 

- The `CLIMATE.CATEGORY` column represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI).

- The `CAUSE.CATEGORY` column includes the categories of all the events causing the major power outages.

## Cleaning and EDA
### Data Cleaning

Our initial dataset comprised 55 columns. To enhance data readability and accuracy, we performed the following cleaning steps: 
1. Deleting instruction rows in data: When we first imported the raw dataset, we realized that the first few rows and columns at the very beginning are instructions on how to use the dataset, not data itself. As a result, we delete those rows and columns. 
2. Cleaning `OUTAGE.START` column: Combine the column `OUTAGE.START.DATE` and `OUTAGE.START.TIME` into just one `pd.Timestamp` column called `OUTAGE.START`. 
3. Cleaning `OUTAGE.RESTORATION` column: Uing the same technique, we also cleaned `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` into `OUTAGE.RESTORATION`. 
4. Column selection: We selected a few of the columns based on interest. Since we want to study the relationship between climate, cause of the outage, and how severe the outage is, we selected seven columns. 
5. Cleaning outliers: After graphing the distribution of `OUTAGE.DURATION` and `DEMAND.LOSS.MW` in histograms, we found that each of these distributions were very skewed. To eliminate outliers that are too extreme, we used the IQR to clean the outliers from these distributions. 

Finally, in the remaining seven columns, we deemed columns `CLIMATE.CATEGORY` and `CAUSE.CATEGORY` as causal indicator variables, as they characterizes the cause of the outage. We also labeled columns `OUTAGE.DURATION` and `DEMAND.LOSS.MW` as severness indicator variables, as they reveal how severe the power outages are. 

Here's the first few rows of the cleaned dataframe: 

| U.S. STATE   | OUTAGE. START       | OUTAGE. RESTORATION  |  OUTAGE. DURATION |  ANOMALY. LEVEL | CLIMATE. CATEGORY  | CAUSE. CATEGORY    |
|:-------------|:--------------------|:---------------------|------------------:|----------------:|:-------------------|:-------------------|
| Minnesota    | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |              3060 |            -0.3 | normal             | severe weather     |
| Minnesota    | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |                 1 |            -0.1 | normal             | intentional attack |
| Minnesota    | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |              3000 |            -1.5 | cold               | severe weather     |
| Minnesota    | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |              2550 |            -0.1 | normal             | severe weather     |
| Minnesota    | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |              1740 |             1.2 | warm               | severe weather     |

### Univariate Analysis

We began our analysis by examining the distribution of severeness indicators like `OUTAGE.DURATION` and `DEMAND.LOSS.MW`. 

<p style="text-align:center"><iframe src="assets/outage_duration.html" width=800 height=600 frameBorder=0></iframe></p>

The plot demonstrates that `OUTAGE.DURATION` is skewed the right. The leftmost outage categoriazation, short duration outage (<1000), is the most common. The rest of the outage duration distribution are quite average.

<p style="text-align:center"><iframe src="assets/demand_loss.html" width=800 height=600 frameBorder=0></iframe></p>

Simillar to the outage duration distribution, the plot above is also skewed to the right. It reveals that small demand losses (< 100MW) are the most common.

Then, we could take a look at the causal indicator variables. 

| Causes   |   CLIMATE.CATEGORY |
|:---------|-------------------:|
| normal   |                620 |
| cold     |                387 |
| warm     |                248 |

As we see in the dataframe above, most of the power outages happen in places where the climate is normal. 

| Causes                        |   CAUSE.CATEGORY |
|:------------------------------|-----------------:|
| severe weather                |              543 |
| intentional attack            |              415 |
| system operability disruption |              112 |
| public appeal                 |               64 |
| equipment failure             |               54 |

As we see the dataframe above, severe weather and itentional attack cause most of the outages. While system operability disruption, public appeal, and quipment failure causes the smallest portion of the outages.

To see the distribution of location, we could create a heatmap using folium.

<p style="text-align:center"><iframe src="assets/heatmap.html" width=800 height=400 frameBorder=0></iframe></p>

As we see on the map, northeastern and midwestern states suffer most of the outages. We guess that population and GDP may have a correlation with amount of outages as California, Texas, and Washington have the greatest amount of outages. 


### Bivariate Analysis

We want to start by analyzing the relationship between `OUTAGE.DURATION` and `CLIMATE.CATEGORY`. 

<p style="text-align:center"><iframe src="assets/biv_1.html" width=800 height=600 frameBorder=0></iframe></p>

The box plot depicts three climate categories: warm, cold, and normal, the conditions during which the power outages occurred. The yxaxis indicates the outage duration measured in minutes. 

The three distributions have similar IQR and shape. However, the median and 75% quartile outage duration is the highest in cold climate. The warm category has the fewest outliers, and the normal category has the most outliers. Overall, the cold climate tends to have longer and more variable outage durations. In contrast, the warm climate tends to have shorter and more consistent durations. This confirms our prediction that power outages are more likely to be severe and impactful in cold climates, where it snows more often than in the warm climates, where there's sunshine most of the time. 

From this box plot, one could hypothesize that climate conditions do not have a strong impact on both the duration and variability of power outages. Therefore, in our hypothesis tests, we will likely not adopt `CLIMATE.CATEGORY` as a feature variable. 

Next, we will use another box plot to analyze the influence of different `CAUSE.CATEGORY`s on the `DEMAND.LOSS.MW` of the outage.

<p style="text-align:center"><iframe src="assets/biv_2.html" width=800 height=600 frameBorder=0></iframe></p>

The box plot shows us that the demand loss values are numeric, continuous, and possibly standardized around zero, which may indicate a deviation from a norm or average situation. Several categories of outage causes are included, such as islanding, fuel supply emergency, public appeal, equipment failure, system operability disruption, intentional attack, and severe weather.

In the plot, every cause category's distribution looks vastly different. The most notable/unique categories are: 
- severe weather: this distribution has a very large IQR and outliers. Its range is the largest of all distributions, and it is also the most balanced distributions. 
- intentional attack: This distribution has an extremely slim IQR, not even visible on the plot. Since its median and quartile ranges lie around `DEMAND.LOSS.MW` = 0, but its outliers span all the way to the largest values, it means that although the majority of the power outages caused by intentaionl attacks do not cost a lot of power, some well-planned attacks can be very costly. 

In summary, the box plot suggests that different causes of power outages result in varying levels of power loss. Severe weather seems to be have the most stable amount of demands loss, while intentional attacks appear to have the most variable distribution. This analysis helps us in understanding which types of events create more significant deviations from normal operating conditions. In the hypothesis testing section, we will be analyzing the relationship between severe weather outages and intentional attack outages, using their demand loss as the standard. 

### Interesting Aggregates 

We first created a pivot table. 

|   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
|              338.5  |                 1521.57 |              328.557 |    259.267  |        1124.37  |          1808.41 |                         590.125 |
|              445.76 |                 1583.2  |              333.366 |     72.6875 |        1385.58  |          1820.49 |                         276.583 |
|              505    |                  255    |              312.557 |    209.833  |         625.333 |          1777.95 |                         495.571 |

The mean of outage duration in each column varies indicating that the there may exits an correlation between `OUTAGE.DURATION` and {`CAUSE.CATEGROY` and `CLIMATE.CATEGORY`}. 

Then, using the pivot table, we created a grouped bar plot. 

<p style="text-align:center"><iframe src="assets/aggregates.html" width=800 height=600 frameBorder=0></iframe></p>

With the Bar Plot computed from the pivot table above, the difference in mean can be more easily observed, especally between warm category and the other two climate category.

## Assessment of Missingness

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