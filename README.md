# power-outage-analysis-part2
DSC 80 Project 5

# Predicting the cause of a major power outage
by Michael Yang

---

My exploratory data analysis on this dataset can be found [here](https://mlyang1.github.io/power-outage-analysis/).

## Framing the Problem

The prediction problem for this project is to predict the cause of a major power outage in the continental United States.
This is a classification problem, and multiclass classification will be performed.
The response variable being predicted is 'CAUSE.CATEGORY' because this is the column containing the primary cause of the outage.
The metric used to evaluate the model is precision because it measures how much of the model's predictions are correct.
At the time of prediction, the known information of the outage would be:
* YEAR
* MONTH
* STATE
* OUTAGE.START
* DEMAND.LOSS.MW
* CUSTOMERS.AFFECTED
* CLIMATE.REGION
* CLIMATE.CATEGORY
* TOTAL.PRICE
* TOTAL.SALES
* TOTAL.CUSTOMERS

## Baseline Model

A baseline Decision Tree Classifier model predicts the cause of outages based on the known categorical information of the outage.
Nominal categorical features were created for this model by one hot encoding the categorical columns: ['YEAR', 'MONTH', 'STATE', 'CLIMATE.REGION', 'CLIMATE.CATEGORY']
The score for this baseline model based on the test data is 0.6447368421052632. This score is not bad but it can be better.


## Final Model

The final Decision Tree Classifier model predicts the cause of outages based on the known categorical as well as known numerical information of the outage.
Nominal categorical features from the baseline model were kept the same.
Numerical features were created for the final model by using mean imputation on missing values and a Standard Scaler on the numerical columns: ['DEMAND.LOSS.MW', 'CUSTOMERS.AFFECTED', 'TOTAL.PRICE', 'TOTAL.SALES', 'TOTAL.CUSTOMERS']
These features improved the model's performance because it added new information about demand loss, customers, price, sales, etc. that further distinguishes different causes from each other, making predictions more accurate.
A GridSearchCV was performed to prevent overfitting and determine the best hyperparameters for the Decision Tree Classifier: {'criterion': 'entropy',
 'max_depth': 7,
 'max_leaf_nodes': 15,
 'min_samples_split': 5}
 The score for the final model after determining the best hyperparameters is 0.7289473684210527. The final model's performance is an improvement over the baseline model's because the testing score is higher and the training score is lower, meaning the model is not overfit to the training data.

## Fairness Analysis

To perform a fairness analysis, the question "Does the model perform worse for outages in the 2000s than it does for outages in the 2010s" was asked.
The evaluation metric used was the precision of the predicted causes compared to the actual causes in the test data.
Null Hypothesis:
The model is fair. Its precision for outages in the 2000s and 2010s are roughly the same, and any differences are due to random chance.
Alternative Hypothesis:
The model is unfair. Its precision for outages in the 2000s is lower than its precision for outages in the 2010s.
The test statistic for the permutation test was the difference in precision between predictions made for outages in the 2000s vs. 2010s.
The p-value is 0.1, with a significance level of 0.05, so we fail to reject the null hypothesis.
The conclusion is that our model is not biased and its precision for outages in the 2000s and 2010s are roughly the same, with any differences being due to random chance.
