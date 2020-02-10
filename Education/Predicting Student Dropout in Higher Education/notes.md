# Predicting Student Dropout in Higher Education

[Link](https://arxiv.org/pdf/1606.06364.pdf)

Student dropout is a major concern in education, with about 40% of students not receiving their degree within 6 years. This costs state and federal governments billions of dollars.

## Data
De-identified, pseudonymized data
* Contains demographic information
* SAT & ACT scores
* Complete transcript records
* Imputed missing SAT/ACT scores using linear regression and mean imputation as well
* No financial data

## Experiments
* Regularized logistic regression, KNN, and random forests to predict binary dropout variable
* 30% test, 70% train/val
* To understand feature importance, they train $k$ separate logistic regressions on the $k$-th feature, trained on training set and tested on testing set
    * What this means: trained logistic regressions with 1 feature using k-fold validation

## Results
* AUC of 0.729 with logistic regression
* GPA in math classes, English classes, chemistry, psychology classes most predictive of dropout (but no higher than accuracy of 54%)


### Cited by
* Diagnosing university student subject proficiency and predicting degree completion in vector space ([Link](https://www.aaai.org/ocs/index.php/AAAI/AAAI18/paper/download/16553/16405))
* Predicting performance of electrical engineering students using cognitive and non-cognitive features for identification of potential dropouts ([Link](https://journals.sagepub.com/doi/pdf/10.1177/0020720916688484))
* STEM-ming the Tide: Predicting STEM attrition using student transcript data ([Link](https://arxiv.org/pdf/1708.09344.pdf))
* Predicting Dropout in Higher Education based on Secondary School Performance ([Link](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=8523888))
*  Predicting student dropout: A machine learning approach ([Link](https://srhe.tandfonline.com/doi/abs/10.1080/21568235.2020.1718520))