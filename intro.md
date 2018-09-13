
# Introduction

## Acknowledgments {-}

All the credit should go to Garrett Grolemund and Hadley Wickham for writing the truly fantastic *R for Data Science* book,
without which these solutions would not exist---literally.

Special thanks to  [\@dongzhuoer](https://github.com/dongzhuoer) for a careful reading of the book
and noticing numerous issues and proposing fixes.

These solutions have benefited from those who fixed problems and contributed 
solutions. Thank you to all of those who contributed on
[GitHub](https://github.com/jrnold/r4ds-exercise-solutions/graphs/contributors) 
(in alphabetical order):
(in alphabetical order) [\@adamblake](https://github.com/adamblake), [\@benherbertson](https://github.com/benherbertson), [\@carajoos](https://github.com/carajoos), [\@dcgreaves](https://github.com/dcgreaves), [\@decoursin](https://github.com/decoursin), [\@dongzhuoer](https://github.com/dongzhuoer), [\@gvwilson](https://github.com/gvwilson), [\@JamesCuster](https://github.com/JamesCuster), [\@jmclawson](https://github.com/jmclawson), [\@KleinGeard](https://github.com/KleinGeard), [\@mjones01](https://github.com/mjones01), [\@nickcorona](https://github.com/nickcorona), [\@nzxwang](https://github.com/nzxwang), and[\@tinhb92](https://github.com/tinhb92).

## Organization {-}

The solutions are organized in the same order, and with the 
same numbers as in *R for Data Science*. Sections without
exercises are given a placeholder.

Like *R for Data Science*, packages used in each chapter are loaded in a code chunk at the start of the chapter in a section titled "Prerequisites".
If a package is used infrequently in solutions it may not 
be loaded, and functions using it will be called using the 
package name followed by two colons, as in `dplyr::mutate()` (see the *R for Data Science* [Introduction](http://r4ds.had.co.nz/introduction.html#running-r-code)).
We will also use `::` to be explicit about the package of a
function.

## Dependencies {-}

You can install all packages used in the solutions with the 
following line of code.

```r
devtools::install_github("jrnold/r4ds-exercise-solutions")
```

## Bugs/Contributing {-}

If you find any typos, errors in the solutions, have an alternative solution,
or think the solution could be improved, I would love your contributions.
Please open an issue at <https://github.com/jrnold/r4ds-exercise-solutions/issues> or a pull request at
<https://github.com/jrnold/r4ds-exercise-solutions/pulls>.

## Colophon {-}



HTML and PDF versions of this book are available at <http://jrnold.github.io/r4ds-exercise-solutions>.
The book is powered by [bookdown](https://bookdown.org) which makes it easy to turn R markdown files into HTML, PDF, and EPUB.

The source of this book is available at <https://github.com/jrnold/r4ds-exercise-solutions>
This book was built from commit [b457d90](https://github.com/jrnold/r4ds-exercise-solutions/tree/b457d90110cb86e5274c7c6fab56db0034c15e52).

This book was built with:

```r
devtools::session_info("R4DSSolutions")
#> Session info -------------------------------------------------------------
#>  setting  value                       
#>  version  R version 3.5.1 (2018-07-02)
#>  system   x86_64, darwin15.6.0        
#>  ui       X11                         
#>  language (EN)                        
#>  collate  en_US.UTF-8                 
#>  tz       America/Los_Angeles         
#>  date     2018-09-07
#> Packages -----------------------------------------------------------------
#>  package        * version    date      
#>  assertthat       0.2.0      2017-04-11
#>  babynames        0.3.0      2017-04-14
#>  backports        1.1.2      2017-12-13
#>  base64enc        0.1-3      2015-07-28
#>  BH               1.66.0-1   2018-02-13
#>  bindr            0.1.1      2018-03-13
#>  bindrcpp       * 0.2.2      2018-03-29
#>  bookdown         0.7.17     2018-08-10
#>  brew             1.0-6      2011-04-13
#>  broom            0.5.0      2018-07-17
#>  callr            3.0.0      2018-08-24
#>  cellranger       1.1.0      2016-07-27
#>  cli              1.0.0      2017-11-05
#>  clipr            0.4.1      2018-06-23
#>  codetools        0.2-15     2016-10-05
#>  colorspace       1.3-2      2016-12-14
#>  condvis          0.4-2      2017-10-11
#>  crayon           1.3.4      2017-09-16
#>  crosstalk        1.0.0      2016-12-21
#>  curl             3.2        2018-03-28
#>  datamodelr       0.2.2.9002 2018-05-27
#>  DBI              1.0.0      2018-05-02
#>  dbplyr           1.2.2      2018-07-25
#>  DiagrammeR       1.0.0      2018-03-01
#>  digest           0.6.16     2018-08-22
#>  downloader       0.4        2015-07-09
#>  dplyr          * 0.7.6      2018-06-29
#>  evaluate         0.11       2018-07-17
#>  fansi            0.3.0      2018-08-13
#>  forcats          0.3.0      2018-02-19
#>  gapminder        0.3.0      2017-10-31
#>  ggplot2          3.0.0.9000 2018-08-24
#>  ggrepel          0.8.0.9000 2018-08-10
#>  glue             1.3.0      2018-08-24
#>  graphics       * 3.5.1      2018-07-05
#>  grDevices      * 3.5.1      2018-07-05
#>  grid             3.5.1      2018-07-05
#>  gridExtra        2.3        2017-09-09
#>  gtable           0.2.0      2016-02-26
#>  haven            1.1.2      2018-06-27
#>  hexbin           1.27.2     2018-01-15
#>  highr            0.7        2018-06-09
#>  hms              0.4.2      2018-03-10
#>  htmldeps         0.1.1      2018-08-10
#>  htmltools        0.3.6      2017-04-28
#>  htmlwidgets      1.2        2018-04-19
#>  httpuv           1.4.5      2018-07-19
#>  httr             1.3.1      2017-08-20
#>  igraph           1.2.2      2018-07-27
#>  influenceR       0.1.0      2015-09-03
#>  jpeg             0.1-8      2014-01-23
#>  jsonlite         1.5        2017-06-01
#>  knitr            1.20       2018-02-20
#>  labeling         0.3        2014-08-23
#>  Lahman           6.0-0      2017-08-15
#>  later            0.7.3      2018-06-08
#>  lattice          0.20-35    2017-03-25
#>  lazyeval         0.2.1      2017-10-29
#>  leaflet          2.0.1      2018-06-04
#>  lubridate        1.7.4      2018-04-11
#>  magrittr       * 1.5        2014-11-22
#>  maps             3.3.0      2018-04-03
#>  markdown         0.8        2017-04-20
#>  MASS             7.3-50     2018-04-30
#>  Matrix           1.2-14     2018-04-13
#>  methods        * 3.5.1      2018-07-05
#>  mgcv             1.8-24     2018-06-23
#>  microbenchmark   1.4-4      2018-01-24
#>  mime             0.5        2016-07-07
#>  modelr           0.1.2      2018-05-11
#>  munsell          0.5.0      2018-06-12
#>  nlme             3.1-137    2018-04-07
#>  nycflights13     1.0.0      2018-06-26
#>  openssl          1.0.2      2018-07-30
#>  pillar           1.3.0      2018-07-14
#>  pkgconfig        2.0.2      2018-08-16
#>  plogr            0.2.0      2018-03-25
#>  plyr             1.8.4      2016-06-08
#>  png              0.1-7      2013-12-03
#>  processx         3.2.0      2018-08-16
#>  promises         1.0.1      2018-04-13
#>  pryr             0.1.4      2018-02-18
#>  ps               1.1.0      2018-08-10
#>  purrr            0.2.5      2018-05-29
#>  r4ds             0.1        2018-08-10
#>  R4DSSolutions    0.1        2018-08-10
#>  R6               2.2.2      2017-06-17
#>  raster           2.6-7      2017-11-13
#>  RColorBrewer     1.1-2      2014-12-07
#>  Rcpp             0.12.18    2018-07-23
#>  readr            1.1.1      2017-05-16
#>  readxl           1.1.0      2018-04-20
#>  rematch          1.0.1      2016-04-21
#>  reprex           0.2.0      2018-06-22
#>  reshape2         1.4.3      2017-12-11
#>  rgexf            0.15.3     2015-03-24
#>  rlang            0.2.2      2018-08-16
#>  rmarkdown        1.10.12    2018-08-24
#>  Rook             1.1-1      2014-10-20
#>  rstudioapi       0.7        2017-09-07
#>  rvest            0.3.2      2016-06-17
#>  scales           1.0.0      2018-08-09
#>  selectr          0.4-1      2018-04-06
#>  shiny            1.1.0      2018-05-17
#>  sourcetools      0.1.7      2018-04-25
#>  sp               1.3-1      2018-06-05
#>  stats          * 3.5.1      2018-07-05
#>  stringi          1.2.4      2018-07-20
#>  stringr          1.3.1      2018-05-10
#>  tibble           1.4.2      2018-01-22
#>  tidyr            0.8.1      2018-05-18
#>  tidyselect       0.2.4      2018-02-26
#>  tidyverse        1.2.1      2017-11-14
#>  tinytex          0.7        2018-08-22
#>  tools            3.5.1      2018-07-05
#>  utf8             1.1.4      2018-05-24
#>  utils          * 3.5.1      2018-07-05
#>  viridis          0.5.1      2018-03-29
#>  viridisLite      0.3.0      2018-02-01
#>  visNetwork       2.0.4      2018-06-14
#>  whisker          0.3-2      2013-04-28
#>  withr            2.1.2      2018-03-15
#>  xfun             0.3        2018-07-06
#>  XML              3.98-1.16  2018-08-19
#>  xml2             1.2.0      2018-01-24
#>  xtable           1.8-2      2016-02-05
#>  yaml             2.2.0      2018-07-25
#>  source                                   
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  Github (rstudio/bookdown@02b0fd1)        
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.1)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.1)                           
#>  CRAN (R 3.5.0)                           
#>  cran (@0.4-2)                            
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  Github (bergant/datamodelr@68ea364)      
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.1)                           
#>  cran (@0.11)                             
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  cran (@0.3.0)                            
#>  Github (tidyverse/ggplot2@adce4e2)       
#>  Github (slowkow/ggrepel@200571c)         
#>  Github (tidyverse/glue@4e74901)          
#>  local                                    
#>  local                                    
#>  local                                    
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  Github (rstudio/htmldeps@c1023e0)        
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  cran (@0.1-8)                            
#>  CRAN (R 3.5.0)                           
#>  cran (@1.20)                             
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.1)                           
#>  CRAN (R 3.5.0)                           
#>  cran (@2.0.1)                            
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.1)                           
#>  local                                    
#>  CRAN (R 3.5.1)                           
#>  cran (@1.4-4)                            
#>  CRAN (R 3.5.0)                           
#>  cran (@0.1.2)                            
#>  cran (@0.5.0)                            
#>  CRAN (R 3.5.1)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.1)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  cran (@0.1-7)                            
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  cran (@0.2.5)                            
#>  Github (hadley/r4ds@03eb8d0)             
#>  local (jrnold/r4ds-exercise-solutions@NA)
#>  CRAN (R 3.5.0)                           
#>  cran (@2.6-7)                            
#>  CRAN (R 3.5.0)                           
#>  cran (@0.12.18)                          
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  cran (@0.2.2)                            
#>  Github (rstudio/rmarkdown@6e56ad8)       
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  local                                    
#>  CRAN (R 3.5.0)                           
#>  cran (@1.3.1)                            
#>  CRAN (R 3.5.0)                           
#>  cran (@0.8.1)                            
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  local                                    
#>  CRAN (R 3.5.0)                           
#>  local                                    
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.1)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  CRAN (R 3.5.0)                           
#>  cran (@2.2.0)
```

<!-- match unopened div --><div>
