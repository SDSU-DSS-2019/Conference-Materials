---
title       : Tidying data with `tidyr`
author      : Adam J Sullivan 
job         : Assistant Professor of Biostatistics
work        : Brown University
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js # {highlight.js, prettify, highlight}
hitheme     :  github     # 
widgets     : [mathjax, quiz, bootstrap, interactive] # {mathjax, quiz, bootstrap}
ext_widgets : {rCharts: [libraries/nvd3, libraries/leaflet, libraries/dygraphs]}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
logo        : publichealthlogo.png
biglogo     : publichealthlogo.png
assets      : {assets: ../../assets}
---  .segue bg:grey







# `tidyr`


--- .segue .quote bg:#C0C8CE


<q>  The principles of tidy data provide a standard way to organize data values within a dataset.</q> 

<q>--Hadley Wickham (2014)</q>








--- .class #id


## What does it do?

The picture below displays this:



<center>
<img src="./assets/img/tidy-spread.png" style="height:100%;width:80%">
</center>


--- .class #id

## Consider this Data:




```
## # A tibble: 12 x 4
##    country      year key             value
##    <fct>       <int> <fct>           <int>
##  1 Afghanistan  1999 cases             745
##  2 Afghanistan  1999 population   19987071
##  3 Afghanistan  2000 cases            2666
##  4 Afghanistan  2000 population   20595360
##  5 Brazil       1999 cases           37737
##  6 Brazil       1999 population  172006362
##  7 Brazil       2000 cases           80488
##  8 Brazil       2000 population  174504898
##  9 China        1999 cases          212258
## 10 China        1999 population 1272915272
## 11 China        2000 cases          213766
## 12 China        2000 population 1280428583
```

--- .class #id

## `key` column

- Notice that in the column of `key`, instead of there being values we see the following variable names:
    - cases
    - population

--- .class #id

## What Should we see?

- In order to use this data we need to have it so the data frame looks like this instead:


```
## # A tibble: 6 x 4
##   country      year  cases population
##   <fct>       <int>  <int>      <int>
## 1 Afghanistan  1999    745   19987071
## 2 Afghanistan  2000   2666   20595360
## 3 Brazil       1999  37737  172006362
## 4 Brazil       2000  80488  174504898
## 5 China        1999 212258 1272915272
## 6 China        2000 213766 1280428583
```

--- .class #id

## Using the `spread()` Function

- Now we can see that we have all the columns representing the variables we are interested in and each of the rows is now a complete observation. 
- In order to do this we need to learn about the `spread()` function:



--- .class #id

## Piping

- If we consider **piping**, we can write this as:

```
data %>%
  spread(key, value)
```

--- .class #id

## `spread()` Example


- Now if we consider table2 , we can see that we have:


```
## # A tibble: 12 x 4
##    country      year key             value
##    <fct>       <int> <fct>           <int>
##  1 Afghanistan  1999 cases             745
##  2 Afghanistan  1999 population   19987071
##  3 Afghanistan  2000 cases            2666
##  4 Afghanistan  2000 population   20595360
##  5 Brazil       1999 cases           37737
##  6 Brazil       1999 population  172006362
##  7 Brazil       2000 cases           80488
##  8 Brazil       2000 population  174504898
##  9 China        1999 cases          212258
## 10 China        1999 population 1272915272
## 11 China        2000 cases          213766
## 12 China        2000 population 1280428583
```

--- .class #id

## `spread()` Example

- Now this table was made for this example so key is the `key` in our `spread()` function and value is the `value` in our `spread()` function.
- We can fix this with the following code:

--- .class #id

## `spread()` Example


```r
table2 %>%
    spread(key,value)
```

```
## # A tibble: 6 x 4
##   country      year  cases population
##   <fct>       <int>  <int>      <int>
## 1 Afghanistan  1999    745   19987071
## 2 Afghanistan  2000   2666   20595360
## 3 Brazil       1999  37737  172006362
## 4 Brazil       2000  80488  174504898
## 5 China        1999 212258 1272915272
## 6 China        2000 213766 1280428583
```

--- .class #id

## `spread()` Example

- We can now see that we have a variable named `cases` and a variable named `population`. 
- This is much more tidy. 







--- .class #id

## On Your Own: RStudio Practice #9

- In this example we will use the dataset `population` that is part of tidyverse. 
- Print this data:


```
## # A tibble: 1 x 3
##   country      year population
##   <chr>       <int>      <int>
## 1 Afghanistan  1995   17586073
```

--- .class #id

## On Your Own: RStudio Practice #9

- You should see the table that we have above, now We have a variable named `year`, assume that we wish to actually have each year as its own variable. 
- Using the `spread()` function, redo this data so that each year is a variable. 
- Your data will look like this at the end:


