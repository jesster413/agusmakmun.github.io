---
layout: post
title: What's for Dinner Tonight?
categories: Recommender-Systems
description: A walk-through of how to build a simple recommender system
excerpt_separator: <!--more-->
published: False
---
*A walk-through of how to build a simple recommender system using MovieLens.*

<!--more-->

Growing up, my mother and I used to have a rule where if she suggested a meal for dinner, I could not wholesale dismiss it.  This meant that I either had to accept that green beans were going to be part of my nightly routine, with their skin rubbing against my teeth like nails on a chalkboard, or, I had to come up with a similarly healthy alternative.  Had I known then what I know today, I would built us a recommender system for dinner so I never had to eat green beans again.

A recommendation system does exactly what it sounds like it does: it recommends products/media/people to users based on a mathematical algorithm, and those recommendations are rated in descending order of what the user might like.

Brief history of rec systems - who uses them.

1/4 to a 1/3 of consumer choices at Amazon are driven by personalized recommendations
Netflix says there recommendation engine reduces churn saving the company in excess of $1 billion a year
Hulu has shown that showing recommended TV shows results in over 3x more clicks than only showing the most popular TV shows.

1.1.3  Explicit data vs Implicit data¶
1.1.3.1  Explicit
Explicity given/pro-actively acquired
Clear signals
Cost associated with acquisition (time/cognitive)
Limited and sparse data because of this
1.1.3.2  Implicit
Provided/collected passively (digital exhaust)
Signals can be difficult to interpret
Enormous quantities

.1.4  Two classical recommendation methods¶
Collaborative Filtering: (similar people)
If you like the same 5 movies as someone else, you'll likely enjoy other movies they like.
There are two main types: (a) Find users who are similar and recommend what they like (user-based), or (b) recommend items that are similar to already-liked items (item-based).
Content-Based Filtering (similar items)
If you enjoy certain characteristics of movies (e.g. certain actors, genre, etc.), you'll enjoy other movies with those characteristics.
Note this can easily be done using machine learning methods! Each movie can be decomposed into features. Then, for each user we compute a model -- the target can be a binary classifier (e.g. "LIKE"/"DISLIKE") or regression (e.g. star rating).

1.2  User-based Collaborative Filtering
We'll first look at user-based filtering. The idea behind this method is finding your taste doppelgänger. This is the person who is most similar to you based upon the ratings both of you have given to a mix of products.

If we want to find the users most similar to user A, we need a similarity metric.

One metric we can use is cosine similarity. Cosine similarity uses the cosine between two vectors to compute a scalar value that represents how closely related these vectors are.

Doesn't this sound a lot like the correlation coefficient? It turns out that cosine similarity is identical to the uncentered correlation coefficient! As a bonus, if the points are mean-centered, then this formula also depicts the Pearson correlation coefficient.

1.5  Cosine similarity using sci-kit learn¶
With that, let's calculate the cosine similarity of A against all other users. We'll start with B. We have a sparse matrix with lot's of missing values... what should we do?









We have looked at the major types of recommender systems in this lesson. Let's quickly wrap up by looking at the pros and cons of each.

1.10.0.1  Collaborative Filtering
Pros:

No need to hand craft features
Cons:

Needs a large existing set of ratings (cold-start problem)
Sparsity occurs when the number of items far exceeds what a person could purchase
1.10.0.2  Content-based Filtering
Pros:

No need for a large number of users
Cons:

Lacks serendipity
May be difficult to generate the right features
Hard to create cross-content recommendations (different feature spaces)
In fact, the best solution -- and the one most likely in use in any large-scale, production system is a combination of both of these. This is known as a hybrid system. By combining the two systems, you can get the best of both worlds.
