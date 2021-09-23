3\_dates\_strings\_and\_factors
================
JR
Last compiled on Wed Sep 22 23:42:40 2021

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

    ## Warning: package 'lubridate' was built under R version 4.0.5

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

    ## [1] "2021-09-22 23:42:41 PDT"

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
ydm(20212109)
```

    ## [1] "2021-09-21"

``` r
mdy("Sep 22 2021")
```

    ## [1] "2021-09-22"

``` r
dmy("22th of Sep '21")
```

    ## [1] "2021-09-22"

``` r
ymd_hms("20210922T16:12:12")
```

    ## [1] "2021-09-22 16:12:12 UTC"

``` r
ymd_hm("2029-11-11 18:18")
```

    ## [1] "2029-11-11 18:18:00 UTC"

``` r
ymd_h("2025-09-09 11")
```

    ## [1] "2025-09-09 11:00:00 UTC"

``` r
yq(202204)
```

    ## [1] "2022-10-01"

``` r
ym("2211")
```

    ## [1] "2022-11-01"

``` r
my(888)
```

    ## [1] "1988-08-01"

``` r
hms("17:11:11")
```

    ## [1] "17H 11M 11S"

### 2.2 Build date or date-time from parts

``` r
dates <- tibble(year = c(2015, 2016, 2017, 2018, 2019, 2020, 2021),
                month = rep(4, 7),
                day = rep(22, 7))

dates
```

    ## # A tibble: 7 x 3
    ##    year month   day
    ##   <dbl> <dbl> <dbl>
    ## 1  2015     4    22
    ## 2  2016     4    22
    ## 3  2017     4    22
    ## 4  2018     4    22
    ## 5  2019     4    22
    ## 6  2020     4    22
    ## 7  2021     4    22

``` r
dates %>% 
  mutate(date = make_date(year, month, day))
```

    ## # A tibble: 7 x 4
    ##    year month   day date      
    ##   <dbl> <dbl> <dbl> <date>    
    ## 1  2015     4    22 2015-04-22
    ## 2  2016     4    22 2016-04-22
    ## 3  2017     4    22 2017-04-22
    ## 4  2018     4    22 2018-04-22
    ## 5  2019     4    22 2019-04-22
    ## 6  2020     4    22 2020-04-22
    ## 7  2021     4    22 2021-04-22

### 2.3 Get and set components (4+3+3+2)

``` r
datetime <- now()

