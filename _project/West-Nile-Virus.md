---
layout: project_single
title:  "West Nile Virus"
slug: "West-Nile-Virus"
---
West Nile Virus (WNV) was first discovered in Uganda in 1937 and persists through the present day in many disparate parts of the world, spreading primarily through female mosquitoes and affecting several types of bird species, as well as other mammals like horses and humans.  Infected victims can either be asymptomatic (75% of cases) or display symptoms like fever, rash, vomiting (20% of cases), encephalitis/meningitis (1% of cases), and in rare instances death (1 out of 1500).[^1]  While the first case of WNV in the US was reported in Queens, NYC in 1999, Chicago has been particularly plagued by the disease since it was first reported in the Windy City in 2001.  As of 2017, Chicago has reported 90 cases of WNV, eight of which resulted in death.[^2]  

Since 2004, Chicago has had set up a surveillance system of mosquito traps designed to monitor which areas of the city were positive for WNV which remains in effect today.  Every week from late spring through the fall, mosquitos in traps across the city are tested for the virus. The results of these tests influence when and where the city will spray airborne pesticides to control adult mosquito populations.

In June 2015, the City of Chicago [released their data](https://www.kaggle.com/c/predict-west-nile-virus) to Kaggle in the form of a competition, asking competitors to analyze their datasets and predict which areas of the city would be more prone to WNV outbreaks, thereby allowing the City and Chicago Department of Public Health (CPHD) to allocate resources more efficiently.  The prize, $40,000.

While I was a little late to the competition, this dataset remains one of the more popular sets to analyze, given its depth, complexity, and uniquely inherent challenges.  Below, I identify the challenges that came up for me when analyzing this dataset as well as the techniques I used to resolve them.

## Going in for a check-up

As with many projects, the raw data was split across a several different dataframes, including a train.csv, a test.csv, a weather.csv, and a spray.csv.  The spray.csv was a collection of locations and timestamps where pesticides were sprayed - I initially set this aside given that it was not immediately obvious to me how I was going to incorporate it into my model.  

Talk about weather.csv here and the columns that it had

weather = weather.drop(['Water1', 'Depart', 'Depth', 'CodeSum'], axis=1)

weather = weather.dropna()

weather = weather.replace('  T', 0)

station1 = weather[weather['Station']==1]




Talk about train.csv here and the shape of it and what columns it had

While the train.csv initially had 'Date', 'Address', 'Species', 'Block', 'Street', 'Trap',
       'AddressNumberAndStreet', 'Latitude', 'Longitude', 'AddressAccuracy',
       'NumMosquitos', 'WnvPresent'

columns, because I grouped by the mean for 'Species', 'Date', and 'Trap' to mirror how the test set was organized, I was left with columns that were capable of being grouped and averaged, namely,'Block', 'Latitude', 'Longitude', 'AddressAccuracy', 'NumMosquitos', and my target column, 'WnvPresent'.  I later dropped 'Block', 'Latitude','Longitude', and 'AddressAccuracy' because I engineered a few columns that I thought would be more helpful (and predictive) of WNV.

In both the train and test sets, there were missing values labeled 'M' as opposed to NaN so while they didn't register as null values, they effectively were.  I decided to fill the rows with the 'M' values with NaN values that I could drop later.






## We're going to have to operate

I decided to engineer a column called LatLong' which was just a tuple of the 'Latitude' and 'Longitude' columns.  I did this because I wanted to calculate the distance of any given point in my train data from 'hot spots' I identified through aggregating my WNV column.  The package I used to calculate distances was [vincenty](https://pypi.org/project/vincenty/) from geopy.distance which takes a tuple.

I calculated distances from two major centroids

centroid1 = (41.974689, -87.890615)
centroid2 = (41.673408, -87.599862)

I then wrote a for-loop that calculated the distances in miles for each of the points from the two centroids and saved the result in a list that I then saved to my dataframe as a new feature called 'Distances1' and 'Distances2'.  I also created another column called 'Close_to_Centroid2' which was a lambda function of my 'Distances2' feature that binarized whether a point was within five miles of the centroid or not.





## What's the Prognosis?

One of the most difficult aspects of this project was the fact that the baseline accuracy for the dataset was 95% accuracy.  This meant that if you were to guess that WNV was not present, you would be correct 95 out of 100 times.  While this may not seem like a big deal, it is very costly to the City to A. spray expensive pesticides where WNV is not present and run the risk of unnecessarily exposing residents to the toxins and B. not spray pesticides where WNV is present and run the risk of residents becoming ill with the virus and potentially being sued for negligence.

Keeping this in mind, I installed the [imblearn](http://contrib.scikit-learn.org/imbalanced-learn/stable/api.html) package which contains a class balancer, SMOTEENN.  What does it stand for. Explain how SMOTEEN works here - it synthetically creates new data points that mirror the under-represented class in order that the majority and minority classes are balanced.  This allows the model to learn more deeply about what characteristics are inherent to the target class.

I ran three models, Random Forest Classifier, XGBoost, and a Balanced Bagging Classifier.

Explain how XGBoost works here.

Explain how Balanaced Bagging Classifier words here.

Talk about scoring on 'roc-auc' instead of accuracy here.  What is roc-auc.



---
[^1]: [http://www.northwestmvcd.org/Northwestmvcd/West_Nile.html](http://www.northwestmvcd.org/Northwestmvcd/West_Nile.html)
[^2]: [http://chicago.cbslocal.com/2018/05/30/west-nile-virus-reported/](http://chicago.cbslocal.com/2018/05/30/west-nile-virus-reported/)
