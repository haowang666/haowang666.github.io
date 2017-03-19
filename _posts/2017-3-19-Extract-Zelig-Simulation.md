I spent so much time today trying to extract Zelig simulation outcomes, online documents are confusing me! [here](http://docs.zeligproject.org/en/latest/getters.html)

The problem is that all the instructions do not specify how to extract results when simulating a sequence of Xs. I figured it out in a cumbersome way:

Using the Cowles data in the effects package as an example
```R
library(effects)
library(Zelig)
library(ZeligChoice)

mydata <- Cowles


set.seed(1)
z.out <- zelig(volunteer ~neuroticism + sex + extraversion, 
model ="logit", data=mydata, cite = FALSE)
neuro.range <- seq(from = 0, to = 24, by = 1)
x <- setx(z.out, neuroticism = neuro.range)
s.out <- sim(z.out, x = x)

#extract ev
myev <- s.out$get_qi(qi='ev', xvalue = 'range')
#convert the list into matrix
myev2 <- as.data.frame(matrix(unlist(myev), nrow =1000))
#create plot data
#This step is to create quantiles
a<- apply(myev2, 2, quantile, probs = c(0.025,0.975, 0.25, 0.75)) 
low <- a[1,]
high <- a[2,]
qt.1 <- a[3,]
qt.3 <- a[4,]
mean <- apply(myev2, 2, mean) 
plotdata <- as.data.frame(cbind(low, high, mean, qt.1, qt.3))
plotdata$neuroticism <- seq(from = 0, to = 24, by =1)

#plot in ggplot2
ggplot(data=plotdata, aes(x = neuroticism))+ 
  geom_line(aes(y =mean)) + 
  geom_line(aes(y =high), linetype="dashed", color="blue") + 
  geom_line(aes(y =low), linetype="dashed", color="blue") + 
  geom_line(aes(y =qt.1), linetype="dashed", color="blue") + 
  geom_line(aes(y =qt.3), linetype="dashed", color="blue") + 
  xlab("Level of Neuroticism (95% CI)") + 
  ylab("Expected Values E(Y|X)") + 
  ggtitle("Marginal Effect of Neuroticism")+ 
  geom_ribbon(aes(ymin=low, ymax=high), alpha=0.1)+
  geom_ribbon(aes(ymin=qt.1, ymax=qt.3), alpha=0.6)
```

There're a few places which needs extra attention: first, in the line 
```R
myev <- s.out$get_qi(qi='ev', xvalue = 'range')
```
xvalue needs to be set as 'range'. Second, since Zelig is doing 1000 simulations at each level of X, the next step is to convert the list of 25 values of X and convert it into
a matrix. I spent several hours reading the outputs and was driven crazy by the numerous lists of sim.out. That's why I don't like 'canned' functions!

Certainly, if you are not bothered with the 'cans', you can use the default function built in the package
```R
plot(s.out)
```

