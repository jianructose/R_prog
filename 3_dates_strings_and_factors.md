3\_dates\_strings\_and\_factors
================
JR
Last compiled on Wed Sep 22 11:30:30 2021

## 1\. Tibbles vs. Data Frames

``` r
example <- data.frame(a = c(1, 5, 9),
                      b = "z",
                      "a",
                      "t")
example
```

    ##   a b X.a. X.t.
    ## 1 1 z    a    t
    ## 2 5 z    a    t
    ## 3 9 z    a    t

``` r
example2 <- tibble(a = c(1, 5, 9),
                   b = "z",
                   "a",
                   "t")

example
```

    ##   a b X.a. X.t.
    ## 1 1 z    a    t
    ## 2 5 z    a    t
    ## 3 9 z    a    t

``` r
class(example2)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

Some tidyverse funs will coerce a df to a tibble, since what the user is
asking for is not possible with a df. eg. `group_by`

``` r
group_by(example,a) %>% 
  class()
```

    ## [1] "grouped_df" "tbl_df"     "tbl"        "data.frame"

## 2\. Working with dates and times

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
today()
```

    ## [1] "2021-09-22"

``` r
class(today())
```

    ## [1] "Date"

``` r
now()
```

    ## [1] "2021-09-22 11:30:34 PDT"

``` r
class(now())
```

    ## [1] "POSIXct" "POSIXt"

### 2.1 Parse date-times (convert strings or numbers to date-times) `ymd(), ydm(), myd(), mdy(), dmy(), dym(), yq()`

``` r
# from string to datetimes
ymd("20210922")
```

    ## [1] "2021-09-22"

``` r
mdy("Sep 22 2021")
```

    ## [1] "2021-09-22"

``` r
dmy("22th of Sep '21")
```

    ## [1] "2021-09-22"
