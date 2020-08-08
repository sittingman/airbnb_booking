# Airbnb New Users Booking

[Airbnb](www.allstate.com) is one of the world’s largest marketplaces for unique, authentic places to stay and things to do, offering over 7 million accommodations and 50,000 handcrafted activities, all powered by local hosts. An economic empowerment engine, Airbnb has helped millions of hospitality entrepreneurs monetize their spaces and their passions while keeping the financial benefits of tourism in their own communities. With more than three quarters of a billion guest arrivals to date, and accessible in 62 languages across 220+ countries and regions, Airbnb promotes people-to-people connection, community and trust around the world.

### Objective
- Predict in which country a new user will make his or her first booking. Evaluation metric is NDCG (Normalized discounted cumulative gain) at k = 5. In other word, making a maximum of 5 predictions on the country of the first booking at the used id level

### Clients & Impacts:

- **Airbnb Leadership**: model can be used as a recommendation system for new users to encourage conversion with better targeted marketing.


- **Airbnb Clients**: more relevant recommendation to clients, which enable smoother user experience on finding the first destination.

### Data Source:
- [Kaggle](https://www.kaggle.com/c/airbnb-recruiting-new-user-bookings/data)
    - To obtain data, download from the link above, unzip and put all csv files into a folder called "data" OR using the script [HERE](https://github.com/sittingman/airbnb_booking/blob/master/0.obtain_data.ipynb)
- [Data Dictionary](https://github.com/sittingman/airbnb_booking/blob/master/data_dict.ipynb)

### Outline of Approach

    #### [Data Cleansing/Wrangling](https://github.com/sittingman/allstate_insure/blob/master/1.data_wrangling.ipynb): 
    - Understanding the shape of the datasets, check for missing value, and align on data types.
    - Findings: 
        - convert all date/time related columns into datetime
        - deal with missing age and through assigning new variables and binning
        - filling first_affiliate_tracked missing value with new category value "missing"no missing data. 

#### [Exploratory Analysis](https://github.com/sittingman/allstate_insure/blob/master/2.exploratory.ipynb): 
Finding variables may correlate with the target variable and eliminate variables that have collinearity. Determine proper transformation based on data structure
- Findings: cont9, cont10, cont12, and cont13 will be dropped as they are highly correlated with existing variables.
- Target variable will require power transformation during model fitting
- Given there are 116 categorical variables, traditional visualization is not effective in identifying features importance. We will apply recursive feature elimination (RFE) and Lasso to narrow down features.
    
#### Machine Learning: 
**Strategy**:
- [Part 1](https://github.com/sittingman/allstate_insure/blob/master/3.ML_p1.ipynb)Identify the best machine learning algorithm based on continuous variables to obtain baseline performance
- [Part 2](https://github.com/sittingman/allstate_insure/blob/master/3.ML_p2.ipynb) Add categorical variables and apply RFE to filter low power features, re-train models, find the best performers
- [Fine tune models](https://github.com/sittingman/allstate_insure/blob/master/4.submit.ipynb) through hyperparameters tuning
    
**Findings:**
- Boosting models perform better than tree base models, which has equivalent performances as linear models. Yet, tree base models have a long processing time. They are not recommended models for this problem.
- The full number of features post one-hot encoding is 1033, of which 123 features were selected after applying LassoCV, RFE, and RFECV dimension reduction methods

**Model performances based on train dataset (test_size =.3, 5-fold cross validation)**

| Model | MAE | Time |
| ---- | ---- | ---- |
|Dummy | 1809 | 1s |
|Linear Reg | 1278 | 3s |
|Ridge | 1278 | 3s|
|SGD | 1278 | 3s|
|Extra Tree | 1249 | 10 mins|
|Random Forecast | 1219 | 23 mins|
|Ada | 1653 | 1 min |
|LightGBM | 1161 | 1 min |
|XGBoost | 1217 | 3 mins |
|CatBoost | 1157 | 19 mins |

We select SGD, Lightbgm, Xgboost, and Catboost to perform hyperparameters tunning.

### Performance Summary

| Model | MAE | Time |
| - | - | - |
|Dummy | 1783 | 1s |
|SGD Regression | 1266 | 4s |
|LightGBM | 1129 | 14s |
|XGBoost | 1149 | 44s |
|CatBoost | 1122| 43s|


### Recommendations/next steps

CatBoost has the best MAE among all models, followed closely by LightGBM.

While the evaluation metric is MAE, clients should be aware it is the average of above 120k insurance claims. MAE understated the impact from outliers to customer satisfaction, particularly for claims payments that are significantly underestimated from the true reimbursement that clients should get. On the other hand, there are low-value claims with high loss predictions. That will result in unnecessary final burdens to Allstate.

The model can be further improved if the definitions of the categorical and continuous variables are given. It will help practitioners to perform more precise features selection and target the outlier issues with a better context. From a high-level, exploratory analysis on the [outliers](https://github.com/sittingman/allstate_insure/blob/master/3.ML-outliers.ipynb), over predictions tend to happen more often at the low-value claims, while under predictions happen toward claims $15K or more in values.

We recommend Allstate to perform audits for claims that have high predicted values (threshold to be determined by Allstate management based on tolerance level). Make it is easy for customers to report inaccurate claims to analyze the miscalculated factors and adjust models accordingly.


[Capstone Report](https://github.com/sittingman/allstate_insure/blob/master/capstone_report_allstate.pdf)

[Presentation Slides](https://github.com/sittingman/allstate_insure/blob/master/allstate_present.pdf)