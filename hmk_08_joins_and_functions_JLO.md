HMK 08 template
================

# Joins

1.  Imagine you’ve found the top 10 most popular destinations using this
    code:

``` r
library(nycflights13)
library(tidyverse)

top_dest <- flights |>
  count(dest, sort = TRUE) |>
  head(10)
```

How can you find all flights to those destinations?

``` r
top_dest_flights <- top_dest |>
  left_join(flights) |>
  select(dest, carrier, flight)

glimpse(group_by(top_dest_flights, dest))
```

    Rows: 141,145
    Columns: 3
    Groups: dest [10]
    $ dest    <chr> "ORD", "ORD", "ORD", "ORD", "ORD", "ORD", "ORD", "ORD", "ORD",…
    $ carrier <chr> "UA", "AA", "MQ", "AA", "AA", "UA", "UA", "AA", "MQ", "B6", "A…
    $ flight  <int> 1696, 301, 3768, 303, 305, 1092, 544, 309, 3737, 905, 313, 580…

# Functions

2.  Write a function to ‘rescale’ a numeric vector by subtracting the
    mean of the vector from each element and then dividing each element
    by the standard deviation.

``` r
rescale02 <- function(x) {
  ((x-mean(x))/sd(x))
}

rescale02(c(0:10))
```

     [1] -1.5075567 -1.2060454 -0.9045340 -0.6030227 -0.3015113  0.0000000
     [7]  0.3015113  0.6030227  0.9045340  1.2060454  1.5075567
