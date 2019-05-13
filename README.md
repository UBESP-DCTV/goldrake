
<!-- README.md is generated from README.Rmd. Please edit that file -->

[![AppVeyor build
status](https://ci.appveyor.com/api/projects/status/github/CorradoLanera/goldrake?branch=master&svg=true)](https://ci.appveyor.com/project/CorradoLanera/goldrake)
[![Travis build
status](https://travis-ci.org/CorradoLanera/goldrake.svg?branch=master)](https://travis-ci.org/CorradoLanera/goldrake)
[![Coverage
status](https://codecov.io/gh/CorradoLanera/goldrake/branch/master/graph/badge.svg)](https://codecov.io/github/CorradoLanera/goldrake?branch=master)

[![lifecycle](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing)
[![CRAN
status](https://www.r-pkg.org/badges/version/goldrake)](https://cran.r-project.org/package=goldrake)

# goldrake

The goal of goldrake is to provide an environment to create
gold-standard databases for a classification task.

## Installation

You can install the development version from
[GitHub](https://github.com/) with the following procedure:

``` r
# install.packages("devtools")
devtools::install_github("CorradoLanera/goldrake")
```

## Usage

The intended is divided in multiple step.

### Setup

``` r
library(goldrake)
mtcars_gr <- goldrake(mtcars) %>% 
   set_gold_classes(c("good", "bad", "so and so")) %>% 
   balance_groups_by(vs, am) %>% 
   # max_sample_each_group(5) %>% # DOES IT WORTH TO BE IMPLEMENTED??
   add_reviewer("Corrado", "Lanera")
#> <U+2714> Classes have been added.
#> <U+2714> New classes: 'good', 'bad', 'so and so'.
#> <U+2714> Balancing variables setted
#> <U+2714> New balancing variables: 'vs', 'am'
#> <U+2714> Reviewer 'Corrado, Lanera' added.

mtcars_gr
#> goldrake classification object
#> 
#> <U+25CF> 32 cases and 11 variables (balanced by vs, am)
#> <U+25CF> Classes: good, bad, so and so.
#> 
#> <U+2714> Data classified: 32 (by everyone) -- 32 (by someone)
#> <U+2716> Data left to classify: 0 (by someone) -- 0 (by everyone)
#> 
#> <U+25CF> Reviewers: Corrado, Lanera.
```

You can add reviewer in any moment (even if the previous ones have
already started to classify objects).

    mtcars_gr %>% 
       add_reviewer("Daniele", "Bottigliengo")      # it asks for a password

### Classify

    ## if it is not the first time
    # mtcars_gr <- read_goldrake("mtcars_gr.goldrake")
    
    # start the interactive classification session 
    mtcars_gr %>% 
        classify_by("Corrado")             # it asks for reviewer's password

At the end of the session, if the stored goldrake was updated by other
reviewer(s), the information will be merged toghether.

### Collect results

    # load_goldrake("mtcars_gr.goldrake") # if it is not already loaded
    
    classified_mtcars <- mtcars_gr %>% 
        extract_classified_tbl()
    
    is.data.frame(classified_mtcars)

## CODE OF CONDUCT

Please note that the `goldrake` project is released with a [Contributor
Code of Conduct](.github/CODE_OF_CONDUCT.md). By contributing to this
project, you agree to abide by its terms.
