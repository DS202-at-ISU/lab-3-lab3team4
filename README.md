
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
deaths <- av %>%
  pivot_longer(starts_with("Death"), names_to = "Time", values_to = "Died") %>% 
  select(
    URL, Name.Alias, Time, Died
  )
deaths$Time <- parse_number(deaths$Time)
View(deaths)


returns <- av %>%
  pivot_longer(starts_with("Return"), names_to = "Time", values_to = "Return") %>% 
  select(
    URL, Name.Alias, Time, Return
  )
returns$Time <- parse_number(returns$Time)
View(returns)
```

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
avengers <- length(unique(deaths[["URL"]]))
numdeaths <- deaths %>% count(Died) %>% filter(Died == "YES")

avg_deaths <- numdeaths$n / avengers
avg_deaths
```

    ## [1] 0.5144509

Each avenger averages 0.5144509 deaths.

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.

#### Anna:

Statement: “I counted 89 total deaths — some unlucky Avengers7 are
basically Meat Loaf with an E-ZPass — and on 57 occasions the individual
made a comeback”

``` r
numdeaths <- deaths %>% count(Died) %>% filter(Died == "YES")
print(numdeaths)
```

    ## # A tibble: 1 × 2
    ##   Died      n
    ##   <chr> <int>
    ## 1 YES      89

``` r
comeback <- returns %>% filter(Return == "YES")
print(count(comeback))
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1    57

From the Avengers Data there is a total of 89 deaths and in total there
are 57 returns of them coming back to life (aka making a comeback). This
means that Five-Thirty-Eight Analysis was correct in its statement about
89 total deaths and 57 comeback occasions.

#### Croix:

“Out of 173 listed Avengers, my analysis found that 69 had died at least
one time after they joined the team.”

``` r
one_plus_deaths <- deaths %>% 
  group_by(URL, Died) %>% 
  summarise(
    mostdeaths = max(Time)
  ) %>% 
  filter(
    Died == 'YES'
  ) %>% nrow()
```

    ## `summarise()` has grouped output by 'URL'. You can override using the `.groups`
    ## argument.

``` r
one_plus_deaths
```

    ## [1] 69

The above code through the summarise function finds the total amount of
times each Avenger died. However all Avengers are included in this
dataframe. Those avengers who never died still have a mostdeaths value
of 1, however have a Died value of “NO”. By filtering to only the rows
where Died has a value of “YES”, we get the list of all the Avengers who
have died, along with the total deaths they experienced, so by getting
the number of rows we get the number of Avengers who have died at least
once. This number is 69, which agrees with the article.

#### Srishti:

#### Brianna:
