---
title       : Transforming Data with `dplyr`
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

# Piping or Chaining Data

--- .class #id

## Piping or Chaining

- We will discuss a concept that will help us greatly when it comes to working with our data. 
- The usual way to perform multiple operations in one line is by nesting. 

--- .class #id

## Piping or Chaining

To consider an example we will look at the data provided in the gapminder package:


```r
library(gapminder)
head(gapminder)
```

```
## # A tibble: 6 x 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
```

--- .class #id

## Nesting vs Chaining

- Let's say that we want to have the GDP per capita and life expectancy Kenya.
- Traditionally speaking we could do this in a nested manner:



```r
filter(select(gapminder, country, lifeExp, gdpPercap), country=="Kenya")
```

--- .class #id

## Nesting vs Chaining

- It is not easy to see exactly what this code was doing but we can write this in a manner that follows our logic much better. 
- The code below represents how to do this with chaining. 


```r
gapminder %>%
    select(country, lifeExp, gdpPercap) %>%
    filter(country=="Kenya")
```

--- .class #id

## Breaking Down the Code

- We now have something that is much clearer to read.
- Here is what our chaining command says:
    1. Take the `gapminder` data
    2. Select the variables: `country`, `lifeExp` and `gdpPercap`.
    3. Only keep information from Kenya. 
- The nested code says the same thing but it is hard to see what is going on if you have not been coding for very long.


--- .class #id

## Breaking Down the Code

- The result of this search is below: 


```
# A tibble: 12 x 3
   country lifeExp gdpPercap
    <fctr>   <dbl>     <dbl>
 1   Kenya  42.270  853.5409
 2   Kenya  44.686  944.4383
 3   Kenya  47.949  896.9664
 4   Kenya  50.654 1056.7365
 5   Kenya  53.559 1222.3600
 6   Kenya  56.155 1267.6132
 7   Kenya  58.766 1348.2258
 8   Kenya  59.339 1361.9369
 9   Kenya  59.285 1341.9217
10   Kenya  54.407 1360.4850
11   Kenya  50.992 1287.5147
12   Kenya  54.110 1463.2493
```

--- .class #id

## What is `%>%`

- In the previous code we saw that we used `%>%` in the command you can think of this as saying ***then***.
- For example:


```r
gapminder %>%
    select(country, lifeExp, gdpPercap) %>%
    filter(country=="Kenya")
```

--- .class #id

## What Does this Mean?

- This translates to:
    - Take Gapminder ***then*** select these columns select(country, lifeExp, gdpPercap) ***then*** filter out so we only keep Kenya


--- .class #id

## Why Chain?

