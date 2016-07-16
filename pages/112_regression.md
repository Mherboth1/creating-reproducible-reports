---
layout: page
title: "perform a linear regression"
---





Packages used in this tutorial  

- readr
- tibble
- dplyr

How to use this tutorial 

- ![](../resources/images/text-icon.png)<!-- --> *add text*: type the prose verbatim into the Rmd file 
- ![](../resources/images/code-icon.png)<!-- --> *add code*: insert a code chunk then transcribe the R code 
- ![](../resources/images/knit-icon.png)<!-- --> *Knit* after each addition. 

### open a new Rmd script

Open a new Rmd file, and name it `04_calibr_regression.Rmd`. Save it to the `scripts` directory.

Edit the YAML header:  

    ---
    title: "Load-cell calibration --- regression"
    author: "your name"
    date: "date"
    output: html_document
    ---

Insert knitr setup code

![](../resources/images/code-icon.png)<!-- --> 

    library(knitr)
    opts_knit$set(root.dir = '../')
    opts_chunk$set(echo = TRUE,  collapse = TRUE)

Learn knitr

- ` collapse = TRUE` affects how source code printout appears in your document, combining the code and results in a single box. 

Knowing the packages we'll be using, we can load them right away, near the top of the file.

![](../resources/images/code-icon.png)<!-- -->


```r
# load packages
library(readr)
library(tibble)
suppressPackageStartupMessages(library(dplyr))

# for numbers printed to the screen
options(digits = 3)
```

Learn R

- `options(digits = 3)` sets decimal places for printing to the screen, but does not round or truncate the numbers

### perform the linear regression 

![](../resources/images/text-icon.png)<!-- -->

    # Perform the linear regression 

![](../resources/images/code-icon.png)<!-- -->


```r
# read the tidy data
calibr_data <- read_csv("data/02_calibr_data-tidy.csv")
head(calibr_data)
```

![](../resources/images/text-icon.png)<!-- -->

    - `input_lb` is the independent variable (*x*)
    - `output_mV` is the dependent variable (*y*)

Learning Rmd:

