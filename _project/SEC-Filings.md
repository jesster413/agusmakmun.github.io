---
layout: project_single
title:  "SEC Filings"
slug: "SEC-Filings"
---
I talked a little bit about my background before I started General Assembly's.  I worked in law enforcement and later on a large-scale federal monitorship mandated by the Eastern District of New York.  Having completed a few projects, I felt ready to apply my new programming skills to a topic that really interested me.

For my capstone, I wanted to see if I could identify a way to predict SEC indictments based on information pulled from the SEC website.  I wanted to do this project to see if there was a way I could help law enforcement professionals identify potential cases of financial crime.  

Talk more here about SEC filings in general and who is required to submit filings and describe the kind of massive scale of data available.  Maybe link to a description of all of the different filings.

My first task was to identify my targets so I could train my model on what suspicious cases might look like.  I researched SEC indictments reports and found that the SEC releases annual reports of their enforcement cases.  [For example](https://www.sec.gov/files/2017-03/secstats2016.pdf), in these reports I found, there is a list of indictments with information about the defendant (either an individual or a corporation) as well as date information and related charges.  I located reports for the years 2006-2016.  Given that these reports were in pdf format, my first task was to find a way to extract the information I needed into a format that I could eventually feed into my model.  I first tried using a package I found that reads in material from pdfs, but because of the inherent unstable process of extracting text, the formula I wrote to extract the text from the first report did not extract text in the same way as the next.  Because I only had a couple of weeks to complete this project and needed to extract text from eleven reports just to identify my targets, I quickly abandoned this method.  I later found a free website that more accurately (and quickly) converted my reports to excel files.  In data science as in many disciplines, sometimes the simplest tool is the best. 
