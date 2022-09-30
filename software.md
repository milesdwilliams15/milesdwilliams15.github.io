---
title: My Software
permalink: /software/
---

<!-- Load an icon library -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">

<div class="topnav">
  <div class="dropdown">
        <button class="dropbtn">
        <i class="fa fa-navicon"></i> Menu</button>
        <div class="dropdown-content">
            <a href="https://milesdwilliams15.github.io/"><i class="fa fa-fw fa-home"></i> Home</a>
            <a href="https://milesdwilliams15.github.io/research/"><i class="fa fa-fw fa-area-chart"></i> Research</a>
            <a href="https://milesdwilliams15.github.io/teaching/"><i class="fa fa-fw fa-mortar-board"></i> Teaching</a>
            <a href="https://github.com/milesdwilliams15/job-market-materials/raw/main/cv.pdf"><i class="fa fa-fw fa-file"></i> My CV</a>
            <a href="{{ site.data.social-media.email.href }}{{ site.data.social-media.email.id }}"><i class="fa fa-fw fa-envelope"></i> Email Me</a>
            <a href="{{ site.github.owner_url }}"><i class="fa fa-fw fa-code-fork"></i> My GitHub</a>
            <a href = "https://milesdwilliams15.github.io/software/"><i class="fa fa-fw fa-gears"></i>My Software</a>
            <a href="https://milesdwilliams15.github.io/blog/"><i class="fa fa-fw fa-pencil"></i> My Blog</a>
        </div>
    </div>
  <a href="https://milesdwilliams15.github.io/"><i class="fa fa-fw fa-home"></i> Home</a>
  <a href="https://milesdwilliams15.github.io/research/"><i class="fa fa-fw fa-area-chart"></i> Research</a>
  <a href="https://milesdwilliams15.github.io/teaching/"><i class="fa fa-fw fa-mortar-board"></i> Teaching</a>
</div>

<p> </p>

## My Software

As a part of my methodologically oriented research agenda, I have developed a number of R statistical packages. Click on the links below to learn more.

<ul>
  <li><a href = "https://github.com/milesdwilliams15/coolorrr">coolorrr</a> - Supports easier porting and application of custom color palettes, produced at <a href = "coolors.co">coolors.co</a>, for use with `ggplot()`.</li>
  <li><a href = "https://github.com/milesdwilliams15/RFA">RFA</a> - Implements <a href = "https://rpubs.com/milesdwilliams15/rfa-vignette">random forest adjustment</a> (RFA). RFA is a method for partialing out the influence of confoudning covariates via random forests.</li>
  <li><a href = "https://github.com/milesdwilliams15/seerrr">seerrr</a> - Tools that simplify the process of doing Monte Carlo or computational power analyses.</li>
  <li><a href = "https://github.com/milesdwilliams15/SARM">SARM</a> - For estimating a modified version of the Strategic Autoregressive Model developed by <a href = "https://www.cambridge.org/core/journals/political-analysis/article/estimating-freeriding-behavior-the-stratam-model/0CBD6176E53848732CEC2C151A491212">Steinwand (2011)</a>.</li>
  <li>oesr (available soon, with Ryan T. Moore) - For visualizing the results from randomized controlled trials using the Office of Evaluation Sciences' <a href = "https://oes.gsa.gov/assets/files/reporting-statistical-results.pdf">recommended style guide</a>.</li>
</ul>

Once upon a time, I also published on RPubs some code I wrote to make producing interaction plots a little more flexible. Existing options like `interplot` usually work well for me, but on occasion I've used models that `interplot` and other packages don't support. Apparently, others out there have found my code useful, too, because I receive emails from time to time from folks who have made good use of it in their own research and who have questions about ways to modify it for their own purposes. I therefore figured the code was worth highlighting here if only to potentially improve its visibility. 

  - ["Plotting Martinal Effects"](https://rpubs.com/milesdwilliams15/381372)
