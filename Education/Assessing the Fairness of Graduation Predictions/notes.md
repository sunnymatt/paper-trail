# Assessing the Fairness of Graduation Predictions

[Link](https://drive.google.com/file/d/1h_RGk4VZj-x0kOKTCISgyoRvWfLE6PLC/view)

Main research questions: are graduation predictors *equitable*? Are they *optimal*? Could they achieve better accuracy by using models specific to certain subgroups?

## Fairness in this scenario
* Anticipate difference in false positive / false negative rates
* Argue that false positives are most important here; students who are not on track to graduate but are predicted to graduate will not be given proper support/mentoring

## Data
* Large R1 publicly-funded university in the South
* Feature set incldues academic performance, financial information, pre-admission info (SAT/ACT), and extra-curricular activities

## Models
* 5 separate models (SVM, Decision Trees, Random Forests, Logistic Regression, SGD classifier)

## Results
* All models had AUC ~ 0.8
### Equity
* White students had higher false positive rates and lower false negative rates
* Hispanic/Latino students had higher false negative rates
* Male students had consistently higher false negative rates
### Optimality
* Usually performance didn't differ more than the standard error; population-specific model saw a decrease in performance on Asian students