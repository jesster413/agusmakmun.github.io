---
layout: post
title: SELECT within SELECT Tutorial (SQL Zoo Series)
categories: SQL
description: A complete list of my answers to SQL Zoo's SELECT within SELECT Tutorial.  Part of a series of SQL Zoo tutorials.
excerpt_separator: <!--more-->
series: "SQL Zoology"
tags:
- SQL
published: True
---
*A complete list of my answers to SQL Zoo's SELECT within SELECT Tutorial.  Part of a series of SQL Zoo tutorials.*

<!--more-->

Here's the second part of a series of SQL Zoo tutorials which focuses on [nested SELECT](https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial) statements for comparative querying.
<br>
### Bigger than Russia
List each country name where the population is larger than that of 'Russia'.

```sql
SELECT name FROM world
WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');
```
<br>

### Richer than UK
Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.

```sql
SELECT name FROM world
WHERE continent = 'EUROPE' AND gdp/population >
    (SELECT gdp/population FROM world
      WHERE name='United Kingdom');
```
<br>

### Neighbors of Argentina and Australia

List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.

```sql
SELECT name, continent FROM world
WHERE continent IN ('South America', 'Oceania')
ORDER BY name;
```
<br>

### Between Canada and Poland

Which country has a population that is more than Canada but less than Poland? Show the name and the population.

```sql
SELECT name, population FROM world
WHERE population >
    (SELECT population FROM world
    WHERE name = 'Canada')
AND population <
    (SELECT population FROM world
    WHERE name = 'Poland');
```
<br>

### Percentages of Germany

Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.

Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.

```sql
SELECT name, CONCAT(ROUND(population/(SELECT population FROM world
WHERE name = 'Germany') * 100), '%') FROM world
WHERE continent = 'Europe';
```
<br>

### Bigger than every country in Europe

Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)

```sql
SELECT name FROM world
WHERE gdp >= ALL(SELECT gdp FROM world
                    WHERE continent = 'Europe' AND gdp>0)
AND continent != 'Europe';
```
<br>

### Largest in each continent

Find the largest country (by area) in each continent, show the continent, the name and the area:

```sql
SELECT continent, name, area FROM world x
WHERE area >= ALL
  (SELECT area FROM world y
      WHERE y.continent=x.continent
          AND area>0);
```
<br>

### First country of each continent (alphabetically)

List each continent and the name of the country that comes first alphabetically.

```sql
SELECT continent, name FROM world x
WHERE name <= ALL
  (SELECT name FROM world y
      WHERE y.continent=x.continent);
```
<br>

### Difficult Questions That Utilize Techniques Not Covered In Prior Sections

Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.

```sql
SELECT name, continent, population FROM world
WHERE continent IN
  (SELECT continent FROM world x
    WHERE
      (SELECT MAX(population) FROM world y
        WHERE x.continent = y.continent) <= 25000000);
```
<br>

Some countries have populations more than three times that of any of their neighbors (in the same continent). Give the countries and continents.

```sql
SELECT name, continent FROM world x
WHERE population > ALL
	(SELECT 3*population FROM world y
		WHERE y.continent=x.continent AND x.name <> y.name);
```
<br>

---
