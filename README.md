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

   #### [Data Cleansing/Wrangling](https://github.com/sittingman/airbnb_booking/blob/master/1.data_expo.ipynb): 
   - Understanding the shape of the datasets, check for missing value, and align on data types.
   - Findings: 
       - convert all date/time related columns into datetime
       - deal with missing age and through assigning new variables and binning
       - filling first_affiliate_tracked missing value with new category value "missing"no missing data. 
       - create new categorical as needed to summary sparse variables

   #### [Exploratory Analysis](https://github.com/sittingman/airbnb_booking/blob/master/1.data_expo.ipynb): 
   - Finding variables may influence the likelihood of a new user making a booking
   - Findings: 
       - almost all variables had some influence on the likelihood of booking (under chi-square test)
       - drop affiliate provide as that is highly predictive by affiliate channel
       - for session data, we will assume the last record of the each user action is the last action for the session, and created binary variables based on content on action type and details.

   
#### Machine Learning: 

    
**Findings:**

**Model performances based on train dataset (test_size =.3, 5-fold cross validation)**


### Performance Summary



### Recommendations/next steps



[Capstone Report]

[Presentation Slides]