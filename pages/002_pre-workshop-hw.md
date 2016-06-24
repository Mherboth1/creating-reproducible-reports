---
layout: page
title:  pre-workshop homework 
tagline: 
---

Prior to the workshop, complete the work on this page to 

- Install [R](https://www.r-project.org/), a free software environment for statistical computing and graphics 
- Install [RStudio](https://www.rstudio.com/), a user interface for R 
- Test the installation by using R as a calculator 
- Create an RStudio "project" 
- Set up a library for R packages 
- Download one package to test the installation 

### windows

Windows users should login to Windows as an administrator to download and install R and RStudio.  

- [Download R for Windows](https://cran.r-project.org/) from the Comprehensive R Archive Network (CRAN) -> Install R for the very first time 
- [Download RStudio](http://www.rstudio.com/products/rstudio/download/) after first installing R 
- [Video instructions](https://www.youtube.com/watch?v=eD07NznguA4) for installing R and RStudio 
- [R installation manual](https://cran.r-project.org/doc/manuals/r-release/R-admin.html#Installing-R-under-Windows) if you need the minutiae 


### mac OS

- [Download R for (Mac) OS X](https://cran.r-project.org/) from the Comprehensive R Archive Network (CRAN). 
- [Download RStudio](http://www.rstudio.com/products/rstudio/download/) after first installing R 
- [Video instructions](https://www.youtube.com/watch?v=Ywj6yNfc5nM) for installing R and RStudio  
- [R installation manual](https://cran.r-project.org/doc/manuals/r-release/R-admin.html#Installing-R-under-OS-X) if you need the minutiae 


### unix-alikes

- [Download R for Unix-alikes](https://cran.r-project.org/doc/manuals/r-release/R-admin.html#Installing-R-under-Unix_002dalikes) 
- [Download RStudio](https://www.rstudio.com/products/rstudio/download/) after first installing R 

### testing

- If you like, create a desktop icon for RStudio 
- You may delete the desktop icons (if any) for R  
- Launch RStudio. You should get a window similar to this [screenshot](http://www.rstudio.com/products/rstudio/). 
- Place your cursor in the *Console* pane  
- In the Console, type `x <- 4^3` and Enter. In R, the assignment operator is `<-`.  
- In the Console, type `x`, and you should see the value  `64` printed to the Console. 
- `Ctrl+L` clears the console. 
- Session -> Restart R to clear all variables from the Environment

### set up an rstudio project 

In the RStudio window 

- File -> New Project -> Empty Project 
- Type in a name for this new directory, e.g., *rr-workshop*, and a location for it on your machine 
- Create Project 

This will be the main project directory for all workshop files 

### set up a package library 

These steps will make it easier to manage R updates and package updates.  

- In your root drive (for example, on Windows, the C: drive), create a new directory (folder) named *R* 
- In RStudio, open a new text file using File -> New File -> Text File 
- Windows users, write the following line of text in the text file. (Mac OS and Unix-alike users, type the equivalent path to the R folder you just made.)

```
R_LIBS_USER="C:/R/library" 
```

- Save this file using the name `.Renviron` in the *rr-workshop* project directory (folder) 
- When creating new projects in the future, copy the `.Renviron` file to the main directory of the new project 

Restart RStudio 

- In the lower right pane, select Packages -> Install 
- In the dialog box, type *readr* 
- Check the box to install dependencies
- Select Install 

If all goes well with the package installation, the software will report that the readr package is installed successfully in the R folder you created earlier. Your should see something like this:


    Installing package into "C:/R/library" (as "lib" is unspecified)
    trying URL "http://ftp.ussg.iu.edu/CRAN/..."
    opened URL
    package "readr" successfully unpacked and MD5 sums checked


### rstudio settings  

In RStudio Tools -> Global options, 

- General: Save workspace to .Rdata on exit, set to *Never* 
- Sweave: Weave Rnw file using, set to *knitr*


--- 
back [learning objectives](001_learning-objectives.html)<br>
next [workshop agenda](003_workshop-agenda.html) 

