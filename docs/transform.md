
# Data transformation

## Introduction


```r
library("nycflights13")
library("tidyverse")
```

## Filter rows with `filter()`

### Exercise <span class="exercise-number">5.2.4.1</span> {.unnumbered .exercise}

<div class="question">

Find all flights that:

1.  Had an arrival delay of two or more hours
1.  Flew to Houston (IAH or HOU)
1.  Were operated by United, American, or Delta
1.  Departed in summer (July, August, and September)
1.  Arrived more than two hours late, but didn’t leave late
1.  Were delayed by at least an hour, but made up over 30 minutes in flight
1.  Departed between midnight and 6am (inclusive)

</div>

<div class="answer">

The answer to each part follows.

1.  Using a couple of functions to knowledge, including `arrange()` and `%>%` (pipe). More on pipe
    later. Viewed the data `?` and `glimpse()` before constructing `filter()` functions.

    
    ```r
    data('flights')
    glimpse(flights)
    #> Observations: 336,776
    #> Variables: 19
    #> $ year           <int> 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013,...
    #> $ month          <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,...
    #> $ day            <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,...
    #> $ dep_time       <int> 517, 533, 542, 544, 554, 554, 555, 557, 557, 55...
    #> $ sched_dep_time <int> 515, 529, 540, 545, 600, 558, 600, 600, 600, 60...
    #> $ dep_delay      <dbl> 2, 4, 2, -1, -6, -4, -5, -3, -3, -2, -2, -2, -2...
    #> $ arr_time       <int> 830, 850, 923, 1004, 812, 740, 913, 709, 838, 7...
    #> $ sched_arr_time <int> 819, 830, 850, 1022, 837, 728, 854, 723, 846, 7...
    #> $ arr_delay      <dbl> 11, 20, 33, -18, -25, 12, 19, -14, -8, 8, -2, -...
    #> $ carrier        <chr> "UA", "UA", "AA", "B6", "DL", "UA", "B6", "EV",...
    #> $ flight         <int> 1545, 1714, 1141, 725, 461, 1696, 507, 5708, 79...
    #> $ tailnum        <chr> "N14228", "N24211", "N619AA", "N804JB", "N668DN...
    #> $ origin         <chr> "EWR", "LGA", "JFK", "JFK", "LGA", "EWR", "EWR"...
    #> $ dest           <chr> "IAH", "IAH", "MIA", "BQN", "ATL", "ORD", "FLL"...
    #> $ air_time       <dbl> 227, 227, 160, 183, 116, 150, 158, 53, 140, 138...
    #> $ distance       <dbl> 1400, 1416, 1089, 1576, 762, 719, 1065, 229, 94...
    #> $ hour           <dbl> 5, 5, 5, 5, 6, 5, 6, 6, 6, 6, 6, 6, 6, 6, 6, 5,...
    #> $ minute         <dbl> 15, 29, 40, 45, 0, 58, 0, 0, 0, 0, 0, 0, 0, 0, ...
    #> $ time_hour      <dttm> 2013-01-01 05:00:00, 2013-01-01 05:00:00, 2013...
    ?flights
    
    filter(flights, arr_delay >= 120) %>% arrange(arr_delay)
    #> # A tibble: 10,200 x 19
    #>    year month   day dep_time sched_dep_time dep_delay arr_time
    #>   <int> <int> <int>    <int>          <int>     <dbl>    <int>
    #> 1  2013     1     2     1806           1629        97     2008
    #> 2  2013     1    10     1801           1619       102     1923
    #> 3  2013     1    13     1958           1836        82     2231
    #> 4  2013     1    13     2145           2005       100        4
    #> 5  2013     1    14     1652           1445       127     1806
    #> 6  2013     1    15     1603           1446        77     1957
    #> # ... with 1.019e+04 more rows, and 12 more variables:
    #> #   sched_arr_time <int>, arr_delay <dbl>, carrier <chr>, flight <int>,
    #> #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
    #> #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
    ```

1.  Flew to Houston (IAH or HOU)
    
    ```r
    filter(flights, dest %in% c('IAH', 'HOU')) %>% arrange(dest)
    #> # A tibble: 9,313 x 19
    #>    year month   day dep_time sched_dep_time dep_delay arr_time
    #>   <int> <int> <int>    <int>          <int>     <dbl>    <int>
    #> 1  2013     1     1     1208           1158        10     1540
    #> 2  2013     1     1     1306           1300         6     1622
    #> 3  2013     1     1     1708           1700         8     2037
    #> 4  2013     1     1     2030           2035        -5     2354
    #> 5  2013     1     2      734            700        34     1045
    #> 6  2013     1     2     1156           1158        -2     1517
    #> # ... with 9,307 more rows, and 12 more variables: sched_arr_time <int>,
    #> #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    #> #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    #> #   minute <dbl>, time_hour <dttm>
    ```

