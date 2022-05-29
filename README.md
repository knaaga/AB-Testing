# AB-Testing: E-Learning Webpage Change Implementation
## Project Overview

#### Dependencies

## Table of Contents
- [Part 1 - Testing a New Webpage to Increase Free Trial Enrollment](#part1)
  - [Problem Statement](#problem_statement1)
  - [Exploratory Data Analysis](#eda)
  - [AB Testing - 2-Sample Proportion Test Approach](#2sampletest)
  - [AB Testing - Regression Approach](#regression)
  - [Recommendation](#recommendation1)
- [Part 2 - Testing a Webpage Experiment to Increase User Retention and Net Enrollment](#part2)
  - [Problem Statement](#problem_statement2)
  - [Usage Funnel and Metrics](#funnel)
  - [Experiment Design](#expoverview)
  - [Metric Choice](#metric)
  - [Standard Error of Metrics](#SE)
  - [Experiment Sizing, Duration and Exposure](#sizing)
  - [Experimental Analysis](#analysis)
  - [Recommendation](#recommendation2)
- [Conclusion](#conclusion)
- [Authors and Acknowledgements](#licensing)

## Part 1 - Testing a New Webpage to Increase Free Trial Enrollment <a name="part1"></a>

## Problem Statement <a name="problem_statement1"></a>
- Same as Jupyter

## Exploratory Data Analysis <a name="eda"></a>
- Data snapshot table
- Cleaning and wrangling
- Contingency table for cleaned data

## AB Testing - 2-Sample Proportion Test Approach <a name="2sampletest"></a>
- Hypothesis formulation
- Simulation/Bootstrapping
  - Histogram
 - Python inbuilt
 - Inference

## AB Testing - Regression Approach <a name="regression"></a>
- Regression/Logistic regression overview
- Logistic regression - single
- Logistic regression - multiple
- Inference

## Recommendation <a name="recommendation1"></a>
- Same as Jupyter

## Part 2 - Testing a Webpage Experiment to Increase User Retention and Net Enrollment <a name="part2"></a>

## Problem Statement <a name="problem_statement2"></a>
- Same as Jupyter (refine)

## Usage Funnel and Metrics <a name="funnel"></a>
- Funnel diagram
- Data overview

### Overview of Metrics
The metrics relevant to this analysis are described below. Note that the unit of diversion here is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward
- Number of cookies: The number of unique cookies to visit the course overview page
- Number of user-ids: The number of users who enroll in the free trial
- Number of clicks: The number of unique cookies to click the "Start free trial" button
- Click-through-probability: The number of unique cookies to click the "Start free trial" button divided by the number of unique cookies to visit the course overview page
- Gross conversion: The number of user-ids to complete checkout and enroll in the free trial divided by the number of unique cookies to click the "Start free trial" button
- Retention: The number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of user-ids to complete checkout
- Net conversion: The number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button

The practical signficance boundary (d_min) for each metric is defined as the difference that would have to be observed before it is considered to be a meaningful change for the business is shown below

| Metric | d_min |
|:-------------------:|:--------------------:|
| Number of cookies  | 3000 |
| Number of user-ids | 50 |
| Number of clicks   | 240 |
| Click-through-probability  | .01 |
| Gross conversion | .01 |
| Retention   | .01 |
|Net conversion   | .0075 |


## Experiment Design <a name="expoverview"></a>
- Explain the experiment
- Cookie - brief explanation
- Webpage diagram
- Funnel diagram

## Metric Choice <a name="metric"></a>
The metrics described previously are classified into invariant metrics and evaluation metrics. Invariant metrics are those which should not change i.e., remain invariant across control and experiment groups. Evaluation metrics are those that are expected to vary between the two groups and can be used to quantify the success of the analysis.

### Invariant Metrics
- Number of cookies: This is the unit of diversion and is expected to be evenly distributed amongst the two groups
- Number of clicks: Equal distribution is expected between the two groups since at this point in the funnel, the experience is the same for all users and therefore elements of the experiment would not be expected to impact clicking the "Start free trial" button
- Click-through-probability: Since this metric is essentially derived from the previous two metrics, by the same reasoning, it is expected to be evenly distributed between the groups

### Evaluation Metrics
Evaluation metrics need to be aligned to business needs. The objective of the website experiment is to:
- Increase user retention (make more free trial users stay beyond the trial period and make at least one payment)
- Decreased gross conversion coupled with increased net conversion (less students enrolling in the free trial but nmore students staying beyond the free trial)

The metrics that are in line with these objectives are:
- Gross conversion
- Retention
- Net conversion

## Standard Error of Metrics <a name="SE"></a>

- Formula
- Sample size and p
- Table

## Experiment Sizing, Duration and Exposure <a name="sizing"></a>
### Sizing
The company provided baseline webpage traffic and engagement data. The baseline conversion rates were calculated using this and were eventually used to determine the number of pageviews that are required to be collected to adequately power the experiment. An alpha value of 0.05 and a beta value of 0.2 was used. 

The baseline data provided and the calculation of baseline conversion rates are shown below
| Metric | Value |
|:-------------------|:--------------------|
| Unique cookies to view course overview page per day | 40000 |
| Unique cookies to click "Start free trial" per day | 3200 |
| Enrollments per day | 660 |
| Payments | 350 |
| Click-through-probability on "Start free trial" | 3200/40000 = 0.08 |
| Probability of enrolling, given click (gross conversion) | 660/3200 = 0.20625 |
| Probability of payment, given enroll (retention) | 350/660 = 0.53 |
| Probability of payment, given click | 350/3200 = 0.1093125 |


**Using sample size calculator from [here](https://www.evanmiller.org/ab-testing/sample-size.html) to calculate the pageviews needed to achieve the target statistical power**

**Gross Conversion**
* Baseline conversion: 20.625%
* Minimum detectable effect: 1%
* Alpha: 5%
* Beta: 20%
* Sensitivity: 80%
* Sample size: 25,835 enrollments/group
* Number of groups = 2 (experiment and control)
* Total sample size = 51,670 enrollments
* Clicks/pageview = 3200/40000 = 0.08
* Pageviews required = 51,670/0.08 = 645,875

**Retention**
* Baseline conversion: 53%
* Minimum detectable effect: 1%
* Alpha: 5%
* Beta: 20%
* Sensitivity: 80%
* Sample size: 39,115 enrollments/group
* Number of groups = 2 (experiment and control)
* Total sample size = 78,230 enrollments
* Enrollments/pageview = 660/40000 = 0.0165
* Pageviews required = 78,230/0.08 = 4,741,212

**Net Conversion**
* Baseline conversion: 10.93%
* Minimum detectable effect: 0.75%
* Alpha: 5%
* Beta: 20%
* Sensitivity: 80%
* Sample size: 27,413 enrollments/group
* Number of groups = 2 (experiment and control)
* Total sample size = 54,826 enrollments
* Clicks/pageview = 3200/40000 = 0.08
* Pageviews required = 54,826/0.08 = 685,325

**Pageviews required is maximum of pageviews for the different metrics. Therefore the required pageviews is 4,741,212**

### Duration and exposure
100% diversion of traffic at 40,000 pageviews/day would require 119 days
On eliminating retention (which has the max pageview requirement currently), the pageview requirement becomes 685,325 and there are two options:
18 day experiment with 100% diversion
36 day experiment with 50% diversion

If 100% of the web traffic is diverted to the experiment, based on 40,000 pageviews per day, it would take around 119 days to complete the experiment, which was deemed too long by the company. On eliminating the retention parameter, the maximum pageview requirement now drops to 685,325. This results in an 18-day experiment using a 100% diversion rate and a 36 days experiment using a 50% diversion rate. Since the company is conducting other experiments in parallel, using 50% of the traffic for this experiment is appropriate. Therefore, it was decided that 685,325 pageview samples will be collected over a period of 36 days. 


- Adjust sample size based on desired exposure

## Experimental Analysis <a name="analysis"></a>
 - Control and experiment data
   -Tables
 - Sanity checks for count and other metrics
   - Tables
 - 2-sample prop test
   - Hypothesis formulation
   - Tables

## Recommendation <a name="recommendation2"></a>
- Same as jupyter

## Conclusion <a name="conclusion"></a>

## Authors and Acknowledgements <a name="licensing"></a>



