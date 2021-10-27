5\_tidy\_control\_flow
================
JR
Last compiled on Tue Oct 26 17:37:51 2021

# 1\. Change or remove specific values

  - `mutate` to selectively change values

  - remove NAs

# 1.1 Selectively change a value `case_when()`, *which is for char only*

``` r
gapminder %>% 
  mutate(country = as.character(country),
         continent = as.character(continent)) %>% 
  mutate(country = case_when(country == "Cambodia" ~ "Kingdom of Cambodia", TRUE ~ country)) %>% 
    filter(str_detect(country, "Cam"))
```

    ## # A tibble: 24 x 6
    ##    country             continent  year lifeExp      pop gdpPercap
    ##    <chr>               <chr>     <int>   <dbl>    <int>     <dbl>
    ##  1 Kingdom of Cambodia Asia       1952    39.4  4693836      368.
    ##  2 Kingdom of Cambodia Asia       1957    41.4  5322536      434.
    ##  3 Kingdom of Cambodia Asia       1962    43.4  6083619      497.
    ##  4 Kingdom of Cambodia Asia       1967    45.4  6960067      523.
    ##  5 Kingdom of Cambodia Asia       1972    40.3  7450606      422.
    ##  6 Kingdom of Cambodia Asia       1977    31.2  6978607      525.
    ##  7 Kingdom of Cambodia Asia       1982    51.0  7272485      624.
    ##  8 Kingdom of Cambodia Asia       1987    53.9  8371791      684.
    ##  9 Kingdom of Cambodia Asia       1992    55.8 10150094      682.
    ## 10 Kingdom of Cambodia Asia       1997    56.5 11782962      734.
    ## # ... with 14 more rows

# 1.2 Selectively change 2+ values with `case_when()`

``` r
gapminder %>% 
  mutate(year = as.double(year)) %>% 
  mutate(year = case_when(year <= 1990 ~ year+2, T~year))
```

    ## # A tibble: 1,704 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <dbl>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1954    28.8  8425333      779.
    ##  2 Afghanistan Asia       1959    30.3  9240934      821.
    ##  3 Afghanistan Asia       1964    32.0 10267083      853.
    ##  4 Afghanistan Asia       1969    34.0 11537966      836.
    ##  5 Afghanistan Asia       1974    36.1 13079460      740.
    ##  6 Afghanistan Asia       1979    38.4 14880372      786.
    ##  7 Afghanistan Asia       1984    39.9 12881816      978.
    ##  8 Afghanistan Asia       1989    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # ... with 1,694 more rows

# 1.3 Removing rows where a specific col holds NAs

``` r
df <- tibble(x = c(3, 1, 2, NA),
             y = c("z", "w", NA, "i"))
df %>% 
  drop_na(x)
```

    ## # A tibble: 3 x 2
    ##       x y    
    ##   <dbl> <chr>
    ## 1     3 z    
    ## 2     1 w    
    ## 3     2 <NA>

# 2\. Iterate over groups of rows

# 2.1 `summarise` calculates summaries over rows

``` r
mtcars %>% 
  summarise(mean_hp = mean(hp),
            mean_mpg = mean(mpg),
            sum_hp = sum(hp))
```

    ##    mean_hp mean_mpg sum_hp
    ## 1 146.6875 20.09062   4694

# 2.2 Iteration with `group_by + summarise`(group by one col or 2+ cols)

``` r
gapminder %>% 
  group_by(continent) %>% 
  summarise(mean_life = mean(lifeExp)) %>% 
  arrange(mean_life)
```

    ## # A tibble: 5 x 2
    ##   continent mean_life
    ##   <fct>         <dbl>
    ## 1 Africa         48.9
    ## 2 Asia           60.1
    ## 3 Americas       64.7
    ## 4 Europe         71.9
    ## 5 Oceania        74.3

# 2.3 any `NA` in `summarise()`

``` r
library(palmerpenguins)


penguins %>% 
  group_by(species) %>% 
  summarise(max_body_mass = max(body_mass_g, na.rm = T))
```

    ## # A tibble: 3 x 2
    ##   species   max_body_mass
    ##   <fct>             <int>
    ## 1 Adelie             4775
    ## 2 Chinstrap          4800
    ## 3 Gentoo             6300

# 2.4

``` r
gapminder %>% 
  group_by(country) %>% 
  mutate(life_gain = lifeExp - first(lifeExp)) %>% 
  # arrange(continent) %>% 
  slice(2) # slice will only slice one of the 12 groups
```

    ## # A tibble: 142 x 7
    ## # Groups:   country [142]
    ##    country     continent  year lifeExp      pop gdpPercap life_gain
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>     <dbl>
    ##  1 Afghanistan Asia       1957    30.3  9240934      821.      1.53
    ##  2 Albania     Europe     1957    59.3  1476505     1942.      4.05
    ##  3 Algeria     Africa     1957    45.7 10270856     3014.      2.61
    ##  4 Angola      Africa     1957    32.0  4561361     3828.      1.98
    ##  5 Argentina   Americas   1957    64.4 19610538     6857.      1.91
    ##  6 Australia   Oceania    1957    70.3  9712569    10950.      1.21
    ##  7 Austria     Europe     1957    67.5  6965860     8843.      0.68
    ##  8 Bahrain     Asia       1957    53.8   138655    11636.      2.89
    ##  9 Bangladesh  Asia       1957    39.3 51365468      662.      1.86
    ## 10 Belgium     Europe     1957    69.2  8989111     9715.      1.24
    ## # ... with 132 more rows

# 3\. `map_*` functions

# 3.1 Iterating over cols of a data frame

  - functionals: takes a function as an input and returns a vector as
    output.

<!-- end list -->

``` r
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
my_map <- function(x, fun){
  out <- vector("double", ncol(x))
  for (i in seq_along(x)){
  out[i] <- fun(x[[i]], na.rm = T)
  }
  out
}


my_map(mtcars, max)
```

    ##  [1]  33.900   8.000 472.000 335.000   4.930   5.424  22.900   1.000   1.000
    ## [10]   5.000   8.000

``` r
seq_along(mtcars)
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11

# 3.2 `purrr::map`

``` r
library(purrr)
map_dbl(mtcars, max)
```

    ##     mpg     cyl    disp      hp    drat      wt    qsec      vs      am    gear 
    ##  33.900   8.000 472.000 335.000   4.930   5.424  22.900   1.000   1.000   5.000 
    ##    carb 
    ##   8.000

``` r
map_df(mtcars, max)
```

    ## # A tibble: 1 x 11
    ##     mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
    ##   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ## 1  33.9     8   472   335  4.93  5.42  22.9     1     1     5     8
