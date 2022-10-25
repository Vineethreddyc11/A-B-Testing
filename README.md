# A-B-Testing
A/B testing on Udacity click stream data to reduce early course cancellations by using control and treatment groups




## Project overview

At the time of this experiment, Udacity courses currently have two options on the home page: "start free trial", and "access course materials".

- If the student clicks **“start free trial”**, they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first.
- If the student clicks **“access course materials”**, they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

**Experiment:** Free Trial Screener

**Goal:** Maximize the course completion rate of **“Free Trial”** users through guiding the students who do not have enough time to **“access course materials”**.

**Experiment Hypothesis:** The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn’t have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches’ capacity to support students who are likely to complete the course.

**Experiment Change:** For the users who click on **“start free trial”**, Udacity will ask the users how much time they are available to devote to the course.

- For users who will devote 5 or more hours per week. It’s the same as usual.
- For users who will devote Less than 5 hours per week, Udacity will suggest the users choose “access course materials”

**Unit of diversion:** cookies
- If the student enrolls in the free trial, they are tracked by user-id from that point forward. **The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page**.

### The Process of Users’ Behaviors

- 1 - Visit Udacity website
- 2 - Open course page (cookies or called pageviews)
- 3 - Choose “start free trial” or “access course materials”.
    - 1 -  “access course materials”
      - 1 - Free users -> no charging
    - 2 - “start free trial” (clicks) 
      - 1 - Less than 5 hours per week for learning -> Experiment change: suggested for switching to “access course materials”
      - 2 - 5 hours per week for learning -> stay here (user-id)
        - 1 - Course completion & Subscription (enrollment & payment)
        - 2 - Course completion & Cancel Subscription (enrollment)
        - 3 - Course incomplete & Subscription (enrollment & payment)
        - 4 - Course incomplete & Cancel Subscription (enrollment)


## Experimental Design

### Hypothesis Testing
- control group, also called group A.
- experiment group, also called group B.
- **Null Hypothesis:** There is no difference in group A and B.
- **Alternative Hypothesis:** There is a difference in group A and B.

## Metric Choice
A/B Testing requires two types of metrics: Invariant Metrics and Evaluation Metrics.

The following are possible metrics:

Any place “unique cookies” are mentioned, the uniqueness is determined by day. (That is, the same cookie visiting on different days would be counted twice.) User-ids are automatically unique since the site does not allow the same user-id to enroll twice.
- **Number of cookies:** That is, number of unique cookies to view the course overview page.
- **Number of user-ids:** That is, number of users who enroll in the free trial. 
- **Number of clicks:** That is, number of unique cookies to click the “Start free trial” button (which happens before the free trial screener is trigger).
- **Click-through-probability:** That is, number of unique cookies to click the “Start free trial” button divided by number of unique cookies to view the course overview page.
- **Gross conversion:** That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the “Start free trial” button.
- **Retention:** That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of user-ids to complete checkout.
- **Net conversion:** That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the “Start free trial” button.

### Invariant metrics:
- Population sizing metrics, based on your unit of diversion. Your population of control and experiment should be comparable.
- Actual invariants, those metrics that shouldn’t change when you run your experiment.

The good invariant metrics are the metrics before the **Experiment Change** happens in **The Process of Users’ Behaviors**, and the invariant metrics should not changed by any reason including your **Experiment Change**. In this experiment, the **Experiment Change** locates in the step while the users are clicking on “start free trial”.

| Metric Name | Metric Formula    | d min                | Notation | Python Notation   | Reason               |
| :-------- | :------- | :------------------------- | :-------- | :------- | :------------------------- |
| Number of cookies | #number of unique cookies to view the course overview page | 3000 cookies || Ccookies | cookies | Population sizing metric|
| Number of clicks | #number of unique cookies to click the “Start free trial” button | 240 clicks || Cclicks | clicks | Population sizing metric|
| Click-through-probability | Cclicks/Ccookies  | 0.01 || CTP | CTP | Population sizing metric|

