# Unraveling the Mysteries Behind Major Power Outages in the U.S.
**Authors:** Charlie Sun, Aaron Feng

# Project Overview
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.



# Framing the Problem

Power outages are critical events with far-reaching societal implications, disrupting daily activities, impacting businesses, and posing potential risks to public safety. Enhancing the resilience and reliability of the electrical grid requires a nuanced understanding of outage patterns and characteristics.

In our analysis, we focus on predicting the duration of power outages, specifically honing in on the West Climate region. This geographic segmentation provides valuable insights into the unique challenges and contributing factors associated with power outages in this region. Climate regions, shaped by prevailing weather conditions, can significantly influence outage causes and consequences. For instance, the West Climate region might be characterized by dry and hot conditions, distinct from other regions experiencing colder temperatures or severe weather events.

Our predictive modeling aims to discern whether a power outage is likely to occur in the West Climate region. This knowledge is pivotal for utilities and policymakers to allocate resources effectively, implement preventive measures, and enhance response plans tailored to the distinctive challenges of the region.

To achieve this, we employ historical data on power outages, focusing on events post-2012. The dataset encompasses various features, including economic indicators like TOTAL.SALES and ANOMALY.LEVEL, as well as geographical and climate-related variables such as U.S._STATE, CLIMATE.CATEGORY, and CAUSE.CATEGORY. The predictive tool we develop is a classification model trained on this dataset. The goal is to provide an accurate prediction of whether a power outage is likely to occur in the West Climate region based on the available information at the time of prediction.

Our analysis involves a multistep process, including data cleaning, outlier removal, and model training. We start by loading the dataset and cleaning relevant columns, ensuring the data is accurate and suitable for analysis. Following this, we apply outlier removal techniques to enhance the model's robustness. The choice of features for prediction involves economic indicators, climate categories, and other relevant variables.

The baseline model, utilizing linear regression, serves as a starting point for comparison. Subsequently, we refine our approach with a more sophisticated model, incorporating geographical and climate-related features. The final model, a Random Forest Regressor, undergoes tuning to optimize its performance. The outcome is a predictive tool capable of estimating power outage duration, with a particular emphasis on the West Climate region.

Our analysis not only provides a valuable predictive tool for outage duration but also contributes to a deeper understanding of the factors influencing power outages in the West Climate region. This knowledge empowers stakeholders to make informed decisions, implement targeted interventions, and enhance the overall resilience of the electrical grid.

By training a classification model using this data, we aim to develop a predictive tool that can assist in identifying whether a power outage is likely to occur in the West Climate region, based on the available information at the time of prediction. This can ultimately contribute to more effective planning, preparation, and mitigation strategies for power outages.

Electricity is the cornerstone of modern civilization, invisibly powering homes, fueling industries, and charging technologies. However, power outages can have significant impacts on society, causing disruptions in daily life, affecting businesses, and posing potential risks to public safety. The United States has experienced significant power outages, highlighting the fragility of the national energy grid. Understanding the patterns and characteristics of power outages can help utilities and policymakers develop strategies to improve the resilience and reliability of the electrical grid. Our project explores the severity and causes of severe weather-induced power outages that affected over 50,000 customers or resulted in losses exceeding 300 MW. We aim to discern how various outage causes distinctly impact human life by examining the correlation between different outage triggers.

**Through the study, we hope to predict the severity of a power outage in the form of its outage duration.** In order to answer this question, we will investigate the correlation between different causes of power outages and see if their distributions are different.

<!-- To make accurate predictions, we will utilize historical data on power outages that have occurred from January 2000 to July 2016. This dataset includes various features such as U.S._STATE, OUTAGE.START, OUTAGE.RESTORATION, OUTAGE.DURATION, DEMAND.LOSS.MW, CLIMATE.CATEGORY, and CAUSE.CATEGORY, each offering insights into the nature and impact of power outages. -->

By training a linear regression and a random forest model using this data, we aim to develop a predictive tool that can assist in determining the duration of a power outage, based on the available information at the time of prediction. This can ultimately contribute to more effective planning, preparation, and mitigation strategies for power outages.

Here is a detailed explanation of each column in the dataset:
<!-- - The U.S._STATE column represents all the states in the continental U.S.
- The OUTAGE.START column indicates the time in pandas datetime format when the outage event started (as reported by the corresponding Utility in the region).
- The OUTAGE.RESTORATION column indicates the time in pandas datetime format when power was restored to all the customers (as reported by the corresponding Utility in the region).
- The OUTAGE.DURATION column means duration of outage events (in minutes).
- The DEMAND.LOSS.MW column represents the mount of peak demand lost during an outage event in Megawatts.
- The CLIMATE.CATEGORY column represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI).
- The CAUSE.CATEGORY column includes the categories of all the events causing the major power outages. -->



## Data Cleaning
Similar to our approach in the previous Project 3 analysis, we began by converting the xlsx file into a csv format and conducted crucial data cleaning procedures to ready the dataset for analysis. Our focus was on refining the dataset to include only relevant features for predicting 'Outage Duration.'

