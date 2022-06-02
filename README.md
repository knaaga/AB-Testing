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
  - [Experiment Design](#expoverview)
  - [Usage Funnel and Metrics](#metrics_and_funnel)
  - [Metric Choice](#metric)
  - [Standard Error of Metrics](#SE)
  - [Experiment Sizing, Duration and Exposure](#sizing)
  - [Experimental Analysis](#analysis)
  - [Recommendation](#recommendation2)
- [Conclusion](#conclusion)
- [Authors and Acknowledgements](#licensing)

## Part 1 - Testing a New Webpage to Increase Free Trial Enrollment <a name="part1"></a>

## Problem Statement <a name="problem_statement1"></a>
An e-learning company has developed a new web page to try and increase the number of users who enroll in their free data science program (in this context, the enrollment will be referred to as converting). They run an A/B test to understand if they should implement this new page. Further, they also tested this web page separately in three countries - US, UK and Canada to understand if the country of residence plays a role in determining the conversion rate of users. The goal here is to analyze these results and help the company make a decision about the new page

## Exploratory Data Analysis <a name="eda"></a>
### Data Overview
The following table shows an overview of the data used for the analysis. It consists of the id of the user, the time they visited the web page, the group they belong to (control or treatment), the landing page they were shown (old or new) and whether or not they were converted to an enrollee

| User ID | Timestamp | Group | Landing Page | Converted |
|:------:|:------:|:------:|:------:|:------:|
851104|11:48.6|control|old_page|0|
804228|01:45.2|control|old_page|0|
661590|55:06.2|treatment|new_page|0|
853541|28:03.1|treatment|new_page|0|
864975|52:26.2|control|old_page|1|

The results of some preliminary data exploration are shown below

|Parameter|Count|
|:------|:------|
|No of entries| 294478|
|No of unique users| 290584|
|No of times new_page and treatment don't line up| 3893|

|Rows with missing values|Count|
|:------|:------|
|user_id|0|
|timestamp|0|
|group|0|
|landing_page|0|
|converted|0|

It can be seen from these tables that there exist certain entries where the participants were either incorrectly assigned to the treatment group after being shown the old page or were incorrectly assigned to the control group after being shown the new page. Another takeaway is that there are no missing entries in the data

### Data Cleaning and Wrangling
In this section, the entries with mismatched groups and landing pages are removed. Following this, duplicate users are removed from the dataframe. These results are shown below

|Parameter|Value|
|:------|:------|
|No of times new_page and treatment don't line up in the new dataframe| 0|
|No of entries in the new dataframe| 290585|
|No of unique users in the new dataframe| 290584|
|No of duplicate users in the new dataframe| 1|

### Contingency Table and Conversion Rates
The contingency table for the cleaned data is shown below

| |Treatment|Control|Total|
|:------:|:------:|:------:|:------:|
|No of converted users| 17264|17489|34753|
|No of non converted users| 128046|127785|255831|
|Total| 145310|145274|290584|

The conversion rates for the two groups and the overall conversion rate is shown below
|Parameter |Value|
|:------:|:------:|
|Treatment group conversion rate| 0.118808|
|Control group conversion rate| 0.120386|
|Overall conversion rate| 0.119597|

## AB Testing - 2-Sample Proportion Test Approach <a name="2sampletest"></a>
### Hypothesis formulation
The null and alternate hypotheses can be formulated as follows:

**Null hypothesis: The treatment group conversion rate (new page) and control group conversion rate (old page) are the same and equal to the overall conversion rate. 
Alternate hypothesis: The treatment group conversion rate (new page) and control group conversion rate (old page) are different**

### Bootstrapping and AB Testing
A sampling distribution of the difference in conversion rates between the two pages is obtained by bootstrapping over 10,000 iterations. This distribution is shown below. The red dashed lines indicate the two sided observed difference in conversion rates between the two groups. 

<img src="https://github.com/knaaga/AB-Testing/blob/main/assets/histogram.jpg" width="500" height="357.14" />

The proportion of samples where the absolute difference was greater than the absolute observed difference is 18.7%, which corresponds to a p-value of 0.187. Further, a two-sample two-sided z-test was performed and this yielded a p-value of 0.187. 

### Inference
Since the p-value obtained was lower than the threshold of 0.05 needed for statistical significance, it can be concluded that the difference in conversion rates between the pages is not statistically signficant. 

## AB Testing - Regression Approach <a name="regression"></a>
- Regression/Logistic regression overview
- Logistic regression - single
- Logistic regression - multiple
- Inference

## Recommendation <a name="recommendation1"></a>
Based on the analysis of the AB test results using different techniques like z-test and regression, it can be concluded that the difference in percentage of users enrolling in the free trial of the data science program between the new and old web page is not statistically significant. When the influence of the countries that the users resided in was taken into account, the difference was not statistically different for UK and US. For Canada however, it was observed that the new web page is about 8% less likely to convert users compared to the old page. Based on these results, the recommendation to the e-commerce company is to not launch the new web page

## Part 2 - Testing a Webpage Experiment to Increase User Retention and Net Enrollment <a name="part2"></a>

## Problem Statement <a name="problem_statement2"></a>
**The company's strategy pivoted from bringing in more users to their free course to maximizing user retention rate. The goal was to set clearer expectations for the students upfront, thus reducing the number of frustrated students who left the free trial. This would eventually help improve overall student experience and improve coaches' capacity to support students who are likely to complete the course. An experimental website design change was implemented to achieve this. The goal here is to determine whether or not to launch this experiment**

## Experiment Design <a name="expoverview"></a>
### Experiment Overview
At the time of this experiment, Udacity courses currently have two options on the course overview page: “start free trial,” and “access course materials.” If the student clicks “start free trial,” they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks “access course materials,” they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

In the experiment, Udacity tested a change where if the student clicked “start free trial,” they were asked how much time they had available to devote to the course. If the student indicated five or more hours per week, they would be taken through the checkout process as usual. If they showed fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a more significant time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial or access the course materials for free instead. This screenshot shows what this experiment looks like.

![test](https://github.com/knaaga/AB-Testing/blob/main/assets/webpage_experiment.png)

### Experiment Setup
#### **Null Hypothesis**
This experiment will not have a significant effect and hence will not be effective in reducing the early Udacity course cancellation.

#### **Alternate Hypothesis**
This experiment will have a significant effect and hence will reduce the number of frustrated students who quit the free trial, without significantly reducing the number of students to continue past the free trial and eventually complete the course.

Unit of Diversion: A unit of diversion is used to define what an individual subject is, in an experiment. In this case, it is a cookie (although if the student enrolls in the free trial, they are tracked by user-id from that point forward). 

### Overview of Metrics and User Conversion Funnel <a name="metrics_and_funnel"></a>
The metrics relevant to this analysis are described below. Note that the unit of diversion here is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward
- Number of cookies: The number of unique cookies to visit the course overview page
- Number of user-ids: The number of users who enroll in the free trial
- Number of clicks: The number of unique cookies to click the "Start free trial" button
- Click-through-probability: The number of unique cookies to click the "Start free trial" button divided by the number of unique cookies to visit the course overview page
- Gross conversion: The number of user-ids to complete checkout and enroll in the free trial divided by the number of unique cookies to click the "Start free trial" button
- Retention: The number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of user-ids to complete checkout
- Net conversion: The number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button

The user conversion funnel is shown below

<img src="https://github.com/knaaga/AB-Testing/blob/main/assets/conversion_funnel.JPG" width="586.875" height="375" />

The practical signficance boundary (d_min) for each metric is defined as the difference that would have to be observed before it is considered to be a meaningful change for the business is shown below

| Metric | d_min |
|:-------------------:|:--------------------:|
| Number of cookies  | 3000 |
| Number of user-ids | 50 |
| Number of clicks   | 240 |
| Click-through-probability  | .01 |
| Gross conversion | .01 |
| Retention   | .01 |
| Net conversion   | .0075 |


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
### Control and Experiment Data Overview
The webpage experiment was conducted based on the sizing, duration and exposure requirements previously established. An overview of the results for both the control and experiment group is shown below

| Control | Experiment |
|:-------------------|:--------------------|
|Cookies	|345543|344660|
|Clicks	|28378	|28325|
|Enrollments	|3785	|3423|
|Payments	|2033	|1945|

### Sanity Checks for Invariant Metrics

Sanity checks were done to ensure that the invariant metrics were evenly distributed across the control and experiment groups. For the count metrics - no.of cookies and no.of clicks, the 95% confidence interval centered around 0.5 was calculated and was found to contain the experimental value. For the click-through probability metric (CTP), the difference between the control and experimental group was considered and the 95% confidence interval centered around this difference was found to contain zero. Therefore, it can be concluded that all metrics pass sanity checks. The implication here is that no.of cookies and no.of clicks are equally distributed between the two groups and the difference in CTP between the two groups is not significant.

| Parameter | Control | Experiment | Prob | StdErr | MargErr | CI_lower | CI_upper | Obs_val | Pass_Sanity |
|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|
|Cookies|345543.0|344660.0|0.5|0.000602|0.001180|0.498820|0.501180|0.499360|True|
|Clicks|28378.0|28325.0|0.5|0.002100|0.004116|0.495884|0.504116|0.499533|True|
|CTP (diff)|0.0821258|0.0821824|0|0.000661|0.001295|0.484462|-0.001352|0.001239|True|

### AB Testing
#### **Results**
A two-sample proportion test was performed to analyze if the evaluation metrics - gross conversion and net conversion were different between the control and experiment groups. The results of this analysis are shown below

| Parameter | D_min | Control | Experiment | Diff | StdErr | MargErr | CI_lower | CI_upper | Result |
|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|
|Gross Conversion|0.01|0.218875|0.198320|-0.020555|0.004370|0.008565|-0.029120|-0.011989|Statistically and Practically Signficant|
|Net Conversion|0.0075|0.117562|0.112688|-0.004874|0.003434|0.006731|-0.011604|0.001857|Neither Statistically and Practically Signficant|

#### **Inference**
For both the metrics, the 95% confidence interval centered around the difference in conversion between the control and experiment groups was calculated. 

For the gross conversion metric, this confidence interval was not found to contain zero. This implies that the difference is statistically signficant. Also, since the absolute value of difference is higher than the minimum observable difference (D_min), it is considered to be practically significant as well. Further, the difference being negative implies that the experiment has led to a reduction in gross conversion rate. 

For the net conversion metric, this confidence interval was found to contain zero. This implies that the difference is not statistically signficant. Also, since the absolute value of difference is lower than the minimum observable difference (D_min), it is not considered to be practically significant as well. 

## Recommendation <a name="recommendation2"></a>
Based on the analysis of the AB test, it can be concluded that the experiment resulted in a decrease in the gross conversion rate of users. In other words, the ratio of the number of users enrolling in the free trial to the number of users clicking on the free trial has reduced. However, the experiment did not increase net enrollment in a statistically significant manner. Therefore, it can be considered partly successfull. The recommendation is to launch the experiment, while continuting to design additional experiments to achieve the goal of improved net enrollment

## Conclusion <a name="conclusion"></a>

## Authors and Acknowledgements <a name="licensing"></a>