- Number of cookies is our population metric. Additionally, the Experiment change happening before the cookies are recorded. Therefore, Number of cookies is an invariant metric.
- Number of clicks, which are recorded before the Experiment change occurred.
- Click-through-probability, which are recorded before the Experiment change occurred.
Why cannot choose Number of user-ids
- Number of user-ids is not a valid invariant metric since it maybe influenced by the Experiment change.

### Evaluation Metrics:
Evaluation metrics are used to measure the quality of the statistical or machine learning model. It is very important to use multiple evaluation metrics to evaluate your model. This is because a model may perform well using one measurement from one evaluation metric, but may perform poorly using another measurement from another evaluation metric. Using evaluation metrics are critical in ensuring that your model is operating correctly and optimally.

| Metric Name | Metric Formula    | d min                | Notation | Python Notation   | Reason               |
| :-------- | :------- | :------------------------- || :-------- | :------- | :------------------------- |
| Gross conversion | Cenrollments/Cclicks | 0.01 || Gross conversion | gross_conversion | The performance of a model|
| Retention | Cpayments/Cenrollments | 0.01 || Retention | retention | The performance of a model|
| Net conversion | Cpayments/Cclicks  | 0.0075 || Net conversion | net_conversion | The performance of a model|


### Calculating Standard Error
For each metric you selected as an evaluation metric, make an analytic estimate of its standard error, given a sample size of 5,000 cookies visiting the course overview page. Enter each estimate in the appropriate box to 4 decimal places

| Metric Name | Standard Error   | 
| :-------- | :------- | 
| Gross conversion | 0.0202 | 
| Retention | 0.0549 | 
| Net conversion | 0.0156  | 


### Sizing
- Calculating Number of Pageviews
- Choosing Duartion and Exposure

Pageviews for Each Evaluation Metric to Achieve Target Statistical Power:

| Metric Name | Baseline Conversion Rate   | d min                | Alpha| Beta  | Power     | Sample Size | Pageviews(cookies) |
| :-------- | :------- | :------------------------- | :-------- | :------- | :------------------------- | :------- | :------------------------- |
| Gross conversion | 0.206250 | 0.010000| 	0.05000 | 0.20000 | 0.80000| 25,835 | 645,875
| Retention | 0.530000 | 0.010000 | 0.05000 | 	0.20000 | 0.80000| 39,115 | 4,741,212
| Net conversion | 0.109312 | 0.007500 | 	0.05000 | 0.20000 | 0.80000| 27,413 | 685,325

Pageviews required is maximum of pageviews required for Gross Conversion, Retention, Net Conversion. Therefore, the required pageviews is 47,41,212.

## Duartion and Exposure
For calculating the minimum duration, we divert 100% daily cookies for this experiment.

| Metric Name | Sample Size   | Minimum pageviews                | Fraction of experiment traffic| Duration | 
| :-------- | :------- | :------------------------- | :-------- | :------- | 
| Gross conversion | 25,835 | 645,875| 	1 | 17 | 
| Retention | 39,115 | 4,741,212 | 	1 | 119|
| Net conversion | 27,413 | 685,325 | 	1 | 18 | 

According to above table, when we assign 100% daily traffic to this experiment, we have the minimum duration. We only have 40,000 pageviews (unique cookies) per day. Therefore, we need to abandon `Retention` as an qualified evaluation metric due to the too long-running experiment.

## Analysis
This data contains the raw information needed to compute the above metrics, broken down day by day.  
The meaning of each column is:
- **Pageviews:** Number of unique cookies to view the course overview page that day.
- **Clicks:** Number of unique cookies to click the course overview page that day.
- **Enrollments:** Number of user-ids to enroll in the free trial that day.
- **Payments:** Number of user-ids who enrolled on that day to remain enrolled for 14 days and thus make a payment. (Note that the date for this column is the start date, that is, the date of enrollment, rather than the date of the payment. The payment happened 14 days later. Because of this, the enrollments and payments are tracked for 14 fewer days than the other columns.)

