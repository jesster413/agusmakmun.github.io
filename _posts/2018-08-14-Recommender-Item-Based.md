---
layout: post
title: What's for Dinner Tonight? Part 1 (Item-Based Collaborative Filtering Recommender System)
author: Jess Chace
cover_image: rec-image.png
cover_alt: machine learning recommender system item-based collaborative filtering
categories: Recommender-Systems
description: A walk-through of an item-based collaborative filtering recommender system using the MovieLens database
tags:
- Recommender Systems
series:
excerpt_separator: <!--more-->
published: False
---
*A walk-through of an item-based collaborative filtering recommender system using the MovieLens database.*

<!--more-->

Growing up, my mother and I used to have a rule where if she suggested a meal for dinner, I could not wholesale dismiss it.  This meant that I either had to accept that green beans were going to be part of my nightly routine, with their skin rubbing against my teeth like nails on a chalkboard, or, I had to come up with a similarly healthy alternative.  Had I known then what I know today, I would have built us a recommender system for dinner so I never had to eat green beans again.

Recommender systems rose in popularity in the 1990s with Amazon being one of the more prominent early adopters.  Today, recommender systems can be seen everywhere - from the movies we watch, to the music we listen to, to the articles we read.  Even people can be recommended as "items" in dating apps like Tinder.  Recommender systems have become so profitable to companies in terms of increasing user activity, maximizing revenue, and reducing customer churn that [Netflix famously offered a million dollars](https://www.netflixprize.com/) to the person/team who could improve the accuracy of their recommendation predictions by 10%.  The winning algorithm was [so complicated](https://www.wired.com/2012/04/netflix-prize-costs/) that the engineering costs to implement it outweighed the benefits.

While the algorithms underpinning the recommendation engines have advanced significantly, the concepts behind them remain relatively simple.  There are two main ways to recommend an item: Collaborative Filtering and Content-based filtering.  Within collaborative-based filtering, items can either be recommended based on identifying users with similar tastes (user-based) or based on items that are rated or ranked similarly (item-based).

In user-based collaborative filtering, recommendations are made to consumers based on their likeness to other consumers.  In other words, the model looks for your taste doppelganger and makes recommendations to you based on what your doppelganger has liked in the past.  In item-based filtering, recommendations are made based on similarity between items using explicit information captured from users.  For example, a movie is recommended to a customer based on it's similar rating to another movie that the customer has watched.

In a future post, I will explore content-based filtering, which is a method of identifying similar items based on the item's features.  For example, Taylor Swift's songs can often be categorized as Pop with lyrics relating to failed relationships.  A similarly-situated artist might be Justin Bieber given how many times he's been rebuffed by Selena Gomez.

### Opening Credits

To begin, I imported two datasets from MovieLens, the movies themselves and the corresponding ratings.  I merged the two dataframes together on the overlapping 'movieId' column, similar to a join in SQL with the following code snippet:

```python
ratings = pd.merge(ratings, movies, how='inner', on='movieId')
```

This created a 'ratings' dataframe with over 100,000 rows of data.  In sum, there were 671 unique userIds and 9,066 unique movies, meaning that many users rated multiple movies.  At the low end of the spectrum, userId 485 rated 20 movies.  At the other end of the spectrum, userId 547 rated 2,391 movies.  If we assume that the average length of a movie is 130 minutes, userId 547 has spent over 5,000 hours watching films.

The most frequently rated film was Forrest Gump with 341 total ratings and an average rating of 4.05.  Here's a plot of the top ten most rated movies in the database:

![most-rated-movies.png](/static/img/most-rated-movies.png)

Overall, the users were generous with their ratings, giving an average rating of 3.54 out of 5.  Of all the movies, however, The Shawshank Redemption received the most 5.0 ratings while users felt the most ambivalent about the 1989 making of Batman, which received the most 3.0 ratings.

![ratings-comparision.png](/static/img/ratings-comparision.png)

### The Exposition

I then transformed the ratings dataframe into a pivot table, designating the userId as the index, the titles of the movies as the columns, and the ratings of the movies as the cell values with the following code snippet:

```python
pivot = pd.pivot_table(ratings, index='userId', columns='title', values='rating')
```

Because not every userId watched every movie, there are many NaN values in this pivot table.  In order to get the data into a format more conducive to analysis, I filled the missing values with zeros, transposed it so that the titles of the movies are now the index column, and converted the dataframe into a compressed sparse row matrix.  This transformation tracks only the userId based ratings for each movie and discards the superfluous NaN values.

```python
sparse_pivot = sparse.csr_matrix(pivot.T.fillna(0))
```

<br>

### The Rising Action

Having pivoted the data, I was then able to calculate a scalar value which represents how similar (or dissimilar) two vectors are using a Euclidean distance formula.  As seen below in the diagram:

Two vectors that are headed in exact same direction with cos(0&deg;) = 1 are the exact same.  

Vectors with a cos(90&deg;) = 0 (a right angle) are dissimilar.  

Finally, vectors that are headed in opposite directions (a degree of 180&deg;) where cos(90&deg;)= − 1 are opposite of each other.  

![vectors.png](/static/img/vectors.png)[^2]

I calculated this scalar value, also known as a similarity metric, between two vectors (movies) using two distinct (but similar) functions within scikit-learn: cosine similarity and pairwise distances.

#### Cosine Similarity

Cosine similarity uses the cosine between two vectors to compute a scalar value, derived from the Euclidean dot formula seen below.

$$
cos(\theta) = \frac{\vec{A} \cdot \vec{B}}{\left\| \vec{A}\right\| \left\| \vec{B}\right\| } \
= \frac{\sum{A_i B_i}}{\sqrt{\sum{A_i^2}}\sqrt{\sum{B_i^2}}}
$$

When calculating the similarity between items, the cosine ranges from -1 (the exact opposite of the selected item) to 1 (the same as the selected item).  A value of 0 indicates that the two items are neither similar nor dissimilar - they are just decorrelated.  Values between -1 and 0 indicate dissimilarity while values between 0 and 1 indicate similarity.  

The equation for cosine similarity is the same as the uncentered correlation coefficient.  However, the data can be centered (and therefore further normalized) by subtracting the center mean of the rows from the data points within the row using the following function:

```python
def mean_center_rows(df):
    return (df.T - df.mean(axis=1)).T
```

With a centered mean, the equation is identical to the Pearson correlation coefficient, meaning that the cosine similarity will range from 0 to 1 instead of -1 to 1.  I incorporated the `mean_center_rows` function when applying scikit-learn's cosine_similarity algorithm to create a similarity matrix from the pivot table:

```python
sim_matrix = cosine_similarity(mean_center_rows(pivot.T).fillna(0))
```

<br>

#### Pairwise Distances

Another way to calculate similarity between two vectors is with scikit-learn's pairwise distances.  Similar to the cosine similarity function, pairwise distances uses cosine to calculate the scalar value by setting the metric to `cosine` as seen below.

```python
distances = pairwise_distances(sparse_pivot, metric='cosine')
```

I then transformed that into a dataframe where the index column matched the columns running across the dataframe with the following code snippet:

```python
distances_df = pd.DataFrame(distances, index=pivot.columns, columns=pivot.columns)
```

This looks a lot like a correlation matrix because it effectively is.  While titles that are the exact same have perfect cosine similarity of 1.0 because they are the same exact movie, values between -1 and 1 indicate dissimilarity or similarity, respectively.  

### The Climax

To illustrate how this recommender system works, let's return to Shawshank Redemption which had an average rating of 4.49 and 311 total ratings.  According to pairwise distances, the most similar movies were:

```
Shawshank Redemption, The (1994)
Average Rating 4.487138263665595
Count of ratings 311
Similar movies
title
Pulp Fiction (1994)                 0.326045
Silence of the Lambs, The (1991)    0.340622
Forrest Gump (1994)                 0.342756
Usual Suspects, The (1995)          0.401438
Schindler's List (1993)             0.407621
Braveheart (1995)                   0.432712
Seven (a.k.a. Se7en) (1995)         0.434131
Fugitive, The (1993)                0.449233
Dances with Wolves (1990)           0.450477
Saving Private Ryan (1998)          0.455761
```

According to the similarity matrix, the most similar movies to Shawshank Redemption are:

```
Shawshank Redemption, The (1994)
Average Rating 4.487138263665595
Count of ratings 311
Similar movies
title
Raven, The (1963)                                             -0.243137
American Ninja 2: The Confrontation (1987)                    -0.228703
Protector, The (a.k.a. Warrior King) (Tom yum goong) (2005)   -0.228703
District 13 (Banlieue 13) (2004)                              -0.225775
Jason and the Argonauts (1963)                                -0.221448
Last Starfighter, The (1984)                                  -0.219225
Land of the Dead (2005)                                       -0.218736
Remo Williams: The Adventure Begins (1985)                    -0.215152
Carnival of Souls (1962)                                      -0.214143
Quiet Earth, The (1985)                                       -0.200115
```

### Final Credits

While collaborative filtering is a good way to identify similar items, it does suffer from the cold start problem:  In order for items to be correlated, and therefore, recommended to users, items require input from users - in this case, a rating.  As items receive more data points from users, the overall recommendation accuracy of the engine will increase.  The problem starts all over again with every new addition to the existing database.  

The advantage to a collaborative filtering approach, however, is that it does not require feature engineering, which, if you've ever tried to recommend a movie to someone, you'll know is a bit of a guessing game.  Do they like the movie because of the actors?  Or because of the genre?  Or the setting?  Or because of the enigmatic way the director captured the true nature of the human experience?  This is content-based filtering, for better or worse, which seeks to recommend items to users based on features similar to the item selected by the user.  In a future post, I will explore this type of recommendation engine in further detail.

For a complete look at the underlying code for this post, head on over to the corresponding [GitHub repo](https://github.com/thedatasleuth/Recommender-System-Item-Based).

---

[^1]:[https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/metrics/pairwise.py](https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/metrics/pairwise.py)
[^2]: [https://www.safaribooksonline.com/library/view/mastering-machine-learning/9781785283451/ba8bef27-953e-42a4-8180-cea152af8118.xhtml](https://www.safaribooksonline.com/library/view/mastering-machine-learning/9781785283451/ba8bef27-953e-42a4-8180-cea152af8118.xhtml)
