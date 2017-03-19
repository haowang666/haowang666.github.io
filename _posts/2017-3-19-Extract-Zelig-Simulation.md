I spent so much time today trying to extract Zelig simulation outcomes, online documents are confusing me! [here](http://docs.zeligproject.org/en/latest/getters.html)

The problem is that all the instructions do not specify how to extract results when simulating a sequence of Xs. I figured it out in a cumbersome way:

Using the Cowles data in the effects package as an example
```R
library(effects)
library(Zelig)
library(ZeligChoice)

mydata <- Cowles

set.seed(1)
z.out <- zelig(volunteer ~neuroticism + sex + extraversion, model ="logit", data=mydata, cite = FALSE)
neuro.range <- seq(from = 0, to = 24, by = 1)
x <- setx(z.out, neuroticism = neuro.range)
s.out <- sim(z.out, x = x)

#extract ev
myev <- s.out$get_qi(qi='ev', xvalue = 'range')
#convert the list into matrix
myev2 <- as.data.frame(matrix(unlist(myev), nrow =1000))
#create plot data
a<- apply(myev2, 2, quantile, probs = c(0.025,0.975, 0.25, 0.75)) 
low <- a[1,]
high <- a[2,]
qt.1 <- a[3,]
qt.3 <- a[4,]
mean <- apply(myev2, 2, mean) 
```
