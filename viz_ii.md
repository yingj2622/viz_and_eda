Visualization
================
Ying Jin
2020/10/1

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --

    ## √ ggplot2 3.3.2     √ purrr   0.3.4
    ## √ tibble  3.0.3     √ dplyr   1.0.2
    ## √ tidyr   1.1.2     √ stringr 1.4.0
    ## √ readr   1.3.1     √ forcats 0.5.0

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(patchwork)
```

## Load the weather data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: C:\Users\jinyc\AppData\Local\cache/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2020-10-01 23:25:20 (7.534)

    ## file min/max dates: 1869-01-01 / 2020-09-30

    ## using cached file: C:\Users\jinyc\AppData\Local\cache/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2020-10-01 23:28:19 (1.703)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: C:\Users\jinyc\AppData\Local\cache/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2020-10-01 22:54:34 (0.879)

    ## file min/max dates: 1999-09-01 / 2020-09-30

## Remember this plot…?

``` r
weather_df %>% 
  ggplot(aes(x = tmin,y=tmax,color = name)) +
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_ii_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

## Label

``` r
weather_df %>% 
  ggplot(aes(x = tmin,y=tmax,color = name)) +
  geom_point(alpha = .5)+
  labs(
    title="Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    caption = "Data from the rnoaa package,temperature in 2017"
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_ii_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

# Scales

Start with the same plot; x and y scales

``` r
weather_df %>% 
  ggplot(aes(x = tmin,y=tmax,color = name)) +
  geom_point(alpha = .5)+
  labs(
    title="Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    caption = "Data from the rnoaa package,temperature in 2017"
  ) +
  scale_x_continuous(
    breaks = c(-15,0,15),
    labels = c("-15 C","0","15 C")
  ) +
  scale_y_continuous(
    position = "right"
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_ii_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Look at color scales

``` r
weather_df %>% 
  ggplot(aes(x = tmin,y=tmax,color = name)) +
  geom_point(alpha = .5)+
  labs(
    title="Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    caption = "Data from the rnoaa package,temperature in 2017"
  )+
  scale_color_hue(
    name = "Location",
    h = c(100,300))
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_ii_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Another way to modify color scale

``` r
weather_df %>% 
  ggplot(aes(x = tmin,y=tmax,color = name)) +
  geom_point(alpha = .5)+
  labs(
    title="Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    caption = "Data from the rnoaa package,temperature in 2017"
  ) +
  viridis::scale_color_viridis(
    name = "Loctation",
    discrete = TRUE
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_ii_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
