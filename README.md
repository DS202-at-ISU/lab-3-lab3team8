
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

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

## Data Tidying

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
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyr)
library(readr)

deaths <- av %>%
  rowwise() %>%  
  mutate(
    Time = sum(c_across(starts_with("Death")) == "YES", na.rm = TRUE),
    #if they died more than they returned, then the Death column is "YES"
    Returned = if_else( Time > sum(c_across(starts_with("Return")) == "YES", na.rm = TRUE), "NO", "YES"),
    Death = if_else(Returned == "YES", "NO", "YES"))%>%
    #end Death calc  
  ungroup() %>%
  select(-Death1, -Death2, -Death3, -Death4, -Death5, -Return1, -Return2, -Return3, -Return4, -Return5)

deaths
```

    ## # A tibble: 173 × 14
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  3 http://marvel.wik… "Anthony …        3068 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Robert B…        2089 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Richard …         612 YES      MALE   ""                 
    ##  7 http://marvel.wik… "Steven R…        3458 YES      MALE   ""                 
    ##  8 http://marvel.wik… "Clinton …        1456 YES      MALE   ""                 
    ##  9 http://marvel.wik… "Pietro M…         769 YES      MALE   ""                 
    ## 10 http://marvel.wik… "Wanda Ma…        1214 YES      FEMALE ""                 
    ## # ℹ 163 more rows
    ## # ℹ 8 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Notes <chr>, Time <int>,
    ## #   Returned <chr>, Death <chr>

Average deaths suffered for an Avenger:

``` r
mean(deaths$Time)
```

    ## [1] 0.5144509

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

## Team Member Hudson Nebbe

### FiveThirtyEight Statement

> Out of 173 listed Avengers, my analysis found that 69 had died at
> least one time after they joined the team.5 That’s about 40 percent of
> all people who have ever signed on to the team.

### Include the code

``` r
hnn <- deaths %>% filter(Time > 0) %>% count()
#number of Time rows greater than 0
hnn
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1    69

``` r
#number of Time rows greater than 0 divided by number of avengers
hnn/173
```

    ##           n
    ## 1 0.3988439

### Include your answer

Using the dplyr filter and count methods, I was able to determine that
the number 69 does make sense according to the article. After that I
checked to make sure the percentage that the author gave made sense.
This statement seems factually backed!

## Team Member Huy Nguyen

### FiveThirtyEight Statement

> There’s a 2-in-3 chance that a member of the Avengers returned from
> their first stint in the afterlife, but only a 50 percent chance they
> recovered from a second or third death.

### Include the code

``` r
# Calculate probability of return after first death using dplyr functionality (if there are more than 1 death, consider that figure returns after first death)
return_prob <- deaths %>% filter(Time > 0) %>% 
  group_by(Time) %>%
  select(starts_with("Return")) %>%
  summarise(
    total_deaths = n(),
    total_returns = sum(. == "YES", na.rm = TRUE),
    prob_return = total_returns / total_deaths
  )
```

    ## Adding missing grouping variables: `Time`

``` r
# Display the results
return_prob[return_prob$Time == 1, ]
```

    ## # A tibble: 1 × 4
    ##    Time total_deaths total_returns prob_return
    ##   <int>        <int>         <int>       <dbl>
    ## 1     1           53            37       0.698

``` r
# Calculate probability of return after second death using dplyr functionality
return_prob_2 <- deaths %>% filter(Time == 3 | Time == 2) %>% 
  select(starts_with("Return")) %>%
  summarise(
    total_deaths = n(),
    total_returns = sum(. == "YES", na.rm = TRUE),
    prob_return = total_returns / total_deaths
  )

# Display the results
return_prob_2
```

    ## # A tibble: 1 × 3
    ##   total_deaths total_returns prob_return
    ##          <int>         <int>       <dbl>
    ## 1           15             6         0.4

### Include your answer

While there’s 69% that a member of the Avengers returned from their
first stint in the afterlife, comparing to the 2-in-3 (~ 67%) value of
the statement, the chance they recovered from a second or third death is
at only 40%, slightly lower than 50%.

## Team Member Jack Olsan

### FiveThirtyEight Statement

> I counted 89 total deaths — some unlucky Avengers are basically Meat
> Loaf with an E-ZPass — and on 57 occasions the individual made a
> comeback.

### Include the code

``` r
# Count the total number of "YES" values in the Return columns
total_comebacks <- av %>%
  select(starts_with("Return")) %>%  # Select only the Return columns
  summarise(total_returns = sum(. == "YES", na.rm = TRUE))  # Count all "YES" values across these columns

total_comebacks$total_returns
```

    ## [1] 57

### Include your answer

I was checking the claim that of the 89 total deaths, 57 made a
comeback. It would seem based on my result after checking the amount of
YES values in the return column, that the claim is true.

## Team Member Madhu Avula

### FiveThirtyEight Statement

> “The MVP of the Earth-616 Marvel Universe Avengers has to be Jocasta —
> an android based on Janet van Dyne and built by Ultron — who has been
> destroyed five times and then recovered five times.”

### Include the code

``` r
jocasta_data <- av %>%
  filter(grepl("Jocasta", Name.Alias))  # Adjust if necessary for exact match

# Count Jocasta’s deaths
deaths_count <- jocasta_data %>%
  select(starts_with("Death")) %>%
  summarise(total_deaths = sum(. == "YES", na.rm = TRUE))

# Count Jocasta’s returns
returns_count <- jocasta_data %>%
  select(starts_with("Return")) %>%
  summarise(total_returns = sum(. == "YES", na.rm = TRUE))

deaths_count$total_deaths
```

    ## [1] 5

``` r
returns_count$total_returns
```

    ## [1] 5

### Include your answer

Based on the analysis, Jocasta has indeed been recorded as having 5
total deaths and 5 returns in the Avengers data set. This confirms the
FiveThirtyEight statement about Jocasta being destroyed five times and
recovered five times. The statement is accurate based on the data set
provided.

Upload your changes to the repository. Discuss and refine answers as a
team.