- A pair of single back ticks produces `typewriter` style font
- A pair of asterisks produces *italic* font
- Learn more about Rmd font styles [here](http://rmarkdown.rstudio.com/authoring_basics.html)

![](../resources/images/code-icon.png)<!-- -->


```r
# perform the regression using lm(y ~ x, data_frame)
regr_results <- lm(output_mV ~ input_lb, calibr_data)
```

Learning R:

- `lm()` is the  *linear model* function 
- `y ~ x` is an R *formula* stating that *y* is a function of *x*
- `y` and `x` are variables (named columns) in `data_frame`
- Learn more by typing `?lm` in your Console

![](../resources/images/code-icon.png)<!-- -->


```r
# examine the result
typeof(regr_results)
attributes(regr_results)
```

Learning R:

- `typeof()` tells us what the object is. 
- `attributes()` yields metadata about the object

### extract relevant results 

![](../resources/images/text-icon.png)<!-- -->

    # Extract relevant results 
    
    The output of `lm()` is a list of 12 named objects. 

    The three named objects we want are  `coefficients`, `residuals`, and `fitted.values`. 

![](../resources/images/code-icon.png)<!-- -->


```r
# Examine the three objects we want
objects_to_subset <- c("coefficients", "residuals", "fitted.values")
relevant_results  <- regr_results[objects_to_subset]
str(relevant_results)
```

- `c()` creates a vector (concatenate)
- Single brackets `[]` subsets a list and returns a list 

### check yourself

Confer with a neighbor.

1. How many elements in the `coefficients` list?
2. What is the numeric value of the regression intercept? of the slope? 
3. How many elements in the `residuals` vector? Is that what you expect? 

In our linear model, `lm()`, the coefficient of the $x^0$ term is labeled *Intercept*; the coefficient of the $x^1$ term is labeled the 
*input_lb* coefficient, i.e., the  coefficient of $x^1$. This is our slope.   (Higher-order polynomial-fits would have additional coefficients, of course, for $x^2, x^3$, etc.)

![](../resources/images/text-icon.png)<!-- -->

    Extract the named objects from inside the `relevant_results` list.

![](../resources/images/code-icon.png)<!-- -->


```r
# first pass returns named numbers
coeff <- relevant_results[['coefficients']] 
resid <- relevant_results[['residuals']]
y_fit <- relevant_results[['fitted.values']]
str(coeff)
str(resid)
str(y_fit)
```

Learn R

- Double brackets `[[]]` subsets a list and returns its components 
- `coeff`, `resid`, and `y_fit` are all *named numbers*

We use the names of the numbers in `coeff` to obtain the slope and intercept of the regression.

![](../resources/images/code-icon.png)<!-- -->


```r
# extract the values
intercept <- coeff[['(Intercept)']]
slope     <- coeff[['input_lb']]
resid     <- unname(resid)
y_fit     <- unname(y_fit)
str(intercept)
str(slope)
str(resid)
str(y_fit)
```

Learn R

- `[[]]` subsets a named number and returns the number 
- `unname()` removes the names from named numbers
- Compare the results of `str()` here with those in the previous code chunk

The previous steps could have been combined, e.g., 

- `intercept <- relevant_results[['coefficients']][['(Intercept)']]`
- `y_fit     <- unname(relevant_results[['fitted.values']])`



### determine sensor accuracy

![](../resources/images/text-icon.png)<!-- -->

    # Determine sensor accuracy

    The ANSI/ISA standard defines accuracy as the maximum positive and maximum negative residual. We conventionally report the largest absolute value of the two. 

![](../resources/images/code-icon.png)<!-- -->


```r
resid_max   <- max(resid)
resid_min   <- abs(min(resid))
resid_bound <- max(resid_max, resid_min)
```

- `max()` and `min()` are R functions that do the obvious
- `abs()` returns the absolute value

![](../resources/images/text-icon.png)<!-- -->

    The standard defines output span as the difference between the max and min y-fitted values. Accuracy is usually reported as a percentage of output span. 

![](../resources/images/code-icon.png)<!-- -->


```r
output_span <- max(y_fit) - min(y_fit)
accuracy    <- resid_bound / output_span * 100
accuracy
```

R coding practice:

- Place a space before and after math operators, e.g., `+`, `-`, `/`, `*`, etc. for readability

### check yourself

Confer with a neighbor.

1. What is your sensor accuracy in percent?

### collect results 

![](../resources/images/text-icon.png)<!-- -->

    # Collect results 

    Extract min/max inputs and outputs for the report. 

![](../resources/images/code-icon.png)<!-- -->


```r
input_min  <- min(calibr_data[['input_lb']])
input_max  <- max(calibr_data[['input_lb']])
output_min <- min(calibr_data[['output_mV']])
output_max <- max(calibr_data[['output_mV']])
```

![](../resources/images/text-icon.png)<!-- -->

    Collect those results I'd like to keep handy. 

![](../resources/images/code-icon.png)<!-- -->


```r
# create a data frame for saving specific results to file
calibr_outcomes <- frame_data(
	~item,         ~num,        ~unit,
	'input_min',   input_min,   'lb',
	'input_max',   input_max,   'lb',
	'output_min',  output_min,  'mV',
	'output_max',  output_max,  'mV',
	'slope',       slope,       'mV/lb',
	'intercept',   intercept,   'mV',
	'resid_bound', resid_bound, 'mV',
	'accuracy',    accuracy,    '%'
)
calibr_outcomes
```

Learn R.

- `frame_data` from the `tibble` package allows us to create a data frame row by row. 
- `~` (in the first row) creates the column names of the data frame
- `item` is the label column and `num` is the value column 

### write results to file

![](../resources/images/text-icon.png)<!-- -->

    # Write results to file
    
    Save the `calibr_outcome` data frame to file. 

![](../resources/images/code-icon.png)<!-- -->


```r
write_csv(calibr_outcomes, "results/04_calibr_outcomes.csv")
```

- `results` directory is in the path because this information is used in the client report
- `write_csv()` operates on data frames, so I assembled `calibr_outcomes` as a data frame 
- I use CSV to abide by the reproducibility principle that everything should be a text file (tab-delimited works too, I just prefer CSV)

### check yourself

Navigate to your scripts and results directories. They should look like this:

    scripts\
      |-- 01_calibr_data-reshaping.Rmd 
      |-- 02_calibr_data-tidying.Rmd
      |-- 03_calibr_graph.Rmd
      `-- 04_calibr_regression.Rmd
      
    results\
      |-- 02_calibr_data-tidying.csv
      |-- 03_calibr_graph.png
      `-- 04_calibr_regression-results.csv

---
Back [create the calibration graph](111_graph.html)<br>
Next [write the client report](113_report.html)