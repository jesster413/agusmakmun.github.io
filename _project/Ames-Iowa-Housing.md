---
layout: project_single
title:  "Ames, Iowa Housing Prices"
slug: "Ames-Iowa-Housing"
---
Buying a house is a major milestone, and it also can be one of the most costly.  According to XYZ, x percent of Americans spend x amount of money towards purchasing their home.  But within that XYZ, there is a wide range of features that contribute to the price of a home.  But within the population, there is a wide range of what is considered desirable in a home, and that is unique to each home buyer.  Some might want more bedrooms to accommodate a larger family.  Others might want more garage space to fit their growing car collection.  And still others might want a wrap-around porch for those hot summer nights.  Regardless of individual preferences, however, everyone wants the most bang for their buck.  To that end, I analyzed a [dataset](https://www.kaggle.com/c/house-prices-advanced-regression-techniques) of homes located in Ames, Iowa to identify what features of a house contribute the most to its price.

## Clean Data

To start, I imputed any missing values with the mean of that column.  When time permits, I would like to go back and use a more sophisticated method of imputing values called [MICE](http://scikit-learn.org/dev/modules/impute.html) which uses an algorithm to calculate a more accurate value.  But for now, I used the mean to fill missing values.  Next, I dummified all of the object columns in both of the train and test datasets.  While most of those columns were the same, there were some that differed between the train and test datasets.  In this iteration of the project, I "subtracted" the test set columns from the training set columns to identify what columns were present in the training set that were not present in the test set.  I then dropped those extra columns in the training set.

In a future iteration of this project, I would not drop the extra training columns.  Instead, I would train on the full set, even if it meant that my predictions would be less accurate because this is more representative of how data is collected and presented in the real world - you won't always have the benefit of knowing what the test set looks like in advance.  Rather, it may come in later and you will have to apply the model to it regardless.

Maybe talk about data leakage.  This is the first time I came up against this issue.  Maybe talk about data integrity.

With that in mind, I set out to dummify the object columns in my training set and if there were columns in the test set that did not match those in the training set, I created new columns within the test set and set their values to zero.

## Selecting Features

Because I was most interested in which features contribute the most to a home's price, I applied SelectKBest to the dataset.  Unsurprisingly, the top ten features that are most predictive of a home's sale price are: 'Overall Qual', 'Year Built', 'Total Bsmt SF', '1st Flr SF', 'Gr Liv Area', 'Garage Cars', 'Garage Area', 'Exter Qual_TA', 'Bsmt Qual_Ex', and 'Kitchen Qual_Ex'.  

I then polynomialized those top ten features, suspecting that the combinations of features would also contribute to the price of a home.  For example, the combination of having a porch *and* a backyard would contribute to the price of a home in a slightly different way than just a porch or just a backyard.

Having scaled my data and conducted a train-test-split, I was ready to feed my data into a Linear Regression model.

## Regularization

Because I was using linear regression, I also wanted to regularize my data using several different regularizing techniques, namely, Ridge, Lasso, and Elastic Net.  

Maybe reference a previous post in which I go more in depth on each regularizer.  
