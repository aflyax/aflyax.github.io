---
layout: post
title: Double-checking NPR's income data
permalink: doublecheck-income
comments: true
tags: [python, pandas, income, economics]
---

According to [NPR.org](http://www.npr.org/blogs/money/2015/02/11/384988128/the-fall-and-rise-of-u-s-inequality-in-2-graphs), "After 1980, only the top 1% saw their incomes rise." [Flowing Data](http://flowingdata.com/2015/02/20/top-1-earners-versus-bottom-90/) quoted this figure:

![](http://flowingdata.com/wp-content/uploads/2015/02/Earners-620x560.png)

You can see the trend more clearly here (note that the y-axis shows growth in year X vs. 1917; so, it's clear that income growth stagnated for the bottom 90% in the last 40 years):

![](/images/income_growth.png)

Their data came from [World Top Incomes Database](http://topincomes.parisschoolofeconomics.eu/#Database).

I decided to double-check this claim using a [dataset](https://www.census.gov/hhes/www/income/data/historical/families/2013/f03AR.xls) from census.gov

<!-- more -->

Python code:

``` python

%matplotlib inline
import pandas as pd
import seaborn as sns
sns.set(context="poster", style="dark")
import mpld3

# csv location = https://github.com/aflyax/Python/blob/master/income/income_2013_dollars.csv
income_df = pd.read_csv("income_2013_dollars.csv", sep='\t', thousands=',')
income_df.columns = ["year", "lowest fifth", "second fifth", "third fifth", "fourth fifth", "top fifth", "top 5%"]
income_df.sort(columns="year", inplace=True)
income_df.head()
```

|    | year | lowest fifth | second fifth | third fifth | fourth fifth | top fifth | top 5% |
|----|------|--------------|--------------|-------------|--------------|-----------|--------|
| 47 | 1966 | 14747        | 32739        | 46844       | 62710        | 107026    | 164340 |
| 46 | 1967 | 14977        | 33490        | 48138       | 64560        | 113997    | 180362 |
| 45 | 1968 | 16143        | 35208        | 50300       | 67270        | 114947    | 177194 |
| 44 | 1969 | 16656        | 36783        | 52635       | 70510        | 120757    | 185476 |
| 43 | 1970 | 16404        | 36271        | 52445       | 70744        | 121652    | 185243 |

```python
income_df.tail()
```

|   | year | lowest fifth | second fifth | third fifth | fourth fifth | top fifth | top 5% |
|---|------|--------------|--------------|-------------|--------------|-----------|--------|
| 4 | 2009 | 16604        | 40232        | 65061       | 98788        | 205788    | 352985 |
| 3 | 2010 | 15944        | 39436        | 64276       | 98064        | 199918    | 334275 |
| 2 | 2011 | 15828        | 38898        | 63212       | 96563        | 205003    | 356839 |
| 1 | 2012 | 15760        | 38739        | 63372       | 96861        | 205503    | 357458 |
| 0 | 2013 | 16109        | 39514        | 63916       | 97207        | 206687    | 358722 |

``` python
ax = income_df.plot(x="year")
ax.set_ylabel("income")
ax.set_xlabel("year")
print("feel free to interact with the graph:")
mpld3.display() # to make interactive graph
```

![](/images/income.png)

This is a slightly different story from "after 1980, only the top 1% saw their incomes rise." (It is also beside the point that neither NPR's nor the above graph show mobility. I.e., the same person in 1970 and today is unlikely to belong to the same "income class".)

Notebook for this code is [here](http://nbviewer.ipython.org/github/aflyax/Python/blob/master/income/income.ipynb).