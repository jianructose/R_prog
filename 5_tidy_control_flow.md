5\_tidy\_control\_flow
================
JR
Last compiled on Mon Oct 25 23:45:34 2021

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
