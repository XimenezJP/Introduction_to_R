Introduction to R
================

## R Programming

R is an open source programming language and software environment for
statistical computing and graphics that is supported by the R Foundation
for Statistical Computing. Today, it is one of the most popular
languages, being used all across the world in a wide variety of domains
and fields.

RStudio is an integrated development environment (IDE) for R. It
includes a console, syntax-highlighting editor that supports direct code
execution, as well as tools for plotting, history, debugging and
workspace
    management.

## Working directory

``` r
getwd()
```

    ## [1] "/Users/joaopaulo/Dropbox/João Paulo/R script/Introduction_to_R"

## Calculating things in R

``` r
3+3
```

    ## [1] 6

``` r
1/100
```

    ## [1] 0.01

``` r
sqrt(4)
```

    ## [1] 2

## Variable and Data Types

There are several different types of data you can use in R. We’ll
examine a few common ones in a little more detail.

### Text

Strings are known as “character” in R. Use the double quotes `"` or
single quotes `'` to wrap around the string

``` r
myname <- "João"
```

We can use the `class()` function to see what data type it is

``` r
class(myname)
```

    ## [1] "character"

### Numbers

Numbers have different classes. The most common two are `integer` and
`numeric`. Integers are whole numbers:

``` r
favourite.integer <- as.integer(8)
print(favourite.integer)
```

    ## [1] 8

``` r
class(favourite.integer)
```

    ## [1] "integer"

Numbers can be `numeric` which are decimals:

``` r
favourite.numeric <- as.numeric(8.8)
print(favourite.numeric)
```

    ## [1] 8.8

``` r
class(favourite.numeric)
```

    ## [1] "numeric"

### Logical (True/False)

We use the `==` to test for equality in R

``` r
class(TRUE)
```

    ## [1] "logical"

``` r
favourite.numeric == 8.8
```

    ## [1] TRUE

``` r
favourite.numeric == 9.9
```

    ## [1] FALSE

### Factors

In order to categorise the categorical variables and store it on
multiple levels, we use the data object called R factor.

``` r
directions <- as.factor(c("North", "North", "West", "South"))
class(directions)
```

    ## [1] "factor"

How many categories are there for disorders and what are they?

``` r
levels(directions)
```

    ## [1] "North" "South" "West"

``` r
nlevels(directions)
```

    ## [1] 3

A factor can be ordered. This makes sense in the context of a ranking
such as a survey response, e.g. from ‘Strongly agree’ to ‘Strong
disagree’.

``` r
myorderedfactor <- factor(directions, levels = c("South", "West", "North"), ordered = TRUE)

levels(myorderedfactor)
```

    ## [1] "South" "West"  "North"

### Vectors

We can create 1D data structures called “vectors”.

``` r
1:10
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10

``` r
2*(1:10)
```

    ##  [1]  2  4  6  8 10 12 14 16 18 20

``` r
seq(0, 10, 2)
```

    ## [1]  0  2  4  6  8 10

``` r
directions <- c("North", "North", "West", "South")
class(directions)
```

    ## [1] "character"

### Matriz

A matrix in R is a two-dimensional rectangular data set and thus it can
be created using vector input to the matrix function

``` r
mat <- matrix (
c(2, 4, 3, 1, 5, 7),        # the data elements
nrow = 2,                   # no. of rows
ncol = 3,                   # no. of columns
byrow = TRUE)               # arranging it by row

print(mat)
```

    ##      [,1] [,2] [,3]
    ## [1,]    2    4    3
    ## [2,]    1    5    7

How to Access Elements of Matrix in R?

``` r
mat[2, 3]
```

    ## [1] 7

``` r
mat[2, ]
```

    ## [1] 1 5 7

``` r
mat[ ,3]
```

    ## [1] 3 7

### Dataframe

A data frame is being used for storing data tables, the vectors that are
contained in the form of a list in a data frame are of equal length.

#### Data Transformation

``` r
#install.packages("tidyverse")
#install.packages("nycflights13")

