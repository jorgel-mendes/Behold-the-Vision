---
title: "Tidy Tuesday 15/10/2019 Big Mtcars"
excerpt_separator: "<!--more-->"
categories:
  - Tidy Tuesday
tags:
  - Tidy Tuesday
  - Smooth line
  - Dispersion plots
  - ggplot
---

# Data description

From From [TidyTuesdays
github](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-10-15):

> This week’s data is from the EPA. The full data dictionary can be
> found at fueleconomy.gov.

> It’s essentially a much much larger and updated dataset covering
> mtcars, the dataset we all know a bit too well\!

> H/t to Ellis Hughes who had a recent blogpost covering this dataset.

# Import data and packages

``` r
library(tidyverse)
library(scales)
library(Cairo)

big_epa_cars <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-10-15/big_epa_cars.csv")
```

# Data

``` r
str(big_epa_cars)
```

    ## Classes 'spec_tbl_df', 'tbl_df', 'tbl' and 'data.frame': 41804 obs. of  83 variables:
    ##  $ barrels08      : num  15.7 30 12.2 30 17.3 ...
    ##  $ barrelsA08     : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ charge120      : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ charge240      : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ city08         : num  19 9 23 10 17 21 22 23 23 23 ...
    ##  $ city08U        : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ cityA08        : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ cityA08U       : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ cityCD         : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ cityE          : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ cityUF         : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ co2            : num  -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 ...
    ##  $ co2A           : num  -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 ...
    ##  $ co2TailpipeAGpm: num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ co2TailpipeGpm : num  423 808 329 808 468 ...
    ##  $ comb08         : num  21 11 27 11 19 22 25 24 26 25 ...
    ##  $ comb08U        : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ combA08        : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ combA08U       : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ combE          : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ combinedCD     : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ combinedUF     : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ cylinders      : num  4 12 4 8 4 4 4 4 4 4 ...
    ##  $ displ          : num  2 4.9 2.2 5.2 2.2 1.8 1.8 1.6 1.6 1.8 ...
    ##  $ drive          : chr  "Rear-Wheel Drive" "Rear-Wheel Drive" "Front-Wheel Drive" "Rear-Wheel Drive" ...
    ##  $ engId          : num  9011 22020 2100 2850 66031 ...
    ##  $ eng_dscr       : chr  "(FFS)" "(GUZZLER)" "(FFS)" NA ...
    ##  $ feScore        : num  -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 ...
    ##  $ fuelCost08     : num  1900 3600 1450 3600 2600 1800 1600 1650 1550 1600 ...
    ##  $ fuelCostA08    : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ fuelType       : chr  "Regular" "Regular" "Regular" "Regular" ...
    ##  $ fuelType1      : chr  "Regular Gasoline" "Regular Gasoline" "Regular Gasoline" "Regular Gasoline" ...
    ##  $ ghgScore       : num  -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 ...
    ##  $ ghgScoreA      : num  -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 ...
    ##  $ highway08      : num  25 14 33 12 23 24 29 26 31 30 ...
    ##  $ highway08U     : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ highwayA08     : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ highwayA08U    : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ highwayCD      : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ highwayE       : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ highwayUF      : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ hlv            : num  0 0 19 0 0 0 0 0 0 0 ...
    ##  $ hpv            : num  0 0 77 0 0 0 0 0 0 0 ...
    ##  $ id             : num  1 10 100 1000 10000 ...
    ##  $ lv2            : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ lv4            : num  0 0 0 0 14 15 15 13 13 13 ...
    ##  $ make           : chr  "Alfa Romeo" "Ferrari" "Dodge" "Dodge" ...
    ##  $ model          : chr  "Spider Veloce 2000" "Testarossa" "Charger" "B150/B250 Wagon 2WD" ...
    ##  $ mpgData        : chr  "Y" "N" "Y" "N" ...
    ##  $ phevBlended    : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
    ##  $ pv2            : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ pv4            : num  0 0 0 0 90 88 88 89 89 89 ...
    ##  $ range          : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ rangeCity      : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ rangeCityA     : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ rangeHwy       : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ rangeHwyA      : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ trany          : chr  "Manual 5-spd" "Manual 5-spd" "Manual 5-spd" "Automatic 3-spd" ...
    ##  $ UCity          : num  23.3 11 29 12.2 21 ...
    ##  $ UCityA         : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ UHighway       : num  35 19 47 16.7 32 ...
    ##  $ UHighwayA      : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ VClass         : chr  "Two Seaters" "Two Seaters" "Subcompact Cars" "Vans" ...
    ##  $ year           : num  1985 1985 1985 1985 1993 ...
    ##  $ youSaveSpend   : num  -2250 -10750 0 -10750 -5750 ...
    ##  $ guzzler        : logi  NA TRUE NA NA NA NA ...
    ##  $ trans_dscr     : chr  NA NA "SIL" NA ...
    ##  $ tCharger       : logi  NA NA NA NA TRUE NA ...
    ##  $ sCharger       : chr  NA NA NA NA ...
    ##  $ atvType        : chr  NA NA NA NA ...
    ##  $ fuelType2      : logi  NA NA NA NA NA NA ...
    ##  $ rangeA         : logi  NA NA NA NA NA NA ...
    ##  $ evMotor        : logi  NA NA NA NA NA NA ...
    ##  $ mfrCode        : logi  NA NA NA NA NA NA ...
    ##  $ c240Dscr       : logi  NA NA NA NA NA NA ...
    ##  $ charge240b     : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ c240bDscr      : logi  NA NA NA NA NA NA ...
    ##  $ createdOn      : chr  "Tue Jan 01 00:00:00 EST 2013" "Tue Jan 01 00:00:00 EST 2013" "Tue Jan 01 00:00:00 EST 2013" "Tue Jan 01 00:00:00 EST 2013" ...
    ##  $ modifiedOn     : chr  "Tue Jan 01 00:00:00 EST 2013" "Tue Jan 01 00:00:00 EST 2013" "Tue Jan 01 00:00:00 EST 2013" "Tue Jan 01 00:00:00 EST 2013" ...
    ##  $ startStop      : logi  NA NA NA NA NA NA ...
    ##  $ phevCity       : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ phevHwy        : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ phevComb       : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  - attr(*, "problems")=Classes 'tbl_df', 'tbl' and 'data.frame': 26930 obs. of  5 variables:
    ##   ..$ row     : int  4430 4431 4432 4433 4442 4443 4444 4448 4449 4450 ...
    ##   ..$ col     : chr  "guzzler" "guzzler" "guzzler" "guzzler" ...
    ##   ..$ expected: chr  "1/0/T/F/TRUE/FALSE" "1/0/T/F/TRUE/FALSE" "1/0/T/F/TRUE/FALSE" "1/0/T/F/TRUE/FALSE" ...
    ##   ..$ actual  : chr  "G" "G" "G" "G" ...
    ##   ..$ file    : chr  "'https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-10-15/big_epa_cars.csv'" "'https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-10-15/big_epa_cars.csv'" "'https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-10-15/big_epa_cars.csv'" "'https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-10-15/big_epa_cars.csv'" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   barrels08 = col_double(),
    ##   ..   barrelsA08 = col_double(),
    ##   ..   charge120 = col_double(),
    ##   ..   charge240 = col_double(),
    ##   ..   city08 = col_double(),
    ##   ..   city08U = col_double(),
    ##   ..   cityA08 = col_double(),
    ##   ..   cityA08U = col_double(),
    ##   ..   cityCD = col_double(),
    ##   ..   cityE = col_double(),
    ##   ..   cityUF = col_double(),
    ##   ..   co2 = col_double(),
    ##   ..   co2A = col_double(),
    ##   ..   co2TailpipeAGpm = col_double(),
    ##   ..   co2TailpipeGpm = col_double(),
    ##   ..   comb08 = col_double(),
    ##   ..   comb08U = col_double(),
    ##   ..   combA08 = col_double(),
    ##   ..   combA08U = col_double(),
    ##   ..   combE = col_double(),
    ##   ..   combinedCD = col_double(),
    ##   ..   combinedUF = col_double(),
    ##   ..   cylinders = col_double(),
    ##   ..   displ = col_double(),
    ##   ..   drive = col_character(),
    ##   ..   engId = col_double(),
    ##   ..   eng_dscr = col_character(),
    ##   ..   feScore = col_double(),
    ##   ..   fuelCost08 = col_double(),
    ##   ..   fuelCostA08 = col_double(),
    ##   ..   fuelType = col_character(),
    ##   ..   fuelType1 = col_character(),
    ##   ..   ghgScore = col_double(),
    ##   ..   ghgScoreA = col_double(),
    ##   ..   highway08 = col_double(),
    ##   ..   highway08U = col_double(),
    ##   ..   highwayA08 = col_double(),
    ##   ..   highwayA08U = col_double(),
    ##   ..   highwayCD = col_double(),
    ##   ..   highwayE = col_double(),
    ##   ..   highwayUF = col_double(),
    ##   ..   hlv = col_double(),
    ##   ..   hpv = col_double(),
    ##   ..   id = col_double(),
    ##   ..   lv2 = col_double(),
    ##   ..   lv4 = col_double(),
    ##   ..   make = col_character(),
    ##   ..   model = col_character(),
    ##   ..   mpgData = col_character(),
    ##   ..   phevBlended = col_logical(),
    ##   ..   pv2 = col_double(),
    ##   ..   pv4 = col_double(),
    ##   ..   range = col_double(),
    ##   ..   rangeCity = col_double(),
    ##   ..   rangeCityA = col_double(),
    ##   ..   rangeHwy = col_double(),
    ##   ..   rangeHwyA = col_double(),
    ##   ..   trany = col_character(),
    ##   ..   UCity = col_double(),
    ##   ..   UCityA = col_double(),
    ##   ..   UHighway = col_double(),
    ##   ..   UHighwayA = col_double(),
    ##   ..   VClass = col_character(),
    ##   ..   year = col_double(),
    ##   ..   youSaveSpend = col_double(),
    ##   ..   guzzler = col_logical(),
    ##   ..   trans_dscr = col_character(),
    ##   ..   tCharger = col_logical(),
    ##   ..   sCharger = col_character(),
    ##   ..   atvType = col_character(),
    ##   ..   fuelType2 = col_logical(),
    ##   ..   rangeA = col_logical(),
    ##   ..   evMotor = col_logical(),
    ##   ..   mfrCode = col_logical(),
    ##   ..   c240Dscr = col_logical(),
    ##   ..   charge240b = col_double(),
    ##   ..   c240bDscr = col_logical(),
    ##   ..   createdOn = col_character(),
    ##   ..   modifiedOn = col_character(),
    ##   ..   startStop = col_logical(),
    ##   ..   phevCity = col_double(),
    ##   ..   phevHwy = col_double(),
    ##   ..   phevComb = col_double()
    ##   .. )

