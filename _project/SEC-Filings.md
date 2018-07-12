---
layout: project_single
title:  "SEC Filings"
slug: "SEC-Filings"
---
I talked a little bit about my background before I started General Assembly's.  I worked in law enforcement and later on a large-scale federal monitorship mandated by the Eastern District of New York.  Having completed a few projects, I felt ready to apply my new programming skills to a topic that really interested me.

For my capstone, I wanted to see if I could identify a way to predict SEC indictments based on information pulled from the SEC website.  I wanted to do this project to see if there was a way I could help law enforcement professionals identify potential cases of financial crime.  

Talk more here about SEC filings in general and who is required to submit filings and describe the kind of massive scale of data available.  Maybe link to a description of all of the different filings.

Every corporation listed on a U.S. stock exchange is required to file annual reports with the SEC. These reports—called “10-K forms”—are intended to help investors and regulators better understand the risks, assets, and current leadership of a company in order to make informed investment decisions. One of the requirements of a 10-K filing is that it must include a section that lists any other companies that are owned by the corporation making the filing. This “Exhibit 21” provides a rough snapshot of the ownership hierarchy of the subsidiary “child” companies along with the location or jurisdiction of incorporation of each subsidiary. Thanks to the efforts of various government transparency advocates, the SEC makes all of this filing information available on the web for free.[^1](http://api.corpwatch.org/documentation/faq.html)

## Identifying the target

My first task was to identify my targets so I could train my model on what suspicious cases might look like.  I researched SEC indictments reports and found that the SEC releases annual reports of their enforcement cases.  [For example](https://www.sec.gov/files/2017-03/secstats2016.pdf), in these reports I found, there is a list of indictments with information about the defendant (either an individual or a corporation) as well as date information and related charges.  I located reports for the years 2006-2016.  Given that these reports were in pdf format, my first task was to find a way to extract the information I needed into a format that I could eventually feed into my model.  I first tried using a package I found that reads in material from pdfs, but because of the inherent unstable process of extracting text, the formula I wrote to extract the text from the first report did not extract text in the same way as the next.  Because I only had a couple of weeks to complete this project and needed to extract text from eleven reports just to identify my targets, I quickly abandoned this method.  I later found a free website that more accurately (and quickly) converted my reports to excel files.  In data science as in many disciplines, sometimes the simplest tool is the best.

With my target dataset in a format I could read into python, I was then able to concatenate my data sets on top of each other and conduct some analysis.

The next big hurdle to overcome was the fact that names of individuals and corporations are not easily fed into searches.  In a future iteration of this project, I would try to use some of kind fuzzing matching algorithm like the [fuzzy wuzzy](https://github.com/seatgeek/fuzzywuzzy) package to feed the names of the defendants into a search box.  

For now, I manually entered the names of the cases into the EDGAR search field to identify their corresponding CIK ID Number.  Because I didn't want to manually search 5,000 entries, I pared my initial list of defendants down to corporations.  Many of the names of the corporate defendants did not turn up a CIK number.  (For some cases, this makes sense given that these corporations were accused of financial crime and may never have filed the required reports).  From the 1600 or so corporate cases I had, I was able to identify 175 corporates with CIK ID numbers.  I finally had my target set.

## Putting the case together

But what about my not_target set?  If I was going to make predictions, I needed to build out my dataset with not_target values.  Initially, I thought about doing the same process over again with just a random list of companies from the S&P500, but after more research, I found a [similar project](http://api.corpwatch.org/) that had already done some of the work that I wanted to.  Given my time constraints, I decided to download their datasets, which also identifies corporations by CIK numbers.  These datasets contained the types of data points I was looking for, namely, locations, years active, and identification numbers.  I suspected that the 175 target corporations I had previously identified were somewhere in the 65,000 rows of corporates, I subset my target data set out of the much larger not_target dataset.  When I compared the shapes of the prior not_target to the new not_target, sure enough, 175 rows were dropped.  I created a new column within my new not_target dataset called 'Indicted' and set it to '0' - my not_target column.  Then, I concatenated my target dataset on the 0 axis with its corresponding 'Indicted' column and set it to 1, meaning, positive for having been indicted.  

With CIK ID numbers for both of my target and not_target, I was able to scrape the SEC website for indices of filings for all of my corporates (target and not_target included) using a nested for loop with the requests and Beautiful soup libraries.  Having saved all of my filings in a list for each of my 65,000 rows, I then transformed those individual filings into their own unique dummy columns and filled each column with the value counts of the corporate.  With this final transformation, I had a working dataset with target and not_target values that I could analyze and subsequently feed into a model.


Indicted Stats: count    175.000000
mean       3.320000

Not_Indicted Stats: count    65324.000000
mean         6.738473

For Indicted: the maximum amount of 10-Qs filed was 14, less than half the amount of 10-Qs filed for the Not_Indicted (30)

## Balancing the scales

Because I had such dramatically imbalanced classes, I knew I needed to implement sampling techniques.  First, I downsampled my majority class from 65,000 to 5,000 to help my models be more sensitive to my target class.  

Then, I took that downsampled dataset and did two train-test-splits on it - the first set would be used to initially train my models on my X_train and y_train.  Within this first set, I would conduct another train-test-split on my X_train and y_train which would be split into my cross-validation set (X_train_val, y_train_val, X_test_val, y_test_val).  Cross validating my models on a dataset that the model hasn't been trained on guards against overfitting my models to any one particular dataset.  If and when I was satisfied with the results of my models and satisfied with how my parameters were tuned, I would then predict on the X_test from the initial train-test-split - another dataset that the models have not yet seen.

Before running the models, however, I wanted to implement SMOTEENN (which I have previously used in another [imbalanced classes project](https://jesster413.github.io/project/West-Nile-Virus/) to synthetically create new data points that mimic the characteristics of my minority class (the Indicted cases).

I ran four models: Random Forest, a Decision Tree, XGBoost, and a Balanced Bagging Classifier.  I chose these models based on their past performance with some of my classification problems as well as their computation speed.  Maybe talk here about how you explained them in depth in the WNV project or should I re-explain here?

## Reaching a verdict

Talk here about cross-val results and the test results.  What was I most concerned with in this project (false positives - because accusing someone of financial crime is a serious thing and can be extremely costly to the SEC, both in terms of potential monetary penalties when the corporation turns around and sues the government as well as lost faith in the justice system.)

Talk about scoring on precision.

precision is the same thing as recall - it is a measure of truthiness.

Confusion Matrix for Random Forest Cross-Val
True Negatives: 935

False Positives: 3

False Negatives: 11

True Positives: 22

Confusion Matrix for XGBoost Cross-Val
True Negatives: 925

False Positives: 13

False Negatives: 15

True Positives: 18

Confusion Matrix for Balanced Bagging Classifer Cross-Val
True Negatives: 927

False Positives: 11

False Negatives: 12

True Positives: 21

Confusion Matrix for Decision Tree Classifer Cross-Val
True Negatives: 919

False Positives: 19

False Negatives: 11

True Positives: 22

precision    recall  f1-score   support

      0       0.99      1.00      1.00      2500
      1       1.00      0.52      0.69        44

avg / total       0.99      0.99      0.99      2544

Feature Selection features: '2-A', 'ADV-H-T', 'DEFR14C', 'DOS', 'SC 14F1/A', 'STOP ORDER',


## Going to Press



https://poseidon01.ssrn.com/delivery.php?ID=750066083101070100069066092117102076007056010023061049023085002108095109006125012111041119107107108043037092089121125101115095060086008008061127028087114087030075090084003017092068013080089027004080117121069009025029066068117074005098027003099005067&EXT=pdf
