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

$$\frac{\partial^2 Pr(Y^*)}{\partial X_1 X_2} = Pr(Y^*)(1 - Pr(Y^*)) \beta_{12} + $$
$$Pr(Y^*)(1 - Pr(Y^*))(1-2Pr(Y^*)) (\beta_1 + \beta_{12} X_2 ) (\beta_2 + \beta_{12} X_1)$$

</p>



<p>
 <a href="http://stats.idre.ucla.edu/stata/seminars/deciphering-interactions-in-logistic-regression/">UCLA stats</a> has an excellent application of doing interaction in Stata. 

</p>




</body>
</html>
