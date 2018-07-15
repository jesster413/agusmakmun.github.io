---
layout: project_single
title:  "SEC Filings"
slug: "SEC-Filings"
---

The FBI and the American Association of Certified Fraud Examiners estimate that white collar crime costs 300 to 600 billion dollars a year.  That’s the equivalent of Warren Buffet’s net worth 3.5 times over, or buying out the debt of New York City's MTA 9 times over, or purchasing the New York Knicks 90 times over.  It’s incredible amount of money and also a very wide range, suggesting that enforcement agencies don't actually know the extent of the damage cause by white collar crime.

And that damage is not just limited to the crime itself - the ripple effects of financial crime are even wider, more opaque, but undoubtedly affect every single one of us.  Companies that are defrauded will raise prices on consumer goods to recoup losses.  Scammed insurance companies will demand higher premiums on their policies. Hedge fund schemes that manipulate equity markets can decrease stock prices. And if the government gets taken for a ride, well, we all know what happens then - more government taxes.

Identifying these bad actors is difficult.  Complex financial schemes usually involve a lot of documents, some of which demand law or accounting degrees to truly understand.  Moreover, as with many government agencies, there's just not enough people to review all of those complex documents.  And the select few who are tasked with reviewing these complex cases are doing it with outdated and insufficient technology.

To that end, I wanted to use data science techniques like web scraping, exploratory data analysis, and machine learning to identify ‘bad actor’ corporations to help financial regulators like the SEC focus their investigations of white collar crime.

Talk more here about SEC filings in general and who is required to submit filings and describe the kind of massive scale of data available.  Maybe link to a description of all of the different filings.

