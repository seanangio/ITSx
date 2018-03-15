---
title: "Preliminary Analysis Quiz"
author: "Sean Angiolillo"
date: "3/13/2018"
output: 
  html_document: 
    keep_md: yes
---



To continue this week's problem set, please use the West Virginia dataset and the steps outlined in this Unit to:

* Perform a linear regression with a segmented regression specification

* Have R report a summary of the results


```r
library(readr)
dat <- read_csv("antipsychotic_study.csv")
dat$time <- 1:nrow(dat)
dat$level <- c(rep(0,8),rep(1,11))
dat$trend <- c(rep(0,8), 1:11)

model_ols <- lm(marketshare ~ time + level + trend, data=dat)
summary(model_ols)
```

```
## 
## Call:
## lm(formula = marketshare ~ time + level + trend, data = dat)
## 
## Residuals:
##       Min        1Q    Median        3Q       Max 
## -0.012357 -0.004585 -0.001393  0.003707  0.018667 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  0.510321   0.006341  80.476  < 2e-16 ***
## time        -0.001988   0.001256  -1.583   0.1342    
## level       -0.017435   0.007436  -2.345   0.0332 *  
## trend       -0.015448   0.001476 -10.465 2.74e-08 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.008138 on 15 degrees of freedom
## Multiple R-squared:  0.9911,	Adjusted R-squared:  0.9893 
## F-statistic: 557.1 on 3 and 15 DF,  p-value: 1.342e-15
```

```r
confint(model_ols)
```

```
##                    2.5 %        97.5 %
## (Intercept)  0.496805308  0.5238375487
## time        -0.004664686  0.0006884954
## level       -0.033284161 -0.0015855360
## trend       -0.018594621 -0.0123019157
```


## Question 1

With reference to the standard regression for the West Virginia data, what are your estimates of the following variables:

The existing level?


```r
coef(model_ols)[1]
```

```
## (Intercept) 
##   0.5103214
```

 The pre-existing trend per quarter?


```r
coef(model_ols)[2]
```

```
##         time 
## -0.001988095
```
 
The estimated level change following the prior authorization policy?


```r
coef(model_ols)[3]
```

```
##       level 
## -0.01743485
```

The estimated change in the trend per quarter following the policy change?


```r
coef(model_ols)[4]
```

```
##       trend 
## -0.01544827
```


## Question 2

Based on your standard model results, did the policy in West Viginia lead to a:

* Level increase in market share

* Level increase and trend decrease in market share

* Trend decrease in market share

* **Level decrease and trend decrease in market share**

*Explanation*
The results of the model suggest a statistically significant decrease in both the level and trend of market share following the start of the prior authorization policy.
