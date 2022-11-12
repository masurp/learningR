R basics: Getting started
================
Philipp Masur
2022-11

-   <a href="#introduction" id="toc-introduction">Introduction</a>
    -   <a href="#what-is-r-and-why-should-you-learn-it"
        id="toc-what-is-r-and-why-should-you-learn-it">What is R and why should
        you learn it?</a>
    -   <a href="#purpose-of-this-tutorial"
        id="toc-purpose-of-this-tutorial">Purpose of this tutorial</a>
-   <a href="#getting-started-with-r"
    id="toc-getting-started-with-r">Getting started with R</a>
    -   <a href="#installing-r" id="toc-installing-r">Installing R</a>
    -   <a href="#installing-rstudio" id="toc-installing-rstudio">Installing
        RStudio</a>
    -   <a href="#using-rstudio" id="toc-using-rstudio">Using RStudio</a>
    -   <a href="#running-code-from-the-r-script"
        id="toc-running-code-from-the-r-script">Running code from the R
        script</a>
    -   <a href="#assigning-values-to-names"
        id="toc-assigning-values-to-names">Assigning values to names</a>
    -   <a href="#using-rstudio-projects" id="toc-using-rstudio-projects">Using
        RStudio projects</a>

# Introduction

## What is R and why should you learn it?

R is an open-source statistical software language, that is currently
among the most popular languages for data science. In comparison to
other popular software packages in social scientific research, such as
SPSS and Stata, R has several notable advantages:

-   R is a programming language, which makes it much more versatile.
    While R focuses on statistical analysis at heart, it facilitates a
    wide-range of features, and virtually any tool for data science can
    be implemented.
-   The range of things you can do with R is constantly being updated. R
    is open-source, meaning that anyone can contribute to its
    development. In particular, people can develop new *packages*, that
    can easily and safely be installed from within R with a single
    command. Since many scholars and industry professionals use R, it is
    likely that any cutting-edge and bleeding-edge techniques that you
    are interested in are already available. You can think of it as an
    app-store for all your data-science needs!
-   R is free. While for students this is not yet a big deal due to free
    or cheap student and university licences, this can be a big plus in
    the commercial sector. Especially for small businesses and
    free-lancers.

The tradeoff is that R has a relatively steep learning curve. Still,
learning R is not as bad as people often fear, and with thanks to the
rising popularity of data science there are now many footholds that make
learning and using R easier and–dare we say–fun. In this course you will
learn the core basics, and see how this immediately grants you access to
using cutting-edge techniques.

## Purpose of this tutorial

The focus of this tutorial is to get you started with R, and to see how
easy it is to start doing some cool stuff. We will not yet dive into how
R and the R syntax really work, so do not be alarmed if you do not
understand the code that you’ll be using. For now, just focus on getting
R running, getting familiar with how to run code, and playing around
with it.

# Getting started with R

For the current course material, you will need to install two pieces of
software.

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcReenaHW13DG0WIxuTpSsBc4h4WBYZE6YImSZkuP0JMiSlItWoR39lvgznbqoO58OnuCJg&usqp=CAU)

-   *R* is the actual R software, that is used to run R code.
-   *RStudio* is a graphical user interface (GUI) that makes working
    with R much easier. While it is not required to use R, and there are
    other GUI’s available, using RStudio is highly recommended.

Both programs can be downloaded for free, and are available for all main
operating systems (Windows, macOS and Linux).

## Installing R

