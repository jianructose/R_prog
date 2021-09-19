2\_data\_types\_and\_operators
================
JR
Last compiled on Sun Sep 19 13:54:02 2021

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

    ##          0%         25%         50%         75%        100% 
    ## -2.11816116 -0.85135154 -0.03296304  0.79359477  2.44648295

``` r
# no names
pmin(cH, quantile(x))
```

    ## [1] -2.11816116 -0.85135154 -0.03296304  0.79359477  1.35000000

``` r
# has names
pmin(quantile(x), cH)
```

    ##          0%         25%         50%         75%        100% 
    ## -2.11816116 -0.85135154 -0.03296304  0.79359477  1.35000000

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

### What is a vection?

  - obj that can contain 1+ elem
  - elem are ordered
  - all must be of the same type (dbl, int, char, log)

<!-- end list -->

``` r
char_vec <- c("joy", "peace", "help", "forest")
char_vec
```

    ## [1] "joy"    "peace"  "help"   "forest"

``` r
typeof(char_vec)
```

    ## [1] "character"

``` r
log_vec <- c(T, T, F, F, F, T)
log_vec
```

    ## [1]  TRUE  TRUE FALSE FALSE FALSE  TRUE

``` r
typeof(log_vec)
```

    ## [1] "logical"

``` r
dbl_vec <- c(2, 1, 3, 5, 4.5)
dbl_vec
```

    ## [1] 2.0 1.0 3.0 5.0 4.5

``` r
typeof(dbl_vec)
```

    ## [1] "double"

``` r
int_vec <- c(1L, 4L, 3L, 2L, 5L)
int_vec
```

    ## [1] 1 4 3 2 5

``` r
typeof(int_vec)
```

    ## [1] "integer"

``` r
str(int_vec)
```

    ##  int [1:5] 1 4 3 2 5

### Hierarchy for data types

``` r
mixed_vec <- c('music', 2L, T, 3.9, 0, 'logical', '3')
typeof(mixed_vec)
```

    ## [1] "character"

char \> dbl \> int \> log

``` r
is.logical(int_vec)
```

    ## [1] FALSE

``` r
is.integer(int_vec)
```

    ## [1] TRUE

``` r
is.double(char_vec)
```

    ## [1] FALSE

``` r
is.character(char_vec)
```

    ## [1] TRUE

``` r
as.logical(int_vec)
```

    ## [1] TRUE TRUE TRUE TRUE TRUE

``` r
as.integer(dbl_vec)
```

    ## [1] 2 1 3 5 4

``` r
as.double(int_vec)
```

    ## [1] 1 4 3 2 5

``` r
as.character(log_vec)
```

    ## [1] "TRUE"  "TRUE"  "FALSE" "FALSE" "FALSE" "TRUE"

### Subset and modify vectors

``` r
name <- c("J", "i", "a", "n", "r", "u")
name[-1]
```

    ## [1] "i" "a" "n" "r" "u"

``` r
name[2:4]
```

    ## [1] "i" "a" "n"

``` r
# get the last elem
name[length(name)]
```

    ## [1] "u"

``` r
name[1] <- "j"
name
```

    ## [1] "j" "i" "a" "n" "r" "u"

``` r
name[1:3] <- c("J", "I", "A")
name
```

    ## [1] "J" "I" "A" "n" "r" "u"

``` r
name[7:9]
```

    ## [1] NA NA NA

``` r
# add additional elem
name[8:12] <- c("-", "N", "a", "t", "u")
name
```

    ##  [1] "J" "I" "A" "n" "r" "u" NA  "-" "N" "a" "t" "u"

``` r
# compares each elem of each vec by position
c(T, T, F)&c(F, T, F)
```

    ## [1] FALSE  TRUE FALSE

``` r
# compares only the first elem of each vec
c(T, T, F)&&c(F, T, F)
```

    ## [1] FALSE

``` r
slice_head(mtcars, n = 6)
```

    ##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
    ## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
    ## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

``` r
str(mtcars)
```

    ## 'data.frame':    32 obs. of  11 variables:
    ##  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
    ##  $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
    ##  $ disp: num  160 160 108 258 360 ...
    ##  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
    ##  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
    ##  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
    ##  $ qsec: num  16.5 17 18.6 19.4 17 ...
    ##  $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
    ##  $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
    ##  $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
    ##  $ carb: num  4 4 1 1 2 1 4 2 2 4 ...

## 4\. Subset and modify data frames

``` r
# rows 3-10 and cols 2-8
mtcars[3:10, 2:8]
```

    ##                   cyl  disp  hp drat    wt  qsec vs
    ## Datsun 710          4 108.0  93 3.85 2.320 18.61  1
    ## Hornet 4 Drive      6 258.0 110 3.08 3.215 19.44  1
    ## Hornet Sportabout   8 360.0 175 3.15 3.440 17.02  0
    ## Valiant             6 225.0 105 2.76 3.460 20.22  1
    ## Duster 360          8 360.0 245 3.21 3.570 15.84  0
    ## Merc 240D           4 146.7  62 3.69 3.190 20.00  1
    ## Merc 230            4 140.8  95 3.92 3.150 22.90  1
    ## Merc 280            6 167.6 123 3.92 3.440 18.30  1

