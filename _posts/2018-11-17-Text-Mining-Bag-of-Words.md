---
layout: post
title: "Text Mining Studying Notes (Bag of Words)"
date: 2018-11-17
output: html_document
share: true
categories: blog
excerpt: "bag of words approach"
tags: [text mining]
---



An template
{% highlight r %}
library(dplyr)

available.packages() %>% 
    tbl_df()
{% endhighlight %}



{% highlight text %}
## # A tibble: 10,276 × 17
##        Package Version Priority                                             Depends
##          <chr>   <chr>    <chr>                                               <chr>
## 1           A3   1.0.0     <NA>                      R (>= 2.15.0), xtable, pbapply
## 2       abbyyR   0.5.0     <NA>                                        R (>= 3.2.0)
## 3          abc     2.1     <NA> R (>= 2.10), abc.data, nnet, quantreg, MASS, locfit
## 4  ABCanalysis   1.2.1     <NA>                                         R (>= 2.10)
## 5     abc.data     1.0     <NA>                                         R (>= 2.10)
## 6     abcdeFBA     0.4     <NA>              Rglpk,rgl,corrplot,lattice,R (>= 2.10)
## 7     ABCoptim  0.14.0     <NA>                                                <NA>
## 8        ABCp2     1.2     <NA>                                                MASS
## 9      ABC.RAP   0.9.0     <NA>                                        R (>= 3.1.0)
## 10       abcrf     1.5     <NA>                                           R(>= 3.1)
##                                   Imports LinkingTo
##                                     <chr>     <chr>
## 1                                    <NA>      <NA>
## 2        httr, XML, curl, readr, progress      <NA>
## 3                                    <NA>      <NA>
## 4                                 plotrix      <NA>
## 5                                    <NA>      <NA>
## 6                                    <NA>      <NA>
## 7                                    Rcpp      Rcpp
## 8                                    <NA>      <NA>
## 9                  graphics, stats, utils      <NA>
## 10 readr, MASS, ranger, parallel, stringr      <NA>
## # ... with 10,266 more rows, and 11 more variables: Suggests <chr>, Enhances <chr>, License <chr>,
## #   License_is_FOSS <chr>, License_restricts_use <chr>, OS_type <chr>, Archs <chr>, MD5sum <chr>,
## #   NeedsCompilation <chr>, File <chr>, Repository <chr>
{% endhighlight %}

![center](/figs/2017-03-20-Package-Search/cran_doge.jpeg)

