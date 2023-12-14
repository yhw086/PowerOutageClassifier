# PowerOutageClassifier
**Authors**: Yihuan Wang, Jiayi Zhu
## Project Overview
This is a data science project investigating the causes of electricity outages.

The dataset used in this study is obtained from Purdue University's laboratory and is titled "Major Power Outage Risks in the U.S.". It comprises 1,534 records, each representing a power outage event in the United States, and includes 55 columns. Building upon the data analysis we have done [previously](https://yhw086.github.io/PowerOutageAnalysis/), we have slightly modified our data cleaning process for this project. We have converted the `xlsx` file into `csv` file and drop unnecessary rows and columns.Details will be included below.  

The dataset used to investigate the topic can be found [here](https://www.sciencedirect.com/science/article/pii/S2352340918307182#s0015). This project is for DSC80 at UCSD.

---
## Framing Investigation Problem
Our project aims to predict the likelihood of power outages being caused by severe weather conditions. Utilizing a two-phase model approach, we start with a base model employing features like `Climate.region`, `Res.sales`, and `Outage.duration`. These features are readily available and provide an initial framework for prediction. As we progress to our final model, we improve by including additional variables such as `U.s._state` and `Customers.affected`, aiming to enhance the precision of our predictions. The underlying hypothesis is that severe weather significantly impacts the incidence and severity of power outages. Our project aims to identify key patterns and factors specific to weather-induced power disruptions. This could have far-reaching implications for emergency response planning, infrastructure resilience enhancement, and policy-making in the energy sector.

**Investigation Problem**: 
### Binary classification Problem
Our project is designed to predict, power outages starting in 2013, whether they are due to severe weather, providing a binary outcome of 1 or 0.

**Response Variable**: 
The response variable is a binary variable indicating whether a power outage is caused by Severe Weather or not. It has two possible values: 1 for outage being caused by Severe Weather and 0 for being caused by other reasons.

### Justification
Our project's objective to predict power outages starting from 2013 and whether they are caused by severe weather stems from the hypothesis that extreme weather conditions significantly influence the frequency and severity of outages. In addition, we made this choice because of the observation that out of the 7 cause categories, about half of all the outages in the dataset were caused by `severe weather`.
By focusing on the binary outcome of a weather-related cause, we wish to uncover patterns and contributing factors that are specific to weather-induced outages. This insight could be pivotal for improving preparedness and response strategies against such disruptions.

**Measuring Metrics**: 
Our choice of evaluation metric will be accuracy since our datset has balanced classes. For example, the code below shows that approximately 50% of the `Cause.categoty` is `severe weather`. We will also use F1 score and a confusion matrix to help visualize the performance. 

### Justification
For our power outage prediction project, we will primarily use accuracy as our evaluation metric because our dataset features balanced classes, with `severe weather` causes making up about half of the cases. To ensure a comprehensive assessment, it's important to account for the importance of both false positives and false negatives in our context, so, we will also employ the F1 score, which integrates precision and recall. Additionally, we'll utilize a confusion matrix to provide a clear visualization of our model's performance, highlighting its strengths and weaknesses in predicting weather-related outages.
This dual approach ensures that our evaluation is not only reflective of overall correctness but is also sensitive to the specific costs associated with incorrect predictions in power outage scenarios.

**Information At Time of Prediction**: 
The features we will be using for our base model include `Climate.region`, `Res.sales`, `Outage.duration`. These features are appropriate because they are likely available before an outage occurs or early into the outage, making them suitable for initial predictions.
The features we will be using for our final model include `Climate.region`, `U.s._state`, `Outage.duration`, `Res.sales`, `Customers.affected`. The addition of `U.s._state` and `Customers.affected` in the final model is reasonable. `U.s._state` can provide localized context which might be critical for predicting outages, and `Customers.affected` could be indicative of the outage's severity, which might correlate with whether it's caused by severe weather.
\n These features will be accessible when we make our predictions since `Climate.region` is the U.S. Climate regions defined for each reagion and not affected by outages. Similarly, `Res.sales` and `U.s._state` correspond to the Electricity consumption in the residential sector and the state names of America. We can also use `Outage.duration` and `Customers.affected` to predict if the outage was caused by `severe weather` since it might be the case that an outage has occurred but we do not know the cause of the outage. Therefore, the features included in this report will be accessible at the time of prediction. 

**Data Processing**:
We change the `Cause.category` column to 0's and 1's corresponding to the entry being equal to `severe weather` or not. 

### Train and Test data split
Data before and including the Year 2012 will be our training data and data starting from 2013 will be our test data. 

Below are the first five rows of the training dataset for our base_ine_model:

|    | Climate Region     | Res.sales | Outage.duration |
|----|--------------------|-----------|-----------------|
|  1 | East North Central | 2.3329e+06| 3060.0          |
|  3 | East North Central | 1.4673e+06| 3000.0          |
|  4 | East North Central | 1.8515e+06| 2550.0          |
|  6 | East North Central | 1.6734e+06| 1860.0          |
|  7 | East North Central | 2.1875e+06| 2970.0          |

Below are the first five rows of the test dataset for our base_ine_model:
|    | Climate Region     | Res.sales | Outage.duration |
|----|--------------------|-----------|-----------------|
|  2 | East North Central | 1.5869e+06| 1.0             |
|  5 | East North Central | 2.0289e+06| 1740.0          |
|  9 | East North Central | 1.8443e+06| 155.0           |
| 10 | East North Central | 1.6886e+06| 3621.0          |
| 11 | East North Central | 1.6886e+06| 7740.0          |

Below are the first five rows of the training dataset for our final_model:
|    | Climate Region     | U.S. State | Outage Duration | Residential Sales | Customers Affected |
|----|--------------------|------------|-----------------|-------------------|--------------------|
|  1 | East North Central | Minnesota  | 3060.0          | 2.332915e+06      | 70000.0            |
|  3 | East North Central | Minnesota  | 3000.0          | 1.467293e+06      | 70000.0            |
|  4 | East North Central | Minnesota  | 2550.0          | 1.851519e+06      | 68200.0            |
|  6 | East North Central | Minnesota  | 1860.0          | 1.676347e+06      | 60000.0            |
|  7 | East North Central | Minnesota  | 2970.0          | 2.187537e+06      | 63000.0            |

Below are the first five rows of the test dataset for our final_model:
|    | Climate Region     | U.S. State | Outage Duration | Residential Sales | Customers Affected |
|----|--------------------|------------|-----------------|-------------------|--------------------|
|  2 | East North Central | Minnesota  | 1.0             | 1.586986e+06      | 124006.57         |
|  5 | East North Central | Minnesota  | 1740.0          | 2.028875e+06      | 250000.0          |
|  9 | East North Central | Minnesota  | 155.0           | 1.844298e+06      | 5941.0            |
| 10 | East North Central | Minnesota  | 3621.0          | 1.688619e+06      | 400000.0          |
| 11 | East North Central | Minnesota  | 7740.0          | 1.688619e+06      | 193000.0          |

---
## Baseline Model


**Features Used**:

**Model Construction**: 

**Model Performance**: 

---
## Final Model


**Model Construction and Choice of Hyperparameter:**
**Model Construction**: 

**Model Performance**: 

## Fairness Analysis


**Group A and Group B**: The two groups we are going to use in this permutation test will

**Null Hypothesis**: 

**Alternative Hypothesis**: 

**Significance Level**: 

**Evaluation Metrics and Test Statistics**: 

**P-Value Result**:

**Conclusion**: 
