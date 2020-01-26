Statistical Analysis
================

## Logistic Regression in R with Healthcare data

We will run a logistic regression to evaluate the effect of calcium and
vitD on the osteoporosis. In this tutorial we will demonstrate some of
the many options the **RNHANES** package. This tutorial was adapted from
**Anisa Dhana’s** tutorial at
    <https://datascienceplus.com/>.

## Prerequisites

``` r
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
library(RNHANES)
library(ggplot2)
library(pROC)
```

    ## Type 'citation("pROC")' for a citation.

    ## 
    ## Attaching package: 'pROC'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     cov, smooth, var

## Prepare the dataset

``` r
d07 = nhanes_load_data("DEMO_E", "2007-2008") %>%
  select(SEQN, cycle, RIAGENDR, RIDAGEYR) %>%
  transmute(SEQN=SEQN, wave=cycle, RIAGENDR, RIDAGEYR) %>% 
  left_join(nhanes_load_data("VID_E", "2007-2008"), by="SEQN") %>%
  select(SEQN, wave, RIAGENDR, RIDAGEYR, LBXVIDMS) %>% 
  transmute(SEQN, wave, RIAGENDR, RIDAGEYR, vitD=LBXVIDMS) %>% 
  left_join(nhanes_load_data("BIOPRO_E", "2007-2008"), by="SEQN") %>%
  select(SEQN, wave, RIAGENDR, RIDAGEYR, vitD, LBXSCA) %>% 
  transmute(SEQN, wave, RIAGENDR, RIDAGEYR, vitD, Calcium = LBXSCA) %>% 
  left_join(nhanes_load_data("OSQ_E", "2007-2008"), by="SEQN") %>%
  select(SEQN, wave, RIAGENDR, RIDAGEYR, vitD, Calcium, OSQ060) %>% 
  transmute(SEQN, wave, RIAGENDR, RIDAGEYR, vitD, Calcium, Osteop = OSQ060)
```

    ## Downloading DEMO_E.XPT to /var/folders/td/3z8r3nmx4nvfrpftx4tft7140000gn/T//RtmpTPDE4g/DEMO_E.XPT

    ## Downloading VID_E.XPT to /var/folders/td/3z8r3nmx4nvfrpftx4tft7140000gn/T//RtmpTPDE4g/VID_E.XPT

    ## Downloading BIOPRO_E.XPT to /var/folders/td/3z8r3nmx4nvfrpftx4tft7140000gn/T//RtmpTPDE4g/BIOPRO_E.XPT

    ## Downloading OSQ_E.XPT to /var/folders/td/3z8r3nmx4nvfrpftx4tft7140000gn/T//RtmpTPDE4g/OSQ_E.XPT

``` r
d09 = nhanes_load_data("DEMO_F", "2009-2010") %>%
  select(SEQN, cycle, RIAGENDR, RIDAGEYR) %>%
  transmute(SEQN=SEQN, wave=cycle, RIAGENDR, RIDAGEYR) %>% 
  left_join(nhanes_load_data("VID_F", "2009-2010"), by="SEQN") %>%
  select(SEQN, wave, RIAGENDR, RIDAGEYR, LBXVIDMS) %>% 
  transmute(SEQN, wave, RIAGENDR, RIDAGEYR, vitD=LBXVIDMS) %>% 
  left_join(nhanes_load_data("BIOPRO_F", "2009-2010"), by="SEQN") %>%
  select(SEQN, wave, RIAGENDR, RIDAGEYR, vitD,  LBXSCA) %>% 
  transmute(SEQN, wave, RIAGENDR, RIDAGEYR, vitD, Calcium = LBXSCA) %>% 
  left_join(nhanes_load_data("OSQ_F", "2009-2010"), by="SEQN") %>%
  select(SEQN, wave, RIAGENDR, RIDAGEYR, vitD, Calcium, OSQ060) %>% 
  transmute(SEQN, wave, RIAGENDR, RIDAGEYR, vitD, Calcium, Osteop = OSQ060)
```

    ## Downloading DEMO_F.XPT to /var/folders/td/3z8r3nmx4nvfrpftx4tft7140000gn/T//RtmpTPDE4g/DEMO_F.XPT

    ## Downloading VID_F.XPT to /var/folders/td/3z8r3nmx4nvfrpftx4tft7140000gn/T//RtmpTPDE4g/VID_F.XPT

    ## Downloading BIOPRO_F.XPT to /var/folders/td/3z8r3nmx4nvfrpftx4tft7140000gn/T//RtmpTPDE4g/BIOPRO_F.XPT

    ## Downloading OSQ_F.XPT to /var/folders/td/3z8r3nmx4nvfrpftx4tft7140000gn/T//RtmpTPDE4g/OSQ_F.XPT

