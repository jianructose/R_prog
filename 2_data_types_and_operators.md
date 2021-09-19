2\_data\_types\_and\_operators
================
JR
Last compiled on Sun Sep 19 12:58:55 2021

## 1\. Maxima and Minima

``` r
x1 <- c(2, 8, 3, 4, 1, 5)               # First example vector
x2 <- c(0, 7, 5, 5, 6, 1) 

x3 <- NA

x4 <- c()

x5 <- as.character(c())

x6 <- c(T, F, F, T, T, T)
# element-wise max
pmax(x1, x2, x3, na.rm = T)
```

    ## [1] 2 8 5 5 6 5

``` r
max(x1, x2, x3, na.rm = T)
```

    ## [1] 8

``` r
min(x1, x2, x3, na.rm = T)
```

    ## [1] 0

``` r
max(x3)
```

    ## [1] NA

``` r
min(x4)
```

    ## Warning in min(x4): no non-missing arguments to min; returning Inf

    ## [1] Inf

``` r
max(x4)
```

    ## Warning in max(x4): no non-missing arguments to max; returning -Inf

    ## [1] -Inf

``` r
length(x4)
```

    ## [1] 0

``` r
max(x5)
```

    ## Warning in max(x5): no non-missing arguments, returning NA

    ## [1] NA

``` r
sum(x6)
```

    ## [1] 4

``` r
x6
```

    ## [1]  TRUE FALSE FALSE  TRUE  TRUE  TRUE

``` r
min(5:1, pi)
```

    ## [1] 1

``` r
pmin(5:1, pi)
```

    ## [1] 3.141593 3.141593 3.000000 2.000000 1.000000

``` r
x <- sort(rnorm(100)); cH <- 1.35
# give the quantile of 5 (0, 0.25, 0.5, 0.75, 1)
quantile(x)
```

    ##         0%        25%        50%        75%       100% 
    ## -2.4536954 -0.7551648 -0.1182584  0.4852547  2.3864132

``` r
# no names
pmin(cH, quantile(x))
```

    ## [1] -2.4536954 -0.7551648 -0.1182584  0.4852547  1.3500000

``` r
# has names
pmin(quantile(x), cH)
```

    ##         0%        25%        50%        75%       100% 
    ## -2.4536954 -0.7551648 -0.1182584  0.4852547  1.3500000

``` r
plot(x, pmin(cH, pmax(-cH, x)), type='b', main = "Huber's function")
```

![](2_data_types_and_operators_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
cut01 <- function(x) pmax(pmin(x, 1), 0)
curve(x^2 - 1/4, -1.5, 1.5, col=222)
curve(cut01(x^2 -1/4), col = 3, add = T,n=600)
```

![](2_data_types_and_operators_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
# pmiax(), pmin() preserve attributes of *first* argument

D <- diag(x = (3:1)/4) ; n0 <- numeric()
D
```

    ##      [,1] [,2] [,3]
    ## [1,] 0.75  0.0 0.00
    ## [2,] 0.00  0.5 0.00
    ## [3,] 0.00  0.0 0.25

``` r
n0
```

    ## numeric(0)

``` r
stopifnot(identical(D, cut01(D)),
          identical(n0, cut01(n0)),
          identical(n0, cut01(NULL)),
          identical(n0, pmax(3:1, n0, 2)),
          identical(n0, pmax(n0, 4)))
```

## 2\. The assignment symbol,\<-

## 3\. Key datatypes in R
