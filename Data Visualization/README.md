Data Visualization
================

## GitHub Documents

This is an R Markdown format used for publishing markdown documents to
GitHub. When you click the **Knit** button all R code chunks are run and
a markdown file (.md) suitable for publishing to GitHub is generated.

## 

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
        theme_bw() +
        theme(panel.grid.major = element_line(colour = "#d3d3d3"),
              panel.grid.minor = element_blank(),
              panel.border = element_blank(),
              panel.background = element_blank(),
              plot.title = element_text(size = 14, family = "Tahoma", face = "bold"),
              text=element_text(family = "Tahoma"),
              axis.title = element_text(face="bold"),
              axis.text.x = element_text(colour="black", size = 11),
              axis.text.y = element_text(colour="black", size = 9),
              axis.line = element_line(size=0.5, colour = "black"))
boxplot
```

    ## Warning: Removed 37 rows containing non-finite values (stat_boxplot).

![Boxplot](Data-Visualization_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

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
        geom_density(position="identity", alpha=0.6) +
        scale_x_continuous(name = "Mean ozone in\nparts per billion",
                           breaks = seq(0, 200, 25),
                           limits=c(0, 200)) +
        scale_y_continuous(name = "Density") +
        ggtitle("Density plot of mean ozone") +
        theme_bw() +
        theme(plot.title = element_text(size = 14, family = "Tahoma", face = "bold"),
              text = element_text(size = 12, family = "Tahoma")) +
        scale_fill_brewer(palette="Accent")
density
```

![Density](http://t-redactyl.io/figure/density_17-1.png)<!-- -->
