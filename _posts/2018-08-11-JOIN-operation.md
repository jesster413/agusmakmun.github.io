---
layout: post
title: The JOIN Operation Tutorial (SQL Zoo Series)
categories: SQL
description: A complete list of my answers to SQL Zoo's SELECT FROM world Tutorial.  Part of a series of SQL Zoo tutorials.
highlight_code: true
series: "SQL Zoology"
tags:
  - SQL
  - SQL Zoo
excerpt_separator: <!--more-->
published: False
---
*A complete list of my answers to SQL Zoo's JOIN Operation Tutorial.  Part of a series of SQL Zoo tutorials.*

<!--more-->

Lately I've been focusing on my Python projects so I thought I'd switch it up and do some SQL practice using [SQL Zoo's](https://sqlzoo.net/) interactive tutorials.  These tutorials are filled with prompts to query databases using SQL.  What's great about this site is that it shows the results of the query right next to the prompt line, allowing the user to verify that the query pulled the desired results.  SQL Zoo confirms answers with a :)

Below is a list of prompts and my answers to the [SELECT FROM world Tutorial](https://sqlzoo.net/wiki/SELECT_from_WORLD_Tutorial) which focuses on SQL basics, including SELECT statements, WHERE statements, lists, OR and XOR (exclusive or), ROUND functions, letter isolation functions, LENGTH functions, and wildcard searches using %.
<br>

#### 1.
The first example shows the goal scored by a player with the last name 'Bender'. The * says to list all the columns in the table - a shorter way of saying matchid, teamid, player, gtime

Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'

```sql
SELECT matchid, player FROM goal
WHERE teamid = 'GER'
```
<br>

#### 2.
From the previous query you can see that Lars Bender's scored a goal in game 1012. Now we want to know what teams were playing in that match.

Notice in the that the column matchid in the goal table corresponds to the id column in the game table. We can look up information about game 1012 by finding that row in the game table.

Show id, stadium, team1, team2 for just game 1012

```sql
SELECT id,stadium,team1,team2
FROM game
WHERE id = 1012
```
<br>

#### 3.
You can combine the two steps into a single query with a JOIN.

SELECT *
  FROM game JOIN goal ON (id=matchid)
The FROM clause says to merge data from the goal table with that from the game table. The ON says how to figure out which rows in game go with which rows in goal - the id from goal must match matchid from game. (If we wanted to be more clear/specific we could say
ON (game.id=goal.matchid)

The code below shows the player (from the goal) and stadium name (from the game table) for every goal scored.

Modify it to show the player, teamid, stadium and mdate for every German goal.

```sql
SELECT player, teamid, stadium, mdate
FROM game JOIN goal ON (id=matchid)
WHERE teamid = 'GER'
```
<br>

#### 4.
Use the same JOIN as in the previous question.

Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'

```sql
SELECT team1, team2, player
FROM game JOIN goal ON (id=matchid)
WHERE player LIKE 'Mario%'
```
<br>

#### 5.
The table eteam gives details of every national team including the coach. You can JOIN goal to eteam using the phrase goal JOIN eteam on teamid=id

Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10

```sql
SELECT player, teamid, coach, gtime
FROM goal JOIN eteam ON (teamid=id)
WHERE gtime<=10
```
<br>

#### 6.
To JOIN game with eteam you could use either
game JOIN eteam ON (team1=eteam.id) or game JOIN eteam ON (team2=eteam.id)

Notice that because id is a column name in both game and eteam you must specify eteam.id instead of just id

List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.

```sql
SELECT mdate, teamname
FROM game JOIN eteam ON (game.team1=eteam.id)
WHERE coach = 'Fernando Santos'
```
<br>

#### 7.
List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'

```sql
SELECT player
FROM game JOIN goal ON (game.id=goal.matchid)
WHERE stadium = 'National Stadium, Warsaw'
```
<br>

#### 8.
The example query shows all goals scored in the Germany-Greece quarterfinal.
Instead show the name of all players who scored a goal against Germany.

```sql
SELECT DISTINCT player
FROM game JOIN goal ON (game.id = goal.matchid)
WHERE (team1 = 'GER' OR team2 = 'GER') AND teamid <> 'GER'
```
<br>

#### 9.
Show teamname and the total number of goals scored.

```sql
SELECT teamname, COUNT(teamid)
FROM eteam JOIN goal ON (eteam.id=goal.teamid)
GROUP BY teamname
```
<br>

#### 10.
Show the stadium and the number of goals scored in each stadium.

```sql
SELECT stadium, COUNT(teamid)
FROM goal JOIN game ON (goal.matchid = game.id)
GROUP BY stadium
```
<br>

#### 11.
For every match involving 'POL', show the matchid, date and the number of goals scored.

```sql
SELECT matchid, mdate, COUNT(teamid) AS goals
FROM goal JOIN game ON (goal.matchid = game.id)
WHERE team1 = 'POL' OR team2 = 'POL'
GROUP BY matchid, mdate
```
<br>

#### 12.
For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'

```sql
SELECT matchid, mdate, COUNT(teamid)
FROM game JOIN goal ON (id=matchid AND (team1 = 'GER' OR team2 = 'GER')
AND teamid='GER')
GROUP BY matchid, mdate
```
<br>

#### 13.
List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.
mdate	team1	score1	team2	score2
1 July 2012	ESP	4	ITA	0
10 June 2012	ESP	1	ITA	1
10 June 2012	IRL	1	CRO	3
...
Notice in the query given every goal is listed. If it was a team1 goal then a 1 appears in score1, otherwise there is a 0. You could SUM this column to get a count of the goals scored by team1. Sort your result by mdate, matchid, team1 and team2.
