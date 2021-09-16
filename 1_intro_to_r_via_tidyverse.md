lec1
================
JR
Last compiled on Wed Sep 15 18:50:09 2021

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

1.  How do you skip rows when reading in data? `read_csv(data="",
    skip=n)`

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

<!-- end list -->

2.  `read_delim(file=..., delim = "\t", col_names=F)` is more flexible
    to get data into R, need to specify delimiter

<!-- end list -->

``` r
cols <- colnames(can_lang1)
can_lang3 <- read_delim("../ds_block1/523_r_prog/private_notes/data/can_lang.tsv", delim = "\t", col_names = cols)
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
can_lang3
```

    ## # A tibble: 214 x 6
    ##    category       language    mother_tongue most_at_home most_at_work lang_known
    ##    <chr>          <chr>               <dbl>        <dbl>        <dbl>      <dbl>
    ##  1 Aboriginal la~ Aboriginal~           590          235           30        665
    ##  2 Non-Official ~ Afrikaans           10260         4785           85      23415
    ##  3 Non-Official ~ Afro-Asiat~          1150          445           10       2775
    ##  4 Non-Official ~ Akan (Twi)          13460         5985           25      22150
    ##  5 Non-Official ~ Albanian            26895        13135          345      31930
    ##  6 Aboriginal la~ Algonquian~            45           10            0        120
    ##  7 Aboriginal la~ Algonquin            1260          370           40       2480
    ##  8 Non-Official ~ American S~          2685         3020         1145      21930
    ##  9 Non-Official ~ Amharic             22465        12785          200      33670
    ## 10 Non-Official ~ Arabic             419890       223535         5585     629055
    ## # ... with 204 more rows

3.  `read_csv(<url>)`

<!-- end list -->

``` r
can_lang4 <- read_csv("https://raw.githubusercontent.com/ttimbers/canlang/master/inst/extdata/can_lang.csv")
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
head(can_lang4)
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

4.  `readxl::read_excel(path=..., sheet=...)`

<!-- end list -->

``` r
can_lang5 <- readxl::read_excel("../ds_block1/523_r_prog/private_notes/data/can_lang.xlsx", )

head(can_lang5)
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

``` r
# read xlsx from the web, you need to download
url <- "https://github.com/ttimbers/canlang/blob/master/inst/extdata/can_lang.xlsx?raw=true"
download.file(url = url, "temp.xlsx")
# can_lang6 <- readxl::read_excel("temp.xlsx")
# head(can_lang6)
```

5.  `write_csv()`

<!-- end list -->

``` r
write_csv(can_lang5, "data/can_lang6.csv")
```

6.  Clean up col names\!🤔 `rename()`, `janitor::clean_names()`

<!-- end list -->

``` r
can_lang8 <- read_csv("../ds_block1/523_r_prog/private_notes/data/can_lang-colnames.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   Category = col_character(),
    ##   Language = col_character(),
    ##   `Mother tongue` = col_double(),
    ##   `Spoken most at home` = col_double(),
    ##   `Spoken most at work` = col_double(),
    ##   `Language known` = col_double()
    ## )

``` r
colnames(can_lang8)
```

    ## [1] "Category"            "Language"            "Mother tongue"      
    ## [4] "Spoken most at home" "Spoken most at work" "Language known"

``` r
census <- read_csv("../ds_block1/523_r_prog/private_notes/data/census_snippet.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Geographic code` = col_double(),
    ##   `Geographic name` = col_character(),
    ##   `Geographic type` = col_character(),
    ##   `Geographic name, Province or territory` = col_character(),
    ##   `Geographic code, Province or territory` = col_double(),
    ##   `Global non-response rate` = col_double(),
    ##   `Data quality flag` = col_double(),
    ##   `Household type` = col_character(),
    ##   `Number of households, 2006` = col_double(),
    ##   `Number of households, 2016` = col_double(),
    ##   `Median household total income (2015 constant dollars), 2005` = col_double(),
    ##   `Median household total income (2015 constant dollars), 2015` = col_double(),
    ##   `Median household total income (2015 constant dollars), % change` = col_double(),
    ##   `Median household after-tax income (2015 constant dollars), 2005` = col_double(),
    ##   `Median household after-tax income (2015 constant dollars), 2015` = col_double(),
    ##   `Median household after-tax income (2015 constant dollars), % change` = col_double()
    ## )

``` r
colnames(census)
```

    ##  [1] "Geographic code"                                                    
    ##  [2] "Geographic name"                                                    
    ##  [3] "Geographic type"                                                    
    ##  [4] "Geographic name, Province or territory"                             
    ##  [5] "Geographic code, Province or territory"                             
    ##  [6] "Global non-response rate"                                           
    ##  [7] "Data quality flag"                                                  
    ##  [8] "Household type"                                                     
    ##  [9] "Number of households, 2006"                                         
    ## [10] "Number of households, 2016"                                         
    ## [11] "Median household total income (2015 constant dollars), 2005"        
    ## [12] "Median household total income (2015 constant dollars), 2015"        
    ## [13] "Median household total income (2015 constant dollars), % change"    
    ## [14] "Median household after-tax income (2015 constant dollars), 2005"    
    ## [15] "Median household after-tax income (2015 constant dollars), 2015"    
    ## [16] "Median household after-tax income (2015 constant dollars), % change"

``` r
clean_census <- janitor::clean_names(census)
colnames(clean_census)
```

    ##  [1] "geographic_code"                                                       
    ##  [2] "geographic_name"                                                       
    ##  [3] "geographic_type"                                                       
    ##  [4] "geographic_name_province_or_territory"                                 
    ##  [5] "geographic_code_province_or_territory"                                 
    ##  [6] "global_non_response_rate"                                              
    ##  [7] "data_quality_flag"                                                     
    ##  [8] "household_type"                                                        
    ##  [9] "number_of_households_2006"                                             
    ## [10] "number_of_households_2016"                                             
    ## [11] "median_household_total_income_2015_constant_dollars_2005"              
    ## [12] "median_household_total_income_2015_constant_dollars_2015"              
    ## [13] "median_household_total_income_2015_constant_dollars_percent_change"    
    ## [14] "median_household_after_tax_income_2015_constant_dollars_2005"          
    ## [15] "median_household_after_tax_income_2015_constant_dollars_2015"          
    ## [16] "median_household_after_tax_income_2015_constant_dollars_percent_change"
