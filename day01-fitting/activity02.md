Activity 2
================

### A typical modeling process

The process that we will use for today’s activity is:

1.  Identify our research question(s),
2.  Explore (graphically and with numerical summaries) the variables of
    interest - both individually and in relationship to one another,
3.  Fit a simple linear regression model to obtain and describe model
    estimates,
4.  Assess how “good” our model is, and
5.  Predict new values.

We will continue to update/tweak/adapt this process and you are
encouraged to build your own process. Before we begin, we set up our R
session and introduce this activity’s data.

## Day 1

### The setup

We will be using two packages from Posit (formerly
[RStudio](https://posit.co/)): `{tidyverse}` and `{tidymodels}`. If you
would like to try the *ISLR* labs using these two packages instead of
base R, [Emil Hvitfeldt](https://www.emilhvitfeldt.com/) (of Posit) has
put together a [complementary online
text](https://emilhvitfeldt.github.io/ISLR-tidymodels-labs/index.html).

- In the **Packages** pane of RStudio (same area as **Files**), check to
  see if `{tidyverse}` and `{tidymodels}` are installed. Be sure to
  check both your **User Library** and **System Library**.

- If either of these are not currently listed, type the following in
  your **Console** pane, replacing `package_name` with the appropriate
  name, and press Enter/Return afterwards.

  ``` r
  # Note: the "eval = FALSE" in the above line tells R not to evaluate this code
  install.packages("package_name")
  ```

- Once you have verified that both `{tidyverse}` and `{tidymodels}` are
  installed, load these packages in the R chunk below titled `setup`.
  That is, type the following:

  ``` r
  library(tidyverse)
  library(tidymodels)
  ```

- Run the `setup` code chunk and/or **knit**
  <img src="../README-img/knit-icon.png" alt="knit" width = "20"/> icon
  your Rmd document to verify that no errors occur.

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.2     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.1     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(tidymodels)
```

    ## ── Attaching packages ────────────────────────────────────── tidymodels 1.1.0 ──
    ## ✔ broom        1.0.5     ✔ rsample      1.1.1
    ## ✔ dials        1.2.0     ✔ tune         1.1.1
    ## ✔ infer        1.0.4     ✔ workflows    1.1.3
    ## ✔ modeldata    1.1.0     ✔ workflowsets 1.0.1
    ## ✔ parsnip      1.1.0     ✔ yardstick    1.2.0
    ## ✔ recipes      1.0.6     
    ## ── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
    ## ✖ scales::discard() masks purrr::discard()
    ## ✖ dplyr::filter()   masks stats::filter()
    ## ✖ recipes::fixed()  masks stringr::fixed()
    ## ✖ dplyr::lag()      masks stats::lag()
    ## ✖ yardstick::spec() masks readr::spec()
    ## ✖ recipes::step()   masks stats::step()
    ## • Search for functions across packages at https://www.tidymodels.org/find/

``` r
library(dplyr)
```

![check-in](../README-img/noun-magnifying-glass.png) **Check in**

Test your GitHub skills by staging, committing, and pushing your changes
to GitHub and verify that your changes have been added to your GitHub
repository.

### The data

The data we’re working with is from the OpenIntro site:
`https://www.openintro.org/data/csv/hfi.csv`. Here is the “about” page:
<https://www.openintro.org/data/index.php?data=hfi>.

In the R code chunk below titled `load-data`, you will type the code
that reads in the above linked CSV file by doing the following:

- Rather than downloading this file, uploading to RStudio, then reading
  it in, explore how to load this file directly from the provided URL
  with `readr::read_csv` (`{readr}` is part of `{tidyverse}`).
- Assign this data set into a data frame named `hfi` (short for “Human
  Freedom Index”).

After doing this and viewing the loaded data, answer the following
questions:

1.  What are the dimensions of the dataset? \[1458, 123\] What does each
    row represent? \[A set of related data\]

The dataset spans a lot of years. We are only interested in data from
year 2016. In the R code chunk below titled `hfi-2016`, type the code
that does the following:

- Filter the data `hfi` data frame for year 2016, and
- Assigns the result to a data frame named `hfi_2016`.

``` r
hfi <- read_csv("hfi.csv")
```

    ## Rows: 1458 Columns: 123
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr   (3): ISO_code, countries, region
    ## dbl (120): year, pf_rol_procedural, pf_rol_civil, pf_rol_criminal, pf_rol, p...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
dim(hfi)
```

    ## [1] 1458  123

``` r
hfi_2016 <- hfi %>% filter(year == "2016") %>% select(year, ISO_code, countries, region, pf_score, pf_expression_control)
```

### 1. Identify our research question(s)

The research question is often defined by you (or your company, boss,
etc.). Today’s research question/goal is to predict a country’s personal
freedom score in 2016.

For this activity we want to explore the relationship between the
personal freedom score, `pf_score`, and the political pressures and
controls on media content index,`pf_expression_control`. Specifically,
we are going to use the political pressures and controls on media
content index to predict a country’s personal freedom score in 2016.

### 2. Explore the variables of interest

Answer the following questions (use your markdown skills) and complete
the following tasks.

2.  What type of plot would you use to display the distribution of the
    personal freedom scores, `pf_score`? \[I will use box plot, since it
    will provide a clear view of the median (center), quartiles
    (spread), and outliers. They are particularly effective for
    identifying outliers and understanding the overall variability in
    the data\] Would this be the same type of plot to display the
    distribution of the political pressures and controls on media
    content index, `pf_expression_control`? \[Yes\]

- In the R code chunk below titled `univariable-plots`, type the R code
  that displays this plot for `pf_score`.
- In the R code chunk below titled `univariable-plots`, type the R code
  that displays this plot for `pf_expression_control`.

``` r
ggplot(hfi_2016, aes(y = pf_score)) +
  geom_boxplot(fill = "grey") +
  ggtitle("Box Plot of Personal Freedom Scores") +
  ylab("Personal Freedom Score")
```

![](activity02_files/figure-gfm/distribution-plots-1.png)<!-- -->

``` r
ggplot(hfi_2016, aes(y = pf_expression_control)) +
  geom_boxplot(fill = "grey") +
  ggtitle("Box Plot of Political Pressures and Controls on Media Content Index") +
  ylab("Political Pressures and Controls on Media Content Index")
```

![](activity02_files/figure-gfm/distribution-plots-2.png)<!-- -->

4.  Comment on each of these two distributions. Be sure to describe
    their centers, spread, shape, and any potential outliers.

Answer: For pf_score: It shows a relatively high median above 7,
indicating that most countries have high personal freedom scores with a
narrow interquartile range, suggesting a low level of variability among
the middle 50% of the countries. but there are a few outliers indicating
some countries with exceptionally low personal freedom scores.

For pf_expression_control: It has a median around 5, with a wider
interquartile range, which points to greater variability in how
countries exert control over media content. This distribution appears
slightly left-skewed, indicating a tail with countries that have lower
levels of media content control; however, there are no significant
outliers depicted in this plot.

3.  What type of plot would you use to display the relationship between
    the personal freedom score, `pf_score`, and the political pressures
    and controls on media content index,`pf_expression_control`?

Answer: I would use scatterplot as it will show how these two variables
are related, whether there is a correlation, and the nature of that
correlation (positive, negative, or none).

- In the R code chunk below titled `relationship-plot`, plot this
  relationship using the variable `pf_expression_control` as the
  predictor/explanatory variable.

``` r
ggplot(hfi_2016, aes(x=pf_score, y= pf_expression_control)) + geom_point() 
```

![](activity02_files/figure-gfm/relationship-plot-1.png)<!-- -->

4.  Does the relationship look linear? If you knew a country’s
    `pf_expression_control`, or its score out of 10, with 0 being the
    most, of political pressures and controls on media content, would
    you be comfortable using a linear model to predict the personal
    freedom score?

Answer: Yes the relationship looks linear and appears to have a positive
trend, and I would be comfortable using a linear model to predict the
personal score.

#### Challenge

For each plot and using your `{dplyr}` skills, obtain the appropriate
numerical summary statistics and provide more detailed descriptions of
these plots. For example, in (4) you were asked to comment on the
center, spread, shape, and potential outliers. What measures
could/should be used to describe these? You might not know of one for
each of those terms.

Answer: For center, I will use median. For spread, I will use IQR and
SD. For shape, I will use skewness & Kurtosis For Outliers, I will
calculate Q1 and Q3 and values outside these bounds will be outliers.

What numerical summary would you use to describe the relationship
between two numerical variables? (hint: explore the `cor` function from
Base R)

``` r
# For pf_score

pf_score_stats <- hfi_2016 %>% summarise(
  Median = median(pf_score, na.rm = TRUE),
  IQR = IQR(pf_score, na.rm = TRUE),
  SD = sd(pf_score, na.rm = TRUE),
  Skewness = e1071::skewness(pf_score, na.rm = TRUE),
  Kurtosis = e1071::kurtosis(pf_expression_control, na.rm = TRUE)
)

pf_score_stats
```

    ## # A tibble: 1 × 5
    ##   Median   IQR    SD Skewness Kurtosis
    ##    <dbl> <dbl> <dbl>    <dbl>    <dbl>
    ## 1   6.93  2.12  1.49   -0.390   -0.948

``` r
# For pf_expression_control

pf_expression_control_stats<- hfi_2016 %>% summarise(
  Median = median(pf_expression_control, na.rm = TRUE),
  IQR = IQR(pf_expression_control, na.rm = TRUE),
  SD = sd(pf_expression_control, na.rm = TRUE),
  Skewness = e1071::skewness(pf_expression_control, na.rm = TRUE),
  Kurtosis = e1071::kurtosis(pf_expression_control, na.rm = TRUE)
)

pf_expression_control_stats
```

    ## # A tibble: 1 × 5
    ##   Median   IQR    SD Skewness Kurtosis
    ##    <dbl> <dbl> <dbl>    <dbl>    <dbl>
    ## 1      5  3.44  2.32  -0.0656   -0.948

``` r
# For relationship between these two

correlation <- cor(hfi_2016$pf_score, hfi_2016$pf_expression_control, use="complete.obs")
correlation
```

    ## [1] 0.8450646

### 3. Fit a simple linear regression model

Regardless of your response to (4), we will continue fitting a simple
linear regression (SLR) model to these data. The code that we will be
using to fit statistical models in this course use `{tidymodels}` - an
opinionated way to fit models in R - and this is likely new to most of
you. I will provide you with example code when I do not think you should
know what to do - i.e., anything `{tidymodels}` related.

To begin, we will create a `{parsnip}` specification for a linear model.

- In the code chunk below titled `parsnip-spec`, replace “verbatim” with
  “r” just before the code chunk title.

``` r
lm_spec <- linear_reg() %>%
  set_mode("regression") %>%
  set_engine("lm")

lm_spec
```

    ## Linear Regression Model Specification (regression)
    ## 
    ## Computational engine: lm

Note that the `set_mode("regression")` is really unnecessary/redundant
as linear models (`"lm"`) can only be regression models. It is better to
be explicit as we get comfortable with this new process. Remember that
you can type `?function_name` in the R **Console** to explore a
function’s help documentation.

The above code also outputs the `lm_spec` output. This code does not do
any calculations by itself, but rather specifies what we plan to do.

Using this specification, we can now fit our model:
$\texttt{pf\score} = \beta_0 + \beta_1 \times \texttt{pf\_expression\_control} + \varepsilon$.
Note, the “\$” portion in the previous sentence is LaTeX snytex which is
a math scripting (and other scripting) language. I do not expect you to
know this, but you will become more comfortable with this. Look at your
knitted document to see how this syntax appears.

- In the code chunk below titled `fit-lm`, replace “verbatim” with “r”
  just before the code chunk title.

``` r
slr_mod <- lm_spec %>% 
  fit(pf_score ~ pf_expression_control, data = hfi_2016)

tidy(slr_mod)
```

    ## # A tibble: 2 × 5
    ##   term                  estimate std.error statistic  p.value
    ##   <chr>                    <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)              4.28     0.149       28.8 4.23e-65
    ## 2 pf_expression_control    0.542    0.0271      20.0 2.31e-45

The above code fits our SLR model, then provides a `tidy` parameter
estimates table.

5.  Using the `tidy` output, update the below formula with the estimated
    parameters. That is, replace “intercept” and “slope” with the
    appropriate values

$\hat{\texttt{pf\score}} = 4.28 + 0.54 \times \texttt{pf\_expression\_control}$

6.  Interpret each of the estimated parameters from (5) in the context
    of this research question. That is, what do these values represent?

Answer: The intercept, The human freedom score with no amount of
political pressure on media is 4.2838153. For each additional amount of
political pressure on media content score, we would expect the human
freedom score to increase by 0.54.

## Day 2

Hopefully, you were able to interpret the SLR model parameter estimates
(i.e., the *y*-intercept and slope) as follows:

> For countries with a `pf_expression_control` of 0 (those with the
> largest amount of political pressure on media content), we expect
> their mean personal freedom score to be 4.28.

> For every 1 unit increase in `pf_expression_control` (political
> pressure on media content index), we expect a country’s mean personal
> freedom score to increase 0.542 units.

### 4. Assessing

#### 4.A: Assess with your Day 1 model

To assess our model fit, we can use $R^2$ (the coefficient of
determination), the proportion of variability in the response variable
that is explained by the explanatory variable. We use `glance` from
`{broom}` (which is automatically loaded with `{tidymodels}` - `{broom}`
is also where `tidy` is from) to access this information.

- In the code chunk below titled `glance-lm`, replace “verbatim” with
  “r” just before the code chunk title.

``` r
glance(slr_mod)
```

    ## # A tibble: 1 × 12
    ##   r.squared adj.r.squared sigma statistic  p.value    df logLik   AIC   BIC
    ##       <dbl>         <dbl> <dbl>     <dbl>    <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1     0.714         0.712 0.799      400. 2.31e-45     1  -193.  391.  400.
    ## # ℹ 3 more variables: deviance <dbl>, df.residual <int>, nobs <int>

After doing this and running the code, answer the following questions:

7.  What is the value of $R^2$ for this model?

Answer: 0.7141

8.  What does this value mean in the context of this model? Think about
    what would a “good” value of $R^2$ would be? Can/should this value
    be “perfect”?

Answer: The $R^2$ value i.e. 71.41% is quite good, indicating a strong
linear association between the two variables.

#### 4.B: Assess with test/train

You previously fit a model and evaluated it using the exact same data.
This is a bit of circular reasoning and does not provide much
information about the model’s performance. Now we will work through the
test/train process of fitting and assessing a simple linear regression
model.

Using the `diamonds` example provided to you in the Day 2 `README`, do
the following

- Create a new R code chunk and provide it with a descriptive tile
  (e.g., `train-test`).
- Set a seed.
- Create an initial 80-20 split of the `hfi_2016` dataset
- Using your initial split R object, assign the two splits into a
  training R object and a testing R object.

``` r
# Set seed before random split
set.seed(2024)

# Putting 80% of the data into the training set
hfi_2016_split <- initial_split(hfi_2016, prop = 0.80)

# Assign the two splits to data frames 
hfi_2016_train <- training(hfi_2016_split)
hfi_2016_test <- testing(hfi_2016_split)

# splits
hfi_2016_train
```

    ## # A tibble: 129 × 6
    ##     year ISO_code countries  region               pf_score pf_expression_control
    ##    <dbl> <chr>    <chr>      <chr>                   <dbl>                 <dbl>
    ##  1  2016 ISL      Iceland    Western Europe           9.08                  8.75
    ##  2  2016 CRI      Costa Rica Latin America & the…     8.17                  8.5 
    ##  3  2016 EGY      Egypt      Middle East & North…     3.89                  1.5 
    ##  4  2016 TZA      Tanzania   Sub-Saharan Africa       6.13                  4.5 
    ##  5  2016 NIC      Nicaragua  Latin America & the…     6.43                  4   
    ##  6  2016 NAM      Namibia    Sub-Saharan Africa       7.39                  6.75
    ##  7  2016 QAT      Qatar      Middle East & North…     5.53                  3.25
    ##  8  2016 CPV      Cape Verde Sub-Saharan Africa       7.99                  7.75
    ##  9  2016 BGD      Bangladesh South Asia               5.30                  3.25
    ## 10  2016 BEN      Benin      Sub-Saharan Africa       7.50                  7.25
    ## # ℹ 119 more rows

``` r
hfi_2016_test
```

    ## # A tibble: 33 × 6
    ##     year ISO_code countries     region            pf_score pf_expression_control
    ##    <dbl> <chr>    <chr>         <chr>                <dbl>                 <dbl>
    ##  1  2016 ALB      Albania       Eastern Europe        7.60                  5.25
    ##  2  2016 ARG      Argentina     Latin America & …     8.10                  5.5 
    ##  3  2016 AUT      Austria       Western Europe        9.25                  8   
    ##  4  2016 AZE      Azerbaijan    Caucasus & Centr…     5.68                  0.25
    ##  5  2016 BLR      Belarus       Eastern Europe        6.06                  2.5 
    ##  6  2016 BLZ      Belize        Latin America & …     7.43                  6.75
    ##  7  2016 BTN      Bhutan        South Asia            6.60                  5   
    ##  8  2016 BGR      Bulgaria      Eastern Europe        8.16                  5.75
    ##  9  2016 BFA      Burkina Faso  Sub-Saharan Afri…     7.46                  7   
    ## 10  2016 CIV      Cote d'Ivoire Sub-Saharan Afri…     7.06                  5.25
    ## # ℹ 23 more rows

Now, you will use your training dataset to fit a SLR model.

- In the code chunk below titled `train-fit-lm`, replace “verbatim” with
  “r” just before the code chunk title and update the data set to your
  training R object you just created.

``` r
slr_train <- lm_spec %>% 
  fit(pf_score ~ pf_expression_control, data = hfi_2016)

tidy(slr_train)
```

    ## # A tibble: 2 × 5
    ##   term                  estimate std.error statistic  p.value
    ##   <chr>                    <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)              4.28     0.149       28.8 4.23e-65
    ## 2 pf_expression_control    0.542    0.0271      20.0 2.31e-45

Notice that you can reuse the `lm_spec` specification because we are
still doing a linear model.

9.  Using the `tidy` output, update the below formula with the estimated
    parameters. That is, replace “intercept” and “slope” with the
    appropriate values

$\hat{\texttt{pf\score}} = 4.2838 + 0.5418 \times \texttt{pf\_expression\_control}$

10. Interpret each of the estimated parameters from (10) in the context
    of this research question. That is, what do these values represent?

Answer: Intercept: This value represents the estimated personal freedom
score when the political pressures and controls on media content. In the
context of the model, if a country had the highest level of media
control, its personal freedom score would still start at around 4.28.
Slope: This number indicates how much the personal freedom score is
expected to increase for each one-point decrease in the control of media
content. The personal freedom score is predicted to increase by
approximately 0.5418 points.

Now we will assess using the testing data set.

- In the code chunk below titled `train-fit-lm`, replace “verbatim” with
  “r” just before the code chunk title and update `data_test` to
  whatever R object you assigned your testing data to above.

``` r
test_aug <- augment(slr_train, new_data = hfi_2016_test)
test_aug
```

    ## # A tibble: 33 × 8
    ##     year ISO_code countries  region pf_score pf_expression_control .pred  .resid
    ##    <dbl> <chr>    <chr>      <chr>     <dbl>                 <dbl> <dbl>   <dbl>
    ##  1  2016 ALB      Albania    Easte…     7.60                  5.25  7.13  0.468 
    ##  2  2016 ARG      Argentina  Latin…     8.10                  5.5   7.26  0.836 
    ##  3  2016 AUT      Austria    Weste…     9.25                  8     8.62  0.628 
    ##  4  2016 AZE      Azerbaijan Cauca…     5.68                  0.25  4.42  1.26  
    ##  5  2016 BLR      Belarus    Easte…     6.06                  2.5   5.64  0.421 
    ##  6  2016 BLZ      Belize     Latin…     7.43                  6.75  7.94 -0.510 
    ##  7  2016 BTN      Bhutan     South…     6.60                  5     6.99 -0.392 
    ##  8  2016 BGR      Bulgaria   Easte…     8.16                  5.75  7.40  0.756 
    ##  9  2016 BFA      Burkina F… Sub-S…     7.46                  7     8.08 -0.621 
    ## 10  2016 CIV      Cote d'Iv… Sub-S…     7.06                  5.25  7.13 -0.0660
    ## # ℹ 23 more rows

This takes your SLR model and applies it to your testing data.

![check-in](../README-img/noun-magnifying-glass.png) **Check in**

Look at the various information produced by this code. Can you identify
what each column represents?

The `.fitted` column in this output can also be obtained by using
`predict` (i.e., `predict(slr_fit, new_data = data_test)`)

``` r
test_pred <- predict(slr_train, new_data = hfi_2016_test)
test_pred
```

    ## # A tibble: 33 × 1
    ##    .pred
    ##    <dbl>
    ##  1  7.13
    ##  2  7.26
    ##  3  8.62
    ##  4  4.42
    ##  5  5.64
    ##  6  7.94
    ##  7  6.99
    ##  8  7.40
    ##  9  8.08
    ## 10  7.13
    ## # ℹ 23 more rows

11. Now, using your responses to (7) and (8) as an example, assess how
    well your model fits your testing data. Compare your results here to
    your results from your Day 1 of this activity. Did this model
    perform any differently?

Answer:

### Model diagnostics

To assess whether the linear model is reliable, we should check for (1)
linearity, (2) nearly normal residuals, and (3) constant variability.
Note that the normal residuals is not really necessary for all models
(sometimes we simply want to describe a relationship for the data that
we have or population-level data, where statistical inference is not
appropriate/necessary).

In order to do these checks we need access to the fitted (predicted)
values and the residuals. We can use `broom::augment` to calculate
these.

- In the code chunk below titled `augment`, replace “verbatim” with “r”
  just before the code chunk title and update `data_test` to whatever R
  object you assigned your testing data to above.

``` r
train_aug <- augment(slr_train, new_data = hfi_2016_test)

train_aug
```

    ## # A tibble: 33 × 8
    ##     year ISO_code countries  region pf_score pf_expression_control .pred  .resid
    ##    <dbl> <chr>    <chr>      <chr>     <dbl>                 <dbl> <dbl>   <dbl>
    ##  1  2016 ALB      Albania    Easte…     7.60                  5.25  7.13  0.468 
    ##  2  2016 ARG      Argentina  Latin…     8.10                  5.5   7.26  0.836 
    ##  3  2016 AUT      Austria    Weste…     9.25                  8     8.62  0.628 
    ##  4  2016 AZE      Azerbaijan Cauca…     5.68                  0.25  4.42  1.26  
    ##  5  2016 BLR      Belarus    Easte…     6.06                  2.5   5.64  0.421 
    ##  6  2016 BLZ      Belize     Latin…     7.43                  6.75  7.94 -0.510 
    ##  7  2016 BTN      Bhutan     South…     6.60                  5     6.99 -0.392 
    ##  8  2016 BGR      Bulgaria   Easte…     8.16                  5.75  7.40  0.756 
    ##  9  2016 BFA      Burkina F… Sub-S…     7.46                  7     8.08 -0.621 
    ## 10  2016 CIV      Cote d'Iv… Sub-S…     7.06                  5.25  7.13 -0.0660
    ## # ℹ 23 more rows

**Linearity**: You already checked if the relationship between
`pf_score` and `pf_expression_control` is linear using a scatterplot. We
should also verify this condition with a plot of the residuals
vs. fitted (predicted) values.

- In the code chunk below titled `fitted-residual`, replace “verbatim”
  with “r” just before the code chunk title.

``` r
ggplot(data = train_aug, aes(x = .pred, y = .resid)) +
  geom_point() +
  geom_hline(yintercept = 0, linetype = "dashed", color = "red") +
  xlab("Fitted values") +
  ylab("Residuals")
```

![](activity02_files/figure-gfm/fitted-residual-1.png)<!-- -->

Notice here that `train_aug` can also serve as a data set because stored
within it are the fitted values ($\hat{y}$) and the residuals. Also note
that we are getting fancy with the code here. After creating the
scatterplot on the first layer (first line of code), we overlay a red
horizontal dashed line at $y = 0$ (to help us check whether the
residuals are distributed around 0), and we also rename the axis labels
to be more informative.

Answer the following question:

11. Is there any apparent pattern in the residuals plot? What does this
    indicate about the linearity of the relationship between the two
    variables?

Answer:

**Nearly normal residuals**: To check this condition, we can look at a
histogram of the residuals.

- In the code chunk below titled `residual-histogram`, replace
  “verbatim” with “r” just before the code chunk title.

``` r
ggplot(data = train_aug, aes(x = .resid)) +
  geom_histogram(binwidth = 0.25) +
  xlab("Residuals")
```

![](activity02_files/figure-gfm/fitted-residual%20histogram-1.png)<!-- -->

Answer the following question:

12. Based on the histogram, does the nearly normal residuals condition
    appear to be violated? Why or why not?

Answer:

**Constant variability**:

13. Based on the residuals vs. fitted plot, does the constant
    variability condition appear to be violated? Why or why not?

Answer:

## Attribution

This document is based on labs from
[OpenIntro](https://www.openintro.org/).

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png"
style="border-width:0" alt="Creative Commons License" /></a><br />This
work is licensed under a
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative
Commons Attribution-ShareAlike 4.0 International License</a>.
