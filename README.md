
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

“There’s a 2-in-3 chance that a member of the Avengers returned from
their first stint in the afterlife”

``` r
deathsNReturns <- merge(deaths, returns, by =c("URL", "Time"))


filter(deathsNReturns, Time == 1)
```

    ##                                                                     URL Time
    ## 1                     http://marvel.wikia.com/2ZP45-9-X-51_(Earth-616)#    1
    ## 2            http://marvel.wikia.com/Abyss_(Ex_Nihilo%27s)_(Earth-616)#    1
    ## 3                    http://marvel.wikia.com/Adam_Brashear_(Earth-616)#    1
    ## 4                       http://marvel.wikia.com/Alani_Ryan_(Earth-616)#    1
    ## 5                http://marvel.wikia.com/Alexander_Summers_(Earth-616)#    1
    ## 6                           http://marvel.wikia.com/Alexis_(Earth-616)#    1
    ## 7                      http://marvel.wikia.com/Amadeus_Cho_(Earth-616)#    1
    ## 8                   http://marvel.wikia.com/America_Chavez_(Earth-616)#    1
    ## 9                   http://marvel.wikia.com/Angelica_Jones_(Earth-616)#    1
    ## 10                   http://marvel.wikia.com/Anthony_Druid_(Earth-616)#    1
    ## 11                    http://marvel.wikia.com/Anthony_Stark_(Earth-616)    1
    ## 12                 http://marvel.wikia.com/Anthony_Stark_(Earth-96020)#    1
    ## 13                    http://marvel.wikia.com/Anya_Corazon_(Earth-616)#    1
    ## 14                            http://marvel.wikia.com/Ares_(Earth-616)#    1
    ## 15                 http://marvel.wikia.com/Ashley_Crawford_(Earth-616)#    1
    ## 16                       http://marvel.wikia.com/Ava_Ayala_(Earth-616)#    1
    ## 17                   http://marvel.wikia.com/Barbara_Morse_(Earth-616)#    1
    ## 18                  http://marvel.wikia.com/Benjamin_Grimm_(Earth-616)#    1
    ## 19                   http://marvel.wikia.com/Bonita_Juarez_(Earth-616)#    1
    ## 20                  http://marvel.wikia.com/Brandon_Sharpe_(Earth-616)#    1
    ## 21                  http://marvel.wikia.com/Brian_Braddock_(Earth-616)#    1
    ## 22                      http://marvel.wikia.com/Brunnhilde_(Earth-616)#    1
    ## 23                http://marvel.wikia.com/Captain_Universe_(Earth-616)#    1
    ## 24                   http://marvel.wikia.com/Carol_Danvers_(Earth-616)#    1
    ## 25                  http://marvel.wikia.com/Cassandra_Lang_(Earth-616)#    1
    ## 26                      http://marvel.wikia.com/Charlie-27_(Earth-691)#    1
    ## 27                    http://marvel.wikia.com/Chris_Powell_(Earth-616)#    1
    ## 28                     http://marvel.wikia.com/Clint_Barton_(Earth-616)    1
    ## 29                    http://marvel.wikia.com/Craig_Hollis_(Earth-616)#    1
    ## 30             http://marvel.wikia.com/Crystalia_Amaquelin_(Earth-616)#    1
    ## 31                   http://marvel.wikia.com/Daisy_Johnson_(Earth-616)#    1
    ## 32                                 http://marvel.wikia.com/Dane_Whitman    1
    ## 33                     http://marvel.wikia.com/Daniel_Rand_(Earth-616)#    1
    ## 34                   http://marvel.wikia.com/David_Alleyne_(Earth-616)#    1
    ## 35                        http://marvel.wikia.com/Deathcry_(Earth-616)#    1
    ## 36              http://marvel.wikia.com/Delroy_Garrett_Jr._(Earth-616)#    1
    ## 37                    http://marvel.wikia.com/DeMarr_Davis_(Earth-616)#    1
    ## 38                   http://marvel.wikia.com/Dennis_Dunphy_(Earth-616)#    1
    ## 39                    http://marvel.wikia.com/Dennis_Sykes_(Earth-616)#    1
    ## 40                      http://marvel.wikia.com/Dinah_Soar_(Earth-616)#    1
    ## 41               http://marvel.wikia.com/Doombot_(Avenger)_(Earth-616)#    1
    ## 42                    http://marvel.wikia.com/Doreen_Green_(Earth-616)#    1
    ## 43                     http://marvel.wikia.com/Dorrek_VIII_(Earth-616)#    1
    ## 44                    http://marvel.wikia.com/Doug_Taggert_(Earth-616)#    1
    ## 45                       http://marvel.wikia.com/Eden_Fesi_(Earth-616)#    1
    ## 46                  http://marvel.wikia.com/Elijah_Bradley_(Earth-616)#    1
    ## 47                   http://marvel.wikia.com/Elvin_Haliday_(Earth-616)#    1
    ## 48                    http://marvel.wikia.com/Emery_Schaub_(Earth-616)#    1
    ## 49                     http://marvel.wikia.com/Eric_Brooks_(Earth-616)#    1
    ## 50                  http://marvel.wikia.com/Eric_Masterson_(Earth-616)#    1
    ## 51                  http://marvel.wikia.com/Eric_O%27Grady_(Earth-616)#    1
    ## 52                            http://marvel.wikia.com/Eros_(Earth-616)#    1
    ## 53                 http://marvel.wikia.com/Eugene_Thompson_(Earth-616)#    1
    ## 54                       http://marvel.wikia.com/Ex_Nihilo_(Earth-616)#    1
    ## 55                 http://marvel.wikia.com/Fiona_(Inhuman)_(Earth-616)#    1
    ## 56                    http://marvel.wikia.com/Gene_Lorrene_(Earth-616)#    1
    ## 57                       http://marvel.wikia.com/Gilgamesh_(Earth-616)#    1
    ## 58       http://marvel.wikia.com/Giuletta_Nefaria_(Masque)_(Earth-616)#    1
    ## 59                    http://marvel.wikia.com/Greer_Nelson_(Earth-616)#    1
    ## 60                     http://marvel.wikia.com/Greg_Willis_(Earth-616)#    1
    ## 61                 http://marvel.wikia.com/Heather_Douglas_(Earth-616)#    1
    ## 62                     http://marvel.wikia.com/Henry_McCoy_(Earth-616)#    1
    ## 63                        http://marvel.wikia.com/Henry_Pym_(Earth-616)    1
    ## 64                         http://marvel.wikia.com/Hercules_(Earth-616)    1
    ## 65                          http://marvel.wikia.com/Hollow_(Earth-616)#    1
    ## 66           http://marvel.wikia.com/Human_Torch_(Android)_(Earth-616)#    1
    ## 67                  http://marvel.wikia.com/Humberto_Lopez_(Earth-616)#    1
    ## 68                     http://marvel.wikia.com/Isabel_Kane_(Earth-616)#    1
    ## 69                 http://marvel.wikia.com/Jacques_Duquesne_(Earth-616)    1
    ## 70           http://marvel.wikia.com/James_Buchanan_Barnes_(Earth-616)#    1
    ## 71                   http://marvel.wikia.com/James_Howlett_(Earth-616)#    1
    ## 72                    http://marvel.wikia.com/James_Rhodes_(Earth-616)#    1
    ## 73                   http://marvel.wikia.com/James_Santini_(Earth-616)#    1
    ## 74                   http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)    1
    ## 75                 http://marvel.wikia.com/Jeanne_Foucault_(Earth-616)#    1
    ## 76                 http://marvel.wikia.com/Jennifer_Takeda_(Earth-616)#    1
    ## 77                http://marvel.wikia.com/Jennifer_Walters_(Earth-616)#    1
    ## 78                    http://marvel.wikia.com/Jessica_Drew_(Earth-616)#    1
    ## 79                   http://marvel.wikia.com/Jessica_Jones_(Earth-616)#    1
    ## 80                         http://marvel.wikia.com/Jocasta_(Earth-616)#    1
    ## 81                       http://marvel.wikia.com/John_Aman_(Earth-616)#    1
    ## 82                     http://marvel.wikia.com/John_Walker_(Earth-616)#    1
    ## 83                 http://marvel.wikia.com/Johnathon_Gallo_(Earth-616)#    1
    ## 84                   http://marvel.wikia.com/Jonathan_Hart_(Earth-616)#    1
    ## 85                 http://marvel.wikia.com/Julia_Carpenter_(Earth-616)#    1
    ## 86                     http://marvel.wikia.com/Julie_Power_(Earth-616)#    1
    ## 87                           http://marvel.wikia.com/Kaluu_(Earth-616)#    1
    ## 88                http://marvel.wikia.com/Katherine_Bishop_(Earth-616)#    1
    ## 89                    http://marvel.wikia.com/Kelsey_Leigh_(Earth-616)#    1
    ## 90                        http://marvel.wikia.com/Ken_Mack_(Earth-616)#    1
    ## 91                    http://marvel.wikia.com/Kevin_Connor_(Earth-616)#    1
    ## 92                 http://marvel.wikia.com/Kevin_Masterson_(Earth-616)#    1
    ## 93           http://marvel.wikia.com/Loki_Laufeyson_(Ikol)_(Earth-616)#    1
    ## 94                       http://marvel.wikia.com/Luke_Cage_(Earth-616)#    1
    ## 95                           http://marvel.wikia.com/Lyra_(Earth-8009)#    1
    ## 96                          http://marvel.wikia.com/Mantis_(Earth-616)#    1
    ## 97                        http://marvel.wikia.com/Mar-Vell_(Earth-616)#    1
    ## 98                    http://marvel.wikia.com/Marc_Spector_(Earth-616)#    1
    ## 99                 http://marvel.wikia.com/Marcus_Milton_(Earth-13034)#    1
    ## 100    http://marvel.wikia.com/Maria_de_Guadalupe_Santiago_(Earth-616)#    1
    ## 101                     http://marvel.wikia.com/Maria_Hill_(Earth-616)#    1
    ## 102              http://marvel.wikia.com/Marrina_Smallwood_(Earth-616)#    1
    ## 103              http://marvel.wikia.com/Martinex_T%27Naga_(Earth-691)#    1
    ## 104                   http://marvel.wikia.com/Matthew_Hawk_(Earth-616)#    1
    ## 105                http://marvel.wikia.com/Matthew_Murdock_(Earth-616)#    1
    ## 106                     http://marvel.wikia.com/Maya_Lopez_(Earth-616)#    1
    ## 107                http://marvel.wikia.com/Melissa_Darrow_(Earth-9201)#    1
    ## 108                http://marvel.wikia.com/Michiko_Musashi_(Earth-616)#    1
    ## 109                  http://marvel.wikia.com/Miguel_Santos_(Earth-616)#    1
    ## 110                  http://marvel.wikia.com/Moira_Brandon_(Earth-616)#    1
    ## 111                   http://marvel.wikia.com/Monica_Chang_(Earth-616)#    1
    ## 112                 http://marvel.wikia.com/Monica_Rambeau_(Earth-616)#    1
    ## 113                     http://marvel.wikia.com/Monkey_Joe_(Earth-616)#    1
    ## 114                 http://marvel.wikia.com/Namor_McKenzie_(Earth-616)#    1
    ## 115               http://marvel.wikia.com/Natalia_Romanova_(Earth-616)#    1
    ## 116 http://marvel.wikia.com/Nathaniel_Richards_(Iron_Lad)_(Earth-6311)#    1
    ## 117             http://marvel.wikia.com/Nicholas_Fury,_Jr._(Earth-616)#    1
    ## 118                http://marvel.wikia.com/Nicholette_Gold_(Earth-691)#    1
    ## 119                      http://marvel.wikia.com/Nightmask_(Earth-616)#    1
    ## 120                       http://marvel.wikia.com/Noh-Varr_(Earth-616)#    1
    ## 121                   http://marvel.wikia.com/Ororo_Munroe_(Earth-616)#    1
    ## 122                  http://marvel.wikia.com/Otto_Octavius_(Earth-616)#    1
    ## 123                http://marvel.wikia.com/Patricia_Walker_(Earth-616)#    1
    ## 124                   http://marvel.wikia.com/Peter_Parker_(Earth-616)#    1
    ## 125                  http://marvel.wikia.com/Philip_Javert_(Earth-921)#    1
    ## 126                http://marvel.wikia.com/Phillip_Coulson_(Earth-616)#    1
    ## 127                 http://marvel.wikia.com/Pietro_Maximoff_(Earth-616)    1
    ## 128             http://marvel.wikia.com/Ravonna_Renslayer_(Earth-6311)#    1
    ## 129                  http://marvel.wikia.com/Reed_Richards_(Earth-616)#    1
    ## 130                   http://marvel.wikia.com/Richard_Jones_(Earth-616)    1
    ## 131                  http://marvel.wikia.com/Richard_Rider_(Earth-616)#    1
    ## 132                    http://marvel.wikia.com/Rita_DeMara_(Earth-616)#    1
    ## 133                 http://marvel.wikia.com/Robert_Baldwin_(Earth-616)#    1
    ## 134             http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)    1
    ## 135                   http://marvel.wikia.com/Robert_Frank_(Earth-616)#    1
    ## 136                http://marvel.wikia.com/Robert_Reynolds_(Earth-616)#    1
    ## 137               http://marvel.wikia.com/Roberto_da_Costa_(Earth-616)#    1
    ## 138             http://marvel.wikia.com/Rogue_(Anna_Marie)_(Earth-616)#    1
    ## 139                  http://marvel.wikia.com/Sam_Alexander_(Earth-616)#    1
    ## 140                 http://marvel.wikia.com/Samuel_Guthrie_(Earth-616)#    1
    ## 141                  http://marvel.wikia.com/Samuel_Wilson_(Earth-616)#    1
    ## 142                     http://marvel.wikia.com/Scott_Lang_(Earth-616)#    1
    ## 143                          http://marvel.wikia.com/Sersi_(Earth-616)#    1
    ## 144                      http://marvel.wikia.com/Shang-Chi_(Earth-616)#    1
    ## 145                  http://marvel.wikia.com/Sharon_Carter_(Earth-616)#    1
    ## 146                  http://marvel.wikia.com/Shiro_Yoshida_(Earth-616)#    1
    ## 147                 http://marvel.wikia.com/Simon_Williams_(Earth-616)#    1
    ## 148                   http://marvel.wikia.com/Stakar_Ogord_(Earth-691)#    1
    ## 149                http://marvel.wikia.com/Stephen_Strange_(Earth-616)#    1
    ## 150                   http://marvel.wikia.com/Steven_Rogers_(Earth-616)    1
    ## 151                    http://marvel.wikia.com/Susan_Storm_(Earth-616)#    1
    ## 152                      http://marvel.wikia.com/T%27Challa_(Earth-616)    1
    ## 153                http://marvel.wikia.com/Takashi_Matsuya_(Earth-616)#    1
    ## 154                  http://marvel.wikia.com/Thaddeus_Ross_(Earth-616)#    1
    ## 155                http://marvel.wikia.com/Thomas_Shepherd_(Earth-616)#    1
    ## 156                    http://marvel.wikia.com/Thor_Odinson_(Earth-616)    1
    ## 157                      http://marvel.wikia.com/Tippy-Toe_(Earth-616)#    1
    ## 158                   http://marvel.wikia.com/Tony_Masters_(Earth-616)#    1
    ## 159                    http://marvel.wikia.com/Val_Ventura_(Earth-616)#    1
    ## 160                    http://marvel.wikia.com/Vance_Astro_(Earth-691)#    1
    ## 161                 http://marvel.wikia.com/Vance_Astrovik_(Earth-616)#    1
    ## 162                        http://marvel.wikia.com/Veranke_(Earth-616)#    1
    ## 163                 http://marvel.wikia.com/Victor_Alvarez_(Earth-616)#    1
    ## 164                  http://marvel.wikia.com/Victor_Mancha_(Earth-616)#    1
    ## 165                          http://marvel.wikia.com/Vision_(Earth-616)    1
    ## 166                 http://marvel.wikia.com/Vision_(Jonas)_(Earth-616)#    1
    ## 167                    http://marvel.wikia.com/Wade_Wilson_(Earth-616)#    1
    ## 168                  http://marvel.wikia.com/Walter_Newell_(Earth-616)#    1
    ## 169                  http://marvel.wikia.com/Wanda_Maximoff_(Earth-616)    1
    ## 170                 http://marvel.wikia.com/Wendell_Vaughn_(Earth-616)#    1
    ## 171                  http://marvel.wikia.com/William_Baker_(Earth-616)#    1
    ## 172                 http://marvel.wikia.com/William_Kaplan_(Earth-616)#    1
    ## 173                   http://marvel.wikia.com/Yondu_Udonta_(Earth-691)#    1
    ##                            Name.Alias.x Died
    ## 1                                  X-51  YES
    ## 2                                        YES
    ## 3                         Adam Brashear   NO
    ## 4                            Alani Ryan   NO
    ## 5                          Alex Summers   NO
    ## 6                                Alexis   NO
    ## 7                           Amadeus Cho   NO
    ## 8                        America Chavez   NO
    ## 9                        Angelica Jones   NO
    ## 10                Anthony Ludgate Druid  YES
    ## 11          Anthony Edward "Tony" Stark  YES
    ## 12                 Anthony Edward Stark  YES
    ## 13                         Anya Corazon   NO
    ## 14                                 Ares  YES
    ## 15                      Ashley Crawford   NO
    ## 16                            Ava Ayala   NO
    ## 17           Barbara Barton (nee Morse)  YES
    ## 18                 Benjamin Jacob Grimm  YES
    ## 19                        Bonita Juarez   NO
    ## 20                       Brandon Sharpe   NO
    ## 21                       Brian Braddock   NO
    ## 22                           Brunnhilde   NO
    ## 23                                        NO
    ## 24             Carol Susan Jane Danvers   NO
    ## 25                          Cassie Lang  YES
    ## 26                           Charlie-27   NO
    ## 27                   Christopher Powell   NO
    ## 28               Clinton Francis Barton  YES
    ## 29                         Craig Hollis   NO
    ## 30           Crystal Amaquelin Maximoff   NO
    ## 31                        Daisy Johnson   NO
    ## 32                         Dane Whitman   NO
    ## 33              Daniel Thomas Rand K'ai   NO
    ## 34                        David Alleyne   NO
    ## 35                                       YES
    ## 36                   Delroy Garrett Jr.   NO
    ## 37                         DeMarr Davis  YES
    ## 38                        Dennis Dunphy  YES
    ## 39                         Dennis Sykes  YES
    ## 40                                       YES
    ## 41                                        NO
    ## 42                        Dorreen Green   NO
    ## 43  Dorrek VIII/Theodore "Teddy" Altman   NO
    ## 44                         Doug Taggert  YES
    ## 45                            Eden Fesi   NO
    ## 46                       Elijah Bradley   NO
    ## 47                        Elvin Haliday   NO
    ## 48                         Emery Schaub   NO
    ## 49                          Eric Brooks   NO
    ## 50                 Eric Kevin Masterson  YES
    ## 51                         Eric O'Grady  YES
    ## 52                                 Eros   NO
    ## 53                       Flash Thompson   NO
    ## 54                                       YES
    ## 55                                Fiona   NO
    ## 56                         Gene Lorrene   NO
    ## 57                                       YES
    ## 58                  "Giulietta Nefaria"  YES
    ## 59                   Greer Grant Nelson   NO
    ## 60                          Greg Willis   NO
    ## 61                      Heather Douglas  YES
    ## 62                       Henry P. McCoy   NO
    ## 63            Henry Jonathan "Hank" Pym  YES
    ## 64                             Heracles   NO
    ## 65                               Yvette   NO
    ## 66                  Jim Hammond (alias)  YES
    ## 67                       Humberto Lopez  YES
    ## 68                            Izzy Kane   NO
    ## 69                     Jacques Duquesne  YES
    ## 70                James Buchanan Barnes   NO
    ## 71                James "Logan" Howlett  YES
    ## 72                      James R. Rhodes   NO
    ## 73                        James Santini   NO
    ## 74                       Janet van Dyne  YES
    ## 75                      Jeanne Foucault   NO
    ## 76                      Jennifer Takeda   NO
    ## 77                     Jennifer Walters  YES
    ## 78                  Jessica Miriam Drew   NO
    ## 79                        Jessica Jones   NO
    ## 80                              Jocasta  YES
    ## 81                            John Aman   NO
    ## 82                       John F. Walker   NO
    ## 83                         Johnny Gallo   NO
    ## 84                        Jonathan Hart  YES
    ## 85                      Julia Carpenter   NO
    ## 86                          Julie Power   NO
    ## 87                                Kaluu   NO
    ## 88              Katherine "Kate" Bishop   NO
    ## 89                   Kelsey Leigh Shorr   NO
    ## 90                             Ken Mack  YES
    ## 91                    Kevin Kale Connor  YES
    ## 92                      Kevin Masterson   NO
    ## 93                       Loki Laufeyson   NO
    ## 94                           Carl Lucas   NO
    ## 95                                 Lyra   NO
    ## 96                               Brandt  YES
    ## 97                             Mar-Vell  YES
    ## 98                         Marc Spector   NO
    ## 99                        Marcus Milton  YES
    ## 100         Maria de Guadalupe Santiago   NO
    ## 101                          Maria Hill   NO
    ## 102                   Marrina Smallwood  YES
    ## 103                     Martinex T'Naga   NO
    ## 104      Matthew Liebowitz (birth name)  YES
    ## 105                        Matt Murdock   NO
    ## 106                          Maya Lopez  YES
    ## 107                                       NO
    ## 108                     Michiko Musashi   NO
    ## 109                       Miguel Santos   NO
    ## 110                       Moira Brandon  YES
    ## 111                        Monica Chang   NO
    ## 112                      Monica Rambeau   NO
    ## 113                                      YES
    ## 114                      Namor McKenzie  YES
    ## 115          Natalia Alianovna Romanova  YES
    ## 116                  Nathaniel Richards   NO
    ## 117  Nicholas Fury, Jr., Marcus Johnson   NO
    ## 118                     Nicholette Gold   NO
    ## 119                                Adam  YES
    ## 120                            Noh-Varr   NO
    ## 121                        Ororo Munroe   NO
    ## 122                       Otto Octavius  YES
    ## 123                        Patsy Walker  YES
    ## 124               Peter Benjamin Parker  YES
    ## 125                      Phillip Javert   NO
    ## 126                     Phillip Coulson   NO
    ## 127                     Pietro Maximoff  YES
    ## 128             Ravonna Lexus Renslayer  YES
    ## 129                       Reed Richards   NO
    ## 130              Richard Milhouse Jones   NO
    ## 131                       Richard Rider  YES
    ## 132                         Rita DeMara  YES
    ## 133                      Robbie Baldwin   NO
    ## 134                 Robert Bruce Banner  YES
    ## 135                 Robert L. Frank Sr.  YES
    ## 136                     Robert Reynolds  YES
    ## 137                    Roberto da Costa   NO
    ## 138                          Anna Marie   NO
    ## 139                       Sam Alexander   NO
    ## 140                      Samuel Guthrie   NO
    ## 141                Samuel Thomas Wilson   NO
    ## 142            Scott Edward Harris Lang  YES
    ## 143                               Circe   NO
    ## 144                           Shang-Chi   NO
    ## 145                       Sharon Carter  YES
    ## 146                       Shiro Yoshida  YES
    ## 147                      Simon Williams  YES
    ## 148                              Stakar   NO
    ## 149      Doctor Stephen Vincent Strange   NO
    ## 150                       Steven Rogers  YES
    ## 151          Susan Richards (nee Storm)   NO
    ## 152                            T'Challa   NO
    ## 153                        Taki Matsuya   NO
    ## 154                       Thaddeus Ross  YES
    ## 155             Thomas "Tommy" Shepherd   NO
    ## 156                        Thor Odinson  YES
    ## 157                                       NO
    ## 158                        Tony Masters   NO
    ## 159                         Val Ventura   NO
    ## 160                      Vance Astrovik   NO
    ## 161                      Vance Astrovik   NO
    ## 162                             Veranke  YES
    ## 163                      Victor Alvarez   NO
    ## 164                       Victor Mancha  YES
    ## 165                Victor Shade (alias)  YES
    ## 166                        Alias: Jonas  YES
    ## 167                         Wade Wilson  YES
    ## 168                       Walter Newell   NO
    ## 169                      Wanda Maximoff  YES
    ## 170                Wendell Elvis Vaughn  YES
    ## 171                       William Baker  YES
    ## 172              William "Billy" Kaplan   NO
    ## 173                        Yondu Udonta   NO
    ##                            Name.Alias.y Return
    ## 1                                  X-51    YES
    ## 2                                           NO
    ## 3                         Adam Brashear       
    ## 4                            Alani Ryan       
    ## 5                          Alex Summers       
    ## 6                                Alexis       
    ## 7                           Amadeus Cho       
    ## 8                        America Chavez       
    ## 9                        Angelica Jones       
    ## 10                Anthony Ludgate Druid    YES
    ## 11          Anthony Edward "Tony" Stark    YES
    ## 12                 Anthony Edward Stark     NO
    ## 13                         Anya Corazon       
    ## 14                                 Ares    YES
    ## 15                      Ashley Crawford       
    ## 16                            Ava Ayala       
    ## 17           Barbara Barton (nee Morse)    YES
    ## 18                 Benjamin Jacob Grimm    YES
    ## 19                        Bonita Juarez       
    ## 20                       Brandon Sharpe       
    ## 21                       Brian Braddock       
    ## 22                           Brunnhilde       
    ## 23                                            
    ## 24             Carol Susan Jane Danvers       
    ## 25                          Cassie Lang    YES
    ## 26                           Charlie-27       
    ## 27                   Christopher Powell       
    ## 28               Clinton Francis Barton    YES
    ## 29                         Craig Hollis       
    ## 30           Crystal Amaquelin Maximoff       
    ## 31                        Daisy Johnson       
    ## 32                         Dane Whitman       
    ## 33              Daniel Thomas Rand K'ai       
    ## 34                        David Alleyne       
    ## 35                                         YES
    ## 36                   Delroy Garrett Jr.       
    ## 37                         DeMarr Davis    YES
    ## 38                        Dennis Dunphy    YES
    ## 39                         Dennis Sykes     NO
    ## 40                                          NO
    ## 41                                            
    ## 42                        Dorreen Green       
    ## 43  Dorrek VIII/Theodore "Teddy" Altman       
    ## 44                         Doug Taggert     NO
    ## 45                            Eden Fesi       
    ## 46                       Elijah Bradley       
    ## 47                        Elvin Haliday       
    ## 48                         Emery Schaub       
    ## 49                          Eric Brooks       
    ## 50                 Eric Kevin Masterson    YES
    ## 51                         Eric O'Grady     NO
    ## 52                                 Eros       
    ## 53                       Flash Thompson       
    ## 54                                          NO
    ## 55                                Fiona       
    ## 56                         Gene Lorrene       
    ## 57                                         YES
    ## 58                  "Giulietta Nefaria"     NO
    ## 59                   Greer Grant Nelson       
    ## 60                          Greg Willis       
    ## 61                      Heather Douglas    YES
    ## 62                       Henry P. McCoy       
    ## 63            Henry Jonathan "Hank" Pym     NO
    ## 64                             Heracles       
    ## 65                               Yvette       
    ## 66                  Jim Hammond (alias)    YES
    ## 67                       Humberto Lopez     NO
    ## 68                            Izzy Kane       
    ## 69                     Jacques Duquesne    YES
    ## 70                James Buchanan Barnes       
    ## 71                James "Logan" Howlett     NO
    ## 72                      James R. Rhodes       
    ## 73                        James Santini       
    ## 74                       Janet van Dyne    YES
    ## 75                      Jeanne Foucault       
    ## 76                      Jennifer Takeda       
    ## 77                     Jennifer Walters    YES
    ## 78                  Jessica Miriam Drew       
    ## 79                        Jessica Jones       
    ## 80                              Jocasta    YES
    ## 81                            John Aman       
    ## 82                       John F. Walker       
    ## 83                         Johnny Gallo       
    ## 84                        Jonathan Hart    YES
    ## 85                      Julia Carpenter       
    ## 86                          Julie Power       
    ## 87                                Kaluu       
    ## 88              Katherine "Kate" Bishop       
    ## 89                   Kelsey Leigh Shorr       
    ## 90                             Ken Mack     NO
    ## 91                    Kevin Kale Connor     NO
    ## 92                      Kevin Masterson       
    ## 93                       Loki Laufeyson       
    ## 94                           Carl Lucas       
    ## 95                                 Lyra       
    ## 96                               Brandt    YES
    ## 97                             Mar-Vell    YES
    ## 98                         Marc Spector       
    ## 99                        Marcus Milton     NO
    ## 100         Maria de Guadalupe Santiago       
    ## 101                          Maria Hill       
    ## 102                   Marrina Smallwood    YES
    ## 103                     Martinex T'Naga       
    ## 104      Matthew Liebowitz (birth name)    YES
    ## 105                        Matt Murdock       
    ## 106                          Maya Lopez    YES
    ## 107                                           
    ## 108                     Michiko Musashi       
    ## 109                       Miguel Santos       
    ## 110                       Moira Brandon     NO
    ## 111                        Monica Chang       
    ## 112                      Monica Rambeau       
    ## 113                                         NO
    ## 114                      Namor McKenzie    YES
    ## 115          Natalia Alianovna Romanova    YES
    ## 116                  Nathaniel Richards       
    ## 117  Nicholas Fury, Jr., Marcus Johnson       
    ## 118                     Nicholette Gold       
    ## 119                                Adam     NO
    ## 120                            Noh-Varr       
    ## 121                        Ororo Munroe       
    ## 122                       Otto Octavius     NO
    ## 123                        Patsy Walker    YES
    ## 124               Peter Benjamin Parker    YES
    ## 125                      Phillip Javert       
    ## 126                     Phillip Coulson       
    ## 127                     Pietro Maximoff    YES
    ## 128             Ravonna Lexus Renslayer    YES
    ## 129                       Reed Richards       
    ## 130              Richard Milhouse Jones       
    ## 131                       Richard Rider     NO
    ## 132                         Rita DeMara    YES
    ## 133                      Robbie Baldwin       
    ## 134                 Robert Bruce Banner    YES
    ## 135                 Robert L. Frank Sr.     NO
    ## 136                     Robert Reynolds    YES
    ## 137                    Roberto da Costa       
    ## 138                          Anna Marie       
    ## 139                       Sam Alexander       
    ## 140                      Samuel Guthrie       
    ## 141                Samuel Thomas Wilson       
    ## 142            Scott Edward Harris Lang    YES
    ## 143                               Circe       
    ## 144                           Shang-Chi       
    ## 145                       Sharon Carter    YES
    ## 146                       Shiro Yoshida    YES
    ## 147                      Simon Williams    YES
    ## 148                              Stakar       
    ## 149      Doctor Stephen Vincent Strange       
    ## 150                       Steven Rogers    YES
    ## 151          Susan Richards (nee Storm)       
    ## 152                            T'Challa       
    ## 153                        Taki Matsuya       
    ## 154                       Thaddeus Ross    YES
    ## 155             Thomas "Tommy" Shepherd       
    ## 156                        Thor Odinson    YES
    ## 157                                           
    ## 158                        Tony Masters       
    ## 159                         Val Ventura       
    ## 160                      Vance Astrovik       
    ## 161                      Vance Astrovik       
    ## 162                             Veranke     NO
    ## 163                      Victor Alvarez       
    ## 164                       Victor Mancha    YES
    ## 165                Victor Shade (alias)    YES
    ## 166                        Alias: Jonas     NO
    ## 167                         Wade Wilson     NO
    ## 168                       Walter Newell       
    ## 169                      Wanda Maximoff    YES
    ## 170                Wendell Elvis Vaughn    YES
    ## 171                       William Baker    YES
    ## 172              William "Billy" Kaplan       
    ## 173                        Yondu Udonta

``` r
yReturn <- filter(deathsNReturns, Return == "YES", na.rm = TRUE)
nrow(yReturn)
```

    ## [1] 57

``` r
nReturn <- filter(deathsNReturns, Return == "NO", na.rm = TRUE)
nrow(nReturn)
```

    ## [1] 32

Observations: There is no Return value on a column if the character did
not die that time. I can simply count the number of return=YES on time =
1 columns and place that over the number of return = YES + return = NO
rows for time = 1

The yReturn data provides the number of times an Avenger returned from
their first death

The nReturn provides the number of times an Avenger did Not return from
their first death

This set of data shows that these Avengers returned from their first
death 57 times and did not return from their first death 32 times. This
provides a return rate of 57/89, which is just below the 2-3 chance
described in the article but not by enough to dispute it as false.
