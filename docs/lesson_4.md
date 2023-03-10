# Lesson 4 Data Structures {#L4}

**Acknowledgments:** This chapter is written after I read Chapter 3 of Hadley Wickham's book, *Advanced R* 2nd Ed, https://adv-r.hadley.nz/vectors-chap.html


In R, the fundamentally important data structure is **vector,** which can be roughly defined as *a tuple of elements.* R vectors include **atomic vectors** and **lists**^[According to Hadley Wickham, *Advanced R* 2nd Ed., "`NULL` is closely related to vectors and often serves the role of a generic zero length vector."]. Other R data structures are built upon vectors by adding more *attributes.* Talking about attributes, I want to quote words from Hadley Wickham (*Advanced R* 2nd Ed, https://adv-r.hadley.nz/vectors-chap.html):

>
The most important attributes are names, dimensions and class.
>

>
Two attributes are particularly important. The dimension attribute turns vectors into matrices and arrays and the class attribute powers the S3 object system.
>


## Atomic Vectors

The most useful atomic vectors are **logical**, **integer**, **double** and **character** types.

**Examples:**

```r
(a_logic_vec <- c(TRUE, FALSE, TRUE))
```

```
## [1]  TRUE FALSE  TRUE
```

```r
(an_integer_vec <- 1L:6L)
```

```
## [1] 1 2 3 4 5 6
```

```r
(a_double_vec <- c(1, 2, 3, 4, 5, 6))
```

```
## [1] 1 2 3 4 5 6
```

```r
(identical(an_integer_vec, a_double_vec))
```

```
## [1] FALSE
```

```r
(a_character_vec <- c('a', 'b', 'c', '123'))
```

```
## [1] "a"   "b"   "c"   "123"
```

We can use R function `typeof()` to find out the type of an atomic vector.

```r
c(typeof(a_logic_vec), 
  typeof(an_integer_vec), 
  typeof(a_double_vec),
  typeof(a_character_vec))
```

```
## [1] "logical"   "integer"   "double"    "character"
```
or

```r
vapply(list(a_logic_vec, 
            an_integer_vec, 
            a_double_vec, 
            a_character_vec), typeof, character(1L))
```

```
## [1] "logical"   "integer"   "double"    "character"
```

**Important:**

1. When different types of atomic-vector variables are on "operation", R automatically changes variables' types. The rule of change is: from *low* to just *high* enough. As for "low" and "high", the order is
$$
\hbox{logical}<\hbox{integer}<\hbox{double}<\hbox{character}
$$
Note that when converting from logical to integer, `TRUE` and `FALSE` become `1L` and `0L`, respectively.
1. R function `length()` gives *length*---how many elements an atomic vector has.
1. When two atomic-vector variables of different lengths are on "operation", R will firstly recycle the shorter variable.
1. We use `vec[index]` or other ways^[For details see Hadley Wickham, https://adv-r.hadley.nz/subsetting.html]  to get values from an atomic vector.

**Examples:**

```r
(a_logic_vec + an_integer_vec)
```

```
## [1] 2 2 4 5 5 7
```

```r
print(c(an_integer_vec, a_double_vec))
```

```
##  [1] 1 2 3 4 5 6 1 2 3 4 5 6
```

```r
print(c(a_logic_vec, a_double_vec, a_character_vec))
```

```
##  [1] "TRUE"  "FALSE" "TRUE"  "1"     "2"     "3"     "4"     "5"     "6"    
## [10] "a"     "b"     "c"     "123"
```

```r
print(a_character_vec[c(1, 4)])
```

```
## [1] "a"   "123"
```


```r
a <- c(1, 1e3, 1e5)
b <- c(1L, 1e3L, 1e5L)
(a == b) # type changes when on operation
```

```
## [1] TRUE TRUE TRUE
```

```r
(identical(a, b))
```

```
## [1] FALSE
```

```r
(as.character(a)) # interesting!
```

```
## [1] "1"     "1000"  "1e+05"
```

```r
(as.character(b))
```

```
## [1] "1"      "1000"   "100000"
```

```r
(as.character(as.integer(a))) # this might be useful in practice.
```

```
## [1] "1"      "1000"   "100000"
```


## Lists

A list and an atomic vector share the common property of "a tuple of elements." A list differs an atomic vector in that each element (item) in a list can be **more complex stuff** but not limited to a logical/integer/double/character value. We use 
`a_list[index]` to subset a list, and we use 
`a_list[[index]]` or `a_list[[item name]]` to have **one item** from a list.
Let's have an example

```r
a_list <- list(item_1 = 1L:3L,
               item_2 = "I like R",
               item_3 = c(TRUE, FALSE),
               item_4 = c(1.8, 2.6, 3.3, 8.9, 10.0))
(is.list(a_list))
```

```
## [1] TRUE
```

```r
print(names(a_list))
```

```
## [1] "item_1" "item_2" "item_3" "item_4"
```

```r
print(length(a_list))
```

```
## [1] 4
```

```r
print(lengths(a_list))
```

```
## item_1 item_2 item_3 item_4 
##      3      1      2      5
```

```r
print(a_list[c(1, 2)])
```

```
## $item_1
## [1] 1 2 3
## 
## $item_2
## [1] "I like R"
```

```r
print(a_list[2])
```

```
## $item_2
## [1] "I like R"
```

```r
print(a_list[[2]])
```

```
## [1] "I like R"
```

```r
print(a_list[["item_2"]])
```

```
## [1] "I like R"
```

## Matrices

A matrix is a vector with the 'dim' attribute.

```r
a_matrix <- 
  structure(1:9,
            dim = c(3, 3))
(is.matrix(a_matrix))
```

```
## [1] TRUE
```

```r
(attributes(a_matrix))
```

```
## $dim
## [1] 3 3
```

```r
print(a_matrix)
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

```r
b_matrix <- matrix(1:9, ncol = 3)
(is.matrix(b_matrix))
```

```
## [1] TRUE
```

```r
(attributes(b_matrix))
```

```
## $dim
## [1] 3 3
```

```r
print(b_matrix)
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

```r
(identical(a_matrix, b_matrix))
```

```
## [1] TRUE
```


## Data Frames

A data frame is a rectangular table with columns and rows. Technically speaking,
a data frame is a **list of atomic vectors**---they have the same length---plus three attributes: a) 'names' (for columns); b) 'row.names'; and 'class =  ???data.frame???' 
**Data frames share the properties of both matrices and lists.**


