[Back to Blog](https://milesdwilliams15.github.io/blog/)

I love working with R Markdown. It makes writing articles and putting
together materials easy, especially when those documents include
statistical analysis or data visualizations. It turns out, I can use R
Markdown to write blog posts, too.

I stumbled onto this post by [Steven
Miller](http://svmiller.com/blog/2019/08/two-helpful-rmarkdown-jekyll-tips/)
from 2019 that provides a nice summary of how to do this. I wanted to
see how it worked myself, so I tried it out in writing this post.

To make it work I simply created a `_source` directory in the RStudio
project I use for my website. In that directory, I created and saved a
.Rmd file for this particular post. Following Steven’s instructions, I
specified the YAML as follows:

    ---
    title: "Using R Markdown to Create Posts"
    output:
      md_document:
        variant: gfm
        preserve_yaml: TRUE
    knit: (function(inputFile, encoding) {
    rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
    author: "Miles D. Williams"
    date: ""
    excerpt: "Writing posts with R Markdown"
    layout: post
    categories:
      - R Markdown
      - Jekyll
    ---

The key to making this work is the settings for `knit`. Usually, when
you knit a .Rmd file to an html or pdf the knitted version is storred in
the same working directory as the raw .Rmd file. I never put together
that you could modify where to store the knitted version. For sites
hosted by Jekyll (like mine), blog posts go in a `_posts` directory. So,
by specifying:

    knit: (function(inputFile, encoding) {
      rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })

I can keep the raw .Rmd file in one directory so that the only files
stored in the `_posts` directory are the actual blog posts I want to
publish on my site.

Then, all I need to do is knit the document and push the changes to
[GitHub](https://github.com/milesdwilliams15/milesdwilliams15.github.io).
*Voila*, I have a blog post that also renders output from R code inline
just as seamlessly as it would be for writing a pdf!

***But, there is a caveat!*** I had to do two more things to make
everything work. First, I needed to set by working directory to the
`_source` directory where my .Rmd file lives. Next, I had to update my
`setup` code chunk to specify where to pull rendered figures from:

    base_dir <- "~/milesdwilliams15.github.io/" # i.e. where the jekyll blog is on the hard drive.
    base_url <- "/" # keep as is
    fig_path <- "_posts/2021-27-15-rmarkdown-and-blogging_files/" # customize to heart's content, I 'spose.

    knitr::opts_knit$set(base.dir = base_dir, base.url = base_url)
    knitr::opts_chunk$set(fig.path = fig_path,
                          cache.path = '../cache/',
                          message=FALSE, warning=FALSE,
                          cache = TRUE) 

Otherwise, figures won’t show up because the knitted .md file will set
the file paths locally—which is no good, no good at all.

However, once all of that has been sorted out, making blogging with
RMarkdown work is as simple has pressing “Knit.” And everything should
render just like it would in a knitted pdf or html. For instance, below
is the boilerplate example text and R script you get when you open a
.Rmd file in RStudio, with everything rendered just as it should be:

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax
for authoring HTML, PDF, and MS Word documents. For more details on
using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that
includes both content as well as the output of any embedded R code
chunks within the document. You can embed an R code chunk like this:

``` r
summary(cars)
```

    ##      speed           dist       
    ##  Min.   : 4.0   Min.   :  2.00  
    ##  1st Qu.:12.0   1st Qu.: 26.00  
    ##  Median :15.0   Median : 36.00  
    ##  Mean   :15.4   Mean   : 42.98  
    ##  3rd Qu.:19.0   3rd Qu.: 56.00  
    ##  Max.   :25.0   Max.   :120.00

## Including Plots

You can also embed plots, for example:

![](https://github.com/milesdwilliams15/milesdwilliams15.github.io/blob/master/_posts/2021-27-15-rmarkdown-and-blogging_files/figure-gfm/pressure-1.png?raw=true)

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.

[Back to Blog](https://milesdwilliams15.github.io/blog/)
