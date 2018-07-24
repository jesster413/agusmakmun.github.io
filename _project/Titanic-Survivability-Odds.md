---
layout: project_single
title:  "Titanic Survivability Odds"
slug: "Titanic-Survivability-Odds"
published: true
---
While the infamous shipwreck happened over 100 years ago, its cultural significance is sunk deep into our collective memory as one of the most tragic manifestations of hubris, classism, and dumb luck.  To that end, I analyzed the data provided on Kaggle's website to determine more specifically how features such as age, gender, class, and wealth predetermined a passenger's fate on April 15, 1911 aboard the USS Titanic.

## "Where to, Miss?"

<iframe src="https://giphy.com/embed/ghvWn8S0jiI0M" width="480" height="203" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/movie-titanic-ghvWn8S0jiI0M">via GIPHY</a></p>


For this project, I wanted to predict a passenger's fate using machine learning across several different types of models.  I was also interested in identifying which features had the greatest impact on a person's chances of survival.

This dataset is neatly packaged in a .csv file that can be downloaded from Kaggle's [Titanic competition](https://www.kaggle.com/c/titanic/data) page.  In some of [my other projects](https://thedatasleuth.github.io/category/Web-scraping) where the data is not so readily accessible, I have used research and web-scraping techniques to assemble my datasets.

## "Remember, they love money, so pretend like you own a gold mine and you're in the club"

Whenever I begin with a new dataset, I like to get an understanding of its shape, stats, and potential pitfalls (null values, outliers, categorical data).  I use descriptors like df.info(), df.shape(), df.describe() as well as a mask I wrote to slice on columns that specifically have null values.

In sum, this dataset has 891 rows across 12 columns, five of which are categorical columns: 'Name',	'Sex',	'Ticket',	'Cabin',	'Embarked' and seven of which are numerical columns: 'PassengerId',	'Survived',	'Pclass',	'Age',	'SibSp',	'Parch',	'Fare'.

Given that the 'Cabin' category was missing almost 80% of its data, I decided not to try to impute the missing values and instead relabeled them as "UI" (Unidentified) from which I then stripped just the first letter from all the cabins in order to make a dummy column called Cabin_category.  This column organized the cabins in this neat array: 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'T', 'U'.  The 'Age' feature, however, was only missing about 20% of its data, so it made sense to me to impute the missing values using MICE within the fancyimpute package.  I decided to drop the two rows in the 'Embarked' feature entirely.

For illustrative purposes, I plotted some graphs with Plotly to help visualize the dataset.  In the 'Age' plot, we can see that the majority of people who died were in the 20-39 bins, whereas the majority of people who lived were in the 0-9 bin.  It makes sense that children would have been prioritized both in terms of what was considered the moral thing to do as well as how much space they took up on the life rafts.  


<div>
    <a href="https://plot.ly/~jesster413/787/?share_key=dGWKE4WznFehAsCcV0SKD0" target="_blank" title="plot from API (19)" style="display: block; text-align: center;"><img src="https://plot.ly/~jesster413/787.png?share_key=dGWKE4WznFehAsCcV0SKD0" alt="plot from API (19)" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="jesster413:787" sharekey-plotly="dGWKE4WznFehAsCcV0SKD0" src="https://plot.ly/embed.js" async></script>
</div>

We can also see in the plot below that analyzes the gender breakdown of the people who died that the majority of those who perished were men - over four times as many as those who survived.  On the other hand, the women who survived outnumbered those who perished by nearly 3:1.

<div>
    <a href="https://plot.ly/~jesster413/791/?share_key=HIZYGI52HOgQjqEKizOPPF" target="_blank" title="plot from API (21)" style="display: block; text-align: center;"><img src="https://plot.ly/~jesster413/791.png?share_key=HIZYGI52HOgQjqEKizOPPF" alt="plot from API (21)" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="jesster413:791" sharekey-plotly="HIZYGI52HOgQjqEKizOPPF" src="https://plot.ly/embed.js" async></script>