--- .class #id

## On Your Own: RStudio Practice #9 


```
## # A tibble: 219 x 20
##    country `1995` `1996` `1997` `1998` `1999` `2000` `2001` `2002` `2003`
##    <chr>    <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>
##  1 Afghan~ 1.76e7 1.84e7 1.90e7 1.95e7 2.00e7 2.06e7 2.13e7 2.22e7 2.31e7
##  2 Albania 3.36e6 3.34e6 3.33e6 3.33e6 3.32e6 3.30e6 3.29e6 3.26e6 3.24e6
##  3 Algeria 2.93e7 2.98e7 3.03e7 3.08e7 3.13e7 3.17e7 3.22e7 3.26e7 3.30e7
##  4 Americ~ 5.29e4 5.39e4 5.49e4 5.59e4 5.68e4 5.75e4 5.82e4 5.87e4 5.91e4
##  5 Andorra 6.39e4 6.43e4 6.41e4 6.38e4 6.41e4 6.54e4 6.80e4 7.16e4 7.56e4
##  6 Angola  1.21e7 1.25e7 1.28e7 1.31e7 1.35e7 1.39e7 1.44e7 1.49e7 1.54e7
##  7 Anguil~ 9.81e3 1.01e4 1.03e4 1.05e4 1.08e4 1.11e4 1.14e4 1.17e4 1.20e4
##  8 Antigu~ 6.83e4 7.02e4 7.22e4 7.42e4 7.60e4 7.76e4 7.90e4 8.00e4 8.09e4
##  9 Argent~ 3.48e7 3.53e7 3.57e7 3.61e7 3.65e7 3.69e7 3.73e7 3.76e7 3.80e7
## 10 Armenia 3.22e6 3.17e6 3.14e6 3.11e6 3.09e6 3.08e6 3.06e6 3.05e6 3.04e6
## # ... with 209 more rows, and 10 more variables: `2004` <int>,
## #   `2005` <int>, `2006` <int>, `2007` <int>, `2008` <int>, `2009` <int>,
## #   `2010` <int>, `2011` <int>, `2012` <int>, `2013` <int>
```


---  .segue bg:grey


# The `gather()` Function


--- .class #id


## The `gather()` Function

- The second `tidyr` function we will look into is the `gather()` function. 
- With `gather()` it may not be clear what exactly is going on, but in this case we actually have a lot of column names the represent what we would like to have as data values. 

--- .class #id



<center>
<img src="./assets/img/tidy-gather.png" style="height:100%;width:80%">
</center>


--- .class #id


## The `gather()` Function Example


- For example, in the last `spread()` practice you created a data frame where variable names were individual years. 
- This may not be what you want to have so you can use the gather function. 


--- .class #id


## Consider `table4`:


```
## # A tibble: 3 x 3
##   country     `1999` `2000`
##   <fct>        <int>  <int>
## 1 Afghanistan    745   2666
## 2 Brazil       37737  80488
## 3 China       212258 213766
```

--- .class #id

## Table 4

- This looks similar to the table you created in the `spread()` practice.
- We now wish to change this data frame so that `year` is a variable and 1999 and 2000 become values instead of variables. 

--- .class #id



## `gather()` Example

- In our example here we would do the following:


```r
table4 %>%
    gather("year", "cases", 2:3)
```

```
## # A tibble: 6 x 3
##   country     year   cases
##   <fct>       <chr>  <int>
## 1 Afghanistan 1999     745
## 2 Brazil      1999   37737
## 3 China       1999  212258
## 4 Afghanistan 2000    2666
## 5 Brazil      2000   80488
## 6 China       2000  213766
```

--- .class #id

## `gather()` Example

- You can see that we have created 2 new columns called `year` and `cases`. 
- We filled these with the previous 2nd and 3rd columns. 
- Note that we could have done this in many different ways too. 


--- .class #id

## `gather()` Example

- For example if we knew the years but not which columns we could do this:

```
table4 %>%
    gather("year", "cases", "1999":"2000")
```

- We could also see that we want to gather all columns except the first so we could have used:

```
table4 %>%
    gather("year", "cases", -1)
```


--- .class #id

## On Your Own: RStudio Practice #10

- Create `population2` from last example:


```r
population2 <- population %>% 
                    spread(year, population)
```

- Now gather the columns that are labeled by year and create columns `year` and `population`. 


--- .class #id

## On Your Own: RStudio Practice #10 


In the end your data frame should look like:


```
## # A tibble: 2 x 3
##   country      year population
##   <chr>       <int>      <int>
## 1 Afghanistan  1995   17586073
## 2 Afghanistan  1996   18415307
```

