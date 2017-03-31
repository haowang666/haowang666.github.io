---
layout: post
title: "Interaction in Logistic Regression"
date: 2017-03-26
output: html_document
share: true
categories: blog
excerpt: "deal with product term in logistic regression"
tags: [rstats]
---


<html>
<head>
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_CHTML">
</script>
</head>

<body>

<p>
Recently I come across the question of the product term in GLM:
$$Pr(Y) = logit(Y^*) = \frac{1}{1+e^{-Y^*}}$$
The question is pretty straightforward in linear regression: 
$$Y = \beta_0  + \beta_{1}x_1 + \beta_{2}x_2 + \beta_{12}x_{1}x_{2}$$

To get the marginal effect of $x_1$, simply take its partial derivative:

$$\frac{\partial Y}{\partial x_1} = \beta_{1} + \beta_{12}x_{2}$$

This funtion is a linear expression of $x_{2}$. Which can demostrate the favoriate social science proposal of 'conditional effect'.
</p>

<p>
However, things become tricky in logistic regression. Does the product term $x_{1}x_{2}$ has the same interaction meaning here? Not Really. In logistic regression, our desired quantity is the expected probability, which is a log transformation of the latent linear predictor $Y^*$. So what is the problem? Because the log transformation has a S curve, this makes the partial derivetive of $x_{1}$ conditioning on the location of all the other variables. To do this formally, we can evaluate our product term:

$$\frac{\partial^2 Pr(Y)}{\partial X_1 X_2} = Pr(Y)(1 - Pr(Y)) \beta_{12} + $$
$$Pr(Y)(1 - Pr(Y))(1-2Pr(Y)) (\beta_1 + \beta_{12} X_2 ) (\beta_2 + \beta_{12} X_1)$$

This says the interaction effect is actually conditioned on both $x_1$ and $x_2$. <a href="http://onlinelibrary.wiley.com/doi/10.1111/j.1540-5907.2009.00429.x/abstract"> Berry et al. (2010) </a> point out if our theoretical expectation of interaction is actually addressed by the functional feature of logistic regression (they call it 'compression'), then there's no need for a product term.  

</p>

<p>
But the problem is that, without a product term, the 'interaction effect' is actually assumed by nature due to the log transformation. 
<a href="https://www.cambridge.org/core/journals/political-science-research-and-methods/article/div-classtitlecompression-and-conditional-effects-a-product-term-is-essential-when-using-logistic-regression-to-test-for-interactiona-hreffn1-ref-typefnadiv/98664E6FF9AA240CCDBCFFC4B29EA7F8">Carlisle Rainey</a> highlights this problem, arguing that adding a product term allows for the model flexibility. It actually does a better job modeling the conditions without interaction effects. 
</p>

<p>
I think Rainey's point is valid, especially if you are not expecting an interaction effect of your variables. But here are some warning points:
</p>

<ul>
<li> If the quantity of interest is the unbounded latent $y^*$, product term has the same interpretation in linear models</li>
<li> In the logistic regression, a significant product term does not indicating an interaction effect on $Pr(Y)$, nor does it refuse an interaction effect</li>
<li> Interpreting odds ratio is hard, plot it! Plot at different levels of moderators if you believe (or not) an interaction effect!</li>


</ul>


</body>
</html>
