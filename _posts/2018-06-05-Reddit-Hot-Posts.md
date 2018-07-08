---
layout: post
title: Reddit Hot Posts
published: True
image: titanic.png

---

Reddit claims to be the front page of the internet, and that's because they are. With an average of 542 million monthly visitors, of which 234 million are unique, Reddit is the third most visited site in the U.S. and ranked sixth in the world. Reddit is subdivided into subreddits, which are themed discussion boards created and populated by Reddit users with links, text, videos and images. These subreddits span an endless array of interests including world news, sports, economics, movies, music, fitness, and more. Reddit members discuss proposed topics in the comments section, and the most popular comments are "up-voted" to the top of the discussion board.  In 2015, Reddit users submitted nearly 75 million posts and followed up with nearly three quarters of a billion comments.  With so many users, submissions, comments, and up-votes, it can seem impossible to craft a post that will ever see the light of day.

For this project, I was tasked with analyzing a subset of Reddit's "Hot Posts" section to identify what, if any, features of a post determine its popularity.  Using data science techniques like exploratory data analysis, predictive modeling, and natural language processing allowed me to efficiently search through a sample of 5,000 posts faster than I ever could have if I had just read through the posts manually.

With an endless supply of things to read on the internet, it seems impossible to write a post that anyone else but your mom will read.  But with a few (hundred) keystrokes, even a platform as wild as Reddit can be neatly distilled into a handful of targeted insights.


## 2. Get 'em while they're hot

![heidi.jpg](/static/img/heidi.jpg)

This was my first project wherein I had to gather my own data (as opposed to downloading a dataset from Kaggle, for example.)  In order to build out my dataset, I scraped 5,000 posts from the "Hot Posts" section of Reddit's website using the 'requests' and 'Beautiful Soup' libraries.  Later on in the project, I used Reddit's popular API wrapper PRAW to grab some comments from my top 50 'hot' posts.  After a few hours (with three second delays in between each set of 25 posts pulled so as not to be black listed by Reddit's servers), my scrape was complete.

With any Reddit post, there are upwards of 85 identifying features, and while I did pull all 85 of those features for each of my 5,000 posts, I decided to limit the features that I built into my dataframe to those I thought would be most predictive of popularity.  I pared the 85 features down to 14 and stored them in variables.  The features I chose to use in my model were: 'author', 'title', 'subreddit', 'subreddit_subscribers', 'score', 'ups', 'downs', 'num_comments', 'num_crossposts', 'selftext', 'pinned', 'stickied', 'wls', 'created_utc.'  If a post had actual original text, it was stored in the 'selftext' feature, however, when I began to explore my dataset in more depth, I learned that many Reddit posts don't actually have original content, but rather, are links to a previous post.



## 3. Explore Data

With my dataframe finally built out, I was able to start exploring the posts I pulled.  

## 4. Clean Data

How I cleaned the data  


## 5. Engineer Features

How I engineered Features

## Popular, you're going to be pop-u-lar

![popular.gif](/static/img/popular.gif)


## 6. Select Features

Which features most greatly contributed


## 7. Fit Models

Linear Regression with Lasso, Ridge


## 8. Model Evaluation

how i evaluated the models

## 9. Answer Question