To install R, you can download it from the [CRAN (comprehensive R
Archive Network) website](https://cran.r-project.org/). Do not be
alarmed by the website’s 90’s asthetics. R itself is cold, dry,
no-nonsense software. The decoration comes with RStudio.

## Installing RStudio

The [RStudio website](https://www.rstudio.com/) contains download links
and installing instructions. You will need to install the free *RStudio
Desktop Open Source License*. Note that the expensive licences do not
offer better features or anything, but just offer additional support and
a commercial license. You can also use the free version when doing
commercial research, but with an AGPL license.

## Using RStudio

Once you have installed R and RStudio, you can start by launching
RStudio. If everything was installed correctly, RStudio will
automatically launch R as well.

The first time you open RStudio, you will likely see three separate
windows. The first thing you want to do is open an R Script to work in.
To do so, go to the toolbar and select File -\> New File -\> R Script.

You will now see four windows split evenly over the four corners of your
screen:

![](img/r1.png)

-   In the **top-left** you have the text editor for the file that you
    are working in. This will most of the time be an R script or
    RMarkdown file.
-   In the **top-right** you can see the data and values that you are
    currently working with (environment) or view your history of input.
-   In the **bottom-left** you have the console, which is where you can
    enter and run code, and view the output. If you run code from your R
    script, it will also be executed in this console.
-   In the **bottom-right** you can browse through files on your
    computer, view help for functions, or view visualizations.

While you can directly enter code into your console (bottom-left), you
should always work with R scripts (top-left). This allows you to keep
track of what you are doing and save every step.

## Running code from the R script

Copy and paste the following example code into your R Script. For now,
don’t bother understanding the syntax itself. Just focus on running it.

``` r
3 + 3
2 * 5
6 / 2
"some text"
"some more text"
sum(1,2,3,4,5)
```

You can **run** parts of the code in an R script by pressing Ctrl +
Enter (on mac this is command + Enter). This can be done in two ways:

-   If you select a piece of text (so that it is highlighted) you can
    press Ctrl + Enter to run the selection. For example, select the
    first three lines (the three mathematical operations) and press
    Ctrl + Enter.
-   If you haven’t made a selection, but your text cursor is in the
    editor, you can press Ctrl + Enter to run the line where the cursor
    is at. This will also move the cursor to the next line, so you can
    *walk* through the code from top to bottom, running each line. Try
    starting on the first line, and pressing Ctrl + Enter six times, to
    run each line separately.

## Assigning values to names

When running the example code, you saw that R automatically
**evaluates** expressions. The calculation 3+3 evaluates to 6, and 2\*5
evaluates to 10. You also saw that the **function** *sum(1,2,3,4,5)*
evaluates to 15 (the sum of the numbers). We’ll address how to use R as
a calculator and how to perform functions at a later time. For now, one
more thing that you need to know about the R syntax is how values can be
**assigned** to names.

In plain terms, **assignment** is how you make R remember things by
assigning them to a name. This works the same way for all sorts of
values, from single numbers to entire datasets. You can choose whether
you prefer the equal sign (=) or the arrow (\<-) for assignment.

``` r
x = 2
y <- "some text"
```

Here we have remembered the number 2 as **x** and the text “some text”
as **y**. If you are working in RStudio (which you should), you can now
also see these names and values in the topright window, under the
“Environment” tab.

We can now use the names to retrieve the values, or to use these values
in new commands.

``` r
x * 5
```

    ## [1] 10

Note that you shouldn’t type the line `## [1] 10`: in this tutorial,
lines starting with `##` show the *output* of commands (2 \* 5 = 10).

## Using RStudio projects

It is best to put all your code in an RStudio *project*. This is
essentially a folder on your computer in which you can store the R files
and data for a project that you are working on. While you do not
necessarily need a project to work with R, they are very convenient, and
we strongly recommend using them.

To create a new project, go to the top-right corner of your RStudio
window. Look for the button labeled **Project: (None)**. Click on this
button, and select New Project. Follow the instructions to create a new
directory with a new project. Name the project “R introduction”.

Now, open a new R script and immediately save it (select File -\> Save
in the toolbar, or press ctrl-s). Name the file
**my_first_r\_script.r**. In the bottom-right corner, under the
**Files** tab, you’ll now see the file added to the project. The
extension **.r** indicates that the file is an R script.
