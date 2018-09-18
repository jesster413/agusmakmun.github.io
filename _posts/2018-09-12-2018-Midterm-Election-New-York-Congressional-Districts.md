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

In anticipation of the 2018 mid-term elections, Enigma Public is hosting a data science competition.  Using their new [SDK package](https://pypi.org/project/enigma-sdk/) which facilitates API calls on their public database, contestants are encouraged to explore topics that have some explanatory power of the midterm elections.  

Under their [Elections](https://public.enigma.com/browse/tag/elections/34) collection, there are six umbrella datasets: Congressional Candidate Disbursements, Federal Election Commission, New York State Board of Elections, PAC Summary, Presidential Campaign Disbursements, and Tafra.  While there's definitely something to dig into in each of these datasets, I chose to explore the New York State Board of Elections dataset filled with 5 years of voter registration statistics by Congressional District.  Below are some of my findings.



### Percentage Change of Voters for both Dems and Reps Before and After 2016 Presidential Election

In an election as divisive as the one in 2016, it makes sense that voters had a strong reaction leading up to and after the election , regardless of political affiliation.  Reflected in the chart below are the changes in percentage of voter registration for both Democrats and Republicans by year.  From 2014 to 2015, with the exceptions of District 4 and District 15, there was a decrease of voter registration in both parties.  From 2015 to 2016, Districts 5 through 14 all showed another decrease in voter registration for both parties.  Notably, these are all districts located in the New York City area.  Outside the city, however, namely in Long Island and upstate New York, there was an increase in voter registration in both parties, although the Democrats saw a greater increase across the board.  From 2016-2017 with the exception of District 23, there was a universal increase for both parties.  Surprisingly, in six districts located in New York City proper where Democrats have traditionally dominated representation, Republican voter registration surpassed Democrat voter registration.  Finally, from 2017 to 2018, there was a decrease in percentage change of voter registration in many of the districts, although Democrats enjoyed an increase in Districts 1, 2, 18, 19, 20, 21, 24, and 25, many of which are currently represented by a Republican congressman.  

![demsreps_percentchange.png](/static/img/demsreps_percentchange.png)

### Pushed to the Edge (A Look at Fringe Parties)

Founded in 2014, the Women's Equality Party (WEP) was created by Andrew Cuomo, the current governor of New York.  The party reached full political party status in 2014 during the gubernatorial elections after receiving the requisite 50,000 votes.  While Cuomo is no longer in control of the party on paper, the party remains deeply supportive of the incumbent governor, supporting his most recent re-election bid in 2018.

The Reform Party (REF), another fringe political party founded in 2014, is a ballot-access qualified party created by [Curtis Sliwa](https://www.nyreformparty.com/endorsements).  Silwa initially founded another group called the Guardian Angels in 1979 and is a regular contributor to several talk radio and television shows, including Fox 5.  

As seen in the plot below, both the Women's Equality Party (WEP) in pink as well as the Reform Party (REF) in lilac have made significant strides in voter registration.

![alldistricts_change.png](/static/img/alldistricts_change.png)

However, not all districts have reacted in the same way to these new parties.  For example, District 1, located in Eastern Long Island, District 2, located on Southern Long Island, and Districts 23 and 27, both located in Western New York, all of which are represented by a Republican congressman, show a significant percentage change in voter registration for the WEP party in 2016.  Similarly for Democrat-represented districts like District 3 located in Northern Long Island, District 7, located in Queens, District 18, lcoated in Orange, Putnam, Southern Dutchess, and Northeastern Westchester, District 20, located in Albany and Schenectady, all saw a huge spike in WEP registration in 2016.    

For the REF party, there was some increase in Republican-represented districts like District 19 in the Hudson Valley and District 24 in Cayuga, Onondaga, Wayne counties in 2016 and District 2, located in Southern Long Island and District 23 located in Western New York in 2017.  The REF party increased its voter registration percentage significantly two years in a row in District 22 located in Central New York.

For Democrat-represented districts, the REF party also increased its voter share most significantly in District 18 in Orange, Putnam, Southern Dutchess, and Northeastern Westchester counties in 2016, and in District 3, Northern Long Island and District 4, Central and Southern Nassau county, and to a lesser extent District 5, Queens, and District 10, an amalgamation of the Upper West Side, the Financial District, Greenwich Village, and Borough Park in Brooklyn, District 16, Northern Bronx and Southern Westchester and District 20 Albany and Schenectady in 2017.

Below is an interactive chart made with Dash.  Feel free to set the focus of the chart by drilling down onto a District using the drop down menu at the top left as well as clicking any combination of the political party boxes on the right.  What trends do you see across districts and parties?  

<iframe src="https://ny-congressional-districts.herokuapp.com/" width="600px" height="520px"></iframe>
