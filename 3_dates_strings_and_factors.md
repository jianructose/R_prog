3\_dates\_strings\_and\_factors
================
JR
Last compiled on Tue Sep 21 18:01:45 2021

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
