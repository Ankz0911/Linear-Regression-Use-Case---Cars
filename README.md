# Linear-Regression-Use-Case---Cars

The dataset contains price , year , model , mileage , body type , engine type , engine capacity and current registration status. 
we start off by importing the data set and viewing it's properties and then we did the following steps to generate a linear regression model. 

A) Preprocessing 
  
  1. we find the number of null values in our dataset by using dataset.isnull() function and then we drop the rows having the null values. 

  2. we begin the preprocessing by looking at the stats available to us using the describe function . 

  3. we find the number of missing values by using the is.null().sum() function.

  4. we notice that year , Mileage and EngineV has missing values .   

  5. we clean the following variables accordingly :
     a) Mileage : we plot the distplot and see a long time on the right side , hence showing outliers . We drop the outliers by 99 percentile using the quantile method of              pandas.
     b) EngineV : did a basic google search and found that engine capacity is usually below 6.5 and noticed that our dataset has values such as 99.99 showing that many times the        user has mentioned 99.99 whenever EngineV was not available . So , we clean the data by removing all rows that have EngineV above 6.5 . 
     c) Year : after drawing a distplot of Year variable , we notice that the tail of the graph is on left side showing outliers implying that we had some cars older than 1980 ,         so by using the quantile method we kept the 99% of the data and discarded 1% .

  6. in this cleaning process , we deleted around 250 incorrect \ missing entries from the dataset. 
 
B) Checking the Linear Regression Assumptions :  
  1. For Linearity : 
    i) after dealing with outliers , we plotted the data points of price with Year , EngineV and Mileage but we could not spot a linear regression on the graph , then we draw a           distplot of the variable "Price" and we see that it's not normally distributed. 
    
    ii) so we converted the price to log price .

    iii) When we plotted the graph of log price , with Year , Mileage and EngineV we could spot a linear regression indicating that the data is now clean and ready for next              check. 
  
  2. No endogeneity : 
    by experience , we can say that this assumption is not violated , but we will still check it after creating the regression. 
    
  3. Normality and homoscedasticity
    i) Normality
    Normality is assumed since it's a big sample following the central limit theorum . 
    
    ii)Zero Mean
    zero mean of the distribution of errors is accomplished through the inclusion of the intercept in the regression since we are already including b0 in our linear regression       equation.
    
    iii) Homoscedasticity
    it holds , as we see a linear relation in the graphs of log price with Year , Mileage and EngineV whereas we have already implemented a log transformation of price , which       is the most common fix for homoscedasticity.
 
  4. No auto correlation
     we don't need to worry about auto correlation as our data is not coming from time-series data or panel data. logically , there is no reason for the data to be correlated.
   
  5. Multicollinearity
    i)a relation is expected inbetween "Year" and "Mileage" since the older the car is , it's expected to have more mileage\kms driven. 
    ii)we use variance inflation factor to determine multicollinearity , we check the 3 continous variables that we have already dealt with , "Year" "Mileage" and "EngineV" ,          since we will deal with the remaining variables later on . 
    iii) we notice that the variable "Year" has a vif of above 10 , thus we drop the variable. 
    
B) Creating dummy variables 
  we create dummy variables using pandas.get_dummies() to autogenerated the dummies of all categorical variables. while creating dummies , we drop the first category from each     variable because if all other categories are 0 then the first category will be 1 automatically. If we don't drop the first category then we will introduce multicollinearity in   the dataset. We save this dataset to data_preprocessed. 

C) Creating the Linear Regression Model
  i) we declare the target as 'log_price' from data_preprocessed and input as all data in data_preprocessed without the column 'log_price'. 
  ii) we standardize the dataset by using standard scaler from sklearn and save it to variable 'inputs_scaled' . 
  iii) we split the data into test train datasets in the ratio of 80:20 using the test_train_split function of sklearn.model_selection. 
  iv) we declare the variable 'reg' as an object of linearregression class and fit the model by using x_train , y_train. 
  v) we declare y_hat = reg.predict(x_train).
  vi) we plot the scatterplot of y_train and y_hat to test the accuracy of our model.
  vii) we draw sns.displot using y_train - y_hat to get residual PDF . The graph further strenghtens that there is zero mean (assumptions of linear regression) .
  viii) we use reg.score(x_train , y_train) and we get 83% which means that our model is explaining 83% of the variability of our dataset. 
  
D) Finding the weight and bias.
  we use the reg.intercept_ and reg.coef_ to find bias and weight and save these to a table called reg_summary.

E) Testing the model 
  i) we create a variable 'y_hat_test' and store reg.predict(x_test) to it.
  ii) then we plot the data points of y_hat_test and y_test to check the accuracy . 
  iii) we create a new dataset named df_pf using pd.DataFrame and store the exponential values of predicted log_price(using np.exp).
  iv) we reset the index values of df_pf to get more accurate results. 
  v) we add the values of 'price' as 'targets' in df_pf and add columns to calculate the difference and difference % . 
  vi) we sort df_pf by difference% to get a better overview. 

  
