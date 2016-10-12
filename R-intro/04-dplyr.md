---
layout: topic
title: Aggregating and analyzing data with dplyr
author: Data Carpentry contributors
---



------------

# Data manipulation using `dplyr`

Bracket subsetting is handy, but it can be cumbersome and difficult to read,
especially for complicated operations. Enter `dplyr`. `dplyr` is a package for
making data manipulation easier.

Packages in R are basically sets of additional functions that let you do more
stuff in R. The functions we've been using, like `str()`, come built into R;
packages give you access to more functions. You need to install a package and
then load it to be able to use it.


```r
install.packages("dplyr") ## install
```

You might get asked to choose a **CRAN mirror** -- this is basically
asking you to choose a site to download the package from. The choice
doesn't matter too much. A local CRAN mirror is a good choice.



```r
library("dplyr")          ## load
```

You only need to install a package once per computer, but you need to load it
every time you open a new R session and want to use that package.

## What is `dplyr`?

> The package `dplyr` provides **easy tools for the most common data
> manipulation tasks**. It is built to work directly with **tidy**
> data frames: `dplyr`'s function take tidy data, transform it, and
> return a new tidy data.

The thinking behind it was largely inspired by the package `plyr`
which has been in use for some time but suffered from being slow in
some cases.` dplyr` addresses this by porting much of the computation
to C++. An additional feature is the ability to work with data stored
directly in an external database. The benefits of doing this are that
the data can be managed natively in a relational database, queries can
be conducted on that database, and only the results of the query
returned.

This addresses a common problem with R in that all operations are conducted in
memory and thus the amount of data you can work with is limited by available
memory. The database connections essentially remove that limitation in that you
can have a database of many 100s GB, conduct queries on it directly and pull
back just what you need for analysis in R.

> We're going to learn some of the most common `dplyr` functions:
> `select()`, `filter()`, `mutate()`, `group_by()`, and `summarize()`
> (or `summarise()`).

### Selecting columns and filtering rows

To select columns of a data frame, use `select()`. The first argument
to this function is the data frame (`surveys`), and the subsequent
arguments are the columns to keep.


```r
select(surveys, plot_id, species_id, weight)
```

```
## Error in select_(.data, .dots = lazyeval::lazy_dots(...)): object 'surveys' not found
```

To choose rows, use `filter()`:


```r
filter(surveys, year == 1995)
```

```
## Error in filter_(.data, .dots = lazyeval::lazy_dots(...)): object 'surveys' not found
```

### Pipes

**But what if you wanted to select and filter?** There are three ways to do this:

- Use intermediate steps, nested functions, or pipes. With the
  intermediate steps, you essentially create a temporary data frame
  and use that as input to the next function. This can clutter up your
  workspace with lots of objects.

- You can also nest functions (i.e. one function inside of
  another). This is handy, but can be difficult to read if too many
  functions are nested as the process from inside out.

- The last option, pipes, are a fairly recent addition to R.

Pipes let you take the output of one function and send it directly to
the next, which is useful when you need to many things to the same
data set.  Pipes in R look like `%>%` and are made available via the
`magrittr` package installed as part of `dplyr`.


```r
surveys %>%
  filter(weight < 5) %>%
  select(species_id, sex, weight)
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

In the above we use the pipe to send the `surveys` data set first
through `filter`, to keep rows where `weight` was less than 5, and
then through `select` to keep the `species`, `sex` and `weight`'
columns. When the data frame is being passed to the `filter()` and
`select()` functions through a pipe, we don't need to include it as an
argument to these functions anymore.

If we wanted to create a new object with this smaller version of the
data we could do so by assigning it a new name:


```r
surveys_sml <- surveys %>%
  filter(weight < 5) %>%
  select(species_id, sex, weight)
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

```r
surveys_sml
```

```
## Error in eval(expr, envir, enclos): object 'surveys_sml' not found
```

> ### Challenge {.challenge}
>
> Using pipes, subset the data to include rows before 1995. Retain columns
> `year`, `sex`, and `weight.`



```r
surveys %>% filter(year < 1995) %>% select(year, sex, weight)
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```


### Mutate

Frequently you'll want to create new columns based on the values in existing
columns, for example to do unit conversions or find the ratio of values in two
columns. For this we'll use `mutate()`.

To create a new column of weight in kg:


