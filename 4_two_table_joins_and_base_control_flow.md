4\_two\_table\_joins\_and\_base\_control\_flow
================
JR
Last compiled on Sat Oct 2 15:04:10 2021

## 1\. Combining two data tables: binds and joins

### 1.1 Binds

Row binding

``` r
fship <- tribble(
  ~Film, ~Race, ~Female, ~Male,
  "The fellowship of the ring", "elf", 1111, 319,
  "The fellowship of the ring", "hobbit", 39, 2983,
  "The fellowship of the ring", "man", 8, 2991
)

fship
```

    ## # A tibble: 3 x 4
    ##   Film                       Race   Female  Male
    ##   <chr>                      <chr>   <dbl> <dbl>
    ## 1 The fellowship of the ring elf      1111   319
    ## 2 The fellowship of the ring hobbit     39  2983
    ## 3 The fellowship of the ring man         8  2991

``` r
rking <- tribble(
  ~Film, ~Race, ~Female, ~Male,
  "The Return Of The King", "elf", 927, 870,
  "The Return Of The King", "hobbit", 5, 2098,
  "The Return Of The King", "man", 761, 2991
)

ttow <- tribble(
  ~Film, ~Race, ~Female, ~Male,
  "The Two Towers", "elf", 711, 251,
  "The Two Towers", "hobbit", 0, 3471,
  "The Two Towers", "man", 671, 5610
)

rking
```

    ## # A tibble: 3 x 4
    ##   Film                   Race   Female  Male
    ##   <chr>                  <chr>   <dbl> <dbl>
    ## 1 The Return Of The King elf       927   870
    ## 2 The Return Of The King hobbit      5  2098
    ## 3 The Return Of The King man       761  2991

``` r
ttow
```

    ## # A tibble: 3 x 4
    ##   Film           Race   Female  Male
    ##   <chr>          <chr>   <dbl> <dbl>
    ## 1 The Two Towers elf       711   251
    ## 2 The Two Towers hobbit      0  3471
    ## 3 The Two Towers man       671  5610

``` r
df1 <- bind_rows(fship, ttow, rking)
df1
```

    ## # A tibble: 9 x 4
    ##   Film                       Race   Female  Male
    ##   <chr>                      <chr>   <dbl> <dbl>
    ## 1 The fellowship of the ring elf      1111   319
    ## 2 The fellowship of the ring hobbit     39  2983
    ## 3 The fellowship of the ring man         8  2991
    ## 4 The Two Towers             elf       711   251
    ## 5 The Two Towers             hobbit      0  3471
    ## 6 The Two Towers             man       671  5610
    ## 7 The Return Of The King     elf       927   870
    ## 8 The Return Of The King     hobbit      5  2098
    ## 9 The Return Of The King     man       761  2991

Column binding

``` r
ttow <- tribble(
  ~Film,
  "The Two Towers",
  "The Two Towers",
  "The Two Towers"
)

ttow
```

    ## # A tibble: 3 x 1
    ##   Film          
    ##   <chr>         
    ## 1 The Two Towers
    ## 2 The Two Towers
    ## 3 The Two Towers

``` r
ttow_data <- tribble(
  ~Race, ~Female, ~Male,
  "elf", 3299, 901,
  "hobbit", 87, 1781,
  "man", 9, 1889,
)

bind_cols(ttow, ttow_data)
```

    ## # A tibble: 3 x 4
    ##   Film           Race   Female  Male
    ##   <chr>          <chr>   <dbl> <dbl>
    ## 1 The Two Towers elf      3299   901
    ## 2 The Two Towers hobbit     87  1781
    ## 3 The Two Towers man         9  1889

### 1.2 Joins

Left join

