R: Basic Statistics
================
Philipp Masur
2022-11

-   <a href="#introduction" id="toc-introduction">Introduction</a>
    -   <a href="#loading-packages-and-data"
        id="toc-loading-packages-and-data">Loading packages and data</a>
    -   <a href="#exploring-the-data-set"
        id="toc-exploring-the-data-set">Exploring the data set</a>
-   <a href="#t-tests" id="toc-t-tests">T-Tests</a>
-   <a href="#anova" id="toc-anova">ANOVA</a>
-   <a href="#regression-analysis" id="toc-regression-analysis">Regression
    Analysis</a>

# Introduction

In this tutorial, we will learn about basic statistical procedures, such
as t-tests, ANOVAs, and regression analysis. It will be fairly quick and
just the pure basics, but it should get you started with regard to
running simple models.

## Loading packages and data

We are going to use a small (sub-) data set that is based on Masur,
DiFranzo, & Bazarova (2021). It represents an online experiment in which
participants were confronted with a social media feed that varied with
regard to a) how many posts disclosed the users visually (0%, 20%, 80%)
and b) how many profile pictures disclosed the users visually (0%, 20%,
80%). Let’s load the data and explore it a bit.

``` r
library(tidyverse)
library(psych)

d <- read_csv("https://raw.githubusercontent.com/masurp/VU_CADC/main/tutorials/data/masur-et-al_data.csv")
head(d)
names(d)
```

## Exploring the data set

Let’s produce some simple descriptive statistics to understand the
sample and the distribution across experimental conditions.

``` r
# Age
describe(d$age)

# Gender
d %>%
  group_by(gender) %>%
  count %>%
  mutate(prop = n/nrow(d))

# Experimental conditions
d %>%
  group_by(norm, profile) %>%
  count %>%
  mutate(prop = n/nrow(d))
```

# T-Tests

In a first step, we are going to test whether norm perceptions (one of
the dependent variables) differs by gender.

``` r
# Recoding of gender
d <- d %>%
  mutate(gender_f = ifelse(gender == 1, "female", 
                           ifelse(gender == 0, "male", NA)))

# Running a simple t-test
t.test(norm_perc ~ gender_f, d)
```

Let’s also plot the results:

``` r
d %>% 
  select(gender_f, norm_perc) %>%
  na.omit %>%
  ggplot(aes(x = gender_f, y = norm_perc)) + 
  stat_summary(stat = "identity")
```

# ANOVA

Let’s turn to the actual exprimental effects that were tested. For this,
we use an Analysis of Variance (ANOVA) as the independent variables are
two factors with three levels each.

``` r
library(emmeans)

# Estimating the model
m1 <- aov(norm_perc ~ norm * profile, d)
summary(m1)

# Estimated marginal means
(m1_means <- emmeans(m1, ~ norm | profile))

# Post-hoc tests
pairs(m1_means, adjust = "tukey")
```

Again, we should plot our results. Interestingly, we can work directly
with the output of the emmeans function!

``` r
m1_means %>% 
    as.tibble %>%
  ggplot(aes(x = norm, 
             y = emmean, 
             ymin = lower.CL, 
             ymax = upper.CL,
             fill = profile)) +
    geom_col(stat = "identity",
             position=position_dodge(),
             color = "white") +
    geom_errorbar(stat = "identity",
                  width = .3,
                  position = position_dodge(.9)) +
  scale_fill_brewer(palette = "Blues") +
    ylim(0,7) +
    labs(x = "Number of posts with disclosure (Norm)", 
         y = "Intention to Self-Disclose",
         fill = "Number of\nidentifiable\nprofile pictures (Profile)") +
  theme_classic()
```

# Regression Analysis

Finally, let’s explore shortly how we can estimate a linear regression
model.

``` r
# Estimating linear, stepwise models
m2a <- lm(disclosure ~ gender_f + norm + profile, d)
m2b <- lm(disclosure ~ gender_f + norm + profile + norm_perc, d)

# Summarize models
summary(m2a)
summary(m2b)

# Confidence intervals
confint(m2b)
```

Again, there are different ways to visualize the results, e.g., using a
coefficient plot or a scatter plot.

``` r
# Coefficient plot
summary(m2b)$coef %>%
  as.data.frame %>%
  rownames_to_column("predictors") %>%
  mutate(ll = Estimate - 1.96*`Std. Error`,
         ul = Estimate + 1.96*`Std. Error`) %>%
  ggplot(aes(x = predictors, y = Estimate, ymin = ll, ymax = ul)) +
  geom_hline(yintercept = 0, linetype = "dashed") +
  geom_pointrange() +
  coord_flip() +
  theme_classic()

# Scatterplot
ggplot(d, aes(x = norm_perc, y = disclosure)) +
  geom_point(alpha = .5) +
  geom_smooth(method = "lm") +
  theme_classic()
```
