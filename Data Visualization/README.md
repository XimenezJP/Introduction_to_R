Data Visualization
================

### Data Visualization

In this tutorial we will demonstrate some of the many options the
**ggplot2** package has for creating and customising graphs. We will use
R’s airquality dataset in the datasets package.

## Prerequisites

``` r
library(ggplot2)
library(RColorBrewer)
library(tidyverse)
```

    ## ── Attaching packages ──────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ tibble  2.1.3     ✓ dplyr   0.8.3
    ## ✓ tidyr   1.0.0     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.4.0
    ## ✓ purrr   0.3.3

    ## ── Conflicts ─────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(datasets)
library(grid)
```

## Boxplot

``` r
airquality <- as.data.frame(airquality)

airquality$Month <- factor(airquality$Month,
                           labels = c("May", "Jun", "Jul", "Aug", "Sep"))

fill <- "#4271AE"
lines <- "#1F3552"

boxplot <- ggplot(airquality, aes(x = Month, y = Ozone)) +
            geom_boxplot(colour = lines, fill = fill,
                         size = 1) +
            scale_y_continuous(name = "Mean ozone in\nparts per billion",
                                  breaks = seq(0, 175, 25),
                                  limits=c(0, 175)) +
            scale_x_discrete(name = "Month") +
            ggtitle("Boxplot of mean ozone by month") +
            theme_bw()
boxplot
```

    ## Warning: Removed 37 rows containing non-finite values (stat_boxplot).

![](https://raw.githubusercontent.com/XimenezJP/Introduction_to_R/master/Data%20Visualization/boxplot.png)<!-- -->

## Density Plot

``` r
airquality <- as.data.frame(airquality)

airquality_trimmed <- airquality %>% 
                      filter(Month == 5 | 
                             Month == 6 |
                             Month == 7) %>%
                      mutate(Month = recode_factor(Month, 
                               `5` = "May", 
                               `6` = "June",
                               `7` = "July"))

density <- ggplot(airquality_trimmed, aes(x = Ozone, fill = Month)) +
            geom_density(position = "stack", alpha = 0.6) +
            scale_x_continuous(name = "Mean ozone in\nparts per billion",
                               breaks = seq(0, 200, 25),
                               limits=c(0, 200)) +
            scale_y_continuous(name = "Density") +
            ggtitle("Density plot of mean ozone") +
            theme_bw() +
            scale_fill_brewer(palette="Accent")
density
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

## Histogram

``` r
airquality <- as.data.frame(airquality)

barfill <- "#4271AE"
barlines <- "#1F3552"

histogram <- ggplot(airquality, aes(x = Ozone)) +
              geom_histogram(aes(y = ..count..), binwidth = 5,
                             colour = barlines, fill = barfill) +
              scale_x_continuous(name = "Mean ozone in\nparts per billion",
                                 breaks = seq(0, 175, 25),
                                 limits=c(0, 175)) +
              scale_y_continuous(name = "Count") +
              ggtitle("Frequency histogram of mean ozone") +
              theme_bw()
histogram
```

    ## Warning: Removed 37 rows containing non-finite values (stat_bin).

    ## Warning: Removed 2 rows containing missing values (geom_bar).

![](Data-Visualization_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->
