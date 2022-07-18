# Exploratory Analysis of Health Statistics for Thailand Insurance in Modern Era

## Introduction

This is a project based on the concept **Innovation of Insurances in Modern's Perspectives**. This will go over analysis on the thai health statistics to find the trend of Insurance Businesses in Thailand and couple more of new ideas from these exploratory analyses.

## Exploratory Analysis of Thailand Average Age and Death Rate of Chronic Diseases over time

This is an analysis for the Average ages and death rate of chronic diseases of Thai people. This analysis will contribute on the decision on health insurances and a vague prediction of the revenue in this business.

### Exploratory and Regression Analysis of Thailand Average Age over time

Click [here](./medianage.ipynb) to redirect to the analysis Notebook

This analysis will go over the Thailand Average Age dataset from [Kaggle](https://www.kaggle.com/datasets/divyansh22/average-age-of-countries). I used the data from year 1950-2020 and make the predictions for 2025 and 2030. In the prediction procedure, I used **Linear Regression** and used statistical tests for verification of linear relationship between time (year) and average age.

#### Linear Regression

**Linear Regression** is the model which tries to calculate the below equation from the data

$$
\textbf{y} = \beta \textbf{x} + \beta_0
$$

where $\beta$ and $\beta_{0}$ are coefficients and intercepts. In case of this data which has **year** as an independent variable and **Thailand Average Age** as a dependent variable, we can reduce the equation to 

$$
\text{Thailand Average Age} = \beta \cdot \text{year} + \beta_0
$$

In order to explain this model, every 1 year passed, the average age increases by $\beta$ year.

#### Linear Regression Result

After that we fit our data into Linear Regression Model and this is the equation we get

$$
\text{Thailand Average Age} \approx 0.29 \cdot (\text{year} - 1950) + 14.38
$$

Note : We don't fit the $\text{year}$ into the regression model directly, we decreased it by 1950 in order to reduce the complication within the coefficients

We then compare our Model Result with the actual data using line plotting

![](https://cdn.discordapp.com/attachments/548279055477374982/997763133269610566/medianage_actual_vs_approx.png)

We can see that the model yields a good fit to data. However to verify further, we need to use the **Pearson Correlation Coefficient** which is used to check whether two variables have the linear relationship to each other.

In this case, the **Pearson Correlation Coefficient** is approximately 0.94 so we can say that two variables have a good linear relationship to each other.

In order to verify further that the model is valid, we need to go to the next step.

#### Confidence Interval and Statistical Tests for Coefficients in Linear Regression Model

##### Confidence Interval

**Confidence Interval** (CI) is the possible range that the population mean, or a population parameters can be. For example in this experiment, we will estimate how thai population average age changes over time. In another interpretation, we can say that we have x% confidence that the parameter is in the given range.

For this experiment, our 95% CI of the parameter that measures how many years the Thailand Average Age changes in 1 year is in the range of $(0.09, 0.49)$, so we can say that we have 95% level of confidence that Thai People Average Age increases by 0.09-0.49 years every 1 year.

##### Statistical Tests for Coefficients

To verify further that the model is valid we need to check

- Null Hypothesis for f-test should be rejected ([Read More](https://github.com/HowToProgramming/4dm4analysis/blob/main/journal/regression_analysis.md#in-depth-analysis-of-one-way-anova))
- Residuals (or Errors) should be **Normally Distributed**

For the first assumption, we need to obtain the **test statistics** and **p-value** using `sklearn.feature_selection.f_regression`. In this study, we got the p-value of $1.538 \times 10^{-7}$ which is less than 0.05. Hence we can conclude that there is a linear relationship between year and Average Age of Thai People.

For the Second Assumption, we use **Shapiro-Wilk test** to verify the normality of the residuals. If the p-value we obtained from the test is less than $\alpha = 0.05$, we conclude that the residual is not Normally Distributed.

In this experiment, we got the p-value of 0.42 which is greater than our significance level. So the residual is Normally Distributed.

#### Predictions for Thailand Average Age in the future using Linear Regression Model

To finish up the observation, we extrapolate the model to the year 2025 (3 years into the future) and 2030 (8 years into the future) in order to get the information about the focusing of insurance business in the future population.

These are the results we get:
- In 2025, the Estimation of Average Age of Thai People will be 36 years old
- In 2030, the Estimation of Average Age of Thai People will be 38 years old

We can see that Thai Society is growing to be the aging society, and we need to do further analysis in business to focus on these people.

### Exploratory and Regression Analysis of Chronic Diseases over time

Click [here](./cancerdie.ipynb) to redirect to the analysis Notebook

This analysis will go over the exploration of Chronic Diseases Death Rates in Thai People with age 30 to 70 from [World Health Statistics Dataset from Kaggle](https://www.kaggle.com/datasets/utkarshxy/who-worldhealth-statistics-2020-complete). The available data for our specified targets are in the period of 2000-2016. We will use the exploratory data analysis (EDA) and Regression Analysis with Log-Odds to observe and predict the Chronic Diseases Death Rates in Thai People.

#### Data Transformation

In this experiment, rather than using chances, we will use "odds ratio" instead. The odds ratio is the alternative of probability that determines, for example if you buy a lottery and there is a 1% chance of winning it, the odds ratio is 1:99. Meaning that 1 in 100 will win the lottery and 99 others will lost, well, theoretically. We calculate the odds ratio by using this formula

$$
\text{Odds Ratio} = \frac{p}{1-p}
$$

We then get the numbers in range $(0, \infty)$. In order to use the regression model, our dependent variable, namely chance or odds need to be in range $(-\infty, \infty)$.

We then continue transforming the data using the log function because $\log(0) \rightarrow -\infty$. After that we can use the transformed data in the linear model.

For experienced Data Scientists / Analysts, the model may look like the **Logistic Regression** but the difference is it is not really a classification, instead we trying to find a linear model to fit the time with log-odds. 

#### Regression Analysis

Alright, time for a real deal, we then do a similar steps to what we've done in [the previous section](#linear-regression) but this time, to calculate how the "chances" or "odds" changes in time, we need to exponentiate the coefficient because of the following derivations:

$$
\log(\frac{p}{1-p}) = \beta x + \beta_0
$$

$$
\frac{p}{1-p} = e^{\beta x + \beta_0}
$$

We can see that if we exponentiate the coefficients, we get the changes in odds over the parameter $x$.

#### Regression Analysis Results and Statistical Tests

- Pearson's Correlation Coefficient : -0.997, there is a negative linear relationship between year and log-odds of death of chronic diseases rate of Thai People
- f_regression p-values: 0.0002, the linear relationship between year and log-odds is significant.
- Shapiro-Wilk Tests for residuals p-value: 0.38, the linear model assumption is complete.
- The coefficient is approximately -0.02 and the exponentiated coefficient is approximately 0.978, that means every year, the odds of a person dies with chronic diseases decreased by 2.2%

And here are the predictions of 2025 and 2030

- In 2025, the estimated chance of death from chronic diseases is 12.12%
- In 2030, the estimated chance of death from chronic diseases is 11.01%

We can see that the loss from chronic diseases will decrease over time.

### Summary

From these informations, we can create a broad conclusion that the Thai Population will be in the age range of 35-45 years in the near future. With the information of the decreasing of chronic diseases death rate and trivially, the PM2.5 Crisis, Climate Change and Global Warming. The Health Insurance business will grow and create a lot of revenues to the company as the life expectancy of those who have diseases increases.

In modern's perspective : As modern people are caring about their relatives or parents, including their own health in this unpleasant situation for healthy condition. The selling point can be how those pollutions, or increases in temperature will affect your health and your relatives' health in a long run.

### Acknowledgements

I would like to thank my parents who unconditionally supported me for a long time. I wouldn't get to this point without them.

I would like to thank to my high school teachers, college professors and internship mentors, especially ones who I actively talk with in email or any social contacts. You gave me opportunities to shine, you gave me inspirations, you gave me who I am today. Thank you so much, with regards.

I would like to thank my friends who supported me. Whether we just knew each other in a freshman year, or one who worked together for years, or one that being inspired by me and I am inspired by (well, you know who you are). You guys are meaning a lot to me and I would like to give an infinite thanks if I manage to get the scholarship. See you soon Australia (hopefully).
