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
\n By focusing on the binary outcome of a weather-related cause, we wish to uncover patterns and contributing factors that are specific to weather-induced outages. This insight could be pivotal for improving preparedness and response strategies against such disruptions.

**Measuring Metrics**: 
Our choice of evaluation metric will be accuracy since our datset has balanced classes. For example, the code below shows that approximately 50% of the `Cause.categoty` is `severe weather`. We will also use F1 score and a confusion matrix to help visualize the performance. 

### Justification
For our power outage prediction project, we will primarily use accuracy as our evaluation metric because our dataset features balanced classes, with `severe weather` causes making up about half of the cases. To ensure a comprehensive assessment, it's important to account for the importance of both false positives and false negatives in our context, so, we will also employ the F1 score, which integrates precision and recall. Additionally, we'll utilize a confusion matrix to provide a clear visualization of our model's performance, highlighting its strengths and weaknesses in predicting weather-related outages.
\n This dual approach ensures that our evaluation is not only reflective of overall correctness but is also sensitive to the specific costs associated with incorrect predictions in power outage scenarios.

**Information At Time of Prediction**: 
The features we will be using for our base model include `Climate.region`, `Res.sales`, `Outage.duration`. These features are appropriate because they are likely available before an outage occurs or early into the outage, making them suitable for initial predictions.
The features we will be using for our final model include `Climate.region`, `U.s._state`, `Outage.duration`, `Res.sales`, `Customers.affected`. The addition of `U.s._state` and `Customers.affected` in the final model is reasonable. `U.s._state` can provide localized context which might be critical for predicting outages, and `Customers.affected` could be indicative of the outage's severity, which might correlate with whether it's caused by severe weather.
\n These feature will be accessible when we make our predictions since `Climate.region` is the U.S. Climate regions defined for each reagion and not affected by outages. Similarly, `Res.sales` and `U.s._state` correspond to the Electricity consumption in the residential sector and the state names of America. We can also use `Outage.duration` and `Customers.affected` to predict if the outage was caused by `severe weather` since it might be the case that an outage has occurred but we do not know the cause of the outage. Therefore, the features included in this report will be accessible at the time of prediction. 

**Data Processing**:
We change the `Cause.category` column to 0's and 1's corresponding to the entry being equal to `severe weather` or not. 

### Train and Test data split
Data before and including the Year 2012 will be our training data and data starting from 2013 will be our test data. 

Below is the first three rows of the training dataset:
| | Year | Month | U.S. State | Postal Code | NERC Region | Climate Region | Anomaly Level | Climate Category | Outage Start Date | Outage Start Time | Outage Restoration Date | Outage Restoration Time | Cause Category | Cause Category Detail | Hurricane Names | Outage Duration | Demand Loss MW | Customers Affected | Residential Price | Commercial Price | Industrial Price | Total Price | Residential Sales | Commercial Sales | Industrial Sales | Total Sales | Residential Percentage | Commercial Percentage | Industrial Percentage | Residential Customers | Commercial Customers | Industrial Customers | Total Customers | Residential Customer Percentage | Commercial Customer Percentage | Industrial Customer Percentage | State Real Gross State Product | U.S. Real Gross State Product | State Real Gross State Product Relative | State Real Gross State Product Change | Utility Real Gross State Product | Total Real Gross State Product | Utility Contribution | Utility Contribution to U.S. | Population | Urban Population Percentage | Urban Cluster Population Percentage | Urban Population Density | Urban Cluster Population Density | Rural Population Density | Urban Area Percentage | Urban Cluster Area Percentage | Land Percentage | Total Water Percentage | Inland Water Percentage | Climate | Cause Dummy |
|----|------|-------|------------|-------------|-------------|----------------------|---------------|------------------|-------------------------|-------------------|-------------------------|-------------------------|------------------|----------------------|-----------------|-----------------|----------------|--------------------|------------------|------------------|-----------------|------------|-------------------|------------------|------------------|------------|-----------------------|------------------------|----------------------|----------------------|---------------------|----------------------|-----------------|-----------------------------------|-----------------------------------|--------------------------------------|----------------------------------|------------------------------|---------------------------|--------------------|------------------------|-----------|-------------------------------|-------------------------------------|-------------------------|---------------------------------|-----------------------|---------------------|----------------------------|-----------------|----------------------|---------------------|-------------------|------------------------|---------|-------------|
| 1 | 2011 | 7 | Minnesota | MN | MRO | East North Central | -0.3 | normal | 2011-07-01 00:00:00 | 17:00:00 | 2011-07-03 00:00:00 | 20:00:00 | severe weather | NaN | NaN | 3060 | NaN | 70000 | 11.6 | 9.18 | 6.81 | 9.28 | 2.33e+06 | 2.11e+06 | 2.11e+06 | 6.56e+06 | 35.55 | 32.23 | 32.20 | 2.31e+06 | 276286 | 10673 | 2.60e+06 | 88.94 | 10.64 | 0.41 | 51268 | 47586 | 1.08 | 1.6 | 4802 | 274182 | 1.75 | 2.2 | 5348119 | 73.27 | 15.28 | 2279 | 1700.5 | 18.2 | 2.14 | 0.6 | 91.59 | 8.41 | 5.48 | normal | 1 |
| 3 | 2010 | 10 | Minnesota | MN | MRO | East North Central | -1.5 | cold | 2010-10-26 00:00:00 | 20:00:00 | 2010-10-28 00:00:00 | 22:00:00 | severe weather | heavy wind | NaN | 3000 | NaN | 70000 | 10.87 | 8.19 | 6.07 | 8.15 | 1.47e+06 | 1.80e+06 | 1.95e+06 | 5.22e+06 | 28.10 | 34.50 | 37.37 | 2.30e+06 | 276463 | 10150 | 2.59e+06 | 88.92 | 10.69 | 0.39 | 50447 | 47287 | 1.07 | 2.7 | 4571 | 267895 | 1.71 | 2.1 |

