---
title: Logit and Heteroskedasticity
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
author: "Miles Williams"
date: "2022-01-19"
excerpt: "Part I of a series"
layout: post
categories: ["GLM", "Bias"]
---

[Back to Blog](https://milesdwilliams15.github.io/blog/)

I know of few political scientists, economists, or other quantitative
researchers who *don’t* use some form of robust standard errors when
estimating linear regression models via OLS.

Why?

Because only under the most ideal and stringent of circumstances will
classic OLS standard errors be efficient.[1] In particular, efficiency
requires that the variance of the regression error is constant and that
individual observations are independent of each other. In practice this
usually never is the case. Instead, the errors may be heteroskedastic
(have non-constant variance) or may not all be independent (they may be
clustered).

Let’s zero in on the problem of non-constant variance
(heteroskedasticity).

When it comes to linear models, the solution to this problem is simple:
just use a robust estimator for the standard errors (there are many to
choose from). Such estimators take into account heterogeneity in the
errors and thus provide more consistent standard errors and more
appropriate statistical inferences.

But, for nonlinear models such as logit, the problem is a little more
insidious, and, therefore, it requires doing more than simply using
robust standard errors.

## The Problem

To illustrate why the problem of heteroskedasticity has different
implications between linear and nonlinear models, let’s take a quick
look at a pair of regressions:

1.  *Y*<sub>*i*</sub> = *β*<sub>0</sub> + *β*<sub>1</sub>*X*<sub>*i*</sub> + *ϵ*<sub>*i*</sub>,
    *ϵ*<sub>*i*</sub> ∼ 𝒩(0,*σ*<sub>*i*</sub><sup>2</sup>);

2.  Pr (*B*<sub>*i*</sub> = 1) = *L*\[(*β*<sub>0</sub>+*β*<sub>1</sub>*X*<sub>*i*</sub>)/*σ*<sub>*i*</sub>\].

Equation 1 is a linear model and equation 2 is a logit model, where the
function *L*( ⋅ ) is just the logistic function:

*L*(*x*) = 1/\[1 + exp ( − *x*)\].

A logit model can be equivalently expressed as

log \[odds(*B*<sub>*i*</sub> = 1)\] = (*β*<sub>0</sub> + *β*<sub>1</sub>*X*<sub>*i*</sub>)/*σ*<sub>*i*</sub>

These models differ in several ways. The first and most obvious
difference is that equation 1 specifies a continuous response
*Y*<sub>*i*</sub> ∈ ℝ as a linear function of *X*<sub>*i*</sub>.
Meanwhile, equation 1 specifies the probability of a binary response
*B*<sub>*i*</sub> ∈ {0, 1} taking the value 1 as a logistic function of
*X*<sub>*i*</sub>.

But the differences go beyond these models’ functional forms. The
difference that’s most material for our concern with heteroskedasticity
is how the variance parameter *σ*<sub>*i*</sub><sup>2</sup> enters each
equation. In the linear model, the variance enters the model through an
additive error term *ϵ*<sub>*i*</sub>. But, in the logit model, the
variance enters the model directly as the denominator of the linear
additive input to the logistic function.

The reason for this difference lies in where the stochastic component of
each model originates. In a linear model, the stochastic element enters
additively, while in the logit model the logitistic curve itself is an
input to a stochastic Bernoulli function.

The practical implication of this difference is that the variance
component of the model *isn’t* part of the identity of the *β*
parameters in the linear model but *is* part of the identity of the *β*s
in the logit model. This is a problem for logit estimation if
*σ*<sub>*i*</sub> is different for each individual observation *i*.

Classic logit relies on a maximum likelihood estimator (MLE) to identify
the *β* parameters. Its likelihood function is:

ℒ = *Π*<sub>*i* = 1</sub><sup>*N*</sup>*L*(*β*<sub>0</sub> + *β*<sub>1</sub>*X*<sub>*i*</sub>)<sup>*B*<sub>*i*</sub></sup>\[(1−*L*(*β*<sub>0</sub>+*β*<sub>1</sub>*X*<sub>*i*</sub>))<sup>1 − *B*<sub>*i*</sub></sup>\],

where the *σ* parameter is dropped because it is assumed constant
(*σ*<sub>*i*</sub> = *σ* = 1 ∀ *i*).

This assumption is only appropriate if *σ*<sub>*i*</sub> ≠ *σ*. When
this isn’t the case, the MLE estimates of the *β*’s will be
biased.\[^2\]

\[^2\] The standard errors will also be inefficient since the standard
errors are calculated directly from the Jacobian of the likelihood
function.

## The Common but Not-quite-right Solution

To deal with heteroskedasticity in nonlinear models like logit, it’s
common (at least in my field of political science) to see researchers
use a robust variance-covariance estimator for standard errors but do
nothing to correct the parameter estimates themselves.

This choice is understandable. Most statistical software packages will
allow users to get something like the robust variance-covariance matrix
for OLS for their MLE estimates. Most software packages will also report
test statistics and p-values based on these “robust” standard errors.

However, just because our statistical software lets us do something,
that doesn’t mean we should. The supposedly robust standard errors that
statistical packages will produce are problematic for a number of
reasons, the first being that they reflect the variance of a parameter
that has not itself be estimated well. Even more, these robust standard
errors are computed using the gradients of the very likelihood function
that fails to capture the non-constant variance in the data. How is it
possible to generate correct standard errors from the gradients of a
mis-specified likelihood function? Answer: it isn’t.

## The Better Solution (but not a silver bullet)

To deal with heteroskedasticity in nonlinear models like logit there
exist a class of heteroskedastic maximum likelihood estimators that
allow for explicitly modeling the variance component of the model.

Such estimators are not a perfect hedge against bias to be sure.
However, in certain instances it is possible to recover both less biased
and more efficient parameter estimates by taking non-constant variance
into account. Consider a scenario where the probability of a binary
outcome is given as

Pr (*B*<sub>*i*</sub> = 1) = *L*\[(*X*<sub>*i*</sub>)/exp(*X*<sub>*i*</sub>)\].

In the above exp (*X*<sub>*i*</sub>) replaces *σ*<sub>*i*</sub>. It
specifies that the probability that the binary response
*B*<sub>*i*</sub> = 1 is not only a function of *X*<sub>*i*</sub>, but
also that the variance changes as a function of *X*<sub>*i*</sub>.

To recover estimates of the relationship between *X*<sub>*i*</sub> and
the response, we would specify a heteroskedastic logit model as follows:

log \[odds(*B*<sub>*i*</sub> = 1)\] = (*β*<sub>0</sub> + *β*<sub>1</sub>*X*<sub>*i*</sub>)/exp (*γ*<sub>0</sub> + *γ*<sub>1</sub>*X*<sub>*i*</sub>).

If we were to compare the estimates from the above to those obtained
from the classic logit model, specified as

log \[odds(*B*<sub>*i*</sub> = 1)\] = *β*<sub>0</sub> + *β*<sub>1</sub>*X*<sub>*i*</sub>,

the heteroskedastic logit estimates should be less biased, and the
standard errors should provide the appropriate coverage for confidence
intervals supporting more informative statistical inferences.

## A Simulation

To quickly show the advantage conferred by heteroskedastic logit, we can
do a quick simulation study in R.

We first need to attach some statistical packages that we’ll need to run
the analysis.

``` r
# Install if not already #
# devtools::install_github("milesdwilliams15/seerrr")

# Packages Needed  # Reason                    #
# ================ # ========================= #
library(seerrr)    # for simulation            #
library(tidyverse) # for grammar               #
library(glmx)      # for heteroskedastic logit #
library(kableExtra)# for making nice tables    #
```

Next, we’ll start by iteratively simulating a data-generating process.

``` r
# Simulate a d.g.p. 1,000 times:
set.seed(101010101) # setting seed for replicability
simulate(
  N = 1000,
  id_var = 1:N,
  X = rnorm(N),
  s = exp(X),
  Y = rbinom(N, 1, 1 / (1 + exp(-(X / s))))
) -> sim_data
```

The above randomly generates a list of datasets of size *N* = 1, 000
drawn from a d.g.p. that includes (1) a predictor variable *X* that is a
normal random variable with mean 0 and standard deviation of 1 and (2) a
binary response variable *Y* that takes the value 1 with probability
*L*(*X*<sub>*i*</sub>/exp (*X*<sub>*i*</sub>).

With multiple samples drawn from the d.g.p., we can now apply our choice
of estimators to model *Y* as a function *X*. Below, I specify a classic
logit model and a heteroskedastic logit model. (See
[documentation](https://www.quantargo.com/help/r/latest/packages/glmx/0.1-1/hetglm)
for `hetglm` in the `glmx` package for more on syntax and usage.)

``` r
# Three estimation approaches #
# =========================== #

# classic logit
logit <- function(...) glm(..., family = binomial)
estimate(
  sim_data,
  Y ~ X,
  vars = "X",
  estimator = logit,
) -> classic_logit

# heteroskedastic logit
het_logit <- function(...) hetglm(..., family = binomial) %>%
  lmtest::coeftest()
estimate(
  sim_data,
  Y ~ X | X,
  vars = "X",
  estimator = het_logit
) -> hetero_logit
```

The above generates estimates that we get using logit and
heteroskedastic logit. We can evaluate these estimates as follows:

``` r
# evaluate classic logit
evaluate(
  classic_logit, what = "bias", truth = 1
) -> classic_logit_eval

# evaluate heteroskedastic logit
evaluate(
  hetero_logit, what = "bias", truth = 1
) -> hetero_logit_eval
```

And we can report the results by writing:

``` r
# comparison
bind_rows(
  classic_logit_eval,
  hetero_logit_eval
) %>%
  bind_cols(
    model = c("logit", "het-logit"), .
  ) %>%
  select(model, bias, coverage) %>%
  kable(digits = 3)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
model
</th>
<th style="text-align:right;">
bias
</th>
<th style="text-align:right;">
coverage
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
logit
</td>
<td style="text-align:right;">
-0.037
</td>
<td style="text-align:right;">
0.925
</td>
</tr>
<tr>
<td style="text-align:left;">
het-logit
</td>
<td style="text-align:right;">
0.005
</td>
<td style="text-align:right;">
0.960
</td>
</tr>
</tbody>
</table>

The heteroskedastic logit clearly outperforms standard logit, both in
terms of bias and in terms of the coverage of the 95 percent confidence
intervals (these should contain, or “cover”, the true parameter value 95
percent of the time).

## Conclusion

The above example was quite simple, which is so by design to make
illustrating the consequences of heteroskedasticity for nonlinear models
as clear as possible.

This discussion has hardly exhausted the many threats to unbiased and
efficient MLE estimation of nonlinear models. Other forms of unmeasured
heterogeneity and even omitted variables (whether or not they are
independent of a response and predictor of interest) can portend
problems that generally don’t apply for linear models.

For this reason, care should be taken when using MLE for nonlinear
regression estimation. Don’t just use logit for a binary response
without interrogating the assumptions that underlie such a modeling
choice.

[Back to Blog](https://milesdwilliams15.github.io/blog/)

[1] For a nice summary, see [Hayes and Cai
(2007)](https://link.springer.com/content/pdf/10.3758/BF03192961.pdf).
