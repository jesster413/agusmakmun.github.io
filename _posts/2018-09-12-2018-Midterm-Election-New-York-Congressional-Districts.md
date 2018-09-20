---
layout: post
title: Voter Registration by New York Congressional Districts for 2018 Midterm Elections
author: Jess Chace
cover_image: newyork.png
cover_alt: 2018 Midterm Elections for New York Congressional Districts
cover_description: Analysis of Voter Registration for the 27 New York Congressional Districts prior to 2018 Midterm Elections.  
categories: Python
description: Analysis of Voter Registration for the 27 New York Congressional Districts in anticipation of the 2018 Midterm Elections
tags:
- Python
excerpt_separator: <!--more-->
published: True
---

*Analysis of Voter Registration for the 27 New York Congressional Districts in anticipation of the 2018 Midterm Elections with interactive Dash charts.*

<!--more-->

In anticipation of the 2018 mid-term elections, Enigma Public is hosting a data science competition.  Using their new [SDK package](https://pypi.org/project/enigma-sdk/) which facilitates API calls on their public database, contestants are encouraged to explore topics that have some explanatory power over the midterm elections.  The competition ends on Friday, September 21, 2018.    

Under their [Elections](https://public.enigma.com/browse/tag/elections/34) collection, there are six umbrella datasets: Congressional Candidate Disbursements, Federal Election Commission, New York State Board of Elections, PAC Summary, Presidential Campaign Disbursements, and Tafra.  While there's definitely something to dig into in each of these datasets, I chose to explore the New York State Board of Elections dataset filled with 5 years of voter registration statistics by Congressional District.  I initially started off this analysis plotting in Matplotlib but halfway through decided to challenge myself and build out my plots using Dash.  I wanted to do this for several reasons: First, I've used [Plotly](https://plot.ly/#/) in a [previous project](https://thedatasleuth.github.io/predictive-modeling/2018/05/21/Predicting-Titanic-Survivability-Odds-Using-Machine-Learning.html) and was really impressed with the interactive hover-over feature and the crispness of the colors.  With [Plotly-Dash](https://plot.ly/products/dash/), I was able to take the user interaction to the next level by including drop-down menus to facilitate more focused analysis.  Second, I felt like building out interactive plots like the ones below encourage engagement with the data as opposed to just reading someone's findings.  I can't think of a better topic than election season to get people engaged with data and thinking about their right to vote.  Also, the kid in me really likes pushing the buttons.  

Have a click through the charts below and feel free to share your own findings in the comments section.

### Voter Registration across Political Parties from 2014-2018

The chart below displays voter registration numbers for all 27 districts of New York which can be filtered by year using the drop-down menu.  Click on any combination of the boxes in the legend to compare voters across political parties.  Here are a few questions to get you started on your analysis:  Did the Democrats lose any districts to the Republicans?  If so, where and when?  What's the greatest party affiliation after the Democrats and Republicans?  Does that political affiliation represent opportunity or apathy?  Where does the Independent Party have the most registered voters?  What other trends can you see?

<br>

<iframe src="https://ny-districts-by-year.herokuapp.com/" width="620px" height="520px"></iframe>

### Percentage Change of Voter Registration

Sometimes it's not enough to just look at the numbers themselves but to actually calculate how the numbers have changed from year to year.  Using the ``.pct_change() `` in Pandas, I was able to identify the percentage change of voter registration across the political parties.  This analysis produced some interesting results, especially as it relates to the fringe parties of New York, namely, the Women's Equality Party (WEP) and the Reform Party (REF).  

Founded in 2014, the Women's Equality Party (WEP) was created by Andrew Cuomo, the current governor of New York.  The party reached full political party status in 2014 during the gubernatorial elections after receiving the requisite 50,000 votes.  While Cuomo is no longer in control of the party on paper, the party remains deeply supportive of the incumbent governor, supporting his most recent re-election bid in 2018.

The Reform Party (REF), another fringe political party founded in 2014, is a ballot-access qualified party created by [Curtis Sliwa](https://www.nyreformparty.com/endorsements).  Silwa initially founded another group called the Guardian Angels in 1979 and is a regular contributor to several talk radio and television shows, including Fox 5.  

Click through the chart below to see how each district has responded to the introduction of these new parties.  Do you see any patterns emerging across districts that are represented by a Democrat as opposed to a Republican?

<br>

<iframe src="https://ny-congressional-districts.herokuapp.com/" width="620px" height="520px"></iframe>

<!-- District 1, located in Eastern Long Island, District 2, located on Southern Long Island, and Districts 23 and 27, both located in Western New York, all of which are represented by a Republican congressman, show a significant percentage change in voter registration for the WEP party in 2016.  Similarly, for Democrat-represented districts like District 3 located in Northern Long Island, District 7, located in Queens, District 18, located in Orange, Putnam, Southern Dutchess, and Northeastern Westchester, District 20, located in Albany and Schenectady, all saw a huge spike in WEP registration in 2016.    

For the REF party, there was some increase in Republican-represented districts like District 19 in the Hudson Valley and District 24 in Cayuga, Onondaga, Wayne counties in 2016 and District 2, located in Southern Long Island and District 23 located in Western New York in 2017.  The REF party increased its voter registration percentage significantly two years in a row in District 22 located in Central New York.

For Democrat-represented districts, the REF party also increased its voter share most significantly in District 18 in Orange, Putnam, Southern Dutchess, and Northeastern Westchester counties in 2016, and in District 3, Northern Long Island and District 4, Central and Southern Nassau County, and to a lesser extent District 5, Queens, and District 10, an amalgamation of the Upper West Side, the Financial District, Greenwich Village, and Borough Park in Brooklyn, District 16, Northern Bronx and Southern Westchester and District 20 Albany and Schenectady in 2017. -->

### Active v. Inactive Voters in 2018

Counting the number of registered voters, however, does not equate to how many people will actually vote.  In the 2016 Presidential election, for example, [58%](https://www.pbs.org/newshour/politics/voter-turnout-2016-elections) of [eligible voters](https://en.wikipedia.org/wiki/Voter_turnout) cast a ballot.  So how can you tell if someone will vote or not?  One way to identify potential voter turnout is to take a look at the active and [inactive voter registration](http://vote.nyc.ny.us/html/voters/faq.shtml)[^1].  Voters who are active are more likely to show up to the polls than are inactive voters.  

Click through the chart below to see the breakdown of active and inactive voters across all districts for each party in 2018.  Is there a party that is consistently active across all districts?  Which party is the most inactive?  Who's more active between the Democrats and Republicans?  Thinking back to the fringe parties percentage change chart, what do you think it means that a newly founded fringe party has inactive voters?

<br>

<iframe src="https://active-inactive-ny-voters2.herokuapp.com/" width="620px" height="520px"></iframe>

### Final Thoughts

The 2018 midterm elections are significant for a number of different reasons.  Republicans retaining control of both the Senate and the House of Representatives means they will be able to continue pushing a more conservative agenda that may include trying to repeal Obamacare again, cutting budgets for social services like welfare, Medicare, and Social Security, and restricting access to abortion.  Moreover, because many gubernatorial and state legislature elections determine how state districts are drawn, whoever is elected will be able to help shape congressional district boundaries in the upcoming 2020 Census, and those boundaries will be in effect for the following ten years.  Here's an [interactive map](https://www.washingtonpost.com/graphics/2018/politics/governors-redistricting/?utm_term=.4dd62658f59e) created by the Washington Post detailing who determines how congressional districts are drawn for each state.

Many of the 2018 Midterm elections will be held on **Tuesday, November 6, 2018**.  All 435 seats in the United States House of Representatives and 35 of the 100 seats in the United States Senate will be contested.[^2]  Vote early and vote often.    

---

[^1]: A voter in inactive status who does not vote in two consecutive Federal Elections is, in the fifth year, removed from the list of the register. The voter must re-register in order to vote.
[^2]: [https://en.wikipedia.org/wiki/United_States_elections,_2018](https://en.wikipedia.org/wiki/United_States_elections,_2018)
