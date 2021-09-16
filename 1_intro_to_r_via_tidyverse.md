lec1
================
JR
Last compiled on Wed Sep 15 17:55:28 2021

## What is included?

  - `readr::read_*` function to load a given rectangular, plain text
    data set into R
  - `<-` assignment symbol to assign values to objects in R
  - `readr::write_csv()` to write a df to a .csv file
  - `select`
  - `filter`
  - `mutate`
  - `arrange`
  - `desc`
  - `slice`
  - `pull`
  - `%in%`
  - `%>%` pipe operator to combine 2+ functions
  - What is “tidy data”? and what are pros and cons of the tidy data
    format?
  - How do you use `tidyr::pivot_wider` and `tidyr::pivot_longer` in R?

<!-- end list -->

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.3     v purrr   0.3.4
    ## v tibble  3.1.0     v dplyr   1.0.5
    ## v tidyr   1.1.2     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## Warning: package 'ggplot2' was built under R version 4.0.4

    ## Warning: package 'tibble' was built under R version 4.0.4

    ## Warning: package 'dplyr' was built under R version 4.0.4

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
can_lang1 <- read_csv("../ds_block1/523_r_prog/private_notes/data/can_lang.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   category = col_character(),
    ##   language = col_character(),
    ##   mother_tongue = col_double(),
    ##   most_at_home = col_double(),
    ##   most_at_work = col_double(),
    ##   lang_known = col_double()
    ## )

``` r
head(can_lang1)
```

    ## # A tibble: 6 x 6
    ##   category       language     mother_tongue most_at_home most_at_work lang_known
    ##   <chr>          <chr>                <dbl>        <dbl>        <dbl>      <dbl>
    ## 1 Aboriginal la~ Aboriginal ~           590          235           30        665
    ## 2 Non-Official ~ Afrikaans            10260         4785           85      23415
    ## 3 Non-Official ~ Afro-Asiati~          1150          445           10       2775
    ## 4 Non-Official ~ Akan (Twi)           13460         5985           25      22150
    ## 5 Non-Official ~ Albanian             26895        13135          345      31930
    ## 6 Aboriginal la~ Algonquian ~            45           10            0        120

1.  `read_csv(data="", skip=n)`

<!-- end list -->

``` r
can_lang2 <- read_csv("../ds_block1/523_r_prog/private_notes/data/can_lang-meta-data.csv", skip = 2, )
```

    ## Parsed with column specification:
    ## cols(
    ##   category = col_character(),
    ##   language = col_character(),
    ##   mother_tongue = col_double(),
    ##   most_at_home = col_double(),
    ##   most_at_work = col_double(),
    ##   lang_known = col_double()
    ## )

``` r
head(can_lang2)
```

    ## # A tibble: 6 x 6
    ##   category       language     mother_tongue most_at_home most_at_work lang_known
    ##   <chr>          <chr>                <dbl>        <dbl>        <dbl>      <dbl>
    ## 1 Aboriginal la~ Aboriginal ~           590          235           30        665
    ## 2 Non-Official ~ Afrikaans            10260         4785           85      23415
    ## 3 Non-Official ~ Afro-Asiati~          1150          445           10       2775
    ## 4 Non-Official ~ Akan (Twi)           13460         5985           25      22150
    ## 5 Non-Official ~ Albanian             26895        13135          345      31930
    ## 6 Aboriginal la~ Algonquian ~            45           10            0        120

  - to skip rows at the bottom, use `n_max=` argument.
  - to see the max line: in **terminal**: `wc -l FILEPATH`, `tail -n 10
    FILEPATH`: print the last 10 lines of the file