``` r
big_epa_cars %>%
  select(make, youSaveSpend, year, fuelType1) %>% 
  group_by(make) %>% 
  mutate(count = n()) %>% 
  ungroup() %>% 
  mutate(isSaving = factor(ifelse(
    youSaveSpend > 0, "yes", "no"
  ), levels = c("yes", "no"))) %>% 
  ggplot(aes(year, youSaveSpend)) + 
  geom_point(aes(color = isSaving), size = .3) +
  geom_smooth(se = FALSE, color = "blue3", size = .7) +
  labs(
    title = "Some type of fuels are more associated with economical cars",
    subtitle = "Gasoline cars are moving towards bigger savings",
    y = "Savings compared to an average car over 5 years",
    color = "Is saving?"
  ) +
  facet_wrap(~fuelType1)+
  scale_color_manual(values = c("#01d28e", "#e25822")) + 
  scale_y_continuous(label = number_format(scale = 1/1000, prefix = "$",
                                         suffix = "k")) + 
  guides(color = guide_legend(override.aes = list(size=1.3))) +
  theme_dark() +
  theme(
    title = element_text(colour = "darkslategrey"),
    legend.title = element_text(hjust = .5),
    panel.grid = element_blank(),
    legend.text = element_text(colour = "darkslategrey"),
    axis.ticks = element_line(color = 'lightslategrey'),
    axis.text = element_text(color = 'darkslategrey'),
    axis.line = element_blank(),
    axis.title.x = element_blank(), 
    panel.background = element_rect(fill = "grey30"),
    strip.background = element_rect(fill = "grey20")
  ) 
```

![](https://raw.githubusercontent.com/jorgel-mendes/Behold-the-Vision/master/docs/assets/images/bic_point-1.png)<!-- -->