library(tidyverse)
```

    ## ── Attaching packages ──────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.2.1     ✓ purrr   0.3.3
    ## ✓ tibble  2.1.3     ✓ dplyr   0.8.3
    ## ✓ tidyr   1.0.0     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.4.0

    ## ── Conflicts ─────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(nycflights13)

summary(flights)
```

    ##       year          month             day           dep_time    sched_dep_time
    ##  Min.   :2013   Min.   : 1.000   Min.   : 1.00   Min.   :   1   Min.   : 106  
    ##  1st Qu.:2013   1st Qu.: 4.000   1st Qu.: 8.00   1st Qu.: 907   1st Qu.: 906  
    ##  Median :2013   Median : 7.000   Median :16.00   Median :1401   Median :1359  
    ##  Mean   :2013   Mean   : 6.549   Mean   :15.71   Mean   :1349   Mean   :1344  
    ##  3rd Qu.:2013   3rd Qu.:10.000   3rd Qu.:23.00   3rd Qu.:1744   3rd Qu.:1729  
    ##  Max.   :2013   Max.   :12.000   Max.   :31.00   Max.   :2400   Max.   :2359  
    ##                                                  NA's   :8255                 
    ##    dep_delay          arr_time    sched_arr_time   arr_delay       
    ##  Min.   : -43.00   Min.   :   1   Min.   :   1   Min.   : -86.000  
    ##  1st Qu.:  -5.00   1st Qu.:1104   1st Qu.:1124   1st Qu.: -17.000  
    ##  Median :  -2.00   Median :1535   Median :1556   Median :  -5.000  
    ##  Mean   :  12.64   Mean   :1502   Mean   :1536   Mean   :   6.895  
    ##  3rd Qu.:  11.00   3rd Qu.:1940   3rd Qu.:1945   3rd Qu.:  14.000  
    ##  Max.   :1301.00   Max.   :2400   Max.   :2359   Max.   :1272.000  
    ##  NA's   :8255      NA's   :8713                  NA's   :9430      
    ##    carrier              flight       tailnum             origin         
    ##  Length:336776      Min.   :   1   Length:336776      Length:336776     
    ##  Class :character   1st Qu.: 553   Class :character   Class :character  
    ##  Mode  :character   Median :1496   Mode  :character   Mode  :character  
    ##                     Mean   :1972                                        
    ##                     3rd Qu.:3465                                        
    ##                     Max.   :8500                                        
    ##                                                                         
    ##      dest              air_time        distance         hour      
    ##  Length:336776      Min.   : 20.0   Min.   :  17   Min.   : 1.00  
    ##  Class :character   1st Qu.: 82.0   1st Qu.: 502   1st Qu.: 9.00  
    ##  Mode  :character   Median :129.0   Median : 872   Median :13.00  
    ##                     Mean   :150.7   Mean   :1040   Mean   :13.18  
    ##                     3rd Qu.:192.0   3rd Qu.:1389   3rd Qu.:17.00  
    ##                     Max.   :695.0   Max.   :4983   Max.   :23.00  
    ##                     NA's   :9430                                  
    ##      minute        time_hour                  
    ##  Min.   : 0.00   Min.   :2013-01-01 05:00:00  
    ##  1st Qu.: 8.00   1st Qu.:2013-04-04 13:00:00  
    ##  Median :29.00   Median :2013-07-03 10:00:00  
    ##  Mean   :26.23   Mean   :2013-07-03 05:22:54  
    ##  3rd Qu.:44.00   3rd Qu.:2013-10-01 07:00:00  
    ##  Max.   :59.00   Max.   :2013-12-31 23:00:00  
    ## 

``` r
filter(flights, month == 1, day == 1)
```

    ## # A tibble: 842 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 832 more rows, and 11 more variables: arr_delay <dbl>, carrier <chr>,
    ## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
    ## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
