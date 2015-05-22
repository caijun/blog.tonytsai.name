---
layout: post
title: Operator for Extracting Named List Elements to Variables
categories: 
- R
tags:
- list Operator
- Variable Extraction
---

In routine data analysis, I often need to convert list elements into variables. For exampple, when I create a function returning multiple values and I would like to use those values as variables separately, as only one object can be returned by the function, the common way to accomplish this task is that multiple values are combined as a list returned by the function, elements of interest are then extracted and assigned to variables. The following codes describe the process.


```r
# define a function returning a matrix and a numeric
fun <- function() list(a = matrix(1:2, nr = 2, nc = 1), b = 2)

# call function to return a list containing multiple returning values
r <- fun()
# extract elements of interest and assgin them to variables
x <- r$a
y <- r$b
```

However, when numerous values have to be returned and lots of assignments have to be written down, the process is really cumbersome. One may argue that it's ok to directly use `list$element` as a variable, but it can't satisify my geek desire that code should be succinct and is easy to read. The best way is to directly return variables `a` and `b` in above case, which is the exact way that MATLAB returns multiple values. Rewrite above example in MATLAB.

```{.matlab}
# define function
function [a, b] = fun()
a(:, 1) = 1:2
b = 2

# call function
[a, b] = fun()
```

Therefore, we could customize a similar operator in R to implement such assignment. Fortunately, G. Grothendieck has done such [work](http://stackoverflow.com/questions/1826519/function-returning-more-than-one-value) ten years ago. He firstly creates a variable named `list`, which is a structure with class attribute of `result`, then defines an S3 method `[<-.result` of class `result`. His solution is as follows


```r
list <- structure(NA, class = "result")
"[<-.result" <- function(x, ..., value) {
   args <- as.list(match.call())
   args <- args[-c(1:2, length(args))]
   length(value) <- length(args)
   for(i in seq(along = args)) {
     a <- args[[i]]
     if(!missing(a)) eval.parent(substitute(a <- v, list(a = a, v = value[[i]])))
   }
   x
}
```

Applying G. Grothendieck's solution to the above case is as simple as following


```r
# extract all variables
list[a, b] <- fun()

# extract first variable without renaming
list[a, ] <- fun()

# extract second variable with renaming
list[, y] <- fun()

# extract all variable with renaming
list[x, y] <- fun()
```

This operator is exactly what I want; however, it can be improved in two cases: one is that there are numerous variables to be extracted, the other is that only elements of interest are explicitly extracted while the other elements can be extracted in a silent way. Hence, I modified `[<-.result()` to make it more easy to use in these two cases. The following are modified codes


```{.r .numberLines}
"[<-.result" <- function(x, ..., value) {
  nv <- names(value)
  args <- as.list(match.call())
  args <- args[-c(1:2, length(args))]
  all <- FALSE
  if ("all" %in% names(args)) {
    all <- args[["all"]]
    stopifnot(is.logical(all))
    args["all"] <- NULL
  }
  drop = TRUE
  if ("drop" %in% names(args)) {
    drop <- args[["drop"]]
    stopifnot(is.logical(drop))
    args["drop"] <- NULL
  }

  if (length(args) == 1) args <- sapply(nv, as.name)
  for (i in seq(along = args)) {
    a <- args[[i]]
    if (missing(a)) {
      if (all) a <- as.name(nv[i]) else next
    }
    if (drop) v <- drop(value[[i]]) else v <- value[[i]]
    eval.parent(substitute(a <- v, list(a = a, v = v)))
  }
  x
}
```

My modification not only keeps the usages created by G. Grothendieck, but also adds two extra parameters (`all` and `drop`) and changes the returning results of `list[] <- value`.In G. Grothendieck's solution, calling `list[] <- fun()` will not return any variables, but in my version, it will return all varaibles correspoding the list elements. The advantage of my solution is to avoid inputting all variables when there are two numerous named list elements.

The default for parameter `all` is `FALSE` to keep the calling way as G. Grothendieck. Setting `all = TRUE` can extract all list elements to variables even you only input partial variable names. The elements without inputting variable names will use the element names as variable names. This is the reason why the list elements are named. This sort usage can reduce amount of inputting variable names in case extra list elements needs to be extracted to variables.

Notice that in above example, list element `a` is a matrix with the second dimenion which has only one level. Adding parameter `drop` can simutaniously drop the redundant dimension during the variable extraction. This sort usage avoids an extra `drop()` operation to keep the extracted variable simple. The default value for argument `drop` is `TRUE`. 

The following codes demonstrate the new usages of modified `[<-.result()`.


```r
# implicitly extract all variabes without renaming. note that by default
# dimensions are dropped.
list[] <- fun()  # return a = 1, b = 2

# set drop to FALSE without dimension dropping
list[x, , drop = FALSE] <- fun()  # return matrix x

# set all to TRUE to extract all elements.
list[x, , all = TRUE] <- fun()  # return x = 1, b = 2
```

The modified codes have been packaged into my package [TTmisc](https://github.com/caijun/TTmisc). For more examples of usage and details refer to the repository [TTmisc]((https://github.com/caijun/TTmisc)) on my github.

At last, it is important to note that the right side of assignment can be just a list object with named elements. It is not necessary to be a function calling. Here, I use function calling only for demonstration.