``` r
am_af_df <- gapminder %>% 
  filter(str_detect(continent, "^A[m|f]")) %>% 
  select(1, 2) %>%
  distinct()
  # group_by(country) %>% 
  # slice(1)

am_af_df
```

    ## # A tibble: 77 x 2
    ##    country      continent
    ##    <fct>        <fct>    
    ##  1 Algeria      Africa   
    ##  2 Angola       Africa   
    ##  3 Argentina    Americas 
    ##  4 Benin        Africa   
    ##  5 Bolivia      Americas 
    ##  6 Botswana     Africa   
    ##  7 Brazil       Americas 
    ##  8 Burkina Faso Africa   
    ##  9 Burundi      Africa   
    ## 10 Cameroon     Africa   
    ## # ... with 67 more rows

``` r
country_codes
```

    ## # A tibble: 187 x 3
    ##    country     iso_alpha iso_num
    ##    <chr>       <chr>       <int>
    ##  1 Afghanistan AFG             4
    ##  2 Albania     ALB             8
    ##  3 Algeria     DZA            12
    ##  4 Angola      AGO            24
    ##  5 Argentina   ARG            32
    ##  6 Armenia     ARM            51
    ##  7 Aruba       ABW           533
    ##  8 Australia   AUS            36
    ##  9 Austria     AUT            40
    ## 10 Azerbaijan  AZE            31
    ## # ... with 177 more rows

``` r
left_join(am_af_df, country_codes, by = c("country" = "country"))
```

    ## # A tibble: 77 x 4
    ##    country      continent iso_alpha iso_num
    ##    <chr>        <fct>     <chr>       <int>
    ##  1 Algeria      Africa    DZA            12
    ##  2 Angola       Africa    AGO            24
    ##  3 Argentina    Americas  ARG            32
    ##  4 Benin        Africa    BEN           204
    ##  5 Bolivia      Americas  BOL            68
    ##  6 Botswana     Africa    BWA            72
    ##  7 Brazil       Americas  BRA            76
    ##  8 Burkina Faso Africa    BFA           854
    ##  9 Burundi      Africa    BDI           108
    ## 10 Cameroon     Africa    CMR           120
    ## # ... with 67 more rows

inner\_join() full\_join() semi\_join() anti\_join()

## 2\. Control flow in base R

### 2.1 `for` loops

  - literating over an obj

<!-- end list -->

``` r
seq <- as.double(seq(2, 11))
seq
```

    ##  [1]  2  3  4  5  6  7  8  9 10 11

``` r
typeof(seq)
```

    ## [1] "double"

``` r
for (n in seq){
  print(n^2)
}
```

    ## [1] 4
    ## [1] 9
    ## [1] 16
    ## [1] 25
    ## [1] 36
    ## [1] 49
    ## [1] 64
    ## [1] 81
    ## [1] 100
    ## [1] 121

Using indices in a for loop `seq_along()`

``` r
# for (i in seq(length(seq))){
#   print(seq[i]**3)
#   print(i)
# }

for (i in seq_along(seq)){
  print(seq[i]^3)
  print(i)
}
```

    ## [1] 8
    ## [1] 1
    ## [1] 27
    ## [1] 2
    ## [1] 64
    ## [1] 3
    ## [1] 125
    ## [1] 4
    ## [1] 216
    ## [1] 5
    ## [1] 343
    ## [1] 6
    ## [1] 512
    ## [1] 7
    ## [1] 729
    ## [1] 8
    ## [1] 1000
    ## [1] 9
    ## [1] 1331
    ## [1] 10

## 2.2 Control Flow: `if, if else` and `else` statements

``` r
thres <- 95.

given <- seq(92, 100)

for (i in given){
if (i > thres){
  print("It is over the limit!")
} else if(i < thres){
  print("under the limit :)")
}  else{
  print("exactly at threshold")
}}
```

    ## [1] "under the limit :)"
    ## [1] "under the limit :)"
    ## [1] "under the limit :)"
    ## [1] "exactly at threshold"
    ## [1] "It is over the limit!"
    ## [1] "It is over the limit!"
    ## [1] "It is over the limit!"
    ## [1] "It is over the limit!"
    ## [1] "It is over the limit!"

``` r
given
```

    ## [1]  92  93  94  95  96  97  98  99 100
