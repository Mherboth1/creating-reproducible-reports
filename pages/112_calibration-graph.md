---
layout: page
title: "calibration graph"
---





Packages used in this tutorial  

- readr
- ggplot2

How to use this tutorial 

- ![](../resources/images/text-icon.png)<!-- --> *add text*: type the prose verbatim into the Rmd file 
- ![](../resources/images/code-icon.png)<!-- --> *add code*: insert a code chunk then transcribe the R code 
- ![](../resources/images/knit-icon.png)<!-- --> *Knit* after each addition. 

### open a new Rmd script

We start a new Rmd script to graph the calibration data with regression line and save the graph to file.  

- Open a new Rmd file, and name it `04_calibr_graph.Rmd`
- Start the new script with the same YAML header as the previous script
- Change the title to `"Load-cell calibration --- graph"`
- Insert the same code chunk for the knitr setup 

Knowing the packages we'll be using, we can load them right away, near the top of the file.

![](../resources/images/code-icon.png)<!-- -->


```r
# load packages
library(readr)
library(ggplot2)
```

You will most likely encounter three main graphical systems in R: `base graphics`, `lattice`, and `ggplot2`. We'll use `ggplot2`, written by the [nerd famous Hadley Wickham](http://priceonomics.com/hadley-wickham-the-man-who-revolutionized-r/), and currently the most popular R graphics system. 

You should learn to recognize syntax from the other systems too. With so many code samples online, you will eventually want to copy someone's code as a basis for a new graph, and you can't use, say, `lattice` syntax in a `ggplot2` environment. To illustrate the difference in the three systems, scatter plots are drawn using: 

- `plot(x, y, ...)` in `base`
- `xyplot(y ~ x, ...)` in `lattice` 
- `geom_point(aes(x, y, ...))` in `ggplot2`

