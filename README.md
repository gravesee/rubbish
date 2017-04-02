
<!-- README.md is generated from README.Rmd. Please edit that file -->
    ## Loading required package: rubbish

What is `rubbish`?
==================

`rubbish` is an R package that helps modelers build scorecards. Scorecard models need to be more than predictive. Regulatory oversight often necessitates they be transparent as well. Model transparency is difficult to enforce in more predictive, non-linear methods such as neural networks, random forest, or gradient boosted decision trees. Often direct modeler intervention is required to ensure proper treatment is given for values of independent variables.

`rubbish` attempts to solve these problems by providing interactive variable manipulation facilities giving the modeler total control over how variables are treated in scorecard models.

Installing `rubbish`
--------------------

The easiest way to install `rubbish` is by using the `devtools` package:

``` r
if (!require(devtools)) install.packages("devtools")
devtools::install_github(repo="Zelazny7/rubbish")
```

Overview of `rubbish`
---------------------

`rubbish` is comprised of three main modeling steps: Bin, Fit, & Adjust. Each of these steps are outlined in the following sections.

### Bin

The Bin phase prepares a data set of explanatory features for modeling by discretizing variables and wrapping in an object that provides data manipulation capabilities. How variables are treated by the binning step depends on their type. Numeric features such as age or income are discretized using a performance metric. For example, if the performance variable is binary, the numeric independent variables are discretized using information value.

If the variable is a categorical field (factors in R) no discretization can be performed; however, the features is still wrapped by a bin object and can be interacted with as such.

``` r
mod <- bin(data=titanic[,-1], y=titanic$Survived)
```

Calling the `bin` function returns a Scorecard object. The Scorecard object contains methods for manipulating and fitting the discretized variables, or "bins" in rubbish parlance. The Scorecard object maintains a list of bins as and keeps track of the operations requested by the modelers. It also contains a list of fitted scorecard models that can be adjusted or reviewed. These capabilities will be expanded upon in later sections.

``` r
> mod
#> 1 models
#>  |-- *  scratch              | 00.0 ks |
```

### Fit

The second step of scorecard developement using `rubbish` is fitting a model. `rubbish` uses the `glmnet` package under the hood to fit a regularized regression model. The discretized variables have their observed performance values substituted for their actual input values to create a completely continuous dataset.

> For example, if an age variable is discretized using binary performance the resulting substitution uses the observed weight-of-evidence rather than the age itself.

Performance substitution is used to put all predictors on the same scale and force a linear relationship between each predictor and the response variables.

``` r
mod$fit("model 1", "initial model with all variables")
```

The model method belonds
