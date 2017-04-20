---
layout: post
title: "Interpret Poisson Regression Coefficient"
date: 2017-04-19
output: html_document
share: true
categories: blog
excerpt: "interpret poisson regression"
tags: [rstats]
---


Interpret GLM coefficients can be tricky, especially if you want to do that in a similar like OLS. I personally prefer calculating the expected outcomes and plot the results, but if you're really into that **odds ratio** stuffs. Here's someway to do that.

In poisson regression, the regression coefficients are  interpreted as the difference between the log of expected counts, where formally, this can be written as 

$$\beta = log(\mu_{x+1}) - log(\mu_x)$$

Let do the exponetial transformation:

$$exp(\beta) = exp[log(\mu_{x+1}) - log(\mu_x)]$$
$$exp(\beta) = \frac{exp(log(\mu_{x+1}))}{exp(log(\mu_x))}$$
$$exp(\beta) = \frac{\mu_{x+1}}{\mu_{x}}$$
$$exp(\beta) - 1  = \frac{\mu_{x+1}}{\mu_{x}} - 1$$
$$exp(\beta) - 1  = \frac{\mu_{x+1} - \mu_{x}}{\mu_{x}}$$

The last equation can be interpreted as the precentage increase of the count number. 


To give you a working example, let first run a poisson regression on an abtritary R dataset. I use [Zelig](http://docs.zeligproject.org/en/latest/zelig-poisson.html) here, results are the same for glm function.

```R
library(Zelig)
#1000 random poisson numbers, lambda = 0.1
y <- rpois(1000, 0.1)
#1000 random standard normal numbers
x <- rnorm(1000, 0, 1)
#combine them
mydata <- as.data.frame(cbind(x, y))
z.out <- zelig(y ~ x, model = "poisson", data = mydata)
summary(z.out)
```

The out puts are
```R
Coefficients:
            Estimate Std. Error z value Pr(>|z|)
(Intercept) -2.44355    0.10740 -22.751   <2e-16
x            0.07271    0.10871   0.669    0.504

(Dispersion parameter for poisson family taken to be 1)

    Null deviance: 433.20  on 999  degrees of freedom
Residual deviance: 432.75  on 998  degrees of freedom
AIC: 606.59

```

We can then use the function `exp(0.07271) - 1` to calculate the percentage changes. The results are

```R
exp(0.07271) - 1
[1] 0.07541862
```
Basically means one unit increase of x increase the number of y by 7 percent.


