
<!-- README.md is generated from README.Rmd. Please edit that file -->

# almanac

<!-- badges: start -->

[![Travis build
status](https://travis-ci.org/DavisVaughan/almanac.svg?branch=master)](https://travis-ci.org/DavisVaughan/almanac)
[![Codecov test
coverage](https://codecov.io/gh/DavisVaughan/almanac/branch/master/graph/badge.svg)](https://codecov.io/gh/DavisVaughan/almanac?branch=master)
[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
<!-- badges: end -->

``` r
library(almanac)
```

almanac implements a *grammar of schedules*, providing the fundamental
building blocks to construct recurrence rules that identify “events”
such as weekends or holidays. Constructing recurrence rules looks a
little like this:

``` r
# Thanksgiving = "The fourth Thursday in November"
on_thanksgiving <- yearly() %>% 
  recur_on_ymonth("November") %>%
  recur_on_wday("Thursday", nth = 4)
```

After constructing a recurrence rule, it can be used to generate dates
that are in the “recurrence set”.

``` r
sch_seq("2000-01-01", "2006-01-01", on_thanksgiving)
#> [1] "2000-11-23" "2001-11-22" "2002-11-28" "2003-11-27" "2004-11-25"
#> [6] "2005-11-24"
```

Or determine if a particular date is a part of the recurrence set.

``` r
sch_in(c("2000-01-01", "2000-11-23"), on_thanksgiving)
#> [1] FALSE  TRUE
```

It also allows you to shift an existing sequence of dates, “stepping
over” dates that are in the recurrence set.

``` r
wednesday_before_thanksgiving <- "2000-11-22"

# Step forward 2 non-event days, stepping over thanksgiving
sch_step(wednesday_before_thanksgiving, n = 2, on_thanksgiving)
#> [1] "2000-11-25"
```

## Learning More

View the vignettes on [the
website](https://davisvaughan.github.io/almanac/index.html) to learn
more about how to use almanac.

  - `vignette("almanac")`

  - `vignette("adjust-and-shift")`

## Installation

You can NOT install the released version of almanac from
[CRAN](https://CRAN.R-project.org) with:

``` r
# NO! install.packages("almanac")
```

And the development version from [GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("DavisVaughan/almanac")
```

## Acknowledgements

almanac has developed as a composite of ideas from multiple different
libraries.

First off, it directly embeds the *amazing* JavaScript library
[rrule](https://github.com/jakubroztocil/rrule) for the core recurrence
set calculations.

The date shifting / adjusting functions are modeled after similar
functions in [QuantLib](https://github.com/lballabio/QuantLib).

The author of [gs](https://github.com/jameslairdsmith/gs), James
Laird-Smith, has been a great collaborator as we have bounced ideas off
of each other. gs attempts to solve a similar problem, but with a
slightly different implementation. The hope is that gs and almanac will
merge, as they currently overlap a large amount.
