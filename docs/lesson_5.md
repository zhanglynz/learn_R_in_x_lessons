# Lesson 5 R Functions {#L5}

Keep this in mind: In R, we use functions to get work done. So learning R in some a sense is learning how to use existed R functions and how to create new R functions. Existed R functions are those included in base R and extended R packages. In this lesson, we firstly list some base R functions, and then touch upon how to write our own R functions.

## Base R Functions

Function       |Description               | Example
:--------------|:-------------------------|:-------------
`:`            |create integer vector     | `x <- 1:5`
`c()`          |for creating vector       | `x <- c(1, 5, 8)`
`rep()`        |for creating vector       | `x <- rep(0, 100)`
`seq()`        |for creating vector       | `x <- seq(0, 1, by = 0.2)`
`length()`     |get vector's length       | `n <- length(x)`
`print()`      |print out                 | `print(x)`
`list()`       |create a list             | `my_list <- list(a = 1, b = 2:6)`
`matrix()`     |create a matrix           | `m <- matrix(1:9, 3, 3)`
`typeof()`     |get vector's type         | `typeof(x)`
`str()`        |show objects' "structure" | `str(x)`
`View()`       |RStudio Viewer: see data  | `View(a_df)`
`plot()`       |for plotting              |
`getwd()`      |get working directory     | `getwd()`
math functions |                          | `abs(x)`; `exp(x)`
stats functions|                          | `mean(x)`; `sd(x)`;`rnorm(10)`

The following code allows us to see many more base R functions:

```r
# Source: https://stackoverflow.com/questions/58476696/list-of-all-functions-in-base-r
base_packages  <-  getOption('defaultPackages')
names(base_packages)  <-  base_packages
lapply(base_packages, function (pkg) ls(paste0('package:', pkg)))
```


## Wrting R Functions

### Basics 

An R function looks like

```r
func_name <- function(arguments)
{
  body
}
```

Some points:

- "Generally, function names should be verbs, and arguments should be nouns." (Wickham and Grolemund, https://r4ds.had.co.nz/functions.html) This is a good convention; anyway, function names should make sense and often should be self explained.
- The body part can have many lines of R code; the last line is often `return(an_object)` (`an_object` can be an atomic vector, list, matrix, data frame or other OO [*Object Oriented*] object) 

### Tips 

- Use "divide and conquer" strategy to break a big task into small ones, and **write a function for each small task rather than write a huge function** for the big task. The advantages of this approach are: a) easier for debugging and maintenance; b) some small functions may be useful for other tasks.
- Use `lapply()` and other helper functions (e.g. `unlist()`) to give a vectorized function---see this in the following example, "get sum of words".
- Using `match.arg()`---see this in the following example, "add, subtract and multiply".
- Function factory---see this in the following example, "create an operation".
- Functions are "objects", and they can be put in a list---see this in the following example, "put function in a list".
- Using `UseMethod`---simple OOP---see this in the following simple example, "using `UseMethod`"

**Example:** get sum of words

```r
letter_to_nbr <- function(a_letter) 
{which(letters %in% a_letter)
}
get_sum_of_a_word <- function(a_word)
{chars <- unlist(strsplit(a_word, split = ""))
 the_nbrs <- vapply(chars, letter_to_nbr, numeric(1))
 return(sum(the_nbrs))
}
get_sum_of_words <- function(word_vec)
{unlist(lapply(word_vec, get_sum_of_a_word))
}
# testing
get_sum_of_a_word("aabbcde")
```

```
## [1] 18
```

```r
get_sum_of_words(c('a', 'ab', 'abc'))
```

```
## [1] 1 3 6
```

**Example:** using `match.arg()`

```r
add_subtr_multi <- function(x, y, method = "sum")
{method <- match.arg(method, c("sum", "subtract", "multipliction"))
 switch(method,
        "sum" = x + y,
        "subtract" = x - y,
        "multipliction" = x * y)
}
# testing
add_subtr_multi(1, 2, "sum")
```

```
## [1] 3
```

```r
add_subtr_multi(2, 1, "subtract")
```

```
## [1] 1
```

```r
add_subtr_multi(10, 10, "multipliction")
```

```
## [1] 100
```


**Example:** function factory

```r
create_an_op <- function(op)
{op <- match.arg(op, c("sum", "subtract", "multipliction"))
 function(x, y) {
   switch(op,
          "sum" = x + y,
          "subtract" = x - y,
          "multipliction" = x * y)
           
 }
}

# testing
plus_op <- create_an_op("sum")
subtr_op <- create_an_op("subtract")
multi_op <- create_an_op("multipliction")
plus_op(1, 2)
```

```
## [1] 3
```

```r
subtr_op(1, 2)
```

```
## [1] -1
```

```r
multi_op(10, 10)
```

```
## [1] 100
```

**Example:** put function in a list

```r
my_summ <- function(data)
{my_func_list <- 
  list("mean" = mean, 
       "median" = median,
       "variance" = var,
       "standard deviation" = sd,
       "range" = range)
  re <- lapply(my_func_list, function(f) f(my_data))
names(re) <- names(my_func_list)
return(re)
}
# testing
my_data <- rnorm(1000)
my_summ(my_data)
```

```
## $mean
## [1] 0.06355647
## 
## $median
## [1] 0.04917199
## 
## $variance
## [1] 1.104772
## 
## $`standard deviation`
## [1] 1.051081
## 
## $range
## [1] -3.575205  3.246290
```

**Example:** using `UseMethod`

```r
find_types <- function(x)
{UseMethod("find_types")
}
find_types.data.frame <- function(x)
{vapply(x, typeof, character(1))
}
find_types.tibble <- find_types.data.table <- find_types.data.frame
# testing
a_df <- data.frame(a = 1:5,
                   b = rnorm(5),
                   c = letters[1:5],
                   d = c(TRUE, TRUE, FALSE, FALSE, TRUE),
                   stringsAsFactors = FALSE)
(find_types(a_df))
```

```
##           a           b           c           d 
##   "integer"    "double" "character"   "logical"
```

```r
a_dt <- data.table::data.table(
  a = 1:5,
  b = rnorm(5),
  c = letters[1:5],
  d = c(TRUE, TRUE, FALSE, FALSE, TRUE))
(find_types(a_dt))
```

```
##           a           b           c           d 
##   "integer"    "double" "character"   "logical"
```

```r
a_tibble <- tibble::tibble(
  a = 1:5,
  b = rnorm(5),
  c = letters[1:5],
  d = c(TRUE, TRUE, FALSE, FALSE, TRUE)
)
(find_types(a_tibble))
```

```
##           a           b           c           d 
##   "integer"    "double" "character"   "logical"
```

