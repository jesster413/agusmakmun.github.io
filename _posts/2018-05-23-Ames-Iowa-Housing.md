---
layout: post
title: Ames, Iowa Housing Prices
published: True
image: titanic.png

---

According to xyz, buying a house is one of the most significant financial investments in a person's life.  But different people value different things - some want to be located in a specific neighborhood, others might want to have as much space as possible, and still others might want a wrap-around porch for those easy summer nights.  People want to get as much bang for their buck, so what if having a two-car garage did not significantly increase the price of the house?  Identifying which features contribute the most to a home's price allows the potential buyer to make the best decision for their lifestyle and budget.  To that end, I analyzed a dataset of homes located in Ames, Iowa to identify which coefficients (features of the house) contributed to the price of the home and then predicted housing prices using Linear Regression.

## 1: Define Question

Want to predict housing price

## 2. Get Data

Got it from Kaggle

## 3. Explore Data

How I dealt with data

## 4. Clean Data

How I cleaned the data  


## 5. Engineer Features

How I engineered Features


## 6. Select Features

Using Select KBest, I identified ten features that contributed the most to the price of a house.

['Overall Qual',
 'Year Built',
 'Total Bsmt SF',
 '1st Flr SF',
 'Gr Liv Area',
 'Garage Cars',
 'Garage Area',
 'Exter Qual_TA',
 'Bsmt Qual_Ex',
 'Kitchen Qual_Ex']


## 7. Polynomialize Features

I polynomialized features within the dataset in order to extract the most correlation out of them.  However, I limited the features that were polynomialized to the top ten features identified by Select K Best in order to reduce computation time and memory used.

## 8. Fit Models

I used a linear regression model with ten features and applied regularization in order to normalize my features.


## 9. Model Evaluation

how i evaluated the models

## 9. Answer Question
