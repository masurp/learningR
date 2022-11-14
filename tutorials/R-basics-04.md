R: Multilevel modelling
================
Philipp Masur
2022-11

-   <a href="#introduction" id="toc-introduction">Introduction</a>
    -   <a href="#loading-packages-and-data"
        id="toc-loading-packages-and-data">Loading packages and data</a>
    -   <a href="#data-preparation" id="toc-data-preparation">Data
        preparation</a>
    -   <a href="#privacy-concerns-across-countries"
        id="toc-privacy-concerns-across-countries">Privacy Concerns across
        countries</a>
-   <a href="#multilevel-modelling" id="toc-multilevel-modelling">Multilevel
    modelling</a>
    -   <a href="#null-model-varying-intercept-only"
        id="toc-null-model-varying-intercept-only">Null model (Varying intercept
        only)</a>
    -   <a href="#level-1-predictors-varying-intercept-fixed-slope"
        id="toc-level-1-predictors-varying-intercept-fixed-slope">Level
        1-predictors, varying intercept, fixed slope</a>
    -   <a href="#level-2-predictors" id="toc-level-2-predictors">Level
        2-predictors</a>
    -   <a href="#varying-intercept-varying-slope"
        id="toc-varying-intercept-varying-slope">Varying intercept, varying
        slope</a>
    -   <a href="#visualizations" id="toc-visualizations">Visualizations</a>

# Introduction

In this tutorial, we are going to learn about simple multilevel models,
an interesting approach to studying processes across countries (aka a
different way of doing comparative research!).

## Loading packages and data

Multilevel modeling works best with the package `lme4` and its extension
`lmerTest`. But, we are also loading the library `foreign` (to load SPSS
data sets) and the `tidyverse` for data wrangling purposes.

``` r
#install.packages("lme4")
#install.packages("lmerTest")
#install.packages("stringi")
library(lme4)
library(lmerTest)
library(stringi)

library(foreign)
library(tidyverse)
library(psych)
```

We load data from the eurobarometer study from 2011.

``` r
d <- read.spss("data/ZA5450_v5-2-0.sav", use.value.labels = F, to.data.frame = T)
dim(d)
```

## Data preparation

We need to engage in some preprocessing, e.g., trim the country names
and combine countries that are still measured in historical regions.

``` r
d$country = stri_trim(d$v7) 
d$country[d$country %in% c("DE-E", "DE-W") ] = "DE"
d$country[d$country %in% c("GB-GBN", "GB-NIR") ] = "UK"
table(d$country)
```

We further center the age variable and divide by 10 (rescaling) and add
a variable that differentiates between new and old members of the EU.

``` r
d$age.c <- (d$v497 - mean(d$v497))/10
d$EU15 <- d$v25
table(d$EU15)
```

The variables of interest are disclosure and privacy concerns. We create
respective mean indices.

``` r
d$SD <- apply(d[,148:161], 1, sum, na.rm = T)
describe(d$SD)

d$PC <- apply(d[,268:273], 1, mean, na.rm = T)
describe(d$PC)
```

## Privacy Concerns across countries

We can quickly check differences in concerns across countris.

``` r
ggplot(d, 
       aes(y = PC, 
           x = country))  + 
  stat_summary(fun.data = "mean_cl_boot", 
               colour = "black")
describe(d$PC)
```

# Multilevel modelling

## Null model (Varying intercept only)

No predictors,

``` r
m0 = lmer(PC~1+(1|country), data = d)
summary(m0)
```

We can quickly compute the intra-class correlation coefficient that
tells us how much variance is explained by country differences.

``` r
table1 <- VarCorr(m0) %>%
  as.tibble
table1

icc <- table1$vcov[1]/(table1$vcov[1]+table1$vcov[2])
icc
```

Using `coef()`, we can also get the means per country

``` r
coef(m0)$country
```

There are two more intersting functions: WIth `fixef()` we geht the
fixed effects, with `ranef()` the random effects and with
`coef() = fixef() + ranef()` the effects per country

``` r
fixef(m0)
ranef(m0)
```

## Level 1-predictors, varying intercept, fixed slope

Let’s add age as a level 1 predictor

``` r
m1 <- lmer(PC ~ 1 + age.c  + (1 | country), data = d)
summary(m1)
```

## Level 2-predictors

We can also add level 2 predictors

``` r
m2 <- lmer(PC ~ 1 + age.c + EU15 + (1 | country), data = d)
summary(m2)
```

## Varying intercept, varying slope

Next, we want to test whether the effect of age differs across
countries. We let the effect vary across country. More technically, we
add a random slope.

``` r
m3 <- lmer(PC ~ 1 + age.c + EU15 + (1 + age.c  | country), data = d)
summary(m3)
```

## Visualizations

Differences in the age effect can easily be plotted using the group
aesthetic.

``` r
ggplot(d, 
       aes(x = v497, 
           y = PC, 
           color = country)) + 
  geom_smooth(method="lm", se = F)
```
