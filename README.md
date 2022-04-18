# Classification-and-Regression

Simple toy project for classification and regression

Individual Project of 'Data Mining' lecture in 2021

<br>

## purpose

### Classification  - pima-indians-diabetes
- I want to find the most influential variable for diabetes.
- I implement several classification models and compare their performance to find the classification model with the highest accuracy.


### Regression - cubic_zirconia
- I want to find the biggest variable that determines the price of diamonds.
- I compare several regression models to identify high R-Squared models and determine the importance of variables.

<br>
<br>

---


## Classification

### Data : pima-indians-diabetes
768 rows, 9 columns

| column                         | type   |
| ------------------------------ | ------ |
| N\_times\_pregnant             | int    |
| oral\_glucose                  | int    |
| blood\_pressure                | int    |
| Triceps\_skin\_fold\_thickness | int    |
| serum\_insulin                 | int    |
| Body\_mass\_index              | float  |
| Diabetes\_pedigree\_function   | float  |
| Age                            | int    |
| target                         | binary |



### Data pre-processing
Many NAN values were included in the data, replacing missing values through KNN.

before

![before](/image/c_before_pre.PNG)


after

![after](/image/c_after_pre.PNG)

Dataset was analyzed by dividing train test by 8:2.

<br>

### Analysis
#### 1. Decision Tree

Average value calculated after 30 trials

<br>

- gini index

    Test Accuarcy : 0.7288    

    Dominatn rule
    
    | Dominant rule                                                                | count |
    | ---------------------------------------------------------------------------- | ----- |
    | oral glucose / age / BMI                                                     | 9     |
    | oral glucose / age / Triceps\_skin\_fold\_thickness / N\_times\_pregnant     | 2     |
    | oral glucose / age / Triceps\_skin\_fold\_thickness / serum\_insulin         | 4     |
    | oral glucose / BMI                                                           | 2     |
    | oral glucose / serum\_insulin  / age                                         | 3     |
    | oral glucose / serum\_insulin  / Triceps\_skin\_fold\_thickness              | 2     |
    | oral glucose / serum\_insulin  / Diabetes\_pedigree\_function / age          | 1     |
    | oral glucose / Triceps\_skin\_fold\_thickness / Diabetes\_pedigree\_function | 4     |
    | oral glucose / Triceps\_skin\_fold\_thickness / age   | 3     |

    Oral glucose is the most influential

<br>

#### 2. Random Forest

- gini index
    | Max\_depth    | 1     | **2**      | 3     | 4      | 5    |
    | ------------- | ----- | ------ | ----- | ------ | ---- |
    | Oob\_score    | 0.708 | **0.7557** | 0.741 | 0.76   | 0.75 |
    | Test accuracy | 0.721 | **0.773**  | 0.766 | 0.7338 | 0.73 |  


    Feature importance

    ![gini_rf](/image/rf_gini.png)

    Influence on diabetes in the order of oral glucose > serum insulin > age > BMI

- entropy
    | Max\_depth    | 1      | **2**      | 3      | 4      | 5      |
    | ------------- | ------ | ------ | ------ | ------ | ------ |
    | Oob\_score    | 0.7199 | **0.7492** | 0.7427 | 0.7508 | 0.7476 |
    | Test accuracy | 0.7468 | **0.7857** | 0.7597 | 0.7532 | 0.7468 |

    Feature importance

    ![entropy_rf](/image/rf_entropy.png)

    Influence on diabetes in the order of oral glucose > serum insulin > age > BMI

<br>

#### 3. Gradient Boosting

Test Accuracy : 0.8571

Feature importance

![gbm](/image/gbm.png)

<br>
<br>

### Result

| Model                      | Accuracy |
| ----------------------- | -------- |
| Decision Tree (gini)    | 0.729    |
| Random forest (gini)    | 0.773    |
| Random forest (entropy) | 0.786    |
| Gradient Boosting       | 0.857    |

- Gradient Boosting performed best
- Oral glucose has the greatest effect on diabetes in all models
- Age was of high importance in the decision tree (gini)
- In other models, serum insulin or BMI was significant