``` r
dat = bind_rows(d07, d09) %>% as.data.frame()
```

### Create categories of Vitamin D

``` r
dat1 = dat %>% 
  mutate(
    vitD_group = case_when(
      vitD < 30 ~ "Deficiency",
      vitD >= 30 & vitD < 50 ~ "Inadequacy",
      vitD >= 50 & vitD <= 125 ~ "Sufficiency"))
```

### Exclude missings

``` r
dat2 = dat1 %>% 
  filter(!is.na(vitD_group), !is.na(Calcium), !is.na(Osteop), Osteop!=9) %>% 
  mutate(Gender = recode_factor(RIAGENDR, 
                           `1` = "Men", 
                           `2` = "Women"),
         Osteop = recode_factor(Osteop, 
                           `1` = 1, 
                           `2` = 0))

head(dat2)
```

    ##    SEQN      wave RIAGENDR RIDAGEYR vitD Calcium Osteop  vitD_group Gender
    ## 1 41475 2007-2008        2       62 58.8     9.5      0 Sufficiency  Women
    ## 2 41477 2007-2008        1       71 81.8    10.0      0 Sufficiency    Men
    ## 3 41479 2007-2008        1       52 78.4     9.0      0 Sufficiency    Men
    ## 4 41482 2007-2008        1       64 61.9     9.1      0 Sufficiency    Men
    ## 5 41483 2007-2008        1       66 53.3     8.9      0 Sufficiency    Men
    ## 6 41485 2007-2008        2       30 39.1     9.3      0  Inadequacy  Women

## Logit regression model

I will use the **glm()** function to run the logistic regression and
then summary() command to get the results.

``` r
fit <- glm(Osteop ~ vitD_group + Calcium + Gender + RIDAGEYR, 
           data = dat2, 
           family = "binomial")
summary(fit)
```

    ## 
    ## Call:
    ## glm(formula = Osteop ~ vitD_group + Calcium + Gender + RIDAGEYR, 
    ##     family = "binomial", data = dat2)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -3.4265   0.1009   0.1894   0.3315   1.0305  
    ## 
    ## Coefficients:
    ##                       Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)            7.81969    1.08054   7.237 4.59e-13 ***
    ## vitD_groupInadequacy  -0.17444    0.20124  -0.867  0.38603    
    ## vitD_groupSufficiency -0.53068    0.18159  -2.922  0.00347 ** 
    ## Calcium                0.10330    0.11404   0.906  0.36506    
    ## GenderWomen           -2.08873    0.12298 -16.984  < 2e-16 ***
    ## RIDAGEYR              -0.07127    0.00330 -21.599  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 4591.4  on 10064  degrees of freedom
    ## Residual deviance: 3553.1  on 10059  degrees of freedom
    ## AIC: 3565.1
    ## 
    ## Number of Fisher Scoring iterations: 7

``` r
round(exp(coef(fit)), 2)
```

    ##           (Intercept)  vitD_groupInadequacy vitD_groupSufficiency 
    ##               2489.14                  0.84                  0.59 
    ##               Calcium           GenderWomen              RIDAGEYR 
    ##                  1.11                  0.12                  0.93