- We still might ask why we would want to do this. 
- Chaining increases readability significantly when there are many commands. 
- With many packages we can replace the need to perform nested arguments. 
- The chaining operator is automatically imported from the [magrittr](https://github.com/smbache/magrittr) package. 

--- .class #id

## User Defined Function


- Let's say that we wish to find the Euclidean distance between two vectors say, `x1` and `x2`. 
- We could use the math formula:

\[\sqrt{\sum(x_1-x_2)^2}\]

--- .class #id

## User Defined Function

- In the nested manner this would be:


```r
x1 <- 1:5; x2 <- 2:6
sqrt(sum((x1-x2)^2))
```

--- .class #id

## User Defined Function


- However, if we chain this we can see how we would perform this mathematically. 

```r
# chaining method
(x1-x2)^2 %>% sum() %>% sqrt()
```
- If we did it by hand we would perform elementwise subtraction of `x2` from `x1` ***then*** we would sum those elementwise values ***then*** we would take the square root of the sum. 


--- .class #id

## User Defined Function



```r
# chaining method
(x1-x2)^2 %>% sum() %>% sqrt()
```

```
## Error in (x1 - x2)^2 %>% sum() %>% sqrt(): could not find function "%>%"
```

- Many of us have been performing calculations by this type of method for years, so that chaining really is more natural for us. 


---  .segue bg:grey


# The `dplyr` Package

---  .segue bg:grey

# The `spread()` Function

--- .class #id


## The `spread()` Function

- The first `tidyr` function we will look into is the `spread()` function. 
- With `spread()` it does similar to what you would expect.  
- We have a data frame where some of the rows contain information that is really a variable name. 
- This means the columns are a combination of variable names as well as some data. 


--- .class #id

## The `dplyr` Package

- Now that we have started to tidy up our data we can see that we have a need to transform this data. 
- We may wish to add additional variables. 
- The `dplyr` package allows us to further work with our data. 

--- .class #id


## `dplyr` Functionality

- With `dplyr` we have five basic verbs that we will learn to work with:
  - `filter()`
  - `select()`
  - `arrange()`
  - `mutate()`
  - `summarize()`


  


---  .segue bg:grey


# Filtering 


--- .class #id

## Filtering


- At this point we will consider how we pick the rows of the data that we wish to work with. 
- If you consider many modern data sets, we have so much information that we may not want to bring it all in at once. 
- R brings data into the RAM of your computer. This means you can be limited for what size data you can bring in at once. 
- Very rarely do you need the entire data set. 
- We will focus on how to pick the rows or observations we want now.


--- .class #id

## Enter the `filter()` Function

- The `filter()` function chooses rows that meet a specific criteria. 
- We can do this with Base R functions or with   `dplyr`. 


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```


--- .class #id

## Filtering Example: The Old Way

- Let's say that we want to look at the data just for the country of Kenya in 2002



```r
gapminder[gapminder$country=="Kenya" & gapminder$year==2002, ]
```

```
## # A tibble: 1 x 6
##   country continent  year lifeExp      pop gdpPercap
##   <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Kenya   Africa     2002    51.0 31386842     1288.
```

--- .class #id

## Filtering Example


- Now this is not very difficult to do, what we have is that we are working with `gapminder` and we only want to keep the rows of data there `country=="Kenya` and `year==2002`. 
- However we could use the `filter()` function to do this in a much easier to read format:


--- .class #id

## Filtering Example: Tidyverse Way

- This means in our example we could perform the following:

```
gapminder %>%
    filter(country=="Kenya", year==2002)
```

Finally we could also only do one filtering at a time and chain it:

```
gapminder %>%
    filter(country=="Kenya") %>%
    filter(year==2002)
```


--- .class #id


## Further Filtering

- `filter()` supports the use of multiple conditions where we can use Boolean. 
- For example if we wanted to consider only observations that have a life expectancy between 49 and 60, we could do the following:


```r
gapminder %>% 
  filter(lifeExp>=49 & lifeExp<60)
```

--- .class #id


## Further Filtering


```
## # A tibble: 373 x 6
##    country    continent  year lifeExp      pop gdpPercap
##    <fct>      <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Albania    Europe     1952    55.2  1282697     1601.
##  2 Albania    Europe     1957    59.3  1476505     1942.
##  3 Algeria    Africa     1967    51.4 12760499     3247.
##  4 Algeria    Africa     1972    54.5 14760787     4183.
##  5 Algeria    Africa     1977    58.0 17152804     4910.
##  6 Bahrain    Asia       1952    50.9   120447     9867.
##  7 Bahrain    Asia       1957    53.8   138655    11636.
##  8 Bahrain    Asia       1962    56.9   171863    12753.
##  9 Bahrain    Asia       1967    59.9   202182    14805.
## 10 Bangladesh Asia       1982    50.0 93074406      677.
## # ... with 363 more rows
```

--- .class #id


## Further Filtering

- We can also use the `filter()` function to remove missing data for us. 
- Previously we learned about a class of functions called `is.`*foo*`()` where *foo* represents a data type. 
- We could choose to only use observations that have life expectancy. 
- That means we wish to not have missing data for life expectancy. 



```r
gapminder %>% filter(!is.na(lifeExp))
```

--- .class #id


## On Your Own: RStudio Practice #5


Using the `filter()` function and chaining: 

- Choose only rows associated with
  - Uganda
  - Rwanda

--- .class #id


## On Your Own: RStudio Practice #5



Your end result should be:


```
## # A tibble: 24 x 6
##    country continent  year lifeExp     pop gdpPercap
##    <fct>   <fct>     <int>   <dbl>   <int>     <dbl>
##  1 Rwanda  Africa     1952    40   2534927      493.
##  2 Rwanda  Africa     1957    41.5 2822082      540.
##  3 Rwanda  Africa     1962    43   3051242      597.
##  4 Rwanda  Africa     1967    44.1 3451079      511.
##  5 Rwanda  Africa     1972    44.6 3992121      591.
##  6 Rwanda  Africa     1977    45   4657072      670.
##  7 Rwanda  Africa     1982    46.2 5507565      882.
##  8 Rwanda  Africa     1987    44.0 6349365      848.
##  9 Rwanda  Africa     1992    23.6 7290203      737.
## 10 Rwanda  Africa     1997    36.1 7212583      590.
## # ... with 14 more rows
```

---  .segue bg:grey



# Selecting


--- .class #id

## Selecting

- The next logical step would be to select the columns we want as well. 
- Many times we have so many columns that we are no interested in for a particular analysis. 
- Instead of slowing down your analysis by continuing to run through extra data, we could just select the columns we care about. 

--- .class #id


## Enter the `select()` Function

- The `select()` function chooses columns that we specify. 
- Again we can do this with base functions or with `dplyr`. 
- We feel that as you continue on with your R usage that you will most likely want to go the route of `dplyr` functions instead.

--- .class #id

## Select Example: Old Way

- Let's say that we want to look at the gapminder data but we are really only interested in the country, life expectancy and year. 
- This seems reasonable if we are a customer and wanted to only know these pieces of information. We could do this with indexing : 


```r
gapminder[, c("country", "lifeExp", "year")]
```


--- .class #id

## Selecting Example: Tidyverse Way 

We could then do the following


```r
gapminder %>%
  select(country, lifeExp, year)
```


--- .class #id

## Selecting Example: Tidyverse Way 




```
## # A tibble: 1,704 x 3
##    country     lifeExp  year
##    <fct>         <dbl> <int>
##  1 Afghanistan    28.8  1952
##  2 Afghanistan    30.3  1957
##  3 Afghanistan    32.0  1962
##  4 Afghanistan    34.0  1967
##  5 Afghanistan    36.1  1972
##  6 Afghanistan    38.4  1977
##  7 Afghanistan    39.9  1982
##  8 Afghanistan    40.8  1987
##  9 Afghanistan    41.7  1992
## 10 Afghanistan    41.8  1997
## # ... with 1,694 more rows
```


--- .class #id

## Removing Columns


- We may wish to pick certain columns that we wish to have but we also may want to remove certain columns. 
- It is quite common to de-identify a dataset before actually distributing it to a research team. 
- The `select()` function will also remove columns. 



--- .class #id

## Removing Columns

- Lets say that we wished to remove the `gdpPercap` and `pop` of the countries:


```r
gapminder %>%
  select(-gdpPercap,-pop)
```

--- .class #id

## Removing Columns

We also could use a vector for this:


```r
cols <- c("gdpPercap", "pop")
gapminder %>%
  select(-one_of(cols))
```

--- .class #id

## Removing Columns

- We can also remove columns that contain a certain phrase in the name. 
- If we were interested in removing any columns that had to do with time we could search for the phrase "co" in the data and remove them:


```r
gapminder %>%
  select(-matches("Per"))
```

--- .class #id

## Removing Columns



```
## # A tibble: 1,704 x 5
##    country     continent  year lifeExp      pop
##    <fct>       <fct>     <int>   <dbl>    <int>
##  1 Afghanistan Asia       1952    28.8  8425333
##  2 Afghanistan Asia       1957    30.3  9240934
##  3 Afghanistan Asia       1962    32.0 10267083
##  4 Afghanistan Asia       1967    34.0 11537966
##  5 Afghanistan Asia       1972    36.1 13079460
##  6 Afghanistan Asia       1977    38.4 14880372
##  7 Afghanistan Asia       1982    39.9 12881816
##  8 Afghanistan Asia       1987    40.8 13867957
##  9 Afghanistan Asia       1992    41.7 16317921
## 10 Afghanistan Asia       1997    41.8 22227415
## # ... with 1,694 more rows
```

--- .class #id

## Unique Observations

- Many times we have a lot of repeats in our data. 
- If we just would like to have an account of all things included then we can use the `unique()` command. 
- Lets assume that we wish to know which countries are in the data. 
- We do not want to have every country listed over and over again so we ask for unique values:


```r
gapminder %>% 
  select(country) %>% 
  unique()
```


--- .class #id

## On Your Own: RStudio Practice # 6

1. Select all but the `pop` column.
2. Remove the year and lifeExp from this new dataframe.
3. Select values which contain "p" in them. 
4. Chain these together so that you run a command and it does all of these things. 



--- .class #id

## On Your Own: RStudio Practice # 6


Your answer should look like:


```
## # A tibble: 1,704 x 1
##    gdpPercap
##        <dbl>
##  1      779.
##  2      821.
##  3      853.
##  4      836.
##  5      740.
##  6      786.
##  7      978.
##  8      852.
##  9      649.
## 10      635.
## # ... with 1,694 more rows
```


---  .segue bg:grey

# Arranging the Data

--- .class #id

## Arranging the Data

- We also have need to make sure the data is ordered in a certain manner. This can be easily done in R with the `arrange()` function. 
- Again we can do this in base R but this is not always a clear path. 

--- .class #id

## Arranging the Data Example: Old Way

- Let's say that we wish to look countries, year and life expectancy. 
- Thensay further we want to sort it by Life Expactancy. 
- In base R we would have to run the following command:


```r
library(gapminder)
library(tidyverse)
```

```
## -- Attaching packages ------------------------------------------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.0     v readr   1.3.1
## v tibble  2.0.1     v purrr   0.2.5
## v tidyr   0.8.2     v stringr 1.3.1
## v ggplot2 3.1.0     v forcats 0.3.0
```

```
## -- Conflicts ---------------------------------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
gapminder[order(gapminder$lifeExp), c("country", "year", "lifeExp")]
```



--- .class #id

## Arranging the Data Example: Tidyverse Way

```r
gapminder %>%
    select(country,year,  lifeExp) %>%
    arrange(lifeExp)
```

--- .class #id

## Arranging the Data Example: Tidyverse Way


```
## # A tibble: 1,704 x 3
##    country       year lifeExp
##    <fct>        <int>   <dbl>
##  1 Rwanda        1992    23.6
##  2 Afghanistan   1952    28.8
##  3 Gambia        1952    30  
##  4 Angola        1952    30.0
##  5 Sierra Leone  1952    30.3
##  6 Afghanistan   1957    30.3
##  7 Cambodia      1977    31.2
##  8 Mozambique    1952    31.3
##  9 Sierra Leone  1957    31.6
## 10 Burkina Faso  1952    32.0
## # ... with 1,694 more rows
```

--- .class #id

## Arranging the Data Example: Tidyverse Way

- With `arrange()` we first use `select()` to pick the only columns that we want and then we arrange by the `lifeExp`. 
- If we had wished to order them in a descending manner we could have simply used the `desc()` function:


```r
gapminder %>%
    select(country,year,  lifeExp) %>%
    arrange(desc(lifeExp))
```

--- .class #id


## More Complex Arrange

- Lets consider that we wish to look at the top 3 Life Expectancies for each year.
- Then we wish to order them from largest to smallest Life Expectancy. 
- We then need to do the following:
    1. Group by year.
    2. Pick the top 3 life expectancy
    3. order them largest to smallest
    



--- .class #id

## More Complex Arrange Continued



```r
gapminder %>% 
  group_by(year) %>%  
  top_n(3, lifeExp) %>% 
  arrange(desc(lifeExp))
```

- Where
    - `group_by()` is a way to group data. This way we perform operations on a group. So top 3 life expectancy are grouped by year. 
    - `top_n()`takes a tibble and returns a specific number of rows based on a chosen value. 


--- .class #id

## More Complex Arrange Continued



```
## # A tibble: 36 x 6
## # Groups:   year [12]
##    country          continent  year lifeExp       pop gdpPercap
##    <fct>            <fct>     <int>   <dbl>     <int>     <dbl>
##  1 Japan            Asia       2007    82.6 127467972    31656.
##  2 Hong Kong, China Asia       2007    82.2   6980412    39725.
##  3 Japan            Asia       2002    82   127065841    28605.
##  4 Iceland          Europe     2007    81.8    301931    36181.
##  5 Hong Kong, China Asia       2002    81.5   6762476    30209.
##  6 Japan            Asia       1997    80.7 125956499    28817.
##  7 Switzerland      Europe     2002    80.6   7361757    34481.
##  8 Hong Kong, China Asia       1997    80     6495918    28378.
##  9 Sweden           Europe     1997    79.4   8897619    25267.
## 10 Japan            Asia       1992    79.4 124329269    26825.
## # ... with 26 more rows
```



--- .class #id

## On Your Own: RStudio Practice #7 

- Perform the following operations:
  - Group by year. 
  - use `sample_n()` to pick 1 observation per year 
  - Arrange by longest to smallest life expectancy. 



--- .class #id





## On Your Own: RStudio Practice


Your answer ***may*** look like:


```
## # A tibble: 12 x 6
## # Groups:   year [12]
##    country      continent  year lifeExp        pop gdpPercap
##    <fct>        <fct>     <int>   <dbl>      <int>     <dbl>
##  1 Cuba         Americas   2002    77.2   11226999     6341.
##  2 Puerto Rico  Americas   1987    74.6    3444468    12281.
##  3 Switzerland  Europe     1972    73.8    6401400    27195.
##  4 China        Asia       1992    68.7 1164970000     1656.
##  5 Cambodia     Asia       2007    59.7   14131858     1714.
##  6 Eritrea      Africa     1997    53.4    4058319      913.
##  7 Mongolia     Asia       1967    51.3    1149500     1226.
##  8 South Africa Africa     1962    50.0   18356657     5769.
##  9 Iran         Asia       1957    47.2   19792000     3290.
## 10 Gambia       Africa     1982    45.6     715523      836.
## 11 Benin        Africa     1952    38.2    1738315     1063.
## 12 Sierra Leone Africa     1977    36.8    3140897     1348.
```

---  .segue bg:grey


# Summarizing Data


--- .class #id

## Summarizing Data

- As you have seen in your own work, being able to summarize information is crucial. 
- We need to be able to take out data and summarize it as well. 
- We will consider doing this using the `summarise()` function. 

--- .class #id

## Summarizing Data: Old Way

- Like in the rest of these lessons, let's consider what happens when we try to to do this in base R. We will:
  1. Create a table grouped by `country`.
  2. Summarize each group by taking mean of `lifeExp`.


```r
head(with(gapminder, tapply(lifeExp, country, mean, na.rm=TRUE)))
head(aggregate(lifeExp ~ country, gapminder, mean))
```

--- .class #id


## Enter `summarise()` Function

- The `summarise()` function is:

```
summarise(.data, ...)
```

- where
  - `.data` is the tibble of interest.
  - `...` is a list of name paired summary functions
    - Such as:
      - `mean()`
      - `median`
      - `var()`
      - `sd()`
      - `min()`
      - `max()
      - ...



--- .class #id

## Summarizing Data Example: Tidyverse Way 

- Consider the logic here:
  1. Group data by country
  2. Find the average life Expectancy of the groups and call it `avg_lifeExp`.
- This is much easier to understand than the  Base R code. 


```r
gapminder %>%
    group_by(country) %>%
    summarise(avg_lifeExp = mean(lifeExp, na.rm=TRUE))
```

--- .class #id


## Summarizing Data Example: Tidyverse Way 



```
## # A tibble: 142 x 2
##    country     avg_lifeExp
##    <fct>             <dbl>
##  1 Afghanistan        37.5
##  2 Albania            68.4
##  3 Algeria            59.0
##  4 Angola             37.9
##  5 Argentina          69.1
##  6 Australia          74.7
##  7 Austria            73.1
##  8 Bahrain            65.6
##  9 Bangladesh         49.8
## 10 Belgium            73.6
## # ... with 132 more rows
```

--- .class #id


## Another Example

- Lets say that we would like to have more than just the averages but we wish to have the minimum and the maximum life expectancies by country:


```r
gapminder %>%
    group_by(country) %>%
    summarise_each(funs(min(., na.rm=TRUE), max(., na.rm=TRUE)), lifeExp)
```


--- .class #id


## Another Example




```
## `summarise_each()` is deprecated.
## Use `summarise_all()`, `summarise_at()` or `summarise_if()` instead.
## To map `funs` over a selection of variables, use `summarise_at()`
```

```
## # A tibble: 142 x 3
##    country       min   max
##    <fct>       <dbl> <dbl>
##  1 Afghanistan  28.8  43.8
##  2 Albania      55.2  76.4
##  3 Algeria      43.1  72.3
##  4 Angola       30.0  42.7
##  5 Argentina    62.5  75.3
##  6 Australia    69.1  81.2
##  7 Austria      66.8  79.8
##  8 Bahrain      50.9  75.6
##  9 Bangladesh   37.5  64.1
## 10 Belgium      68    79.4
## # ... with 132 more rows
```

--- .class #id

## On Your Own: RStudio Practice #8 

- The following is a new function:
  - Helper function `n()` counts the number of rows in a group
- Then for each year and continent:
  - count total lifeExp
  - Sort in descending order. 

--- .class #id

## On Your Own: RStudio Practice # 8 

- Your answer should look like:


```r
gapminder %>%
    group_by(continent,year) %>%
    summarise(lifeExp_count = n()) %>%
    arrange(desc(lifeExp_count))
```

--- .class #id

## On Your Own: RStudio Practice # 8 

- We could also have used what is called the  `tally()` function:


```r
gapminder %>%
    group_by(country, year) %>%
    tally(sort = TRUE)
```

```
## # A tibble: 1,704 x 3
## # Groups:   country [142]
##    country      year     n
##    <fct>       <int> <int>
##  1 Afghanistan  1952     1
##  2 Afghanistan  1957     1
##  3 Afghanistan  1962     1
##  4 Afghanistan  1967     1
##  5 Afghanistan  1972     1
##  6 Afghanistan  1977     1
##  7 Afghanistan  1982     1
##  8 Afghanistan  1987     1
##  9 Afghanistan  1992     1
## 10 Afghanistan  1997     1
## # ... with 1,694 more rows
```

---  .segue bg:grey


# Adding New Variables


--- .class #id

## Adding New Variables

- There is usually no way around needing a new variable in your data. 
- For example, most medical studies have height and weight in them, however many times what a researcher is interested in using is Body Mass Index (BMI). 
- We would need to add BMI in. 


--- .class #id

## Adding New Variables

- Using the `tidyverse` we can add new variables in multiple ways
  - `mutate()`
  - `transmute()`



--- .class #id


## Differences Between `mutate()` and `transmute()`

- There is only one major difference between `mutate()` and `transmutate` and that is what it keeps in your data. 
  - `mutate()`
      - creates a new variable
      - It keeps all existing variables
  - `transmute()`
      - creates a new variable.
      - It only keeps the new variables


--- .class #id

## Example

- Let's say we wish to have a variable called gdp We want to basically do:

\[\text{gdp} = gdpPercap\times pop\]


- We can first do this with `mutate()`:


```r
gapminder %>% 
  select(country, gdpPercap, pop) %>%
  mutate(gdp = gdpPercap*pop)
```

--- .class #id

## Example: Mutate


```
## # A tibble: 1,704 x 4
##    country     gdpPercap      pop          gdp
##    <fct>           <dbl>    <int>        <dbl>
##  1 Afghanistan      779.  8425333  6567086330.
##  2 Afghanistan      821.  9240934  7585448670.
##  3 Afghanistan      853. 10267083  8758855797.
##  4 Afghanistan      836. 11537966  9648014150.
##  5 Afghanistan      740. 13079460  9678553274.
##  6 Afghanistan      786. 14880372 11697659231.
##  7 Afghanistan      978. 12881816 12598563401.
##  8 Afghanistan      852. 13867957 11820990309.
##  9 Afghanistan      649. 16317921 10595901589.
## 10 Afghanistan      635. 22227415 14121995875.
## # ... with 1,694 more rows
```

--- .class #id

## Example: Transmute



```r
gapminder %>% 
  select(country, gdpPercap, pop) %>%
  transmute(gdp = gdpPercap*pop)
```


--- .class #id

## Example: transmute



```
## # A tibble: 1,704 x 1
##             gdp
##           <dbl>
##  1  6567086330.
##  2  7585448670.
##  3  8758855797.
##  4  9648014150.
##  5  9678553274.
##  6 11697659231.
##  7 12598563401.
##  8 11820990309.
##  9 10595901589.
## 10 14121995875.
## # ... with 1,694 more rows
```

---  .segue bg:grey



# Further Summaries

--- .class #id

## Further Summaries

- We have so far discussed how one could find the basic number summaries:
  - mean
  - median
  - standard deviation
  - variance
  - minimum 
  - maximum
- However there are many more operations that you may wish to do for summarizing data. 
- In fact many of the following examples are excellent choices for working with categorical data which does not always make sense to do the above summaries for. 


--- .class #id

## Further Summaries

- We will consider:
  1. Grouping and Counting
  2. Grouping, Counting and Sorting
  3. Other Groupings
  4. Counting Groups


--- .class #id



## Grouping and Counting


- We have seen the functions `tally()` and `count()`. 
- Both of these can be used for grouping and counting. 
- They also are very concise in how they are called. 


--- .class #id



## Grouping and Counting


- For example if we wished to know how many countrries there were by year, we would use `tally()` in this manner: 


```r
gapminder %>%
  group_by(year) %>% 
  tally()
```

--- .class #id



## Grouping and Counting

- Where as we could do the same thing with `count()`


```r
gapminder %>% 
  count(year)
```

*Notice: `count()` allowed for month to be called inside of it, removing the need for the `group_by()` function. 


--- .class #id



## Grouping, counting and sorting.

- Both `tally()` and `count()` have an argument called `sort()`. 
- This allows you to go one step further and group by, count and sort at the same time. 
- For `tally()` this would be:


```r
gapminder %>% group_by(year) %>% tally(sort=TRUE)
```

--- .class #id

## Grouping, counting and sorting: `tally()`


```
## # A tibble: 12 x 2
##     year     n
##    <int> <int>
##  1  1952   142
##  2  1957   142
##  3  1962   142
##  4  1967   142
##  5  1972   142
##  6  1977   142
##  7  1982   142
##  8  1987   142
##  9  1992   142
## 10  1997   142
## 11  2002   142
## 12  2007   142
```




--- .class #id

## Grouping with other functions

- We can also sum over other values rather than just counting the rows like the above examples. 
- For example let us say we were interested in know the total GDP for countries in a given continent and year.
- We could do this with the `summarise()` function, `tally()` function or the `count()` function:


```r
gapminder %>% 
  group_by(continent, year) %>% 
  summarise(total_gdp = sum(gdp))
```


--- .class #id

## Grouping with other functions



```
## Error in summarise_impl(.data, dots): Evaluation error: object 'gdp' not found.
```


--- .class #id

## What happened?

- Where did gdp go?
- We made it before but we did not save it. 

--- .class #id

## Saving our work


```r
gapminder <- gapminder %>%
    mutate(gdp=gdpPercap*pop)
```

--- .class #id

## Try again:



```
## # A tibble: 5 x 2
##   continent total_gdp
##   <fct>         <dbl>
## 1 Africa      1.30e13
## 2 Americas    1.14e14
## 3 Asia        9.00e13
## 4 Europe      9.70e13
## 5 Oceania     4.52e12
```


--- .class #id

## Grouping with other functions

- For  `tally()` we could do:


```r
gapminder %>% 
  group_by(continent) %>% 
  tally(wt = gdp)
```

*Note: in `tally()` the `wt` stands for weight and allows you to weight the sum based on the gdp*. 

--- .class #id

## Grouping with other functions

- With the `count()` function we also use `wt`:



```r
gapminder %>% count(continent, wt = gdp)
```

```
## # A tibble: 5 x 2
##   continent       n
##   <fct>       <dbl>
## 1 Africa    1.30e13
## 2 Americas  1.14e14
## 3 Asia      9.00e13
## 4 Europe    9.70e13
## 5 Oceania   4.52e12
```

--- .class #id

## Counting Groups


- We may want to know how large our groups are. To do this we can use the following functions:
  - `group_size()` is a function that returns counts of group. 
  - `n_groups()` returns the number of groups

--- .class #id

## Counting Groups

- So if wanted to count the number of countries by continent, we could group by continent and find the groups size using `group_size()`:


```r
gapminder %>% 
  group_by(continent) %>% 
  group_size()
```


--- .class #id

## Counting Groups




```
## [1] 624 300 396 360  24
```


--- .class #id

## Counting Groups


- If we just wished to know how many years were represented in our data we could use the `n_groups()` function:



```r
gapminder %>% 
  group_by(year) %>% 
  n_groups()
```



--- .class #id

## Counting Groups


```
## [1] 12
```