select(flights, year:day)
```

    ## # A tibble: 336,776 x 3
    ##     year month   day
    ##    <int> <int> <int>
    ##  1  2013     1     1
    ##  2  2013     1     1
    ##  3  2013     1     1
    ##  4  2013     1     1
    ##  5  2013     1     1
    ##  6  2013     1     1
    ##  7  2013     1     1
    ##  8  2013     1     1
    ##  9  2013     1     1
    ## 10  2013     1     1
    ## # … with 336,766 more rows

``` r
select(flights, -c(year:day))
```

    ## # A tibble: 336,776 x 16
    ##    dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier
    ##       <int>          <int>     <dbl>    <int>          <int>     <dbl> <chr>  
    ##  1      517            515         2      830            819        11 UA     
    ##  2      533            529         4      850            830        20 UA     
    ##  3      542            540         2      923            850        33 AA     
    ##  4      544            545        -1     1004           1022       -18 B6     
    ##  5      554            600        -6      812            837       -25 DL     
    ##  6      554            558        -4      740            728        12 UA     
    ##  7      555            600        -5      913            854        19 B6     
    ##  8      557            600        -3      709            723       -14 EV     
    ##  9      557            600        -3      838            846        -8 B6     
    ## 10      558            600        -2      753            745         8 AA     
    ## # … with 336,766 more rows, and 9 more variables: flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

``` r
rename(flights, tail_num = tailnum)
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tail_num <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
flights_sml <- select(flights, 
  year:day, 
  ends_with("delay"), 
  distance, 
  air_time
)

mutate(flights_sml,
  gain = dep_delay - arr_delay,
  speed = distance / air_time * 60
)
```

    ## # A tibble: 336,776 x 9
    ##     year month   day dep_delay arr_delay distance air_time  gain speed
    ##    <int> <int> <int>     <dbl>     <dbl>    <dbl>    <dbl> <dbl> <dbl>
    ##  1  2013     1     1         2        11     1400      227    -9  370.
    ##  2  2013     1     1         4        20     1416      227   -16  374.
    ##  3  2013     1     1         2        33     1089      160   -31  408.
    ##  4  2013     1     1        -1       -18     1576      183    17  517.
    ##  5  2013     1     1        -6       -25      762      116    19  394.
    ##  6  2013     1     1        -4        12      719      150   -16  288.
    ##  7  2013     1     1        -5        19     1065      158   -24  404.
    ##  8  2013     1     1        -3       -14      229       53    11  259.
    ##  9  2013     1     1        -3        -8      944      140     5  405.
    ## 10  2013     1     1        -2         8      733      138   -10  319.
    ## # … with 336,766 more rows

``` r
transmute(flights,
  gain = dep_delay - arr_delay,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```

    ## # A tibble: 336,776 x 3
    ##     gain hours gain_per_hour
    ##    <dbl> <dbl>         <dbl>
    ##  1    -9 3.78          -2.38
    ##  2   -16 3.78          -4.23
    ##  3   -31 2.67         -11.6 
    ##  4    17 3.05           5.57
    ##  5    19 1.93           9.83
    ##  6   -16 2.5           -6.4 
    ##  7   -24 2.63          -9.11
    ##  8    11 0.883         12.5 
    ##  9     5 2.33           2.14
    ## 10   -10 2.3           -4.35
    ## # … with 336,766 more rows

``` r
delays <- flights %>% 
  group_by(dest) %>% 
  summarise(
    count = n(),
    dist = mean(distance, na.rm = TRUE),
    delay = mean(arr_delay, na.rm = TRUE)
  ) %>% 
  filter(count > 20, dest != "HNL")
delays
```

    ## # A tibble: 96 x 4
    ##    dest  count  dist delay
    ##    <chr> <int> <dbl> <dbl>
    ##  1 ABQ     254 1826   4.38
    ##  2 ACK     265  199   4.85
    ##  3 ALB     439  143  14.4 
    ##  4 ATL   17215  757. 11.3 
    ##  5 AUS    2439 1514.  6.02
    ##  6 AVL     275  584.  8.00
    ##  7 BDL     443  116   7.05
    ##  8 BGR     375  378   8.03
    ##  9 BHM     297  866. 16.9 
    ## 10 BNA    6333  758. 11.8 
    ## # … with 86 more rows
