lec1
================
JR
Last compiled on Thu Sep 16 19:46:27 2021

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

## Single data frame manipulations

``` r
library(gapminder)

head(gapminder)
```

    ## # A tibble: 6 x 6
    ##   country     continent  year lifeExp      pop gdpPercap
    ##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ## 1 Afghanistan Asia       1952    28.8  8425333      779.
    ## 2 Afghanistan Asia       1957    30.3  9240934      821.
    ## 3 Afghanistan Asia       1962    32.0 10267083      853.
    ## 4 Afghanistan Asia       1967    34.0 11537966      836.
    ## 5 Afghanistan Asia       1972    36.1 13079460      740.
    ## 6 Afghanistan Asia       1977    38.4 14880372      786.

### 1\. `select`, `filter` to subset cols and rows

``` r
# to limit df output to 10 rows
options(repr.matrix.max.rows = 10)
select(gapminder, year, lifeExp)
```

    ## # A tibble: 1,704 x 2
    ##     year lifeExp
    ##    <int>   <dbl>
    ##  1  1952    28.8
    ##  2  1957    30.3
    ##  3  1962    32.0
    ##  4  1967    34.0
    ##  5  1972    36.1
    ##  6  1977    38.4
    ##  7  1982    39.9
    ##  8  1987    40.8
    ##  9  1992    41.7
    ## 10  1997    41.8
    ## # ... with 1,694 more rows

``` r
select(gapminder, country:lifeExp)
```

    ## # A tibble: 1,704 x 4
    ##    country     continent  year lifeExp
    ##    <fct>       <fct>     <int>   <dbl>
    ##  1 Afghanistan Asia       1952    28.8
    ##  2 Afghanistan Asia       1957    30.3
    ##  3 Afghanistan Asia       1962    32.0
    ##  4 Afghanistan Asia       1967    34.0
    ##  5 Afghanistan Asia       1972    36.1
    ##  6 Afghanistan Asia       1977    38.4
    ##  7 Afghanistan Asia       1982    39.9
    ##  8 Afghanistan Asia       1987    40.8
    ##  9 Afghanistan Asia       1992    41.7
    ## 10 Afghanistan Asia       1997    41.8
    ## # ... with 1,694 more rows

``` r
filter(gapminder, lifeExp < 29)
```

    ## # A tibble: 2 x 6
    ##   country     continent  year lifeExp     pop gdpPercap
    ##   <fct>       <fct>     <int>   <dbl>   <int>     <dbl>
    ## 1 Afghanistan Asia       1952    28.8 8425333      779.
    ## 2 Rwanda      Africa     1992    23.6 7290203      737.

``` r
# AND filter
filter(gapminder, country == "Rwanda", year > 1979)
```

    ## # A tibble: 6 x 6
    ##   country continent  year lifeExp     pop gdpPercap
    ##   <fct>   <fct>     <int>   <dbl>   <int>     <dbl>
    ## 1 Rwanda  Africa     1982    46.2 5507565      882.
    ## 2 Rwanda  Africa     1987    44.0 6349365      848.
    ## 3 Rwanda  Africa     1992    23.6 7290203      737.
    ## 4 Rwanda  Africa     1997    36.1 7212583      590.
    ## 5 Rwanda  Africa     2002    43.4 7852401      786.
    ## 6 Rwanda  Africa     2007    46.2 8860588      863.

``` r
# OR filter
filter(gapminder, lifeExp > 80 | year == 2007)
```

    ## # A tibble: 150 x 6
    ##    country     continent  year lifeExp       pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>     <int>     <dbl>
    ##  1 Afghanistan Asia       2007    43.8  31889923      975.
    ##  2 Albania     Europe     2007    76.4   3600523     5937.
    ##  3 Algeria     Africa     2007    72.3  33333216     6223.
    ##  4 Angola      Africa     2007    42.7  12420476     4797.
    ##  5 Argentina   Americas   2007    75.3  40301927    12779.
    ##  6 Australia   Oceania    2002    80.4  19546792    30688.
    ##  7 Australia   Oceania    2007    81.2  20434176    34435.
    ##  8 Austria     Europe     2007    79.8   8199783    36126.
    ##  9 Bahrain     Asia       2007    75.6    708573    29796.
    ## 10 Bangladesh  Asia       2007    64.1 150448339     1391.
    ## # ... with 140 more rows