1.  Were operated by United, American, or Delta
    
    ```r
    filter(flights, carrier %in% c('UA', 'AA', 'DL')) %>% arrange(carrier)
    #> # A tibble: 139,504 x 19
    #>    year month   day dep_time sched_dep_time dep_delay arr_time
    #>   <int> <int> <int>    <int>          <int>     <dbl>    <int>
    #> 1  2013     1     1      542            540         2      923
    #> 2  2013     1     1      558            600        -2      753
    #> 3  2013     1     1      559            600        -1      941
    #> 4  2013     1     1      606            610        -4      858
    #> 5  2013     1     1      623            610        13      920
    #> 6  2013     1     1      628            630        -2     1137
    #> # ... with 1.395e+05 more rows, and 12 more variables:
    #> #   sched_arr_time <int>, arr_delay <dbl>, carrier <chr>, flight <int>,
    #> #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
    #> #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
    ```

1.  Departed in summer (July, August, and September)
    
    ```r
    filter(flights, month >= 7, month <= 9)
    #> # A tibble: 86,326 x 19
    #>    year month   day dep_time sched_dep_time dep_delay arr_time
    #>   <int> <int> <int>    <int>          <int>     <dbl>    <int>
    #> 1  2013     7     1        1           2029       212      236
    #> 2  2013     7     1        2           2359         3      344
    #> 3  2013     7     1       29           2245       104      151
    #> 4  2013     7     1       43           2130       193      322
    #> 5  2013     7     1       44           2150       174      300
    #> 6  2013     7     1       46           2051       235      304
    #> # ... with 8.632e+04 more rows, and 12 more variables:
    #> #   sched_arr_time <int>, arr_delay <dbl>, carrier <chr>, flight <int>,
    #> #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
    #> #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
    ```

1.  Arrived more than two hours late, but didn't leave late
    
    ```r
    filter(flights, dep_delay <= 0, arr_delay > 120)
    #> # A tibble: 29 x 19
    #>    year month   day dep_time sched_dep_time dep_delay arr_time
    #>   <int> <int> <int>    <int>          <int>     <dbl>    <int>
    #> 1  2013     1    27     1419           1420        -1     1754
    #> 2  2013    10     7     1350           1350         0     1736
    #> 3  2013    10     7     1357           1359        -2     1858
    #> 4  2013    10    16      657            700        -3     1258
    #> 5  2013    11     1      658            700        -2     1329
    #> 6  2013     3    18     1844           1847        -3       39
    #> # ... with 23 more rows, and 12 more variables: sched_arr_time <int>,
    #> #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    #> #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    #> #   minute <dbl>, time_hour <dttm>
    ```

1.  Were delayed by at least an hour, but made up over 30 minutes in flight
    
    ```r
    filter(flights, dep_delay >= 60 & dep_delay - arr_delay > 30) %>% 
      arrange(dep_delay, arr_delay)
    #> # A tibble: 1,844 x 19
    #>    year month   day dep_time sched_dep_time dep_delay arr_time
    #>   <int> <int> <int>    <int>          <int>     <dbl>    <int>
    #> 1  2013     2    26     1000            900        60     1513
    #> 2  2013    12    22     1030            930        60     1534
    #> 3  2013     7    20     1725           1625        60     1838
    #> 4  2013    12    27     1929           1829        60     2205
    #> 5  2013     6    28     1105           1005        60     1228
    #> 6  2013     3    10      930            830        60     1207
    #> # ... with 1,838 more rows, and 12 more variables: sched_arr_time <int>,
    #> #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    #> #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    #> #   minute <dbl>, time_hour <dttm>
    ```

1.  Departed between midnight and 6 a.m. (inclusive)
    
    ```r
    filter(flights, between(dep_time, 0, 600)) %>% arrange(dep_time)
    #> # A tibble: 9,344 x 19
    #>    year month   day dep_time sched_dep_time dep_delay arr_time
    #>   <int> <int> <int>    <int>          <int>     <dbl>    <int>
    #> 1  2013     1    13        1           2249        72      108
    #> 2  2013     1    31        1           2100       181      124
    #> 3  2013    11    13        1           2359         2      442
    #> 4  2013    12    16        1           2359         2      447
    #> 5  2013    12    20        1           2359         2      430
    #> 6  2013    12    26        1           2359         2      437
    #> # ... with 9,338 more rows, and 12 more variables: sched_arr_time <int>,
    #> #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    #> #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    #> #   minute <dbl>, time_hour <dttm>
    ```

</div>


