6\_functions\_and\_testings
================
JR
Last Compiled on Sat Oct 30 00:09:17 2021

# 1\. Functions

# 1.1 Defining functions in R:

  - Use `fun_name <- function(...arguments...) {...body...}` to create a
    function and give it a name

<!-- end list -->

``` r
add <- function(x, y){
  x + y
}

add(22, 33.3)
```

    ## [1] 55.3

``` r
add <- function(x, y){
  if (!is.numeric(x) | !is.numeric(y)){
    return(NA)
  }
  x + y
}

add("3", 0)
```

    ## [1] NA

# 1.2 Default function arguments:

``` r
repeat_string <- function(x, n=4){
  repeated <- ""
  for (i in seq_along(1:n)){
    repeated <- paste0(repeated, x)
  }
  repeated
}

repeat_string("JIMMY", 8)
```

    ## [1] "JIMMYJIMMYJIMMYJIMMYJIMMYJIMMYJIMMYJIMMY"

``` r
paste("hello", "world", "t", sep=":")
```

    ## [1] "hello:world:t"

``` r
paste0("hello", "world", "t", sep=":")
```

    ## [1] "helloworldt:"

``` r
paste0(1:12, c("st", "nd", "rd", rep("th", 9)))
```

    ##  [1] "1st"  "2nd"  "3rd"  "4th"  "5th"  "6th"  "7th"  "8th"  "9th"  "10th"
    ## [11] "11th" "12th"

``` r
paste0("", "4")
```

    ## [1] "4"
