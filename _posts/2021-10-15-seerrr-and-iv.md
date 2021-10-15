---
title: "Explaining Endogeneity and Instrumental Variables with seerrr"
author: "Miles"
date: 2021-10-15
layout: post
categories: ["R", "Methods"]
editor_options: 
  chunk_output_type: inline
output: 
  html_document: 
    df_print: kable
---

Building intuition for what *endogeneity* is and how *instrumental variables* (IV) help us to deal with it is hard. I find that running a simulation helps me to better grasp what the problem is, what it implies, and how IV helps.

To that end, tools in the [`seerrr`](https://github.com/milesdwilliams15/seerrr) package for `R` make devising, implementing, and summarizing such a simulation quite easy. So I'm using this post as an opportunity to do two things: (1) to provide some programmatic intuition for conceptualizing the problem of endogenous variables and (2) to illustrate the convenience of using `seerrr` for this, and by extension, other simulation-based analyses.

## Endogeneity

First, let's address *endogeneity*. What is it? 

This question is best answered by way of an illustration

![why](/assets/images/a-dag.jpg){:class="img-responsive"}

```{R}
library(seerrr) 
```


```{R}
sim <- simulate(
  N = 1000,
  U = rnorm(N),
  Z = rnorm(N),
  X = Z + U + rnorm(N),
  Y = X + U + rnorm(N)
)
```


```{R}
lm_est <- estimate(
  data = sim, Y ~ X, "X", se_type = "stata"
)
iv_est <- estimate(
  data = sim, Y ~ X | Z, "X", se_type = "stata",
  estimator = iv_robust
)
```


```{R}
bind_rows(
  evaluate(iv_est, truth = 1, what = "bias") %>%
    mutate(estimator = "IV"),
  evaluate(lm_est, truth = 1, what = "bias") %>%
    mutate(estimator = "OLS")
)
```


