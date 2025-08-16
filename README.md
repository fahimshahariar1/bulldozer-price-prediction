# bulldozer-price-prediction
In this machine learning project, our goal will be to predict the sale price of bulldozers
## 1. Problem Definition
> How well can we predict the future sale price of a bulldozer, given its characteristics and previous examples of how much similar bulldozers have been sold for
## 2. Data
You can find the dataset on Kaggle: https://www.kaggle.com/c/bluebook-for-bulldozers/data

Some of the important Data are
* Train.csv is the training set, which contains data through the end of 2011.
* Valid.csv is the validation set, which contains data from January 1, 2012, to April 30, 2012. You make predictions on this set throughout the majority of  the competition. Your score on this set is used to create the public leaderboard.
* Test.csv is the test set, which won't be released until the last week of the competition. It contains data from May 1, 2012, and November 2012. Your score on the test set determines your final rank for the competition.
## 3. Evaluation
The evaluation metric for this competition is the RMSLE (root mean squared log error) between the actual and predicted auction prices.
## 4. Feature:
> Kaggle provides a data dictionary detailing all of the features of the dataset that you can view here: https://docs.google.com/spreadsheets/d/15BRJM7E4vBXZfs_xqV-hDMagQChiRPyrdzN96NXCE50/edit?usp=sharing

Importing training and validation sets
 
df = pd.read_csv("bluebook-for-bulldozers/TrainAndValid.csv",low_memory=False)
df.info()
Then we explore our dataset, if it has any null values, or compare the saledate with the SalePrice column, and plot it to check the output 
![image](https://github.com/user-attachments/assets/15bc34b6-7a84-44d8-bfb1-7a71ee2677b6)
See at the bottom, the saleDate column is mixed because it has a different type, like an object

![image](https://github.com/user-attachments/assets/0726b9a3-3ca5-4484-a336-1bdc5e926ae3)
### Parsing Dates
When we work with time series data, we want to enrich the time and date component as much as possible
>We can do that by telling pandas which of our columns has dates in it using the `parse_date` parameter
>parse_date is a function that converts a string representation of a date into a date object
Now we will import the dataset again, but this time with the parse_date parameter
Then, if we recheck the data, you'll see it has a different type for the saledate column
And now, if we recheck the scatter, we will see it has a better distribution

![image](https://github.com/user-attachments/assets/a3ae1e96-f22f-4440-a653-a9e13b39624e)
### Sort the dataframe by saleDate
> When working with time series data, it's a good idea to sort it by date
Now we will create a copy of the dataframe so that we can make changes to the copy and it doesn't mess up the original data
df_tmp = df.copy()
df_tmp.head(5)
Up next, we will
### Add datetime parameter for `saledate` column
which is going to help us organize the dataframe a bit more, and it will be easier to work with
df_tmp["saleYear"] = df_tmp.saledate.dt.year
df_tmp["saleMonth"] = df_tmp.saledate.dt.month
df_tmp["saleDay"] = df_tmp.saledate.dt.day
df_tmp["saleDayofWeek"] = df_tmp.saledate.dt.day_of_week
df_tmp["saleDayofYear"] = df_tmp.saledate.dt.day_of_year
Now that we have enriched our Dataframe with date time features, we can remove the saledate column.
## 5. Modelling
> We have done a fair bit of EDA, now we will try to do some modelling and build a model using Scikit learn.
Remember, you can find the right estimator or model using the scikit-learn map
> https://scikit-learn.org/stable/machine_learning_map.html
Now, once we start our modelling, we will see that there are a lot of errors that we are getting. That's because our data is not in the best shape. There might be non-null values, values that might not be numbers of any kind that we can convert and so forth.
So, our first work would be to convert those values, such as strings, to categories
### Convert string to categories
> One way we can turn all of our data into numbers is by converting it into pandas categories
> You can check a few in here https://pandas.pydata.org/pandas-docs/version/1.4.4/reference/general_utility_functions.html
First, we will be finding all the labels or columns that have string values
**for label, content in df_tmp.items():
    if pd.api.types.is_string_dtype(content):
        print(label)**
As you can see, we have made changes to the columns that contain string values, but our dataset still has some missing values that need to be filled. Without it, we will again run into an error 
Then we will convert them into pandas categories
**for label, content in df_tmp.items():
    if pd.api.types.is_string_dtype(content):
        df_tmp[label] = content.astype("category").cat.as_ordered()**
## Fill missing values
### Fill numeric missing values first
We will first sort out the columns that are numeric and has missing values
**for label, content in df_tmp.items():
    if pd.api.types.is_numeric_dtype(content):
        print(label)**
 Then we will check which numeric column has null values
**for label, content in df_tmp.items():
    if pd.api.types.is_numeric_dtype(content):
        if pd.isnull(content).sum():
            print(label)**
 Now we try to fill those missing values with the median
 **Fill Numeric rows with the median
for label, content in df_tmp.items():
    if pd.api.types.is_numeric_dtype(content):
        if pd.isnull(content).sum():
          df_tmp[label+"_is_mssing)"] = pd.isnull(content)
          df_tmp[label] = content.fillna(content.median())**
Now, although we have filled all the missing numerical values, we still have some categorical variables that have missing values
## Filling and turning categorical variables into numbers
> #Check for columns that aren't numeric
for label, content in df_tmp.items():
    if not pd.api.types.is_numeric_dtype(content):
        print(label)
> #Turn categorical variables into numbers and fill missing
for label, content in df_tmp.items():
    if not pd.api.types.is_numeric_dtype(content):
        df_tmp[label+"_is_missing"] = pd.isnull(content)
        df_tmp[label] = pd.Categorical(content).codes+1
The reason why we are using + 1 is because, by default, pandas, when we convert, assigns a number -1 to all those missing values.
Now if we check, there shouldn't be any more missing values 
        
