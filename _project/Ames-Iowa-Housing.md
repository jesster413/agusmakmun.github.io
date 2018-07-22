---
layout: project_single
title:  "Ames, Iowa Housing Prices"
slug: "Ames-Iowa-Housing"
published: True
---
Buying a house is a major milestone, and it also can be one of the most costly.  According to XYZ, x percent of Americans spend x amount of money towards purchasing their home.  But within that XYZ, there is a wide range of features that contribute to the price of a home.  But within the population, there is a wide range of what is considered desirable in a home, and that is unique to each home buyer.  Some might want more bedrooms to accommodate a larger family.  Others might want more garage space to fit their growing car collection.  And still others might want a wrap-around porch for those hot summer nights.  Regardless of individual preferences, however, everyone wants the most bang for their buck.  To that end, I analyzed a [dataset](https://www.kaggle.com/c/house-prices-advanced-regression-techniques) of homes located in Ames, Iowa to identify what features of a house contribute the most to its sale price.

## Building the Foundation

To start, I wanted to get a sense of what the dataset looked like, both externally and internally.  The initial shape of the data consisted of 2051 rows and 234 columns, with both numerical (numbers) and categorical (words) data.  I knew I would need to convert the categorical data into a form that I could later feed into a model, but first, I wanted to see if I had any null values to contend with.  Sure enough, both my numerical and categorical columns had null values, as seen in the plot below.

![null-values.png](/static/img/null-values.png)

Based on the fact that 'Pool QC', 'Misc Feature', 'Alley', and 'Fence' all were missing over 80% of their data, I decided to drop those columns entirely, thinking that what few values existed in these columns would not be enough to inform the model one way or another, and would also not be statistically significant from which to impute new data.

For the rest of the columns, I decided to impute the missing data based on the data I had with the [fancyimpute](https://github.com/iskandr/fancyimpute/tree/master/fancyimpute) package which creates values that mimics the characteristics of values similar to them using a k-nearest neighbors distance calculation.  This is similar to another technique related to upsampling the minority class that I've used in other projects.

I first imputed the numerical columns given that the data was already in the correct data type.  I decided to use 3 k-nearest neighbors for calculation purposes and saved the imputed (and existing) data in a dataframe called 'imputed_numerical' which I decided to tack on to the initial train dataframe later.  

The categorical columns, however, were not as easy to impute.  Because fancy impute only takes in numbers, I decided to effectively "LabelEncode" my values while simultaneously skipping over the null values (so as not encode them and nullify the imputation exercise.)  With my string values encoded with numerical values, I imputed the remaining null values with nine k-nearest neighbors.  Having imputed all of the missing values, I then dropped the columns in the train set with the missing values and concatenated both the imputed_numerical and imputed_categorical onto the train dataframe, effectively replacing the initial columns with the missing values. I dummified the categorical columns that were not missing data to try to maintain as much interpretability as possible by assigning a new column to each unique value in the categorical columns.  Having finished cleaning the data, it was ready to be fed into some models.

<!-- Next, I dummified all of the object columns in both of the train and test datasets.  While most of those columns were the same, there were some that differed between the train and test datasets.  In this iteration of the project, I "subtracted" the test set columns from the training set columns to identify what columns were present in the training set that were not present in the test set.  I then dropped those extra columns in the training set.

In a future iteration of this project, I would not drop the extra training columns.  Instead, I would train on the full set, even if it meant that my predictions would be less accurate because this is more representative of how data is collected and presented in the real world - you won't always have the benefit of knowing what the test set looks like in advance.  Rather, it may come in later and you will have to apply the model to it regardless.

Maybe talk about data leakage.  This is the first time I came up against this issue.  Maybe talk about data integrity.

With that in mind, I set out to dummify the object columns in my training set and if there were columns in the test set that did not match those in the training set, I created new columns within the test set and set their values to zero. -->

## Choosing the Framework

Because I was most interested in which features contribute the most to a home's price, I applied SelectKBest to the dataset.  Unsurprisingly, the top ten features that are most predictive of a home's sale price were: 'Overall Qual', 'Year Built', '1st Flr SF', 'Gr Liv Area', 'Garage Yr Blt', 'Total Bsmt SF', 'Garage Area', 'Garage Cars', 'Bsmt Qual', and 'Exter Qual_TA.'  Looking at these features, it makes sense that the overall quality of a home is most predictive of the value of a home, as does the square footage of the first floor, the basement, and garage area.  No matter how complicated a data science project is, I always find it useful to take a step back sometimes and ask myself whether the results make sense.  Machine learning isn't some magical black box (although sometimes it feels that way) but rather, it's a tool that extends the human analytical capabilities to crunch numbers faster and across far larger datasets than anyone has time for manually.  Having checked the top ten features, I then used SelectKBest to identify the top 100 most predictive features of a home's sale price to feed into the model.  I then polynomialized those features to capture the information gained from the interactions between features.  I limited the features I polynomialized to 100 out of concern for computation issues.

Having scaled and train-test-split my dataset, I was ready to feed my data into some Linear Regression models.

## Regularization (Normalizing the coefficients)

Because I was using linear regression, I also wanted to regularize my data using several different regularizing techniques, namely, Ridge, Lasso, and Elastic Net.  

Maybe reference a previous post in which I go more in depth on each regularizer.  

## Evaluating the Offer

Talk here about model Evaluation

Talk about coefficients and interpretability
