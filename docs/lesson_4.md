# Lesson 4 Data Structure {#L4}

In R, the fundamentally important data structure is **vector**, which can be roughly defined as *a tuple of elements.* We can divide R vectors into two classes: a) **atomic vectors**; b) (other) **structured vectors**.

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
(a_double_vec <- seq(0, 1, by = 0.2))
```

```
## [1] 0.0 0.2 0.4 0.6 0.8 1.0
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
##  [1] 1.0 2.0 3.0 4.0 5.0 6.0 0.0 0.2 0.4 0.6 0.8 1.0
```

```r
print(c(a_logic_vec, a_double_vec, a_character_vec))
```

```
##  [1] "TRUE"  "FALSE" "TRUE"  "0"     "0.2"   "0.4"   "0.6"   "0.8"   "1"    
## [10] "a"     "b"     "c"     "123"
```

## Structured Vecotrs

In this section, we talk about

- list
- matrix
- data frame

A list and an atomic vector share the common property of "a tuple of elements." A list differs an atomic vector in that each element (item) in a list can be **more complex stuff** but not limited to logical/integer/double/character value. Let's have an example

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

In R, a matrix is a vector plus more attributes, and a data frame is a list (but with more attributes). 

- For matrix and data frame, we have the **row-and-column** concept. Columns 1-n in a data frame, correspond to items 1-n, respectively, in a list.
- All the columns in a matrix must have the same "structure"---e.g. if column 1 is an integer vector of length 5, then all the other columns must be integer vectors of length 5.
- Columns in a data frame all have the same "length", but they can have different "structures"---e.g. column 1 is a double vector of length 6; column 2 is a character vector of length 6; column 3 is a logical vector of length 6.

**Examples:**

```r
(a_matrix <- matrix(1L:9L, ncol = 3, nrow = 3, byrow = TRUE))
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]    4    5    6
## [3,]    7    8    9
```

```r
(another_matrix <- matrix(letters[1:9], ncol = 3))
```

```
##      [,1] [,2] [,3]
## [1,] "a"  "d"  "g" 
## [2,] "b"  "e"  "h" 
## [3,] "c"  "f"  "i"
```

```r
(a_df <- data.frame(ID = letters[1:5],
                    x = 1:5,
                    y = "YES",
                    Z = c(TRUE, FALSE, FALSE, TRUE, FALSE),
                    stringsAsFactors = FALSE)
  
)
```

```
##   ID x   y     Z
## 1  a 1 YES  TRUE
## 2  b 2 YES FALSE
## 3  c 3 YES FALSE
## 4  d 4 YES  TRUE
## 5  e 5 YES FALSE
```

```r
print(dim(a_matrix))
```

```
## [1] 3 3
```

```r
print(length(a_matrix))
```

```
## [1] 9
```

```r
print(lengths(a_matrix))
```

```
##      [,1] [,2] [,3]
## [1,]    1    1    1
## [2,]    1    1    1
## [3,]    1    1    1
```

```r
(is.vector(a_matrix)) # this ONLY shows that matrix is NOT atomic vector
```

```
## [1] FALSE
```

```r
print(dim(a_df))
```

```
## [1] 5 4
```

```r
print(length(a_df))
```

```
## [1] 4
```

```r
print(lengths(a_df))
```

```
## ID  x  y  Z 
##  5  5  5  5
```

```r
(names(a_df))
```

```
## [1] "ID" "x"  "y"  "Z"
```

```r
(is.list(a_df))
```

```
## [1] TRUE
```

