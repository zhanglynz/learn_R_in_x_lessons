# Lesson 6 R Packages {#L6}

An R package basically is package of R functions plus metadata and documentations.



Base R (the R that we download from CRAN) has at least these R packages

- base
- methods
- stats
- utils
- graphics
- grDevices
- datasets

We call all the other packages that not included in base R **extended R packages**. In this lesson, we will very briefly introduce some extended R packages, and then touch upon writing R packages.

## Extended R Packages

package name      | remarks
:-----------------|:-------------------------------
`tidyverse`       | a set of packages; for data manipulation/analysis/visualization
`data.table`      | for data manipulation; enabling fast code
`lubridate`       | for dealing with dates, times etc.
`shiny`           | for creating shiny apps
`bookdown`        | for writing books
`blogdown`        | for creating web sites
`devtools`        | for creating R packages

**NB:** We can see all packages included in `tidyverse`

```r
tidy_pkgs <- tidyverse::tidyverse_packages(include_self = FALSE)
print(tidy_pkgs)
```

```
##  [1] "broom"         "cli"           "crayon"        "dbplyr"       
##  [5] "dplyr"         "dtplyr"        "forcats"       "ggplot2"      
##  [9] "googledrive"   "googlesheets4" "haven"         "hms"          
## [13] "httr"          "jsonlite"      "lubridate"     "magrittr"     
## [17] "modelr"        "pillar"        "purrr"         "readr"        
## [21] "readxl"        "reprex"        "rlang"         "rstudioapi"   
## [25] "rvest"         "stringr"       "tibble"        "tidyr"        
## [29] "xml2"
```
We can Install all these packages in the tidyverse by running `install.packages("tidyverse")` .
Among these 29 packages, **ggplot2**, **dplyr**, **tidyr**,
**readr**, **purrr**, **tibble**, **stringr**, and **forcats** are the so-called *core packages*, which will be loaded all together if we run `library(tidyverse)`. See this: https://www.tidyverse.org/packages/

These articles may be of our interest:

- https://support.posit.co/hc/en-us/articles/201057987-Quick-list-of-useful-R-packages
- https://www.datacamp.com/tutorial/top-ten-most-important-packages-in-r-for-data-science
- https://www.r-bloggers.com/2021/04/15-essential-packages-in-r-for-data-science/
- https://towardsdatascience.com/the-most-underrated-r-packages-254e4a6516a1



## Writing R Packages

The following steps are distilled from Chapter 2 of Hadley Wickham and Jenny Bryan (https://r-pkgs.org/).

I am going to make an R package called `BookStarter`.

- Preparation: Create an empty repo at Github, e.g. "git@github.com:zhanglynz/BookStarter.git"
- Use `create_package()` (only once) to scaffold a new R package
````
library(devtools)
create_package("F:/Projects/BookStarter") 
````
- Use `use_r()` to start an  R function 
```` 
library(devtools)
use_r("start_book") 
In the source editor and put the cursor at the start of the new function
Now do Code > Insert roxygen skeleton
````
- `load_all()`
- `check()`
- Edit DESCRIPTION
- Use `use_mit_license()` (only use once)
- Use `document()` to create help document for the function
- `check()` # again
- Testing
````
use_testthat() # only use it once
use_test("start_book")
````
- Use `use_package("some_a_package")` to add  dependence
- Gitlab/Github # Git the project; push to Gitlab
````
# push an existing repository from the command line
git init
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:zhanglynz/BookStarter.git
git push -u origin main
````
- `use_readme_rmd()` # only use it once
- `check()` # again 
- `install()`