## Sanity Checks (A/A Test)

This check is primarily for the invariant metrics. For invariant metrics we expect equal diversion into the experiment and control group. We will test this at the 95% confidence interval.


|  | Margin of Error   | CI lower bound              | CI upper bound| Observed | 	Statistical Significance| Practical Significance |
| :-------- | :------- | :------------------------- | :-------- | :------- | :------- |  :------- |  
| cookies | 0.0012	|0.4988|	0.5012	|0.5006|	False|	False | 
| clicks | 0.0041|	0.4959|	0.5041	|0.5005	|False	|False|
|CTP| 0.0009|	0.0812	|0.083	|0.0821	|None	|False |

## Result Analysis
95% Confidence interval for the difference between the experiment and control group for evaluation metrics. The result is satistically significant only when the 95% confidence interval does not include zero.
|  | Margin of Error   | CI lower bound              | CI upper bound| Observed | 	Statistical Significance| Practical Significance |
| :-------- | :------- | :------------------------- | :-------- | :------- | :------- |  :------- |  
| Gross conversion |0.0086 |	-0.0291|	-0.012|	-0.0206|	True	|True|
| Net conversion | 0.0067|	-0.0116|	0.0019	|-0.0049|	False|	False|

## Sign Test
Sign test is just an another method to validate the result obtained above. The sensitivity of sign test is lower than that of the above test.

- The p-value of Gross conversion is 0.0026, which is less than 0.05. Therefore, Gross conversion is statistical significance.
- The p-value of Net conversion is 0.6776, which is greater than 0.05. Therefore, Net conversion is not statistical significance.

## Summary
An experiment was conducted in which potential Udacity students were diverted by cookie into two groups, experiment and control. The experiment group was asked to input the amount of time they are willing to devote to study, after clicking a "start free trial button", whereas the control group was not. Three invariant metrics (Number of Cookies, Number of clicks on "start free trial", and Click-Through-Probability) were chosen for purposes of validation and sanity checking while Gross Conversion (enrollment/cookie) and Net Conversion (payments/cookie) served as evaluation metrics. The null hypothesis is that there is no difference in the evaluation metrics between the groups, futhermore, a practical signifcance threshold was set for each metric. The requirement for launching the experiment is that the null hypothesis must be rejected for ALL evaluation metrics and that the difference between branches must meet or exceed the practical signficance threshold. Because our acceptance criteria requires statiscally signifcant differences for ALL evaluation metrics, the use of the Bonferonni correction is not appropriate. The Bonferonni correction is a method for controlling for type I errors (false positives) when using multiple metrics in which relevance of ANY of the metrics matches the hypothesis. In this case the risk of type I errors increases as the number of metrics increases (signifcance by random chance). In our case in which ALL metrics must be relevant to launch, the risk of type II errors (false negatives) increases as the number of metrics increases, so it stands to reason that controlling for false positives is not consistent with our acceptance criteria.


Analysis revealed the expected equal distribution of cookies into the control and experimental groups, for the invariant metrics, at the 95% CI. A difference in gross conversion was found to be statistically signficant at the 95% CI, and the null hypothesis was rejected. Gross conversion also met the practical signficance threshold. Net Conversion was found to be neither statistically nor practically signficant at the 95% CI.

## Recommendation
This experiment was designed to determine whether filtering students as a function of study time commitment would improve the overall student experience and the coaches' capacity to support students who are likely to complete the course, without significantly reducing the number of students who continue past the free trial. A statistically and practically signficant decrease in Gross Conversion was observed but with no significant differences in Net Conversion. This translates to a decrease in enrollment not coupled to an increase in students staying for the requisite 14 days to trigger payment. Considering this, my recomendation is not to launch, but rather to pursue other experiments.



