flextable R package
================

-   [API](#api)
-   [Installation](#installation)
-   [Quick demo](#quick-demo)

<!-- README.md is generated from README.Rmd. Please edit that file -->
[![Build Status](https://travis-ci.org/davidgohel/flextable.svg?branch=master)](https://travis-ci.org/davidgohel/flextable) [![Coverage Status](https://img.shields.io/codecov/c/github/davidgohel/flextable/master.svg)](https://codecov.io/github/davidgohel/flextable?branch=master) [![CRAN version](http://www.r-pkg.org/badges/version/flextable)](https://cran.r-project.org/package=flextable) ![](http://cranlogs.r-pkg.org/badges/grand-total/flextable) [![Project Status: WIP - Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](http://www.repostatus.org/badges/latest/wip.svg)](http://www.repostatus.org/#wip)

API
---

A tabular reporting tool that let R users create tables for reporting and control their formatting properties and their layout.

-   Manage headers
-   Format text, paragraphs, cells and border of any element
-   Separate data from displayed values
-   Outut availables are Microsoft Word, PowerPoint and HTML (as an `htmlwidgets` object).

Installation
------------

You can get the development version from GitHub:

``` r
devtools::install_github("davidgohel/oxbase")
devtools::install_github("davidgohel/flextable")
```

Quick demo
----------

`flextable` is designed for use with the `%>%` pipe operator. You can make this `%>%` operator available in your R workspace by loading package [`magrittr`](https://cran.r-project.org/package=magrittr).

``` r
library("magrittr")
library("flextable")
library("oxbase")
library("dplyr")
```

The following dataset will be used as input paramter of `flextable` call:

``` r
mydata <- iris %>% 
  group_by(Species) %>% 
  do( head(., n = 3) )
mydata
#> Source: local data frame [9 x 5]
#> Groups: Species [3]
#> 
#>   Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
#>          <dbl>       <dbl>        <dbl>       <dbl>     <fctr>
#> 1          5.1         3.5          1.4         0.2     setosa
#> 2          4.9         3.0          1.4         0.2     setosa
#> 3          4.7         3.2          1.3         0.2     setosa
#> 4          7.0         3.2          4.7         1.4 versicolor
#> 5          6.4         3.2          4.5         1.5 versicolor
#> 6          6.9         3.1          4.9         1.5 versicolor
#> 7          6.3         3.3          6.0         2.5  virginica
#> 8          5.8         2.7          5.1         1.9  virginica
#> 9          7.1         3.0          5.9         2.1  virginica
```

The following styles are to be used later.

``` r
def_txt <- fp_text(font.size = 9, font.family = "Arial", color = "gray40")
def_head <- update(def_txt, font.size = 11, color = "gray20")
def_cell <- fp_cell( border = fp_border(color = "gray") )
```

First, define the flextable.

``` r
ft <- flextable(mydata, col_keys = c("Species", 
    "Sepal.Length", "Sepal.Width", "Petal.Length", 
    "Petal.Width") )
```

Add titles to header part.

``` r
ft <- flextable(mydata, col_keys = c("Species", 
    "Sepal.Length", "Sepal.Width", "Petal.Length", 
    "Petal.Width") ) %>%
  set_header_labels(Sepal.Length = "Length", 
             Sepal.Width = "Width",
             Petal.Length = "Length",
             Petal.Width = "Width", 
             Species = "Species" ) %>%
  add_header(Sepal.Length = "Sepal", 
             Sepal.Width = "Sepal",
             Petal.Length = "Petal",
             Petal.Width = "Petal", 
             Species = "Species") 
```

Merge some cells.

``` r
ft <- ft %>% 
  merge_h(part = "header") %>% 
  merge_v(part = "header") %>% 
  merge_v(j = "Species" ) 
```

Then apply some formatting properties :

``` r
ft %>% 
  theme_tron() %>% 
  autofit() %>% 
  tabwid()
```

![](README_files/figure-markdown_github/tabwid_example-1.png)