``` r
# subset rows 25-33, row 33 is NA
mtcars[25:33, ]
```

    ##                   mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Pontiac Firebird 19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
    ## Fiat X1-9        27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## Porsche 914-2    26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## Lotus Europa     30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## Ford Pantera L   15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
    ## Ferrari Dino     19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
    ## Maserati Bora    15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
    ## Volvo 142E       21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
    ## NA                 NA  NA    NA  NA   NA    NA    NA NA NA   NA   NA

``` r
dim(mtcars)
```

    ## [1] 32 11

``` r
# subset col 3-8
mtcars[3:8]
```

    ##                      disp  hp drat    wt  qsec vs
    ## Mazda RX4           160.0 110 3.90 2.620 16.46  0
    ## Mazda RX4 Wag       160.0 110 3.90 2.875 17.02  0
    ## Datsun 710          108.0  93 3.85 2.320 18.61  1
    ## Hornet 4 Drive      258.0 110 3.08 3.215 19.44  1
    ## Hornet Sportabout   360.0 175 3.15 3.440 17.02  0
    ## Valiant             225.0 105 2.76 3.460 20.22  1
    ## Duster 360          360.0 245 3.21 3.570 15.84  0
    ## Merc 240D           146.7  62 3.69 3.190 20.00  1
    ## Merc 230            140.8  95 3.92 3.150 22.90  1
    ## Merc 280            167.6 123 3.92 3.440 18.30  1
    ## Merc 280C           167.6 123 3.92 3.440 18.90  1
    ## Merc 450SE          275.8 180 3.07 4.070 17.40  0
    ## Merc 450SL          275.8 180 3.07 3.730 17.60  0
    ## Merc 450SLC         275.8 180 3.07 3.780 18.00  0
    ## Cadillac Fleetwood  472.0 205 2.93 5.250 17.98  0
    ## Lincoln Continental 460.0 215 3.00 5.424 17.82  0
    ## Chrysler Imperial   440.0 230 3.23 5.345 17.42  0
    ## Fiat 128             78.7  66 4.08 2.200 19.47  1
    ## Honda Civic          75.7  52 4.93 1.615 18.52  1
    ## Toyota Corolla       71.1  65 4.22 1.835 19.90  1
    ## Toyota Corona       120.1  97 3.70 2.465 20.01  1
    ## Dodge Challenger    318.0 150 2.76 3.520 16.87  0
    ## AMC Javelin         304.0 150 3.15 3.435 17.30  0
    ## Camaro Z28          350.0 245 3.73 3.840 15.41  0
    ## Pontiac Firebird    400.0 175 3.08 3.845 17.05  0
    ## Fiat X1-9            79.0  66 4.08 1.935 18.90  1
    ## Porsche 914-2       120.3  91 4.43 2.140 16.70  0
    ## Lotus Europa         95.1 113 3.77 1.513 16.90  1
    ## Ford Pantera L      351.0 264 4.22 3.170 14.50  0
    ## Ferrari Dino        145.0 175 3.62 2.770 15.50  0
    ## Maserati Bora       301.0 335 3.54 3.570 14.60  0
    ## Volvo 142E          121.0 109 4.11 2.780 18.60  1

``` r
# extract col 3 as a vec
(mtcars[[3]])
```

    ##  [1] 160.0 160.0 108.0 258.0 360.0 225.0 360.0 146.7 140.8 167.6 167.6 275.8
    ## [13] 275.8 275.8 472.0 460.0 440.0  78.7  75.7  71.1 120.1 318.0 304.0 350.0
    ## [25] 400.0  79.0 120.3  95.1 351.0 145.0 301.0 121.0

``` r
(mtcars$drat)
```

    ##  [1] 3.90 3.90 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 3.92 3.07 3.07 3.07 2.93
    ## [16] 3.00 3.23 4.08 4.93 4.22 3.70 2.76 3.15 3.73 3.08 4.08 4.43 3.77 4.22 3.62
    ## [31] 3.54 4.11

``` r
options(repr.matrix.max.rows=10)

mtcars
```

    ##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
    ## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
    ## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
    ## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
    ## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    ## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
    ## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
    ## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
    ## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
    ## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
    ## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
    ## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
    ## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
    ## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
    ## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    ## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    ## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
    ## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
    ## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
    ## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
    ## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
    ## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
    ## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
    ## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
    ## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
    ## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2

``` r
# subset df
mtcars[mtcars$cyl == 6, ]
```

    ##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4      21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag  21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    ## Hornet 4 Drive 21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
    ## Valiant        18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
    ## Merc 280       19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
    ## Merc 280C      17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
    ## Ferrari Dino   19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6

``` r
mtcars[mtcars$drat>=4, ]
```

    ##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Fiat 128       32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    ## Honda Civic    30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    ## Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
    ## Fiat X1-9      27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## Ford Pantera L 15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
    ## Volvo 142E     21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2

``` r
# modify df

mtcars$kml <- mtcars$mpg/2.3521458
head(mtcars)
```

    ##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb      kml
    ## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 8.928018
    ## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 8.928018
    ## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1 9.693277
    ## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1 9.098075
    ## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 7.950187
    ## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1 7.695101
