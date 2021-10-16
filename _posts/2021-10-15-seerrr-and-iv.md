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

<div class="topnav">
    <a class="active" href="https://milesdwilliams15.github.io/"><strong>Home</strong></a>
    <a href="https://github.com/milesdwilliams15/job-market-materials/raw/main/cv.pdf"><strong>CV</strong></a>
    <a href="https://milesdwilliams15.github.io/blog/"><strong>Blog</strong></a>
    <a href="{{ site.github.owner_url }}"><strong>GitHub</strong></a>
    <a href = "{{ site.data.social-media.email.href }}{{ site.data.social-media.email.id }}" title="Email me"><strong>Email</strong></a>
    <div class="dropdown">
        <button class="dropbtn"><strong>About</strong> <i class="fa fa-caret-down"></i></button>
        <div class="dropdown-content">
            <a href = "https://milesdwilliams15.github.io/research/"><strong>Research</strong></a>
            <a href = "https://milesdwilliams15.github.io/software/"><strong>Software</strong></a>
            <a href = "https://milesdwilliams15.github.io/teaching/"><strong>Teaching</strong></a>
        </div>
    </div>
</div>  
<br/>

Building intuition for what *endogeneity* is and how *instrumental variables* (IV) help us to deal with it is hard. I find that running a simulation helps me to better grasp what the problem is, what it implies, and how IV helps.

To that end, tools in the [`seerrr`](https://github.com/milesdwilliams15/seerrr) package for `R` make devising, implementing, and summarizing such a simulation quite easy. So I'm using this post as an opportunity to do two things: (1) to provide some programmatic intuition for conceptualizing the problem of endogenous variables and (2) to illustrate the convenience of using `seerrr` for this, and by extension, other simulation-based analyses.


## Endogeneity

First, let's address *endogeneity*. What is it? 

This question is best answered by way of an illustration. Enter *a DAG*...

![](/assets/images/a-dag.jpg){:class="img-responsive"}

The above image depicts a causal relationship among four variables, only three of which we can observe and two of which we want to estimate the causal relationship between. 

$Y$ is our outcome of interest and $X$ our explanatory variable. We would like to be able to identify the causal effect of $X$ on $Y$. The problem, however, is that both $X$ and $Y$ are affected by some unobserved variable $U$. Unless we can account for $U$ in our analysis our estimate of the effect of $X$ on $Y$ will not be accurate.

Enter our *instrumental variable*, $Z$. As the DAG above illustrates, while $Y$ and $X$ are both affected by $U$, only $X$ is affected by $Z$. We can leverage this fact to our advantage. Since $Z$ has no effect on $Y$, but only on $X$, and is also independent of unobserved confounding $U$, we can isolate variation in $X$ explained by $Z$ to identify the causal effect of $X$ on $Y$.

When we do this, we're estimating what's called a *local average treatment effect* (LATE). This is in lieu of an *average treatment effect* (ATE). The "local" modifier alerts us to the fact that the effect of $X$ on $Y$ is identified by zeroing in on cases in our data that are best explained by the instrument $Z$.


## A Simulation with seerrr

If the above is still too abstract, a simulation in `R` may help to make things more concrete. Programming usually helps me grasp concepts that otherwise go over my head by letting me "tangibly" play with said concept.

First, I attach the `seerrr` package by writing:

```{R}
library(seerrr) 
```

Next, I prep some data for simulation. The data-generating process I'm going with is quite simple. For a sample of $N$ = 1,000 observations I generate an "unobserved" normal variable `U`, an observed and *exogenous* instrument `Z`, my causal variable of interest `X`, and my outcome `Y`. 

`X` is simply an additive function of the instrument `Z`, unobserved confounding `U`, and some random noise. `Y` conversely is an additive function of `X`, unobserved confounding, and some random noise.

By default `simulate` iterates the data-generating process 200 times. We'll keep with that default and simulate the data as follows:

```{R}
sim <- simulate(
  N = 1000,     # sample size
  U = rnorm(N), # unobserved confounder
  Z = rnorm(N), # observed instrument
  X = Z + U + rnorm(N), # causal variable
  Y = X + U + rnorm(N)  # outcome
)
```

By construction the *true* effect of `X` on `Y` is equal to 1 ($Y = X + U = 1 \times X + 1 \times U$, after all). In a world where we can observe `U` and control for it in a regression analysis, we can recover an unbiased estimate of the effect of `X` on `Y` quite easily.

We can check this using `seerrr`'s `estimate` function as follows and then using `evaluate` to evaluate the estimator's performance:

```r
# iteratively estimate linear model
cl_est <- estimate( 
  data = sim, Y ~ X + U, "X", se_type = "stata"
)
# evaluate its performance
evaluate(cl_est, truth = 1, what = "bias")
```

This gives us the following output:

```
# A tibble: 1 x 5
  term       bias      mse coverage power
  <fct>     <dbl>    <dbl>    <dbl> <dbl>
1 X     -0.000523 0.000499    0.947     1
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


