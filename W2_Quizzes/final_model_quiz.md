---
title: "Final Model Quiz"
author: "Sean Angiolillo"
date: "3/14/2018"
output: 
  html_document: 
    keep_md: yes
---



Thinking back to the last exercise, run the model which you determined was the best to deal with any autocorrelation issues in the West Virginia Dataset.

After running that model, perform checks to assess the following comparisons and report the p-values for the likelihood ratio tests.


```r
# Load the necessary libraries
library(nlme)
library(car)
library(readr)

# Read in the dataset
dat <- read_csv("antipsychotic_study.csv")

# add time, level and trend variables
dat$time <- 1:nrow(dat)
dat$level <- c(rep(0,8),rep(1,11))
dat$trend <- c(rep(0,8), 1:11)

# Fit the GLS regression model
model_final <- gls(marketshare ~ time + level + trend,
  data = dat,
  correlation = NULL,
  method = "ML")

summary(model_final)
```

```
## Generalized least squares fit by maximum likelihood
##   Model: marketshare ~ time + level + trend 
##   Data: dat 
##         AIC       BIC   logLik
##   -123.3965 -118.6743 66.69826
## 
## Coefficients:
##                  Value   Std.Error   t-value p-value
## (Intercept)  0.5103214 0.006341281  80.47608  0.0000
## time        -0.0019881 0.001255761  -1.58318  0.1342
## level       -0.0174348 0.007435931  -2.34468  0.0332
## trend       -0.0154483 0.001476156 -10.46520  0.0000
## 
##  Correlation: 
##       (Intr) time   level 
## time  -0.891              
## level  0.351 -0.591       
## trend  0.758 -0.851  0.174
## 
## Standardized residuals:
##        Min         Q1        Med         Q3        Max 
## -1.7089041 -0.6340067 -0.1926221  0.5126114  2.5814660 
## 
## Residual standard error: 0.007231033 
## Degrees of freedom: 19 total; 15 residual
```

## Question 1

Compare the final model based on the ACF and PACF plots to one with 1 additional AR() term. What is the p-value?


```r
# Compare to 1 additional AR term
model_p1 <- update(model_final, correlation = corARMA(p = 1,form = ~time))
anova(model_final, model_p1)$p
```

```
## [1]        NA 0.9915365
```

Perform the same comparison to a model with 1 additional MA() term. What is the p-value for this comparison?


```r
# And now to 1 additional MA term
model_q1 <- update(model_final, correlation = corARMA(q = 1,form = ~time))
anova(model_final,model_q1)$p
```

```
## [1]         NA 0.01239086
```