```r
surveys %>%
  mutate(weight_kg = weight / 1000)
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

If this runs off your screen and you just want to see the first few rows, you
can use a pipe to view the `head()` of the data (pipes work with non-dplyr
functions too, as long as the `dplyr` or `magrittr` packages are loaded).


```r
surveys %>%
  mutate(weight_kg = weight / 1000) %>%
  head
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

The first few rows are full of NAs, so if we wanted to remove those we could
insert a `filter()` in this chain:


```r
surveys %>%
  mutate(weight_kg = weight / 1000) %>%
  filter(!is.na(weight)) %>%
  head
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

`is.na()` is a function that determines whether something is or is not an `NA`.
The `!` symbol negates it, so we're asking for everything that is not an `NA`.

### Split-apply-combine data analysis and the summarize() function

Many data analysis tasks can be approached using the "split-apply-combine"
paradigm:

1. split the data into groups
2. apply some analysis to each group, and
3. then combine the results.

`dplyr` makes this very easy through the use of the `group_by()`
function. `group_by()` splits the data into groups upon which some
operations can be run. For example, if we wanted to group by sex and
find the number of rows of data for each sex, we would do:


```r
surveys %>%
  group_by(sex) %>%
  tally()
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

`group_by()` is often used together with `summarize()` which collapses each
group into a single-row summary of that group. So to view mean `weight` by sex:


```r
surveys %>%
  group_by(sex) %>%
  summarize(mean_weight = mean(weight, na.rm = TRUE))
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

You can group by multiple columns too:


```r
surveys %>%
  group_by(sex, species_id) %>%
  summarize(mean_weight = mean(weight, na.rm = TRUE))
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

Looks like most of these species were never weighed. We could then discard rows
where `mean_weight` is `NaN` (`NaN` refers to "Not a Number") using `filter()`:


```r
surveys %>%
  group_by(sex, species_id) %>%
  summarize(mean_weight = mean(weight, na.rm = TRUE)) %>%
  filter(!is.nan(mean_weight))
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

All of a sudden this isn't running of the screen anymore. That's because `dplyr`
has changed our `data.frame` to a `tbl_df`. This is a data structure that's very
similar to a data frame; for our purposes the only difference is that it won't
automatically show tons of data going off the screen.

You can also summarize multiple variables at the same time:


```r
surveys %>%
  group_by(sex, species_id) %>%
  summarize(mean_weight = mean(weight, na.rm = TRUE),
            min_weight = min(weight, na.rm = TRUE)) %>%
  filter(!is.nan(mean_weight))
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

> ### Challenge {.challenge}
>
> How many times was each `plot_type` surveyed?



```r
surveys %>% group_by(plot_type) %>% tally()
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

```r
table(surveys$plot_type)
```

```
## Error in table(surveys$plot_type): object 'surveys' not found
```

> ### Challenge {.challenge}
>
> Use `group_by()` and `summarize()` to find the mean, min, and max hindfoot
> length for each species.


```r
surveys %>% 
    filter(!is.na(hindfoot_length)) %>% 
    group_by(species) %>% 
    summarise(minhf = min(hindfoot_length), 
              maxhf = max(hindfoot_length), 
              avghf = mean(hindfoot_length)) 
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

```r
surveys %>% 
    group_by(species) %>% 
    summarise(minhf = min(hindfoot_length, na.rm = TRUE), 
              maxhf = max(hindfoot_length, na.rm = TRUE), 
              avghf = mean(hindfoot_length, na.rm = TRUE)) 
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

> ### Challenge {.challenge}
>
> What was the heaviest animal measured in each year? Return the columns `year`,
> `genus`, `species`, and `weight`. See also the cheatsheet below for help.


```r
surveys %>% group_by(year) %>% top_n(n = 1, wt = weight)
```

```
## Error in eval(expr, envir, enclos): object 'surveys' not found
```

### Summary

* `select`: select columns
* `filter`: select rows
* `mutate`: create new columns
* `group_by`: split-apply-combine
* `summarise`: collapse each group into a single-row summary of that
  group
* `%>%`: to pipe (chain) operations

[Handy dplyr cheatsheet](http://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)

*Much of this lesson was copied or adapted from Jeff Hollister's [materials](http://usepa.github.io/introR/2015/01/14/03-Clean/)*