</div>

I was also interested in analyzing whether a person's wealth played a role in their chances of survival, so I plotted a graph that analyzed the price of a passenger's ticket as well as their class and cabin.  The size of the bubble corresponds to the price of the ticket (the bigger the bubble, the more expensive the ticket.)  From this plot, it looks like the passengers who were located in Passenger Class 1, Cabin B and held an expensive ticket were more likely to survive.


<div>
    <a href="https://plot.ly/~jesster413/554/?share_key=uPkCoBfkZF2cpBiDxKE2rj" target="_blank" title="plot from API (12)" style="display: block; text-align: center;"><img src="https://plot.ly/~jesster413/554.png?share_key=uPkCoBfkZF2cpBiDxKE2rj" alt="plot from API (12)" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="jesster413:554" sharekey-plotly="uPkCoBfkZF2cpBiDxKE2rj" src="https://plot.ly/embed.js" async></script>
</div>


## "You don't know what hand you're gonna get dealt next"


Here's where I engineered my features, whether it was dropping columns entirely or creating new columns.  For this dataset, I decided to drop the PassengerId, Name, and Ticket columns since their values do not contribute to predicting whether someone survived or not.  Whether someone was male or female, however, did so I encoded those columns: 0 for male and 1 for female.  I also dummified the Embarked and Cabin_category columns to determine whether those features played a role in survivability.  Finally, I added columns like 'IsReverend' which encoded whether someone was a Reverend or not and 'FamilyCount' which combined the values of 'SibSp' and 'Parch' to give further detail to the model.  

In order to get the most of the dataset, I polynomialized all of my features after enigneering them.  Every feature was multiplied by every other feature in order to build out and identify the features that are most highly correlated to each other.  231 features were built out.


After polynomializing, I scaled my data using StandardScaler.



<iframe src="https://giphy.com/embed/Y4aRyFaavT9ss" width="460" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/Y4aRyFaavT9ss">via GIPHY</a></p>


## "It is a mathematical certainty."


Because this is a binary prediction, I chose to build two types of models: one using Logistic Regression and the other using k-Nearest Neighbor Classifier.  Within my LogReg model I tried using both Ridge and Lasso penalties each with an alpha of 1 and 10.  For my kNN models, I tried k=3, k=5, k=15, and k=25.  All seemed to perform pretty well, but which one should I choose to make my predictions?

To more easily compare my R^2 scores, I generated a table to catalogue my training and test scores across all of my models.  As expected, my training scores were higher than my test scores given that I fit my models to my training data.  It looks like the models all fared pretty well, however, it looks like kNN=3 is overfit given the discrepancy in the training v. test data.  On the other hand, kNN=25 looks like a great model given how close the training and test scores are.  This means that if more data were to become available in other test dataset, the kNN=25 model would account for about 80% variance of the variance from the test dataset.

| Model  | Penalty | Alpha | Train Score | Test Score |
|--------|---------|-------|-------------|------------|
| LogReg | Lasso   | 1     | 0.863       | 0.816      |
| LogReg | Lasso   | 10    | 0.759       | 0.757      |
| LogReg | Ridge   | 1     | 0.740       | 0.730      |
| LogReg | Ridge   | 10    | 0.762       | 0.762      |
| kNN    |         | 3     | 0.873       | 0.757      |
| kNN    |         | 5     | 0.855       | 0.771      |
| kNN    |         | 15    | 0.845       | 0.798      |
| kNN    |         | 25    | 0.803       | 0.802      |


In the end, though, while kNN=25 did perform well, I'm going to go with the LogReg model with penalty Lasso and alpha = 1 given that it performed the best overall.

##  "A woman's heart is a deep ocean of secrets."


You can see all of the code associated with this project in the corresponding [GitHub repo](https://github.com/thedatasleuth/Titanic-Survival-Predictions).
