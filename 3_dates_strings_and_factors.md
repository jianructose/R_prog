3\_dates\_strings\_and\_factors
================
JR
Last compiled on Sat Sep 25 17:04:29 2021

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

    ## [1] "2021-09-25"

``` r
class(today())
```

    ## [1] "Date"

``` r
now()
```

    ## [1] "2021-09-25 17:04:35 PDT"

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

    ## [1] "2021-09-25"

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

    ## [1] 25

``` r
# day of a week
paste0("Today is ", wday(datetime, abbr = F, label = T, week_start = 1))
```

    ## [1] "Today is Saturday"

``` r
# day of a quarter
qday(datetime)
```

    ## [1] 87

``` r
# day in a month
mday(datetime)
```

    ## [1] 25

``` r
paste0("Today is the ",yday(datetime), "th day of the year!")
```

    ## [1] "Today is the 268th day of the year!"

``` r
hour(datetime)
```

    ## [1] 17

``` r
minute(datetime)
```

    ## [1] 4

``` r
second(datetime)
```

    ## [1] 36.26992

``` r
print(paste0("It is the ", week(datetime),"th week of the year!"))
```

    ## [1] "It is the 39th week of the year!"

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

    ## [1] "2021-09-25 17:04:36 PDT"

``` r
datetime
```

    ## [1] "2021-09-25 17:04:36 PDT"

## 3\. String manipulations(`stringr, tidyr,`)

### 3.1 Detect Matches and Subset Strings`str_detect(), str_subset()`

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

### 3.2 Join and Split `str_c(), str_c(..., collapse=""),`

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
# sep connects btwn vectors, collapse connects all vec into a single one
```

How to concatenate char vectors when in df

``` r
tibble(x = fruit[22:27], y = fruit[30:35]) %>% 
  unite(col = "mixed flavor", x, y, remove = F, sep = " + ")
```

    ## # A tibble: 6 x 3
    ##   `mixed flavor`      x           y         
    ##   <chr>               <chr>       <chr>     
    ## 1 cucumber + feijoa   cucumber    feijoa    
    ## 2 currant + fig       currant     fig       
    ## 3 damson + goji berry damson      goji berry
    ## 4 date + gooseberry   date        gooseberry
    ## 5 dragonfruit + grape dragonfruit grape     
    ## 6 durian + grapefruit durian      grapefruit

### 3.3 Mutate Strings `str_sub(), str_replace(), str_replace_na(), tidyr::replace_na()`

``` r
fruit %>% 
  str_subset("fruit") %>% 
  str_replace("fruit", "vege")
```

    ## [1] "breadvege"   "dragonvege"  "grapevege"   "jackvege"    "kiwi vege"  
    ## [6] "passionvege" "star vege"   "ugli vege"

## 4\. Examples with gapminder

``` r
library(gapminder)
slice_head(gapminder, n = 6)
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

### How many countries have name starting from “Al”?

``` r
gapminder %>% 
  filter(str_detect(country, "^Al")) %>% 
  select(1) %>% 
  distinct() %>% 
  nrow()
```

    ## [1] 2

### How many country names end in *tan*?

``` r
gapminder %>% 
  select(1) %>% 
  filter(str_detect(country, "tan$")) %>% 
  distinct()
```

    ## # A tibble: 2 x 1
    ##   country    
    ##   <fct>      
    ## 1 Afghanistan
    ## 2 Pakistan

### What are countries contain “, Dem. Rep.”

``` r
gapminder %>% 
  filter(str_detect(country, "\\, Dem. Rep.")) %>% 
  distinct(country) %>% 
  mutate(country = str_replace(country,
                               "\\, Dem. Rep.",
                               " Democratic Republic"))
```

    ## # A tibble: 2 x 1
    ##   country                  
    ##   <chr>                    
    ## 1 Congo Democratic Republic
    ## 2 Korea Democratic Republic

### Extract matches`str_extract(), str_extract_all()`

``` r
typeof(sentences)
```

    ## [1] "character"

``` r
head(sentences)
```

    ## [1] "The birch canoe slid on the smooth planks." 
    ## [2] "Glue the sheet to the dark blue background."
    ## [3] "It's easy to tell the depth of a well."     
    ## [4] "These days a chicken leg is a rare dish."   
    ## [5] "Rice is often served in round bowls."       
    ## [6] "The juice of lemons makes fine punch."

``` r
colors <- "red|yellow|pink|green|purple|blue|orange"

# to see how many color matches
sum(!is.na(str_extract(string = sentences, pattern = colors)))
```

    ## [1] 59

``` r
# it returns a list
str_extract_all(string = sentences, pattern = colors) %>% 
  head()
```

    ## [[1]]
    ## character(0)
    ## 
    ## [[2]]
    ## [1] "blue"
    ## 
    ## [[3]]
    ## character(0)
    ## 
    ## [[4]]
    ## character(0)
    ## 
    ## [[5]]
    ## character(0)
    ## 
    ## [[6]]
    ## character(0)

