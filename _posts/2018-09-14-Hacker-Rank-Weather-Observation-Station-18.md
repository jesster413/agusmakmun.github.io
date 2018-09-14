---
layout: post
title: Weather Observation Station 18
author: Jess Chace
cover_image: hackerrank.png
cover_alt: Hacker Rank tutorials for Python and SQL
description:
categories: Tutorial
highlight_code: true
series: "Hacker Rank"
tags:
- SQL
excerpt_separator: <!--more-->
published: True
---
### Prompt

Consider *P<sub>1</sub>(a,b)* and *P<sub>2</sub>(c,d)* to be two points on a 2D plane.

*a* happens to equal the minimum value in Northern Latitude (LAT_N in STATION).

*b* happens to equal the minimum value in Western Longitude (LONG_W in STATION).

*c* happens to equal the maximum value in Northern Latitude (LAT_N in STATION).

*d* happens to equal the maximum value in Western Longitude (LONG_W in STATION).

Query the [Manhattan Distance](https://xlinux.nist.gov/dads/HTML/manhattanDistance.html) between points *P<sub>1</sub>* and *P<sub>2</sub>* and round it to a scale of 4 decimal places.

The STATION table is described as follows:

| Field  | Type        |
|--------|-------------|
| ID     | NUMBER      |
| CITY   | VARCHAR2(21)|
| STATE  | VARCHAR2(2) |
| LAT_N  | NUMBER      |
| LONG_W | NUMBER      |

<br>
where LAT_N is the northern latitude and LONG_W is the western longitude.

<!-- ### Discussion

Talk about Manhattan Distance here.  talk about x and y matching up -->


### Answer

```sql
SELECT ROUND(ABS(MIN(LAT_N) - MAX(LAT_N)) + ABS(MIN(LONG_W) - MAX(LONG_W)),4) FROM STATION
```
