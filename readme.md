A bit on base vs tidyverse for new R learners
================
misken

Recently I was working on a research project involving comparison of a
bunch of predictive models (polynomial regression, natural cubic splines
and a nonlinear power type model) and I figured I’d use
[caret](https://topepo.github.io/caret/index.html). I’d used it before
[doing some similar
work](https://misken.github.io/blog/obsim_caret_part3/) as, somewhat
like [sci-kit learn](https://scikit-learn.org/stable/) in the Python
world, it provides a unified interface to a whole bunch of model
families. This is even more important in R as models are spread across
numerous packages and have widely varying interfaces or APIs.
Unfortunately, the `nls()` function wasn’t supported as a model engine
in caret and I didn’t immediately want to try creating a custom `caret`
model, though this is definitely supported.

So, I figured this was a good excuse to poke around in the
[tidymodels]() world. As someone who teaches an [intro R and Python for
data analytics
course](http://www.sba.oakland.edu/faculty/isken/teaching.html), I was
well aware of the tidyverse and use packages such as dplyr, tidyr, and
readr in my course (yes, of course I cover ggplot but it does predate
the tidyverse). However, I hadn’t really dug into the various tidymodels
tools such as [broom](https://broom.tidymodels.org/),
[parsnip](https://parsnip.tidymodels.org/),
[workflows](https://workflows.tidymodels.org/),
[rsample](https://rsample.tidymodels.org/),
[tune](https://tune.tidymodels.org/),
[yardstick](https://yardstick.tidymodels.org/) or
[recipes](https://recipes.tidymodels.org/). Spoiler alert - `nls()`
isn’t supported in tidymodels either but that’s a topic I’ll address in
a separate blog post in which I’ll share the details of my use of these
tools in our research project.

For now, I want to mention that in exploring these tools, I ended up
going down the rabbit hole of the base R vs tidyverse debate for new R
learners. As both a teacher, consultant and researcher/developer using R
in very different ways, this is a fascinating debate and I learned much
in digging into it again, as one must with the constant evolution of the
R ecosystem. So, I’m really just using this post as a place to list some
useful resources for others interested in this debate as well as adding
my modest two cents.

## The base R vs tidyverse debate

The tidyverse has become increasingly popular and with this popularity
has come more scrutiny. In particular, there’s a healthy debate on
whether new R users should first learn base R and then move on to the
tidyverse or whether they should immediately be taught the tidyverse
approach. There is much to be learned from this debate as people
exchange ideas, approaches and different approaches. R is a rich and
deep language and debates like this get into some nooks and crannies
that many would never otherwise know about.

A few good resources on this debate include the following. Within these
are links to numerous related articles, posts, tweets, and rants.

-   The [TidyverseSkeptic](https://github.com/matloff/TidyverseSkeptic)
    project by Norm Matloff is a well known essay on why new R learners
    should be taught base R first. Check out the Issues for some heated
    discussion. Matloff is also the author of [fasteR: The fast lane to
    learning R](https://github.com/matloff/fasteR).
-   David Robinson argues the tidyverse first side in posts such as
    http://varianceexplained.org/r/teach-tidyverse/
    http://varianceexplained.org/r/why-I-use-ggplot2/
-   Data Carpentry has a post on [base R and tidy
    equivalents](https://tavareshugo.github.io/data_carpentry_extras/base-r_tidyverse_equivalents/base-r_tidyverse_equivalents.html)
-   One criticism of the tidyverse is that it can lead to dependency
    bloat - learn more from [this essay](https://www.tinyverse.org/)
    about the “tinyverse”.
-   There’s no doubt that ggplot is awesome, but [check out what can be
    done if you have a good grasp of base plotting in
    R](https://github.com/karoliskoncevicius/tutorial_r_introduction/blob/main/baseplotting.md).
    When I read this, it felt a bit like matplotlib, the venerable
    Python based plotting package.

There are a few Reddit threads that address this topic including [this
one](https://www.reddit.com/r/rprogramming/comments/ondo0y/what_do_you_think_of_r_beginners_learning/)
and [this
one](https://www.reddit.com/r/rprogramming/comments/rd4ksl/i_am_concerned_about_the_tidyverse_and_its_impact/).
One of the comments I found in this thread makes a good point: -

> In mathematics class they see y=f(x), which is essentially output \<-
> processing(input). In flowcharts you’ll often see input -\>
> processing() -\> output. Take 10 minutes in a class to teach the
> students to mentally connect these two, and everything should be fine.

## What I think, say, and do about this issue

I tell my students that it really isn’t an either-or question and in my
course you will learn both base R and tidyverse approaches. At the end
of the day, we use R to solve problems and the more tools and
understanding you have to tackle those problems, the better off you will
be. Here’s a hodge podge of thoughts on base R vs tidyverse for new R
learners.

### Vector thinking and R Studio / R Markdown

I start out my class with fundamental base R things such as vectors. The
notion of operating on vectors is new to most and I stress that
“thinking in vectors” will help them not only with learning R but also
with Python. By covering vectors first we can use things like boolean
masks, a technique that proves handy in base R as well as in the widely
used Python packages, [pandas](https://pandas.pydata.org/) and
[numpy](https://numpy.org/).

``` r
y <- -5:4
print(y)
```

     [1] -5 -4 -3 -2 -1  0  1  2  3  4

``` r
y[y > 0]
```

    [1] 1 2 3 4

At the same time, students learn their way around R Studio and R
Markdown documents and R scripts. The R Studio IDE (and related things
like R Markdown and now Quarto) transcend this debate. They are simply
really useful tools for teaching, learning and using R.

### Brackets and dollar signs

Right after the introduction to vectors comes the introduction to
dataframes. I start them out with a solid intro to accessing rows and
columns of dataframes through various combinations of the square
brackets, vectors of column names, boolean vectors, numeric slices, and
the `$`. I just cannot see learning R without learning these basic
things. They are going to see brackets, slicing, boolean vectors, and
various column accessor methods in Python, too.

### Early wins and ggplot

For new R learners, I do believe there is great value in some “early
wins”. Creating beautiful plots with `ggplot2` (which I’ll just refer to
as ggplot from now on) in our second R session is definitely a huge
early win that helps motivate students to keep climbing the admittedly
steep learning curve associated with R. I do show a few base plot
examples but then shift quickly into learning about the Grammar of
Graphics and ggplot. Most of my students come from an Excel heavy
background (I also teach a [spreadsheet based analytics
course](http://www.sba.oakland.edu/faculty/isken/teaching.html)) and
seeing faceted ggplots is an eye-opener for them (as they imagine trying
to pull off the same thing in Excel).

### dplyr, the pipe, and debugging

For my [session on group by
analysis](http://www.sba.oakland.edu/faculty/isken/courses/mis5470_f22/data_wrangling_r.html),
my teaching has changed over time. I used to begin with the base `apply`
family of functions and then show `plyr` (and `dplyr` when it replaced
`plyr` a few years later). Now, I jump right into dplyr but still
include material on the apply approach. One reason for this is that many
of our students have some SQL knowledge and they immediately grasp the
similarities between dplyr and SQL - the code kinda feels the same. We
do relatively simple examples focusing on the main dplyr verbs.

As for the pipe, I introduce it as just one approach to computing in R.

**Example verbage from my class notes**

The verb functions in **dplyr** share some traits that make them very
amenable to being chained together.

-   each takes a data frame as its first argument
-   the rest of the function arguments specify what’s to be done
-   each returns a data frame

They can be used in standard ways as functions but can also be chained
together using the \`%\>% pipe operator from the **magrittr** package.
We’ll do it both ways and you can decide for yourself when to use which
approach. The pipe just takes whatever is the result of the left side is
and pipes it into the first argument of whatever is on the right side.

For example, compare the following two approaches to computing Euclidean
distance between two vectors:

``` r
# create two vectors and calculate Euclidian distance between them
x1 <- 1:5; x2 <- 2:6
sqrt(sum((x1-x2)^2))
```

    [1] 2.236068

``` r
# chaining or piping method
(x1-x2)^2 %>% 
  sum() %>% 
  sqrt()
```

    [1] 2.236068

As for the pipe making it more difficult to debug complex dplyr
statements, I agree. So, I also show that we can always create
intermediate variables to aid in debugging. Also, it’s easy to highlight
a portion of a complex dplyr statement and just run that portion. So,
there are ways to help alleviate this concern. No one is forced to use
the pipe. When I first encountered the pipe and some of the debugging
difficulties associated with it, I realized that it wasn’t much
different from the challenges associated with debugging complex SQL
statements. Reading piped dplyr code is pretty easy and is like reading
SQL.

### The tidyverse

In my class, we never load the `tidyverse` library. Instead, we load
individual libraries that we need. If you need `stringr` and `dplyr`,
load `stringr` and `dplyr`. I don’t want my students to think of the
tidyverse as this monolithic all or nothing thing they have to either
totally buy in to or entirely reject. It’s a collection of opinionated,
useful libraries. Use the bits you need. No need for dogma.

### Modeling

So, far, all of the linear and logistic regression and tree-based
modeling I do in my class is done with base R tools. Part of the reason
for this is that `tidymodels` is relatively new, still undergoing API
changes and not as widely used (afaik) as the base things like `glm` or
`randomForest`. Furthermore, since this is an introductory class of R
newbies, the benefits of the higher levels of abstraction provided by
the tidymodels tools aren’t really needed and come at a cost of
obscuring some modeling details.

Even when it comes to things like data partitioning, I start by showing
simple approaches using `sample()` and row selection with a vector. Then
we move on to use tools like `caret::createDataPartition()` instead of
`sample()` and discuss the advantages of a sampling procedure that
attempts to keep both data partitions somewhat similar with respect to
the target variable.

``` r
set.seed(547)
pct_test <- 0.2
testrecs <- sample(nrow(Default), pct_test * nrow(Default))
default_test <- Default[testrecs,]
default_train <- Default[-testrecs,]  # Negative in front of vector means "not in"
```

I do bring it to the students attention that while some models conform
to “standards” (such as having a `predict` method), there can be much
variation in how different modeling packages expose their API and this
can be a challenge. Tools like caret and its “successor”, tidymodels can
really help address this challenge. In the Python world, the popularity
of scikit-learn is partially due to its consistent API and one stop shop
approach to building predictive modeling.

In addition, tools like pipelines in sckit-learn or workflows in
tidymodels can help modelers avoid things like leaking training data
knowledge into the test data by properly coordinating data preprocessing
and resampling schemes. However, I think there is significant value in
first learning to do the steps “by hand” using base R (or Python) before
the details get buried in higher level workflow abstractions. Let’s face
it, many popular predictive modeling techniques such as neural networks
and random forests are already opaque enough without the entire modeling
workflow being made less transparent to the new R modeler.

### Bottom line

I start with base R but with very early recognition of the tidyverse.
Then the two are interleaved and compared and contrasted as we go.
Multiple ways of doing things is par for the course in programming and R
is no different. Students will find both base R and tidyverse approaches
in places like StackOverflow. They need exposure to both. But…, if
forced with the false choice of teaching one or the other in an R for
newbies class, I’d teach base R - and I’d still teach ggplot because it
predates the tidyverse (sticks fingers in ears, nah, nah, can’t hear
you) and it’s just so good.
