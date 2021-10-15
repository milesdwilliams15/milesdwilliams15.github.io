---
title: "Explaining Instrumental Variables with `seerrr`"
author: "Miles"
date: "`r Sys.Date()`"
excerpt: "How to use tools in the `seerrr` package to demonstrate the problem of endogeneity and the power of IV."
layout: post
categories:
  - R
  - seerrr
  - Methodology
---

```r
library(seerrr) 
```


```r
sim <- simulate(
  N = 1000,
  U = rnorm(N),
  Z = rnorm(N),
  X = Z + U + rnorm(N),
  Y = X + U + rnorm(N)
)
```


```r
lm_est <- estimate(
  data = sim, Y ~ X, "X", se_type = "stata"
)
iv_est <- estimate(
  data = sim, Y ~ X | Z, "X", se_type = "stata",
  estimator = iv_robust
)
```


```r
bind_rows(
  evaluate(iv_est, truth = 1, what = "bias") %>%
    mutate(estimator = "IV"),
  evaluate(lm_est, truth = 1, what = "bias") %>%
    mutate(estimator = "OLS")
)
```

