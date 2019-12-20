
<!-- README.md is generated from README.Rmd. Please edit that file -->

# tidyweb

This package aims at easing the conversion of data coded according to
the German Policy Agenda [codebook]() into data coded according to the
master [codebook]() from the Comparative Agenda Project.

## Install

``` r
# remotes::install_github("benjaminguinaudeau/GermanyCAP")

library(magrittr) # import pipe %>%
library(GermanyCAP)
```

## How to use?

``` r

# Generate fake data
data <- tibble::tibble(
  doc = letters[1:10], # 10 documents to code
  minor = c(1910, 1308, 1223, 1608, 1804, 105, 104, 2504, 202, 601) # 10 german subtopic associated with each document
) %>%
  dplyr::glimpse()
#> Observations: 10
#> Variables: 2
#> $ doc   <chr> "a", "b", "c", "d", "e", "f", "g", "h", "i", "j"
#> $ minor <dbl> 1910, 1308, 1223, 1608, 1804, 105, 104, 2504, 202, 601
```

Please note that the conversion is mono-directional. Topics from the
master codebook cannot be converted into German topics. Indeed, while
converting, several German subtopics are merged into one single master
kategorie, so that it is not possible to traceback the German category
given the international category. For instance, the topics on the
reunification policy (25XX in the German topic) are coded in 2099
according to the master codebook. Any 25xx topic should therefore get
the international code 2099, but there is no way to systematically know
which german code should be matched with the international code 2099
(2099, 2501, … or 2504).

The function **recode\_into\_master** will take minor topic from the
German codebook and return the corresponding minor topic in the master
codebook.

``` r
data %>%
  dplyr::mutate(minor_master = recode_into_master(minor)) %>%
  dplyr::glimpse()
#> Observations: 10
#> Variables: 3
#> $ doc          <chr> "a", "b", "c", "d", "e", "f", "g", "h", "i", "j"
#> $ minor        <dbl> 1910, 1308, 1223, 1608, 1804, 105, 104, 2504, 202, …
#> $ minor_master <dbl> 1910, 1308, 1523, 1608, 1804, 105, 104, 2099, 202, …
```

The package also includes a function able to extract major topic given
minor topics. The funtion can be used for any codebook.

``` r
data %>%
  dplyr::mutate(major = extract_major(minor)) %>%
  dplyr::glimpse()
#> Observations: 10
#> Variables: 3
#> $ doc   <chr> "a", "b", "c", "d", "e", "f", "g", "h", "i", "j"
#> $ minor <dbl> 1910, 1308, 1223, 1608, 1804, 105, 104, 2504, 202, 601
#> $ major <dbl> 19, 13, 12, 16, 18, 1, 1, 25, 2, 6
```
