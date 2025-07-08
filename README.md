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
Then we explore our dataset, if it has any null values, or compare the saledate with the SalePrice column and plot it to check the output 
![image](https://github.com/user-attachments/assets/15bc34b6-7a84-44d8-bfb1-7a71ee2677b6)
See at the bottom, the saleDate column mixed because it has a different type like object

![image](https://github.com/user-attachments/assets/0726b9a3-3ca5-4484-a336-1bdc5e926ae3)
### Parsing Dates
When we work with time series data, we want to enrich the time and date component as much as possible
>We can do that by telling pandas which of our columns has dates in it using the `parse_date` parameter
>parse_date is a function that converts a string representation of a date into a date object
Now we will import the dataset again but this time with parse_date parameter
Then if we recheck the data you'll see it has a different type for the saledate column
and now if we check the scatter again, we will see it has better distribution

![image](https://github.com/user-attachments/assets/a3ae1e96-f22f-4440-a653-a9e13b39624e)
### Sort the dataframe by saleDate
> When working with time series data, it's a good idea to sort it by date
Now we will create a copy of the dataframe so that we can make changes to te copy one and it doesn't mess up the original data
df_tmp = df.copy()
df_tmp.head(5)
Up next, we will
### Add datetime parameter for `saledate` column
which is going to help us organize the dataframe a bit more and it will be easier to work with