Below is the first three rows of the test dataset:
| | Year | Month | U.S. State | Postal Code | NERC Region | Climate Region | Anomaly Level | Climate Category | Outage Start Date | Outage Start Time | Outage Restoration Date | Outage Restoration Time | Cause Category | Cause Category Detail | Hurricane Names | Outage Duration | Demand Loss MW | Customers Affected | Residential Price | Commercial Price | Industrial Price | Total Price | Residential Sales | Commercial Sales | Industrial Sales | Total Sales | Residential Percentage | Commercial Percentage | Industrial Percentage | Residential Customers | Commercial Customers | Industrial Customers | Total Customers | Residential Customer Percentage | Commercial Customer Percentage | Industrial Customer Percentage | State Real Gross State Product | U.S. Real Gross State Product | State Real Gross State Product Relative | State Real Gross State Product Change | Utility Real Gross State Product | Total Real Gross State Product | Utility Contribution | Utility Contribution to U.S. | Population | Urban Population Percentage | Urban Cluster Population Percentage | Urban Population Density | Urban Cluster Population Density | Rural Population Density | Urban Area Percentage | Urban Cluster Area Percentage | Land Percentage | Total Water Percentage | Inland Water Percentage | Climate | Cause Dummy |
|----|------|-------|------------|-------------|-------------|----------------------|---------------|------------------|-------------------------|-------------------|-------------------------|-------------------------|-------------------|----------------------|-----------------|-----------------|----------------|--------------------|------------------|------------------|-----------------|------------|-------------------|------------------|------------------|------------|-----------------------|------------------------|----------------------|----------------------|---------------------|----------------------|-----------------|-----------------------------------|-----------------------------------|--------------------------------------|----------------------------------|------------------------------|---------------------------|--------------------|------------------------|-----------|-------------------------------|-------------------------------------|-------------------------|---------------------------------|-----------------------|---------------------|----------------------------|-----------------|----------------------|---------------------|-------------------|------------------------|---------|-------------|
| 2 | 2014 | 5 | Minnesota | MN | MRO | East North Central | -0.1 | normal | 2014-05-11 00:00:00 | 18:38:00 | 2014-05-11 00:00:00 | 18:39:00 | intentional attack | vandalism | NaN | 1 | NaN | 124006.57 | 12.12 | 9.71 | 6.49 | 9.28 | 1.59e+06 | 1.81e+06 | 1.89e+06 | 5.28e+06 | 30.03 | 34.21 | 35.73 | 2.35e+06 | 284978 | 9898 | 2.64e+06 | 88.83 | 10.79 | 0.37 | 53499 | 49091 | 1.09 | 1.9 | 5226 | 291955 | 1.79 | 2.2 | 5457125 | 73.27 | 15.28 | 2279 | 1700.5 | 18.2 | 2.14 | 0.6 | 91.59 | 8.41 | 5.48 | normal | 0 |
| 5 | 2015 | 7 | Minnesota | MN | MRO | East North Central | 1.2 | warm | 2015-07-18 00:00:00 | 02:00:00 | 2015-07-19 00:00:00 | 07:00:00 | severe weather | NaN | NaN | 1740 | 250 | 250000 | 13.07 | 10.16 | 7.74 | 10.43 | 2.03e+06 | 2.16e+06 | 1.78e+06 | 5.97e+06 | 33.98 | 36.21 | 29.78 | 2.37e+06 | 289044 | 9812 | 2.67e+06 | 88.82 | 10.81 | 0.37 | 54431 | 49844 | 1.09 | 1.7 | 4873 | 292023 | 1.67 | 2.2 | 5489594 | 73.27 | 15.28 | 2279 | 1700.5 | 18.2 | 2.14 | 0.6 | 91.59 | 8.41 | 5.48 | extreme | 1 |
| 9 | 2015 | 3 | Minnesota | MN | MRO | East North Central | 0.6 | warm | 2015-03-16 00:00:00 | 07:31:00 | 2015-03-16 00:00:00 | 10:06:00 | intentional attack | sabotage | NaN | 155 | 20 | 5941 | 11.53 | 8.89 | 6.61 | 9.03 | 1.84e+06 | 1.96e+06 | 1.80e+06 | 5.60e+06 | 32.94 | 34.95 | 32.07 | 2.37e+06 | 289044 | 9812 | 2.67e+06 | 88.82 | 10.81 | 0.37 | 54431 | 49844 | 1.09 | 1.7 | 4873 | 292023 | 1.67 | 2.2 | 5489594 | 73.27 | 15.28 | 2279 | 1700.5 | 18.2 | 2.14 | 0.6 | 91.59 | 8.41 | 5.48 | extreme | 0 |


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
