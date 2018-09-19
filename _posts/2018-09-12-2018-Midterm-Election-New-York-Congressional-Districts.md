---
layout: post
title: Voter Registration by New York Congressional Districts for 2018 Midterm Elections
author: Jess Chace
cover_image: newyork.png
cover_alt: 2018 Midterm Elections for New York Congressional Districts
cover_description: Analysis of Voter Registration for the 27 New York Congressional Districts in anticipation of the 2018 Midterm Elections.  
categories: Python
description: Analysis of Voter Registration for the 27 New York Congressional Districts in anticipation of the 2018 Midterm Elections with an interactive Dash chart.
tags:
- Python
excerpt_separator: <!--more-->
published: True
---

*Analysis of Voter Registration for the 27 New York Congressional Districts in anticipation of the 2018 Midterm Elections with an interactive Dash chart.*

<!--more-->

In anticipation of the 2018 mid-term elections, Enigma Public is hosting a data science competition.  Using their new [SDK package](https://pypi.org/project/enigma-sdk/) which facilitates API calls on their public database, contestants are encouraged to explore topics that have some explanatory power over the midterm elections.    

Under their [Elections](https://public.enigma.com/browse/tag/elections/34) collection, there are six umbrella datasets: Congressional Candidate Disbursements, Federal Election Commission, New York State Board of Elections, PAC Summary, Presidential Campaign Disbursements, and Tafra.  While there's definitely something to dig into in each of these datasets, I chose to explore the New York State Board of Elections dataset filled with 5 years of voter registration statistics by Congressional District.  Below are some of my findings.

### Voter Registration across Political Parties from 2014-2018

The interactive chart below created with Dash displays voter registration numbers for all 27 districts of New York which can be filtered by year using the drop down menu.  Click on any combination of the boxes in the legend to compare voters across political parties.  Here's a few questions to get you started on your analysis:  Did the Democrats lose any districts to the Republicans?  If so, where and when?  What's the greatest party affiliation after the Democrats and Republicans?  Does that political affiliation represent opportunity or apathy?  Where does the Independent Party have the most registered voters?  What other trends can you see?

<iframe src="https://ny-districts-by-year.herokuapp.com/" width="620px" height="520px"></iframe>

### Percentage Change of Voter Registration

Sometimes it's not enough to just look at the numbers themselves, but to actually calculate how the numbers have changed from year to year.  Using the ``.pct_change() `` in Pandas, I was able to identify the percentage change of voter registration across the political parties.  This analysis produced some interesting results, especially as it relates to the fringe parties of New York, namely, the Women's Equality Party (WEP) and the Reform Party (REF).  

Founded in 2014, the Women's Equality Party (WEP) was created by Andrew Cuomo, the current governor of New York.  The party reached full political party status in 2014 during the gubernatorial elections after receiving the requisite 50,000 votes.  While Cuomo is no longer in control of the party on paper, the party remains deeply supportive of the incumbent governor, supporting his most recent re-election bid in 2018.

The Reform Party (REF), another fringe political party founded in 2014, is a ballot-access qualified party created by [Curtis Sliwa](https://www.nyreformparty.com/endorsements).  Silwa initially founded another group called the Guardian Angels in 1979 and is a regular contributor to several talk radio and television shows, including Fox 5.  

Click through the chart below to see how each district has responded to the introduction of these new parties.  Do you see any patterns emerging across districts that are represented by a Democrat as opposed to a Republican?

<iframe src="https://ny-congressional-districts.herokuapp.com/" width="620px" height="520px"></iframe>

District 1, located in Eastern Long Island, District 2, located on Southern Long Island, and Districts 23 and 27, both located in Western New York, all of which are represented by a Republican congressman, show a significant percentage change in voter registration for the WEP party in 2016.  Similarly, for Democrat-represented districts like District 3 located in Northern Long Island, District 7, located in Queens, District 18, located in Orange, Putnam, Southern Dutchess, and Northeastern Westchester, District 20, located in Albany and Schenectady, all saw a huge spike in WEP registration in 2016.    

For the REF party, there was some increase in Republican-represented districts like District 19 in the Hudson Valley and District 24 in Cayuga, Onondaga, Wayne counties in 2016 and District 2, located in Southern Long Island and District 23 located in Western New York in 2017.  The REF party increased its voter registration percentage significantly two years in a row in District 22 located in Central New York.

For Democrat-represented districts, the REF party also increased its voter share most significantly in District 18 in Orange, Putnam, Southern Dutchess, and Northeastern Westchester counties in 2016, and in District 3, Northern Long Island and District 4, Central and Southern Nassau County, and to a lesser extent District 5, Queens, and District 10, an amalgamation of the Upper West Side, the Financial District, Greenwich Village, and Borough Park in Brooklyn, District 16, Northern Bronx and Southern Westchester and District 20 Albany and Schenectady in 2017.