``` r
# extract n. from sentences
noun <- "(a|the) ([^ ]+)"

(str_match_all(sentences, noun)) %>% 
  head()
```

    ## [[1]]
    ##      [,1]         [,2]  [,3]    
    ## [1,] "the smooth" "the" "smooth"
    ## 
    ## [[2]]
    ##      [,1]        [,2]  [,3]   
    ## [1,] "the sheet" "the" "sheet"
    ## [2,] "the dark"  "the" "dark" 
    ## 
    ## [[3]]
    ##      [,1]        [,2]  [,3]   
    ## [1,] "the depth" "the" "depth"
    ## [2,] "a well."   "a"   "well."
    ## 
    ## [[4]]
    ##      [,1]        [,2] [,3]     
    ## [1,] "a chicken" "a"  "chicken"
    ## [2,] "a rare"    "a"  "rare"   
    ## 
    ## [[5]]
    ##      [,1] [,2] [,3]
    ## 
    ## [[6]]
    ##      [,1] [,2] [,3]

## 5\. Factor inspection

``` r
str(gapminder$continent)
```

    ##  Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...

``` r
levels(gapminder$continent)
```

    ## [1] "Africa"   "Americas" "Asia"     "Europe"   "Oceania"

``` r
nlevels(gapminder$continent)
```

    ## [1] 5

``` r
class(gapminder$continent)
```

    ## [1] "factor"

``` r
gapminder$continent %>% 
  head()
```

    ## [1] Asia Asia Asia Asia Asia Asia
    ## Levels: Africa Americas Asia Europe Oceania

## 1\. Drop unused levels

``` r
nlevels(gapminder$country)
```

    ## [1] 142

``` r
h_countries <- gapminder %>% 
  filter(country %in% c("Egypt", "Haiti", "Romania", "Thailand", "Venezuela"))

h_countries
```

    ## # A tibble: 60 x 6
    ##    country continent  year lifeExp      pop gdpPercap
    ##    <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Egypt   Africa     1952    41.9 22223309     1419.
    ##  2 Egypt   Africa     1957    44.4 25009741     1459.
    ##  3 Egypt   Africa     1962    47.0 28173309     1693.
    ##  4 Egypt   Africa     1967    49.3 31681188     1815.
    ##  5 Egypt   Africa     1972    51.1 34807417     2024.
    ##  6 Egypt   Africa     1977    53.3 38783863     2785.
    ##  7 Egypt   Africa     1982    56.0 45681811     3504.
    ##  8 Egypt   Africa     1987    59.8 52799062     3885.
    ##  9 Egypt   Africa     1992    63.7 59402198     3795.
    ## 10 Egypt   Africa     1997    67.2 66134291     4173.
    ## # ... with 50 more rows

``` r
nlevels(h_countries$country)
```

    ## [1] 142

``` r
h_countries$country %>% 
  fct_drop() %>% 
  nlevels
```

    ## [1] 5

## 2\. Change the order of levels

``` r
gapminder$continent %>% 
  levels()
```

    ## [1] "Africa"   "Americas" "Asia"     "Europe"   "Oceania"

``` r
gapminder$continent %>% 
  fct_infreq() %>% 
  levels()
```

    ## [1] "Africa"   "Asia"     "Europe"   "Americas" "Oceania"

``` r
gap2 <- gapminder %>% 
  mutate(continent = fct_infreq(continent))
gap2$continent %>% 
  levels()
```

    ## [1] "Africa"   "Asia"     "Europe"   "Americas" "Oceania"

``` r
# or reverse freq
gap2$continent %>% 
  fct_rev() %>% 
  levels()
```

    ## [1] "Oceania"  "Americas" "Europe"   "Asia"     "Africa"

## 3\. Reorder levels by their relationship w/ another variable `fct_reorder()`

``` r
#  reorder countries by median lifeExp ascending
fct_reorder(gapminder$country, gapminder$lifeExp) %>% 
  levels() %>% 
  head()
```

    ## [1] "Sierra Leone"  "Guinea-Bissau" "Afghanistan"   "Angola"       
    ## [5] "Somalia"       "Guinea"

``` r
# reorder by min lifeExp
fct_reorder(gapminder$country, gapminder$lifeExp, min) %>% 
  levels() %>% 
  head()
```

    ## [1] "Rwanda"       "Afghanistan"  "Gambia"       "Angola"       "Sierra Leone"
    ## [6] "Cambodia"

## 4\. Manually reorder factor levels

``` r
gapminder$continent %>% 
  fct_relevel("Asia", "Africa") %>% 
  levels()
```

    ## [1] "Asia"     "Africa"   "Americas" "Europe"   "Oceania"