To achieve this, we removed unnecessary rows and columns, ensuring that the selected features directly contribute to the prediction of 'Outage Duration.' This streamlined dataset forms the basis for our predictive modeling, enabling us to identify patterns and characteristics associated with power outages in different states and regions.


In the initial phase of data cleaning, we took the following steps to refine our dataset:

1. Converted the original xlsx file into a csv format, ensuring a standardized data structure.
2. Dropped unnecessary rows that did not contribute to the predictive modeling of 'Outage Duration.'
3. Kept all relevant columns intact, as each of them played a crucial role in capturing the symbolic characteristics of the states or regions we aimed to predict.

This process allowed us to create a streamlined dataset, enhancing its suitability for training and evaluating our predictive models. Below are the entries from the cleaned dataset:

```plaintext
| OBS | YEAR | MONTH | U.S._STATE | POSTAL.CODE | NERC.REGION | CLIMATE.REGION | ANOMALY.LEVEL | CLIMATE.CATEGORY | OUTAGE.START.DATE | OUTAGE.START.TIME | ... | PCT_LAND | PCT_WATER_TOT | PCT_WATER_INLAND | OUTAGE.START | OUTAGE.RESTORATION |
|-----|------|-------|-------------|-------------|-------------|----------------|---------------|-------------------|---------------------|-----|----------|---------------|-------------------|-----------------------|
| 1   | 2011 | 7.0   | Minnesota   | MN          | MRO         | East North Central | -0.3        | normal           | 2011-07-01        | 17:00:00          | ... | 91.59    | 8.41          | 5.48              | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 |
| 2   | 2014 | 5.0   | Minnesota   | MN          | MRO         | East North Central | -0.1        | normal           | 2014-05-11        | 18:38:00          | ... | 91.59    | 8.41          | 5.48              | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 |
| 3   | 2010 | 10.0  | Minnesota   | MN          | MRO         | East North Central | -1.5        | cold             | 2010-10-26        | 20:00:00          | ... | 91.59    | 8.41          | 5.48              | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 |
| 4   | 2012 | 6.0   | Minnesota   | MN          | MRO         | East North Central | -0.1        | normal           | 2012-06-19        | 04:30:00          | ... | 91.59    | 8.41          | 5.48              | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 |
| 5   | 2015 | 7.0   | Minnesota   | MN          | MRO         | East North Central | 1.2         | warm             | 2015-07-18        | 02:00:00          | ... | 91.59    | 8.41          | 5.48              | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 |
| ... | ...  | ...   | ...         | ...         | ...         | ...            | ...           | ...               | ...               | ...               | ... | ...      | ...           | ...               | ...                   |
| 1526| 2011 | 6.0   | Idaho       | ID          | WECC        | Northwest        | -0.3        | normal           | 2011-06-15        | 16:00:00          | ... | 98.89    | 1.11          | 1.11              | 2011-06-15 16:00:00 | 2011-06-16 06:30:00 |
| 1529| 2016 | 7.0   | Idaho       | ID          | WECC        | Northwest        | -0.3        | normal           | 2016-07-19        | 15:45:00          | ... | 98.89    | 1.11          | 1.11              | 2016-07-19 15:45:00 | 2016-07-19 19:25:00 |
| 1530| 2011 | 12.0  | North Dakota | ND         | MRO         | West North Central | -0.9       | cold             | 2011-12-06        | 08:00:00          | ... | 97.60    | 2.40          | 2.40              | 2011-12-06 08:00:00 | 2011-12-06 20:00:00 |
| 1532| 2009 | 8.0   | South Dakota | SD         | RFC         | West North Central | 0.5        | warm             | 2009-08-29        | 22:54:00          | ... | 98.31    | 1.69          | 1.69              | 2009-08-29 22:54:00 | 2009-08-29 23:53:00 |
| 1533| 2009 | 8.0   | South Dakota | SD         | MRO         | West North Central | 0.5        | warm             | 2009-08-29        | 11:00:00          | ... | 98.31    | 1.69          | 1.69              | 2009-08-29 11:00:00 | 2009-08-29 14:01:00 |
```


## Prediction Problem
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.

## Features Selection
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.

## Metric of Evaluation
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.



# Baseline Model
## Model Description
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.

## Feature Transformation
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.

## Model Performance
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.



# Final Model
## Model Description
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.

## Feature Transformation
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.

## Model Performance
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.



# Fairness Analysis
## Accuracy Analysis
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.

## R-squared Analysis
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.

## Model Performance
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec nec justo enim. Cras eget eros ipsum. Duis bibendum mi ut lectus lacinia, dictum consectetur lectus elementum. Praesent scelerisque risus sollicitudin ante venenatis commodo. Maecenas aliquam, leo ac pellentesque bibendum, magna leo feugiat risus, ut luctus tortor lectus non augue. Curabitur posuere, est vitae rhoncus consectetur, leo eros varius leo, non imperdiet erat risus quis justo. Vivamus consectetur facilisis leo, eget faucibus arcu.






<p style="text-align:center"><iframe src="assets/missingness_1.html" width=800 height=600 frameBorder=0></iframe></p>