``` r
# %in%
filter(gapminder, country %in% c("Mexico", "United States", "Canada"))
```

    ## # A tibble: 36 x 6
    ##    country continent  year lifeExp      pop gdpPercap
    ##    <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Canada  Americas   1952    68.8 14785584    11367.
    ##  2 Canada  Americas   1957    70.0 17010154    12490.
    ##  3 Canada  Americas   1962    71.3 18985849    13462.
    ##  4 Canada  Americas   1967    72.1 20819767    16077.
    ##  5 Canada  Americas   1972    72.9 22284500    18971.
    ##  6 Canada  Americas   1977    74.2 23796400    22091.
    ##  7 Canada  Americas   1982    75.8 25201900    22899.
    ##  8 Canada  Americas   1987    76.9 26549700    26627.
    ##  9 Canada  Americas   1992    78.0 28523502    26343.
    ## 10 Canada  Americas   1997    78.6 30305843    28955.
    ## # ... with 26 more rows

``` r
gap_under_29 <- filter(gapminder, lifeExp < 29)

select(gap_under_29, country, year)
```

    ## # A tibble: 2 x 2
    ##   country      year
    ##   <fct>       <int>
    ## 1 Afghanistan  1952
    ## 2 Rwanda       1992

``` r
select(filter(gapminder, lifeExp < 29), country, year)
```

    ## # A tibble: 2 x 2
    ##   country      year
    ##   <fct>       <int>
    ## 1 Afghanistan  1952
    ## 2 Rwanda       1992

``` r
gapminder %>% 
  filter(lifeExp < 29) %>% 
  select(country, year)
```

    ## # A tibble: 2 x 2
    ##   country      year
    ##   <fct>       <int>
    ## 1 Afghanistan  1952
    ## 2 Rwanda       1992

``` r
new_df <- gapminder %>% 
  filter(country == "Cambodia") %>% 
  select(year, lifeExp)
```

### 3\. `mutate` to make new variables.

  - apply **vectorized functions** to columns. Vectorized funs take
    vectors as input and return vectors of the same length as output.

<!-- end list -->

``` r
gapminder %>% 
  mutate(tot_gdp = pop * gdpPercap,
         pop_thousands = pop / 1000,
         lifeExp = round(lifeExp, 0)
         )
```

    ## # A tibble: 1,704 x 8
    ##    country    continent  year lifeExp     pop gdpPercap    tot_gdp pop_thousands
    ##    <fct>      <fct>     <int>   <dbl>   <int>     <dbl>      <dbl>         <dbl>
    ##  1 Afghanist~ Asia       1952      29  8.43e6      779.    6.57e 9         8425.
    ##  2 Afghanist~ Asia       1957      30  9.24e6      821.    7.59e 9         9241.
    ##  3 Afghanist~ Asia       1962      32  1.03e7      853.    8.76e 9        10267.
    ##  4 Afghanist~ Asia       1967      34  1.15e7      836.    9.65e 9        11538.
    ##  5 Afghanist~ Asia       1972      36  1.31e7      740.    9.68e 9        13079.
    ##  6 Afghanist~ Asia       1977      38  1.49e7      786.    1.17e10        14880.
    ##  7 Afghanist~ Asia       1982      40  1.29e7      978.    1.26e10        12882.
    ##  8 Afghanist~ Asia       1987      41  1.39e7      852.    1.18e10        13868.
    ##  9 Afghanist~ Asia       1992      42  1.63e7      649.    1.06e10        16318.
    ## 10 Afghanist~ Asia       1997      42  2.22e7      635.    1.41e10        22227.
    ## # ... with 1,694 more rows

### 4\. `arrange()` to order rows by values of a col or cols (low to high), use with `desc()` to order from high to low