date(datetime)
```

    ## [1] "2021-09-22"

``` r
year(datetime) 
```

    ## [1] 2021

``` r
isoyear(datetime)
```

    ## [1] 2021

``` r
epiyear(datetime)
```

    ## [1] 2021

``` r
month(datetime)
```

    ## [1] 9

``` r
day(datetime)
```

    ## [1] 22

``` r
# day of a week
paste0("Today is ", wday(datetime, abbr = F, label = T, week_start = 1))
```

    ## [1] "Today is Wednesday"

``` r
# day of a quarter
qday(datetime)
```

    ## [1] 84

``` r
# day in a month
mday(datetime)
```

    ## [1] 22

``` r
paste0("Today is the ",yday(datetime), "th day of the year!")
```

    ## [1] "Today is the 265th day of the year!"

``` r
hour(datetime)
```

    ## [1] 23

``` r
minute(datetime)
```

    ## [1] 42

``` r
second(datetime)
```

    ## [1] 42.16242

``` r
print(paste0("It is the ", week(datetime),"th week of the year!"))
```

    ## [1] "It is the 38th week of the year!"

``` r
quarter(datetime)
```

    ## [1] 3

``` r
semester(datetime)
```

    ## [1] 2

``` r
am(datetime)
```

    ## [1] FALSE

``` r
pm(datetime)
```

    ## [1] TRUE

``` r
leap_year(datetime)
```

    ## [1] FALSE

``` r
update(datetime)
```

    ## [1] "2021-09-22 23:42:42 PDT"

``` r
datetime
```

    ## [1] "2021-09-22 23:42:42 PDT"

## 3\. String manipulations(`stringr, tidyr,`)

### 3.1 `str_detect(), str_subset()`

``` r
fruit
```

    ##  [1] "apple"             "apricot"           "avocado"          
    ##  [4] "banana"            "bell pepper"       "bilberry"         
    ##  [7] "blackberry"        "blackcurrant"      "blood orange"     
    ## [10] "blueberry"         "boysenberry"       "breadfruit"       
    ## [13] "canary melon"      "cantaloupe"        "cherimoya"        
    ## [16] "cherry"            "chili pepper"      "clementine"       
    ## [19] "cloudberry"        "coconut"           "cranberry"        
    ## [22] "cucumber"          "currant"           "damson"           
    ## [25] "date"              "dragonfruit"       "durian"           
    ## [28] "eggplant"          "elderberry"        "feijoa"           
    ## [31] "fig"               "goji berry"        "gooseberry"       
    ## [34] "grape"             "grapefruit"        "guava"            
    ## [37] "honeydew"          "huckleberry"       "jackfruit"        
    ## [40] "jambul"            "jujube"            "kiwi fruit"       
    ## [43] "kumquat"           "lemon"             "lime"             
    ## [46] "loquat"            "lychee"            "mandarine"        
    ## [49] "mango"             "mulberry"          "nectarine"        
    ## [52] "nut"               "olive"             "orange"           
    ## [55] "pamelo"            "papaya"            "passionfruit"     
    ## [58] "peach"             "pear"              "persimmon"        
    ## [61] "physalis"          "pineapple"         "plum"             
    ## [64] "pomegranate"       "pomelo"            "purple mangosteen"
    ## [67] "quince"            "raisin"            "rambutan"         
    ## [70] "raspberry"         "redcurrant"        "rock melon"       
    ## [73] "salal berry"       "satsuma"           "star fruit"       
    ## [76] "strawberry"        "tamarillo"         "tangerine"        
    ## [79] "ugli fruit"        "watermelon"

``` r
fruit[str_detect(fruit, "fruit")]
```

    ## [1] "breadfruit"   "dragonfruit"  "grapefruit"   "jackfruit"    "kiwi fruit"  
    ## [6] "passionfruit" "star fruit"   "ugli fruit"

``` r
my_fruit <- str_subset(fruit, "fruit")
my_fruit
```

    ## [1] "breadfruit"   "dragonfruit"  "grapefruit"   "jackfruit"    "kiwi fruit"  
    ## [6] "passionfruit" "star fruit"   "ugli fruit"

Split string by delimiter `str_split(), str_split_fixed()`

``` r
str_split(my_fruit, " ")
```

    ## [[1]]
    ## [1] "breadfruit"
    ## 
    ## [[2]]
    ## [1] "dragonfruit"
    ## 
    ## [[3]]
    ## [1] "grapefruit"
    ## 
    ## [[4]]
    ## [1] "jackfruit"
    ## 
    ## [[5]]
    ## [1] "kiwi"  "fruit"
    ## 
    ## [[6]]
    ## [1] "passionfruit"
    ## 
    ## [[7]]
    ## [1] "star"  "fruit"
    ## 
    ## [[8]]
    ## [1] "ugli"  "fruit"

``` r
str_split_fixed(my_fruit, " ", 2)
```

    ##      [,1]           [,2]   
    ## [1,] "breadfruit"   ""     
    ## [2,] "dragonfruit"  ""     
    ## [3,] "grapefruit"   ""     
    ## [4,] "jackfruit"    ""     
    ## [5,] "kiwi"         "fruit"
    ## [6,] "passionfruit" ""     
    ## [7,] "star"         "fruit"
    ## [8,] "ugli"         "fruit"

If the to-be-split variable in a df

``` r
my_fruit[5] <- "yellow kiwi fruit"
my_fruit
```

    ## [1] "breadfruit"        "dragonfruit"       "grapefruit"       
    ## [4] "jackfruit"         "yellow kiwi fruit" "passionfruit"     
    ## [7] "star fruit"        "ugli fruit"

``` r
tibble(unsplit = my_fruit) %>% 
  separate(unsplit, into = c("pre", "mid","post"), sep = " ")
```

    ## Warning: Expected 3 pieces. Missing pieces filled with `NA` in 7 rows [1, 2, 3,
    ## 4, 6, 7, 8].

    ## # A tibble: 8 x 3
    ##   pre          mid   post 
    ##   <chr>        <chr> <chr>
    ## 1 breadfruit   <NA>  <NA> 
    ## 2 dragonfruit  <NA>  <NA> 
    ## 3 grapefruit   <NA>  <NA> 
    ## 4 jackfruit    <NA>  <NA> 
    ## 5 yellow       kiwi  fruit
    ## 6 passionfruit <NA>  <NA> 
    ## 7 star         fruit <NA> 
    ## 8 ugli         fruit <NA>

``` r
str_length(my_fruit)
```

    ## [1] 10 11 10  9 17 12 10 10

``` r
str_sub(my_fruit, 1, 4) <- "AAAA"
my_fruit
```

    ## [1] "AAAAdfruit"        "AAAAonfruit"       "AAAAefruit"       
    ## [4] "AAAAfruit"         "AAAAow kiwi fruit" "AAAAionfruit"     
    ## [7] "AAAA fruit"        "AAAA fruit"

### 3.2 Join and Split `str_c(), str_c(..., collapse="")`

``` r
head(fruit) %>% 
  str_c(sep = "=", collapse = "|")
```

    ## [1] "apple|apricot|avocado|banana|bell pepper|bilberry"

``` r
fruit[1:4]
```

    ## [1] "apple"   "apricot" "avocado" "banana"

``` r
fruit[5:8]
```

    ## [1] "bell pepper"  "bilberry"     "blackberry"   "blackcurrant"

``` r
str_c(fruit[5:8], fruit[1:4], sep = "4", collapse = "99")
```

    ## [1] "bell pepper4apple99bilberry4apricot99blackberry4avocado99blackcurrant4banana"

``` r
# sep = 
```