```r
my_df_1 <- 
  structure(list(1:5,
                 letters[1:5],
                 c(TRUE, FALSE, TRUE, FALSE, TRUE)),
            names = c('a', 'b', 'c'),
            row.names = 1:5,
            class = "data.frame")
is.data.frame(my_df_1)
```

```
## [1] TRUE
```

```r
attributes(my_df_1)
```

```
## $names
## [1] "a" "b" "c"
## 
## $row.names
## [1] 1 2 3 4 5
## 
## $class
## [1] "data.frame"
```

```r
my_df_2 <- data.frame(a = 1:5, 
                      b = letters[1:5],
                      c = c(TRUE, FALSE, TRUE, FALSE, TRUE),
                      row.names = 1:5,
                      stringsAsFactors = FALSE)
identical(my_df_1, my_df_2)
```

```
## [1] TRUE
```

## Factors and Dates

Again, I quote words from Hadley Wickham (*Advanced R* 2nd Ed, https://adv-r.hadley.nz/vectors-chap.html):

About factors

>
Factors are built on top of an integer vector with two attributes: a class, ???factor???, which makes it behave differently from regular integer vectors, and levels, which defines the set of allowed values.
>

>
It???s usually best to explicitly convert factors to character vectors if you need string-like behaviour.
>

About Dates

>
Date vectors are built on top of double vectors. They have class ???Date??? and no other attributes.
>

