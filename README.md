# Airbnb New Users Booking

[Airbnb](www.allstate.com) is one of the world’s largest marketplace for unique, authentic places to stay and things to do, offering over 7 million accommodations and 50,000 handcrafted activities, all powered by local hosts. An economic empowerment engine, Airbnb has helped millions of hospitality entrepreneurs monetize their spaces and passions while keeping the financial benefits of tourism in their communities. With more than three-quarters of a billion guest arrivals to date, and accessible in 62 languages across 220+ countries and regions, Airbnb promotes people-to-people connection, community, and trust.

### Objective
- Predict in which country a new user will make his or her first booking. The evaluation metric is NDCG (Normalized discounted cumulative gain) at k = 5. In other words, making a maximum of 5 predictions on the country of the first booking at the used id level. NDCG is often used to measure the effectiveness of web search engine It measures the usefulness, or gain, of the search result based on its position within the result.

### Clients & Impacts:

- **Airbnb Leadership**: model will be used as a recommendation system for new users to encourage conversion with better-targeted marketing.


- **Airbnb Clients**: more relevant recommendations to clients, enabling a smoother user experience to find the first destination.

### Data Source:
- [Kaggle](https://www.kaggle.com/c/airbnb-recruiting-new-user-bookings/data)
    - To obtain data, download from the link above, unzip and put all csv files into a folder called "data" OR using the script [HERE](https://github.com/sittingman/airbnb_booking/blob/master/0.obtain_data.ipynb)
- [Data Dictionary](https://github.com/sittingman/airbnb_booking/blob/master/data_dict.ipynb)

### Outline of Approach

   #### [Data Cleansing/Wrangling](https://github.com/sittingman/airbnb_booking/blob/master/1.data_expo.ipynb): 
   - Understanding the shape of the datasets, check for missing value, and align on data types.
   - Findings: 
       - convert all date/time related columns into DateTime
       - deal with missing age and through assigning new variables and binning
       - filling first_affiliate_tracked missing value with new category value "missing"no missing data. 
       - create new categorical as needed to summarize sparse variables

   #### [Exploratory Analysis](https://github.com/sittingman/airbnb_booking/blob/master/1.data_expo.ipynb): 
   - Identify variables that may influence the likelihood of a new user making a booking
   - Findings: 
       - almost all variables under "train" dataset had some influence on the likelihood of booking (under chi-square test)
       - drop affiliate provide as that is highly predictive by the affiliate channel (using predictive power score)
       - for "session" data, we will assume the last record of each user action is the last data row for each user id, and created binary variables based on content on action type and details

   
   #### [Machine Learning](https://github.com/sittingman/airbnb_booking/blob/master/4.ML-clf.ipynb): 
   - Strategy:
       - apply feature selection method to narrow down numbers of binary variables from action details and type. Use LassoCV, followed by Recursive Feature Elimination (RFE) using Logistics Regression and Random Forest as regressors.
       - combine features set as identify from exploratory analysis with the action details binary variables
       - compare base model performance across five types of classification models based on NDCG score. Pick top three models for hyperparameters tunning
       - apply [deep learning method](https://github.com/sittingman/airbnb_booking/blob/master/4.ML-deep.ipynb) using Keras to identify potential performance improvement
    
   - Findings:
      - the first test on one train test suggested that all models have ndgc scores around 0.8
      - once validated with 4 folds cross-validation, tree base models and light gbm appears to have the best results
      - Perform hyperparameters tunning using Bayesian optimization with hyperopt, train-set results are as follow:
          | Models | ndcg on train set |
          | --- |  --- |     
          |extra trees | 0.822 |
          |random forecast | 0.806 |
          |lightgbm| 0.824 | 
       - Perform deep learning using Keras, ndcg score is 0.826
          
### Performance Summary 

| Models | Kaggle ndcg | Time |
| --- |  --- | --- |    
|dummy classifier | 0.66094 | 
|extra trees | 0.85212 | 1m 42s |
|extra trees (tuned) | 0.86487 |8m 49s |
|random forecast | 0.85788 | 1m 7s
|random forecast (tuned) | 0.85359 | 25s |
|lightgbm| 0.86849 | 22s |
|lightgbm tuned | 0.86769 | 41s | 
|Deep learning (keras) | 0.86891 | 44s |


### Recommendations/next steps

Lightgbm (no tunning) performed better than the tree-based models and is recommended as the winning model. 
 
Target variable is skewed toward US (70%). Perhaps analysis could be done at a more micro level within US to make the predictions more precise – need extra data points from Airbnb.
 
Keras deep learning model had a slight improvement over the lightgbm with a rather simple set up of one hidden layer. This suggests a further improvement in ndcg is possible should clients preferred to investigate further. However, the computation will get expensive. 
 
More features could be generated under the session activities data since this study only considered the last action details. Robust evaluation is needed to determine what features to choose, as the dataset could become complex and result in an overfitting problem.


[Capstone Report](https://github.com/sittingman/airbnb_booking/blob/master/Capstone%20Report_airbnb.pdf)

[Presentation Slides](https://github.com/sittingman/airbnb_booking/blob/master/airbnb_presentation.pdf)