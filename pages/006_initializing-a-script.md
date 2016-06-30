---
layout: page
title: initializing an Rmd script
---

[//]: create an R Markdown script
[//]: render the pre-populated file
[//]: cleanup the directory 
[//]: add the first code chunk to initialize knitr




### open a new R markdown script 

In your workshop main directory, 

- Launch your *rr-workshop.Rproj* 
- File --> New File --> R Markdown 
- Output Format -> HTML 
- Save As --> using filename `01_calibr_data-tidying.Rmd` to the `scripts/` directory

The Rmd file is pre-populated with prose and some markdown syntax. Edit the meta-data header:

```
---
title: "Load-cell calibration --- data tidying"
author: "your name"
date: "2016-08-24"
output: html_document
---
```



### render the script 

To *knit* the Rmd file and create the output document, 

- Save the file 
- ![knit html icon](../resources/images/knit-html-icon.png)  or File --> Knit Document

The output should appear in the RStudio *Viewer* pane. If you compare the pre-populated Rmd file with the rendered output document, you'll see example of commonly-needed syntax to:  

- write headings and paragraphs 
- include executable chunks of R code 
- link to a URL 
- mark bold text  
- create a graph 

We'll see most of these Rmd structures again as we work through the tutorials. 


### output to alternative formats 

If you have MS Word installed on your machine (or Libre/Open Office on Unix-alikes), you can render the Rmd to Word using the Knit pull-down menu. We'll use Word later for a client report. 

- ![knit html icon](../resources/images/knit-word-icon.png) 

If you have TeX installed on your machine (MiKTeX on Windows, MacTeX 2013+ on OS X, TeX Live 2013+ on Unix-alikes), you can render the Rmd to PDF. (LaTeX users: using the Rnw file format you can embed R code in LaTeX files in much the same way we embed R code in Rmd files.)

- ![knit html icon](../resources/images/knit-pdf-icon.png) 

We'll use HTML output for most of our exploratory work---it renders quickly and we can ignore pagination. So for now: 

- ![knit html icon](../resources/images/knit-html-icon.png) 


### cleanup  

In the Rmd file, 

- Keep the meta-data header 
- Delete all the rest of the pre-populated text
- Save

In the directory, 

- Delete the docx and pdf output files (if any), leaving the directory with: 

```
scripts\
  |-- 01_calibr_data-tidying.html 
  `-- 01_calibr_data-tidying.Rmd 
```


### initializing knitr

A *code chunk* is R code embedded in the Rmd script. The lines of R code are preceded by

<pre><code>```{r}</code></pre>

then you write the R code, and end with

<pre><code>```</code></pre>

We have three methods for inserting code chunks: 

- You can type the delimiters directly (the three tick marks are "back ticks", not single-quotes)
- From the RStudio menu bar, Code --> Insert Chunk 
- Click the button ![](../resources/images/insert-code-chunk-icon.png)

The first code chunk we'll write comes at the top of the file, just after the YAML header. This code sets some options for *knitr* (the package that knits together our prose and R code results). 

Copy this code chunk verbatiom into your Rmd script. 

<pre class="r"><code>```{r setup, include = FALSE}
library(knitr)
opts_knit$set(root.dir = '../')
opts_chunk$set(echo = TRUE)
<code>```</code>
</code></pre>

- `setup` in  the code chunk header is a label. Labels are optional, but if used, every label must be unique.
- `include = FALSE` in the code chunk header suppresses printing for this chunk. The code is still run, but nothing is printed to the output document.
- The *library()* function loads the *knitr* package
- `root-dir` sets the working directory for *knitr* to match the working directory for the RStudio project.  
- `echo = TRUE` applies to all subsequent code chunks in the script,  printing your R code verbatim to the output document---useful during exploratory computing.
- Save and Knit. 


--- 
Back<br>
Next