Every corporation listed on a U.S. stock exchange is required to file annual reports with the SEC. These reports—called “10-K forms”—are intended to help investors and regulators better understand the risks, assets, and current leadership of a company in order to make informed investment decisions. One of the requirements of a 10-K filing is that it must include a section that lists any other companies that are owned by the corporation making the filing. This “Exhibit 21” provides a rough snapshot of the ownership hierarchy of the subsidiary “child” companies along with the location or jurisdiction of incorporation of each subsidiary. Thanks to the efforts of various government transparency advocates, the SEC makes all of this filing information available on the web for free.[^1](http://api.corpwatch.org/documentation/faq.html)

## Identifying the target

My first task was to identify my targets so I could train my model on what suspicious cases might look like.  I researched SEC indictments reports and found that the SEC releases annual reports of their enforcement cases.  [For example](https://www.sec.gov/files/2017-03/secstats2016.pdf), in these reports I found, there is a list of indictments with information about the defendant (either an individual or a corporation) as well as date information and related charges.  I located reports for the years 2006-2016.  Given that these reports were in pdf format, my first task was to find a way to extract the information I needed into a format that I could eventually feed into my model.  I first tried using a package I found that reads in material from pdfs, but because of the inherent unstable process of extracting text, the formula I wrote to extract the text from the first report did not extract text in the same way as the next.  Because I only had a couple of weeks to complete this project and needed to extract text from eleven reports just to identify my targets, I quickly abandoned this method.  I later found a free website that more accurately (and quickly) converted my reports to excel files.  In data science as in many disciplines, sometimes the simplest tool is the best.

With my target dataset in a format I could read into python, I was then able to concatenate my data sets on top of each other and conduct some analysis.

The next big hurdle to overcome was the fact that names of individuals and corporations are not easily fed into searches.  In a future iteration of this project, I would try to use some of kind fuzzing matching algorithm like the [fuzzy wuzzy](https://github.com/seatgeek/fuzzywuzzy) package to feed the names of the defendants into a search box.  

For now, I manually entered the names of the cases into the EDGAR search field to identify their corresponding CIK ID Number.  Because I didn't want to manually search 5,000 entries, I pared my initial list of defendants down to corporations.  Many of the names of the corporate defendants did not turn up a CIK number.  (For some cases, this makes sense given that these corporations were accused of financial crime and may never have filed the required reports).  From the 1600 or so corporate cases I had, I was able to identify 175 corporates with CIK ID numbers.  I finally had my target set.

## Putting the case together

But what about my not_target set?  If I was going to make predictions, I needed to build out my dataset with not_target values.  Initially, I thought about doing the same process over again with just a random list of companies from the S&P500, but after more research, I found a [similar project](http://api.corpwatch.org/) that had already done some of the work that I wanted to.  Given my time constraints, I decided to download their datasets, which also identifies corporations by CIK numbers.  These datasets contained the types of data points I was looking for, namely, locations, years active, and identification numbers.  I suspected that the 175 target corporations I had previously identified were somewhere in the 65,000 rows of corporates, I subset my target data set out of the much larger not_target dataset.  When I compared the shapes of the prior not_target to the new not_target, sure enough, 175 rows were dropped.  I created a new column within my new not_target dataset called 'Indicted' and set it to '0' - my not_target column.  Then, I concatenated my target dataset on the 0 axis with its corresponding 'Indicted' column and set it to 1, meaning, positive for having been indicted.  

Need to define CIK ID

With CIK ID numbers for both of my target and not_target, I was able to scrape the SEC website for indices of filings for all of my corporates (target and not_target included) using a nested for loop with the requests and Beautiful soup libraries.  Having saved all of my filings in a list for each of my 65,000 rows, I then transformed those individual filings into their own unique dummy columns and filled each column with the value counts of the corporate.  With this final transformation, I had a working dataset with target and not_target values that I could analyze and subsequently feed into a model.

## Witness Selection

Using Select KBest, I identified features that were most correlated to my minority class, including temporal data such as 'max_year', 'min_year', 'most_recent', and 'Years_Active'

This is for mean number of active years.  So what this is saying is that the average amount of years active for the Indicted class (175 cases) is just under half the average number of years for the Not_Indicted class (65,324).  And this makes sense given that many of the indicted corporates were not even active for a full year, as visualized in the plot below.  

![Years_Active.png](/static/img/Years_Active.png)

In addition to the number of years a corporation has been active, Select KBest also identified several filings that were highly predictive of the minority class:

'2-A' (Report of Sales and Uses of Proceeds, is a requirement under Rule 257 of Regulation A of the Securities Exchange Act of 1933[^2]

'ADV-H-T' (Application for Temporary Hardship)[^3]

'DEFR14C' (Definitive revised information statement materials[^4])

SEC Form DEFR14C is a revision to form DEFA14C, a filing with the Securities and Exchange Commission (SEC) that discloses important information but is not connected to the solicitation of proxy votes. The information contained in a DEFA14C (or DEFR14C) form can cover a multitude of items related to corporate actions that must be disclosed to shareholders. However, these matters typically do not reach a level of significance that would necessitate a shareholder vote for approval. (https://www.investopedia.com/terms/s/sec-form-defr14c.asp)

'SC 14F1/A' (Statement regarding change in majority of directors
pursuant to Rule 14f-1)[^6])

The Indicted class filed these types of filings more frequently than did the Not Indicted class, as illustrated in the plot below.

![filings-updated.png](/static/img/filings-updated.png)

## Balancing the scales

My baseline accuracy for this project was 99.7%.  This means that if you guessed that a corporation was not indicted (0 in the target column), you would be right 99.7% of the time.  With my majority and minority classes so imbalanced, it is difficult for the machine to pick up on the (very subtle) signals that the minority class is sending out.  To counter-act this imbalance, I used several different class balancing techniques to amplify the minority class's signals.

First, I used a resampling technique called downsampling from scikit-learn's utils package.  This technique allowed me to reduce the majority population to whatever size I wanted.  I chose to downsample my majority class from 65,324 down to 10,000.

Talk here about using SMOTEENN and describing what it does - maybe confirm shapes?

Then, I took that downsampled dataset and did two train-test-splits on it - the first set would be used to initially train my models on my X_train and y_train.  Within this first set, I would conduct another train-test-split on my X_train and y_train which would be split into my cross-validation set (X_train_val, y_train_val, X_test_val, y_test_val).  Cross validating my models on a dataset that the model hasn't been trained on guards against overfitting my models to any one particular dataset.  If and when I was satisfied with the results of my models and satisfied with how my parameters were tuned, I would then predict on the X_test from the initial train-test-split - another dataset that the models have not yet seen.

Before running the models, however, I wanted to implement SMOTEENN (which I have previously used in another [imbalanced classes project](https://jesster413.github.io/project/West-Nile-Virus/) to synthetically create new data points that mimic the characteristics of my minority class (the Indicted cases).

I ran four models: Random Forest, a Decision Tree, XGBoost, and a Balanced Bagging Classifier.  I chose these models based on their past performance with some of my classification problems as well as their computation speed.  Maybe talk here about how you explained them in depth in the WNV project or should I re-explain here?

## Reaching a verdict

When running my models, I decided to score on precision because I was most concerned with reducing false positives while also trying to increase the number of true positives as much as possible.  The equation for precision, seen below, calculates the positive predictive value but penalizes the score by how many positive values were predicted incorrectly.  In my project, a false positive would be a corporate that was predicted to be indicted but should not have been.

$$Precision = \frac{True Positive}{True Positive + False Positive}$$

In a slightly different metric, recall measures the true positive rate by penalizing the score with false negatives (seen below).  In this project, a false negative would be a corporate that should have been indicted but wasn't.  

$$Recall = \frac{True Positive}{True Positive + False Negative}$$

In sum, both precision and recall are measures of truthiness but with slightly different perspectives.  Because it is a much more serious thing to indict a corporation that is innocent (both in terms of potential monetary penalties when the corporation turns around and sues the government as well as lost faith in the justice system) than to not indict a potentially guilty corporation, I decided that precision was the most important measure of success.

To visualize this decision, I plotted two confusion matrices of the models I ran to represent the cross-validation run (left) and the test run (right).  Despite the name, a confusion matrix is actually quite a simple tool to interpret.  Simply, it is a matrix of four values: true positives (values that were predicted positive and supposed to be positive), false positives (values that were predicted positive but supposed to be negative), true negatives (values that were predicted negative and supposed to be negative), and false negatives (values that were predicted negative but supposed to be positive) to compare the number of false positives, true positives, and false negatives.  In the cross-validation run, both the Decision Tree and XGBoost Classifiers kept the false positives to 0 while predicting 14 true positives.  They both left 19 false negatives on the table (corporations that should have been indicted but weren't).  The Random Forest and Balanced Bagging Classifiers both predicted more true positives (18 and 21, respectively) but they also predicted more false positives (5 and 17, respectively).  In my mind, the benefit of indicting 4 to 7 more corporations does not outweigh the cost of inappropriately accusing 5 to 17 corporations (and the attendant damage caused to the company for being embroiled in litigation).

![confusion-matrix.png](/static/img/confusion-matrix.png)

To focus in on the Indicted class even more, I also plotted the classification reports of each the models.  Similar to a confusion matrix, a classification report is another tool that can be used to evaluate model performance other than accuracy.  Some of the outputs of a classification report are: precision, recall, and an f1-score. Because the Decision Tree and XGBoost Classifiers both reduced false positives to 0, they have perfect precision scores of 1.0.  However, because they both left corporations "on the table", their recall scores are not as great.  Because the goal of this project was to identify as many corporations as possible to further investigate without investigating a corporation that should not be, I considered these metrics a success.

![classification-report](/static/img/classification-report.png)

Computation time

XGBoost: 16.1 minutes

<!-- https://poseidon01.ssrn.com/delivery.php?ID=750066083101070100069066092117102076007056010023061049023085002108095109006125012111041119107107108043037092089121125101115095060086008008061127028087114087030075090084003017092068013080089027004080117121069009025029066068117074005098027003099005067&EXT=pdf -->

[^1]: [https://www.investopedia.com/terms/s/sec-form-1-a.asp](https://www.investopedia.com/terms/s/sec-form-1-a.asp)
[^2]:[https://www.investopedia.com/terms/s/sec-form-2-a.asp](https://www.investopedia.com/terms/s/sec-form-2-a.asp)
[^3]:[https://www.iard.com/support_hardship](https://www.iard.com/support_hardship)
[^4]: [https://en.wikipedia.org/wiki/SEC_filing](https://en.wikipedia.org/wiki/SEC_filing)
[^5]: [https://en.wikipedia.org/wiki/SEC_filing](https://en.wikipedia.org/wiki/SEC_filing)
[^6]: [https://www.sec.gov/info/edgar/forms/edgform.pdf](https://www.sec.gov/info/edgar/forms/edgform.pdf)