<br>
<br>

---


## Regression

### Data : cubic_zirconia
53940 rows , 10 columns

| column  | type        |
| ------- | ----------- |
| Carat   | float       |
| Cut     | categorical |
| Color   | categorical |
| Clarity | categorical |
| Depth   | float       |
| Table   | float       |
| X       | float       |
| Y       | float       |
| Z       | float       |
| Price   | int         |

<br>

### Data Pre-processing

Translating cut, color, and clarity through one-hot encoding because the regression model does not accept categorical variables

<center>

![one_hot_column](/image/one_hot_col.PNG)

</center>
<center>

$\downarrow$

</center>

<center>

![one_hot](/image/one_hot.PNG)

</center> 


<br>

Correlation

![corr](/image/corr.PNG)

After checking multicollinearity, remove them in the order X > Z > Y

### Analysis
#### 1. Linear Regression

R-Squared : 0.917

![Linear_Regression](/image/LR.PNG)

Carat's coefficient is very high => Carat has a very high price impact

<br>

#### 2. Regression tree

| max depth | 1      | 2      | 3       | 4      | 5      | 6      | 7      | 8       | 9      | 10      | 11      | 12     | 13     | 14     | 16      |
| --------- | ------ | ------ | ------- | ------ | ------ | ------ | ------ | ------- | ------ | ------- | ------- | ------ | ------ | ------ | ------- |
| train     | 0.6106 | 0.8251 | 0.8873 | 0.9172 | 0.9404 | 0.9561 | 0.9662 | 0.9731  | 0.9782 | 0.9819  | **0.9849**  | 0.9875 | 0.9899 | 0.9921 | 0.9956  |
| test      | 0.5993 | 0.825  | 0.8854  | 0.9149 | 0.9382 | 0.9553 | 0.9649 | 0.9703 | 0.9741 | 0.9762 | **0.9764** | 0.9757 | 0.9741 | 0.9726 | 0.9703 |

Feature importance

![RT](/image/importance_rt.png)

Carat's coefficient is very high => Carat has a very high price impact

<br>

#### 3. Random Forest

| max depth | 5       | 6       | 7       | 8       | 9       | 10      | 11      | 12      | 13      | 14      | 15       | 16     |
| --------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | -------- | ------ |
| train     | 0.9461 | 0.9607 | 0.9706 | 0.9772 | 0.9813 | 0.9845  | 0.9867 | 0.9891 | **0.9911** | 0.9928  | 0.9941 | 0.9952 |
| test      | 0.9443 | 0.9604 | 0.9698 | 0.9753 | 0.9785 | 0.9801 | 0.9810   | 0.9813 | **0.9813** | 0.9813 | 0.9812  | 0.9811 |

Feature importance

![RF](/image/importance_rf.png)

Carat's coefficient is very high => Carat has a very high price impact

<br>

#### 4. Gradient Boosting

| max depth | 3       | 4      | 5       | 6       | 7       | 8       | 9       | 10      | 11     |
| --------- | ------- | ------ | ------- | ------- | ------- | ------- | ------- | ------- | ------ |
| train     | 0.9764 | 0.9824 | 0.9848 | 0.9867 | **0.9887**  | 0.9904 | 0.9927 | 0.9947 | 0.9966 |
| test      | 0.9760 | 0.9809 | 0.9818 | 0.9824 | **0.9824** | 0.9821  | 0.9824 | 0.9816 | 0.9807 |

Feature importance

![GBM](/image/importance_gbm.png)

Carat's coefficient is very high => Carat has a very high price impact

<br>
<br>


### Result


| Model                    | R-Squared |
| ------------------------ | --------- |
| Linear Regression        | 0.917     |
| Regression Tree          | 0.976     |
| Random forest Regression | 0.981     |
| Gradient Boosting        | 0.982     |


- The performance of each model is very good because it was analyzed through well-refined data

- In all models, the variable that affects diamond prices the most is CARAT

- Influence of other variables is insignificant

- Diamonds of the same carat should be considered for clarity and color