Robert Kabacoff, the author of the online reference [Quick-R](http://www.statmethods.net/) and the book [R in Action  2/e](https://www.manning.com/books/r-in-action-second-edition) (if you buy one book to help you learn R, I recommend this one), summarizes R graphics systems [here](http://www.slideshare.net/kabacoff/r-for-data-visualization-and-graphics-31577136?next_slideshow=1). Also, an anonymous blogger compares lattice to ggplot2 in a series of posts [here](https://learnr.wordpress.com/2009/08/26/ggplot2-version-of-figures-in-lattice-multivariate-data-visualization-with-r-final-part/). 

Having used `lattice` for years now, I am a novice `ggplot2` user like you, but I'm convinced it's the package to learn --- all the cool kids are using it. 

### data

![](../resources/images/text-icon.png)<!-- -->

    # Data
    
    Read the data set saved during the regression analysis.

![](../resources/images/code-icon.png)<!-- -->


```r
graph_data <- read_csv("results/03_calibr_regression_graph-data.csv")
str(graph_data)
```

### check yourself

Confer with a neighbor.

1. How many variables in this data frame?
2. How will they be used in creating the graph? 

### building the graph in layers 
    
![](../resources/images/text-icon.png)<!-- -->

    # Building the graph in layers 
    
    Just show the data. 

![](../resources/images/code-icon.png)<!-- -->


```r
calibr_graph <- ggplot(data = graph_data) +
	geom_point(aes(x = input_lb, y = output_mV))
```

Learning ggplot2

- `ggplot(data = graph_data)` brings in the data frame
- `+` is the syntax for "add the next layer"
- `geom_point()` is a **geom**etric object. We use points because we're drawing a scatterplot
- `aes()` is an **aes**thetic mapping from the data to the `geom_point`. Here, *x* and *y* are the input and output columns from the data frame

![](../resources/images/code-icon.png)<!-- -->


```r
print(calibr_graph)
```

That's all it takes to obtain a basic scatterplot. If you don't care for the default graphic settings, don't worry, I'll show you how to edit basic settings. 


![](../resources/images/text-icon.png)<!-- -->

    Draw the regression line.  

![](../resources/images/code-icon.png)<!-- -->


```r
calibr_graph <- ggplot(data = graph_data) +
	geom_line(aes(x = input_lb, y = fit_mV)) + 
	geom_point(aes(x = input_lb, y = output_mV))
```

Learning ggplot2

- `geom_line()` is a geom for drawing lines
- Layers are drawn consecutively, so by drawing `geom_point()` after `geom_line()`, the points (data markers) overprint the line. 

![](../resources/images/code-icon.png)<!-- -->


```r
print(calibr_graph)
```

### check yourself

Confer with a neighbor

1. Explain why `geom_line()` and  `geom_point()` have different `aes()` arguments.  

### changing the attributes of graphical elements

![](../resources/images/text-icon.png)<!-- -->

    # Changing the attributes of graphical elements 
    
    Start with the line and the data markers.

![](../resources/images/code-icon.png)<!-- -->


```r
calibr_graph <- ggplot(data = graph_data) +
	geom_line(aes(x = input_lb, y = fit_mV), size = 0.5, color = 'gray70') + 
	geom_point(aes(x = input_lb, y = output_mV), size = 1.5, shape = 21, 
						 stroke = 0.7,  color = 'black', fill= 'gray70')
```

Learning ggplot2

- The line is edited in size (line width in mm) and color. 
- The point is edited in size (diameter in mm),  [shape](http://docs.ggplot2.org/current/vignettes/ggplot2-specs.html), stroke and color (outer circle), and fill color. 
- R has 657 built-in named colors such as `gray70` and `black` I've used here. You can see the full list of color names by typing `colors()` in the Console. View the colors [here](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf). 

![](../resources/images/code-icon.png)<!-- -->


```r
print(calibr_graph)
```

![](../resources/images/text-icon.png)<!-- -->

    Edit the axis labels. 

![](../resources/images/code-icon.png)<!-- -->


```r
calibr_graph <- calibr_graph + 
	xlab("Applied force (lb)") + 
	ylab("Sensor reading (mV)")
```

Learning ggplot2

- This chunk could be read "Assign `calibr_graph` to itself, add a layer for the x-axis label, and add a layer for the y-axis label."

![](../resources/images/code-icon.png)<!-- -->


```r
print(calibr_graph)
```

![](../resources/images/text-icon.png)<!-- -->

    Edit the locations of the axis markings to match the test points. 

![](../resources/images/code-icon.png)<!-- -->


```r
calibr_graph <- calibr_graph + 
	scale_x_continuous(breaks = seq(from = 0.5, to = 4.5, by = 1)) +
	scale_y_continuous(breaks = seq(from = 10, to = 90, by = 20))
```

Learning R

- `breaks` is ggplot2 syntax for tick mark locations
- Sequences have the form `seq (from = ..., to = ..., by = ...)` as shown here. You can shorten the code by omitting the *key words* as long as the numbers are in the order shown. For example, I could have written `seq(10, 90, 20)` and obtained the same result, the vector `(10, 30, 50, 70, 90)`. 

![](../resources/images/code-icon.png)<!-- -->


```r
print(calibr_graph)
```

![](../resources/images/text-icon.png)<!-- -->

    Assign a different theme. 

![](../resources/images/code-icon.png)<!-- -->


```r
calibr_graph <- calibr_graph + 
	theme_light()
```

Learning ggplot2

- A theme, in ggplot2, is a collection of settings that controls the appearance of the graph. Example images of the built-in themes are [here](http://docs.ggplot2.org/current/ggtheme.html). 
- Additional themes are available in the [ggthemes package](https://github.com/jrnold/ggthemes), including themes inspired by Stephen Few, Edward Tufte, The Economist magazine, and even the classic 2003 ugly gray charts in Excel ("for ironic purposes only").  

![](../resources/images/code-icon.png)<!-- -->


```r
print(calibr_graph)
```

![](../resources/images/text-icon.png)<!-- -->

    Edit the theme.  

![](../resources/images/code-icon.png)<!-- -->


```r
calibr_graph <- calibr_graph +
		theme(panel.grid.minor = element_blank(),  
					axis.ticks.length = unit(2, "mm"))
```

Learning ggplot2

- `element_blank()` turns off the minor grid lines
- `axis.ticks.length` edits the length of the axis tick marks

The ability to edit or create a theme is the essential tool for customizing graphs in `ggplot2`. I've show two small customizations; the full list is given [here](http://rstudio-pubs-static.s3.amazonaws.com/3364_d1a578f521174152b46b19d0c83cbe7e.html).  

![](../resources/images/code-icon.png)<!-- -->


```r
print(calibr_graph)
```

### putting it all together 

![](../resources/images/text-icon.png)<!-- -->

    # Putting it all together
    
    We've written out the layers in separate code chunks for clarity, but they can be assembled into one code chunk in the same order as before. 
    
![](../resources/images/code-icon.png)<!-- -->


```r
calibr_graph <- ggplot(graph_data) +
	geom_line(aes(input_lb, fit_mV), size = 0.4, color = 'gray70') + 
	geom_point(aes(input_lb, output_mV), size = 1.5, stroke = 0.7, shape = 21, 
						 color = 'black', fill = 'gray70') + 
	xlab("Applied force (lb)") + 
	ylab("Sensor reading (mV)") +
	scale_x_continuous(breaks = seq(0.5, 4.5, 1)) +
	scale_y_continuous(breaks = seq(10, 90, 20)) +
	theme_light() +
	theme(panel.grid.minor = element_blank())
```

Learn ggplot2. Earlier, we shortened the `seq()` function by writing the arguments in a particular order. We can do the same with other functions: 

- From `ggplot()`, you can omit `data = `, leaving the data frame name
- From `aes()`, you can omit `x = ` and `y = `, leaving the x-variable and y-variable names in that order

![](../resources/images/code-icon.png)<!-- -->


```r
# print to screen
print(calibr_graph)
```

And, print to file. 

![](../resources/images/code-icon.png)<!-- -->


```r
# print to file
ggsave("results/04_calibr_graph.png", plot = calibr_graph, 
			 width = 6, height = 4, units = "in", dpi = 300)
```

Learning ggplot2

- Save to the `results` directory because the graph will be used in a report
- `dpi = 300` for using in a print document
- `dpi` $\times$ `width` and `height` in inches yields screen dimension in pixels (72 ppi for web resolution is a  [myth](http://www.photoshopessentials.com/essentials/the-72-ppi-web-resolution-myth/)).

### advanced graphics

There's much more we can do to bring the graph up to publication standards. 

I've prepared an [advanced](113_advanced_graph.html) (and completely optional) tutorial for those of you who want to know MORE and you want it NOW.  Topics include: 

- Jitter the data markers to reduce printing overlap and set a random seed so jittering is repeatable 
- Use the results table to dynamically assign units to the axis labels and set  tick mark locations
- Use the results table to dynamically edit the calibration equation and print it on the graph
- Format the calibration equation for significant figures
- Change font sizes