``` r
gapminder %>% 
  arrange(desc(lifeExp))
```

    ## # A tibble: 1,704 x 6
    ##    country          continent  year lifeExp       pop gdpPercap
    ##    <fct>            <fct>     <int>   <dbl>     <int>     <dbl>
    ##  1 Japan            Asia       2007    82.6 127467972    31656.
    ##  2 Hong Kong, China Asia       2007    82.2   6980412    39725.
    ##  3 Japan            Asia       2002    82   127065841    28605.
    ##  4 Iceland          Europe     2007    81.8    301931    36181.
    ##  5 Switzerland      Europe     2007    81.7   7554661    37506.
    ##  6 Hong Kong, China Asia       2002    81.5   6762476    30209.
    ##  7 Australia        Oceania    2007    81.2  20434176    34435.
    ##  8 Spain            Europe     2007    80.9  40448191    28821.
    ##  9 Sweden           Europe     2007    80.9   9031088    33860.
    ## 10 Israel           Asia       2007    80.7   6426679    25523.
    ## # ... with 1,694 more rows

### 5\. `slice(.data, ...)` to index rows by position, `pull(.data, var = -1)` to extract column values as a vector. Choose by name or index

``` r
gapminder %>% 
  arrange(desc(lifeExp)) %>% 
  slice(1) %>% 
  pull(lifeExp,country,)
```

    ##  Japan 
    ## 82.603

### 6\. what is **tidy data**??

  - each variable is in its own col
  - each obs, or case is in its own row

### 7\. `tidyr::pivot_longer`

``` r
table4a
```

    ## # A tibble: 3 x 3
    ##   country     `1999` `2000`
    ## * <chr>        <int>  <int>
    ## 1 Afghanistan    745   2666
    ## 2 Brazil       37737  80488
    ## 3 China       212258 213766

``` r
table4a %>% 
  pivot_longer(cols = `1999`:`2000`, names_to="year", values_to = "cases")
```

    ## # A tibble: 6 x 3
    ##   country     year   cases
    ##   <chr>       <chr>  <int>
    ## 1 Afghanistan 1999     745
    ## 2 Afghanistan 2000    2666
    ## 3 Brazil      1999   37737
    ## 4 Brazil      2000   80488
    ## 5 China       1999  212258
    ## 6 China       2000  213766

``` r
# a less verbose and efficient way to specify this
table4a %>% 
pivot_longer(2:3, names_to="year", values_to="cases")
```

    ## # A tibble: 6 x 3
    ##   country     year   cases
    ##   <chr>       <chr>  <int>
    ## 1 Afghanistan 1999     745
    ## 2 Afghanistan 2000    2666
    ## 3 Brazil      1999   37737
    ## 4 Brazil      2000   80488
    ## 5 China       1999  212258
    ## 6 China       2000  213766

``` r
table4a %>% 
gather(2:3, key="year", value="cases")
```

    ## # A tibble: 6 x 3
    ##   country     year   cases
    ##   <chr>       <chr>  <int>
    ## 1 Afghanistan 1999     745
    ## 2 Brazil      1999   37737
    ## 3 China       1999  212258
    ## 4 Afghanistan 2000    2666
    ## 5 Brazil      2000   80488
    ## 6 China       2000  213766

### 8\. `pivot_wider(), spread(data, key, value, ...)`

``` r
table2
```

    ## # A tibble: 12 x 4
    ##    country      year type            count
    ##    <chr>       <int> <chr>           <int>
    ##  1 Afghanistan  1999 cases             745
    ##  2 Afghanistan  1999 population   19987071
    ##  3 Afghanistan  2000 cases            2666
    ##  4 Afghanistan  2000 population   20595360
    ##  5 Brazil       1999 cases           37737
    ##  6 Brazil       1999 population  172006362
    ##  7 Brazil       2000 cases           80488
    ##  8 Brazil       2000 population  174504898
    ##  9 China        1999 cases          212258
    ## 10 China        1999 population 1272915272
    ## 11 China        2000 cases          213766
    ## 12 China        2000 population 1280428583

``` r
table2 %>% 
  spread(key=type, value = count)
```

    ## # A tibble: 6 x 4
    ##   country      year  cases population
    ##   <chr>       <int>  <int>      <int>
    ## 1 Afghanistan  1999    745   19987071
    ## 2 Afghanistan  2000   2666   20595360
    ## 3 Brazil       1999  37737  172006362
    ## 4 Brazil       2000  80488  174504898
    ## 5 China        1999 212258 1272915272
    ## 6 China        2000 213766 1280428583
