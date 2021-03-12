Analyzing Text from New York Times Article Abstracts
================
Clara Fong
2021-03-11

  - [Summary of Report](#summary-of-report)
  - [Accessing API and Creating Data
    Frame](#accessing-api-and-creating-data-frame)
      - [Data Source:](#data-source)
      - [Tidying the Text](#tidying-the-text)
  - [Data Visualization](#data-visualization)
      - [Distribution of Migration-Related News by Continental
        Region](#distribution-of-migration-related-news-by-continental-region)
      - [Frequncy of Migrant Stories Over
        Time](#frequncy-of-migrant-stories-over-time)
      - [Article Word Count Over Time](#article-word-count-over-time)
      - [Europe Stories and Word Count](#europe-stories-and-word-count)
      - [Estimating a Linear Model](#estimating-a-linear-model)
  - [Session info](#session-info)

## Summary of Report

Extending on last week’s homework assignment, I wanted to continue
looking at New York Times articles related to international migrants,
broadly speaking. For homework 8, I had spent a majority of my efforts
figuring out how to create a GET request and work with the New York
Times API, so most of my analysis was based on variables of the articles
themselves that I could pull out (e.g., year of published article and
the word count, the frequency of regions discussed in the articles, if
publication date can predict word count, etc.). This time, I wanted to
focus on the content of the actual article and examine how journalists
are talking about migration in these articles across the world.

## Accessing API and Creating Data Frame

### Data Source:

I used the [New York Times Developers
API](https://developer.nytimes.com/apis) to build this data frame. After
creating an account and reading the documentation on how to submit a
query, I was able to filter for all stories in the past ten years
(2010-2020) relating to migrants. Note: this slightly differs from
homework 8 because I only looked at the past five years for that
asssignment. I wanted to specifically look at the Foreign “newsdesk”,
which is another way to filter for stories because I was curious to see
what was being addressed internationally. Details of how I created the
data frame and code can be found on my [R markdown file](nyt.Rmd). To
convert the `.json` file to a data frame, I leaned heavily on Professor
Terman’s PLSC 31101 [course
website](https://plsc-31101.github.io/course/collecting-data-from-the-web.html#writing-api-queries).

### Tidying the Text

(notes)

    ## # A tibble: 3,993 x 2
    ##    word          n
    ##    <chr>     <int>
    ##  1 migrants    162
    ##  2 european     66
    ##  3 people       60
    ##  4 europe       54
    ##  5 refugees     53
    ##  6 mexico       52
    ##  7 border       51
    ##  8 united       51
    ##  9 migrant      50
    ## 10 thousands    49
    ## # … with 3,983 more rows

    ## # A tibble: 9,793 x 3
    ##    pub_date             line word        
    ##    <dttm>              <int> <chr>       
    ##  1 2020-11-06 14:57:42     1 paris       
    ##  2 2020-11-06 14:57:42     1 unknown     
    ##  3 2020-11-06 14:57:42     1 police      
    ##  4 2020-11-06 14:57:42     1 intelligence
    ##  5 2020-11-06 14:57:42     1 officials   
    ##  6 2020-11-06 14:57:42     1 pledged     
    ##  7 2020-11-06 14:57:42     1 allegiance  
    ##  8 2020-11-06 14:57:42     1 terrorist   
    ##  9 2020-11-06 14:57:42     1 claimed     
    ## 10 2020-11-06 14:57:42     1 stated      
    ## # … with 9,783 more rows

## Data Visualization

### Distribution of Migration-Related News by Continental Region

As a first step at looking at the NYT articles data, I wanted to see
what the distribution of article frequency looked like by each
continental region.

![](nyt_text_analysis_files/figure-gfm/analysis1-1.png)<!-- -->

This simple data visualization shows us the frequency of events relating
to migrants and migration reported by the New York Times int he past
five years. Unsurprisingly, the highest count is in Europe, likely
resulting to the migrant crisis during the Arab Spring movement, but it
is interesting to note that the Americas and Asia Pacific have
relatively similar frequency, so it might be worth looking deeper into
what are the kinds of stories being talked about.

### Frequncy of Migrant Stories Over Time

I also think it’s worth exploring how the frequency and length of these
stories changed over the past five years, so we can also create another
bar plot to look at the frequency of articles relating to migration over
the past five years. Furthermore, we can segment this by continent and
observe which regions of the world are seeing the most relevance in
migration stories.

![](nyt_text_analysis_files/figure-gfm/analysis%202-1.png)<!-- -->![](nyt_text_analysis_files/figure-gfm/analysis%202-2.png)<!-- -->

Again, perhaps unexpectedly, Europe has dominated the stories on
migration for the past five years and more or less accounts for the
decrease in migrant stories since 2015. Australia, the Middle East, and
Asia Pacific all see a small uptick in articles during this 2015-2016
period as well. Other than this trend, there’s a comparably smaller
spike in migration articles in Americas in 2018.

### Article Word Count Over Time

Just to add in another continuous variable, it might be interesting to
see if word count of these stories have increased or decreased over
time. My initial hunch is that as the crisis is just unraveling,
journalists are trying to public short bytes of news to capture current
events, and as the crisis becomes less novel (albeit still as pressing),
there may be longer pieces that emerge from the NYT later in the years.

![](nyt_text_analysis_files/figure-gfm/analysis%203-1.png)<!-- -->

This is a pretty flat linear relationship, so we might want to see if we
can parse these data points out by each individual continent and observe
that relationship.

![](nyt_text_analysis_files/figure-gfm/analysis%204-1.png)<!-- -->

Generally, there’s a somewhat positive linear relationship between all
the continents except the Middle East and Asia Pacific. However, it’s
also worth noting the sheer number of data points in Europe, so below
I’ve attempted to look at Europe as a specific case.

### Europe Stories and Word Count

I can also see that there are way more data points from 2015-2016, so we
can further narrow in the data to look at just Europe and see if my
previous hunch might be true.

![](nyt_text_analysis_files/figure-gfm/analysis%203%20pt%202-1.png)<!-- -->

We can see that there isn’t a really solid relationship here between
publication date the word count of articles in 2015-2016 for Europe.
There is a generally larger frequency of stories form July 2015 to
January 2015, however, which makes sense given the time line of the
world events.

### Estimating a Linear Model

Finally, given that we could see some kind of loose linear relationship
between word count and publication year, I thought it might be worth
looking into a basic linear relationship between the two.

| Term        |      Estimate | Standard Error |  Statistic |   P-Value |
| :---------- | ------------: | -------------: | ---------: | --------: |
| (Intercept) | \-956.2478830 |    452.0553220 | \-2.115334 | 0.0349361 |
| pub\_date   |     0.0000013 |      0.0000003 |   4.118917 | 0.0000451 |

So, a basic model shows us that if we regress `pub_date` on
`word_count`, we get a pretty flat, almost zero, estimate, and a P-value
of basically zero.

| Output | Estimator | Estimate |
| :----- | :-------- | -------: |
| rmse   | standard  | 508.9478 |

As for using this model to predict other points on the data set, we can
below see that our RSME is 508.9477766.

## Session info

``` r
devtools::session_info()
```

    ## ─ Session info ───────────────────────────────────────────────────────────────
    ##  setting  value                               
    ##  version  R version 4.0.1 (2020-06-06)        
    ##  os       Red Hat Enterprise Linux 8.3 (Ootpa)
    ##  system   x86_64, linux-gnu                   
    ##  ui       X11                                 
    ##  language (EN)                                
    ##  collate  en_US.UTF-8                         
    ##  ctype    en_US.UTF-8                         
    ##  tz       America/Chicago                     
    ##  date     2021-03-11                          
    ## 
    ## ─ Packages ───────────────────────────────────────────────────────────────────
    ##  package      * version    date       lib source        
    ##  assertthat     0.2.1      2019-03-21 [2] CRAN (R 4.0.1)
    ##  backports      1.2.1      2020-12-09 [2] CRAN (R 4.0.1)
    ##  broom        * 0.7.3      2020-12-16 [2] CRAN (R 4.0.1)
    ##  callr          3.5.1      2020-10-13 [2] CRAN (R 4.0.1)
    ##  cellranger     1.1.0      2016-07-27 [2] CRAN (R 4.0.1)
    ##  class          7.3-17     2020-04-26 [2] CRAN (R 4.0.1)
    ##  cli            2.2.0      2020-11-20 [2] CRAN (R 4.0.1)
    ##  codetools      0.2-16     2018-12-24 [2] CRAN (R 4.0.1)
    ##  colorspace     2.0-0      2020-11-11 [2] CRAN (R 4.0.1)
    ##  crayon         1.3.4      2017-09-16 [2] CRAN (R 4.0.1)
    ##  curl           4.3        2019-12-02 [2] CRAN (R 4.0.1)
    ##  DBI            1.1.0      2019-12-15 [2] CRAN (R 4.0.1)
    ##  dbplyr         2.0.0      2020-11-03 [2] CRAN (R 4.0.1)
    ##  desc           1.2.0      2018-05-01 [2] CRAN (R 4.0.1)
    ##  devtools       2.3.2      2020-09-18 [2] CRAN (R 4.0.1)
    ##  dials        * 0.0.9      2020-09-16 [2] CRAN (R 4.0.1)
    ##  DiceDesign     1.8-1      2019-07-31 [2] CRAN (R 4.0.1)
    ##  digest         0.6.27     2020-10-24 [2] CRAN (R 4.0.1)
    ##  dplyr        * 1.0.2      2020-08-18 [2] CRAN (R 4.0.1)
    ##  ellipsis       0.3.1      2020-05-15 [2] CRAN (R 4.0.1)
    ##  evaluate       0.14       2019-05-28 [2] CRAN (R 4.0.1)
    ##  fansi          0.4.1      2020-01-08 [2] CRAN (R 4.0.1)
    ##  farver         2.0.3      2020-01-16 [2] CRAN (R 4.0.1)
    ##  forcats      * 0.5.0      2020-03-01 [2] CRAN (R 4.0.1)
    ##  foreach        1.5.1      2020-10-15 [2] CRAN (R 4.0.1)
    ##  fs             1.5.0      2020-07-31 [2] CRAN (R 4.0.1)
    ##  furrr          0.2.1      2020-10-21 [2] CRAN (R 4.0.1)
    ##  future         1.21.0     2020-12-10 [2] CRAN (R 4.0.1)
    ##  generics       0.1.0      2020-10-31 [2] CRAN (R 4.0.1)
    ##  ggplot2      * 3.3.3      2020-12-30 [2] CRAN (R 4.0.1)
    ##  globals        0.14.0     2020-11-22 [2] CRAN (R 4.0.1)
    ##  glue           1.4.2      2020-08-27 [2] CRAN (R 4.0.1)
    ##  gower          0.2.2      2020-06-23 [2] CRAN (R 4.0.1)
    ##  GPfit          1.0-8      2019-02-08 [2] CRAN (R 4.0.1)
    ##  gtable         0.3.0      2019-03-25 [2] CRAN (R 4.0.1)
    ##  haven          2.3.1      2020-06-01 [2] CRAN (R 4.0.1)
    ##  highr          0.8        2019-03-20 [2] CRAN (R 4.0.1)
    ##  hms            0.5.3      2020-01-08 [2] CRAN (R 4.0.1)
    ##  htmltools      0.4.0      2019-10-04 [2] CRAN (R 4.0.1)
    ##  httr         * 1.4.2      2020-07-20 [2] CRAN (R 4.0.1)
    ##  infer        * 0.5.3      2020-07-14 [2] CRAN (R 4.0.1)
    ##  ipred          0.9-9      2019-04-28 [2] CRAN (R 4.0.1)
    ##  iterators      1.0.13     2020-10-15 [2] CRAN (R 4.0.1)
    ##  janeaustenr    0.1.5      2017-06-10 [2] CRAN (R 4.0.1)
    ##  jsonlite     * 1.7.2      2020-12-09 [2] CRAN (R 4.0.1)
    ##  knitr          1.30       2020-09-22 [2] CRAN (R 4.0.1)
    ##  labeling       0.4.2      2020-10-20 [2] CRAN (R 4.0.1)
    ##  lattice        0.20-41    2020-04-02 [2] CRAN (R 4.0.1)
    ##  lava           1.6.8.1    2020-11-04 [2] CRAN (R 4.0.1)
    ##  lhs            1.1.1      2020-10-05 [2] CRAN (R 4.0.1)
    ##  lifecycle      0.2.0      2020-03-06 [2] CRAN (R 4.0.1)
    ##  listenv        0.8.0      2019-12-05 [2] CRAN (R 4.0.1)
    ##  lubridate    * 1.7.9.2    2020-11-13 [2] CRAN (R 4.0.1)
    ##  magrittr       2.0.1      2020-11-17 [2] CRAN (R 4.0.1)
    ##  MASS           7.3-51.6   2020-04-26 [2] CRAN (R 4.0.1)
    ##  Matrix         1.2-18     2019-11-27 [2] CRAN (R 4.0.1)
    ##  memoise        1.1.0      2017-04-21 [2] CRAN (R 4.0.1)
    ##  mgcv           1.8-31     2019-11-09 [2] CRAN (R 4.0.1)
    ##  modeldata    * 0.1.0      2020-10-22 [2] CRAN (R 4.0.1)
    ##  modelr         0.1.8      2020-05-19 [2] CRAN (R 4.0.1)
    ##  munsell        0.5.0      2018-06-12 [2] CRAN (R 4.0.1)
    ##  nlme           3.1-148    2020-05-24 [2] CRAN (R 4.0.1)
    ##  nnet           7.3-14     2020-04-26 [2] CRAN (R 4.0.1)
    ##  parallelly     1.22.0     2020-12-13 [2] CRAN (R 4.0.1)
    ##  parsnip      * 0.1.4      2020-10-27 [2] CRAN (R 4.0.1)
    ##  pillar         1.4.7      2020-11-20 [2] CRAN (R 4.0.1)
    ##  pkgbuild       1.2.0      2020-12-15 [2] CRAN (R 4.0.1)
    ##  pkgconfig      2.0.3      2019-09-22 [2] CRAN (R 4.0.1)
    ##  pkgload        1.1.0      2020-05-29 [2] CRAN (R 4.0.1)
    ##  plyr           1.8.6      2020-03-03 [2] CRAN (R 4.0.1)
    ##  prettyunits    1.1.1      2020-01-24 [2] CRAN (R 4.0.1)
    ##  pROC           1.16.2     2020-03-19 [2] CRAN (R 4.0.1)
    ##  processx       3.4.5      2020-11-30 [2] CRAN (R 4.0.1)
    ##  prodlim        2019.11.13 2019-11-17 [2] CRAN (R 4.0.1)
    ##  ps             1.5.0      2020-12-05 [2] CRAN (R 4.0.1)
    ##  purrr        * 0.3.4      2020-04-17 [2] CRAN (R 4.0.1)
    ##  R6             2.5.0      2020-10-28 [2] CRAN (R 4.0.1)
    ##  RColorBrewer   1.1-2      2014-12-07 [2] CRAN (R 4.0.1)
    ##  Rcpp           1.0.5      2020-07-06 [2] CRAN (R 4.0.1)
    ##  readr        * 1.4.0      2020-10-05 [2] CRAN (R 4.0.1)
    ##  readxl         1.3.1      2019-03-13 [2] CRAN (R 4.0.1)
    ##  recipes      * 0.1.15     2020-11-11 [2] CRAN (R 4.0.1)
    ##  remotes        2.2.0      2020-07-21 [2] CRAN (R 4.0.1)
    ##  reprex         0.3.0      2019-05-16 [2] CRAN (R 4.0.1)
    ##  rlang          0.4.10     2020-12-30 [2] CRAN (R 4.0.1)
    ##  rmarkdown      2.6        2020-12-14 [2] CRAN (R 4.0.1)
    ##  rpart          4.1-15     2019-04-12 [2] CRAN (R 4.0.1)
    ##  rprojroot      2.0.2      2020-11-15 [2] CRAN (R 4.0.1)
    ##  rsample      * 0.0.8      2020-09-23 [2] CRAN (R 4.0.1)
    ##  rstudioapi     0.13       2020-11-12 [2] CRAN (R 4.0.1)
    ##  rvest          0.3.6      2020-07-25 [2] CRAN (R 4.0.1)
    ##  scales       * 1.1.1      2020-05-11 [2] CRAN (R 4.0.1)
    ##  sessioninfo    1.1.1      2018-11-05 [2] CRAN (R 4.0.1)
    ##  SnowballC      0.7.0      2020-04-01 [2] CRAN (R 4.0.1)
    ##  stringi        1.5.3      2020-09-09 [2] CRAN (R 4.0.1)
    ##  stringr      * 1.4.0      2019-02-10 [2] CRAN (R 4.0.1)
    ##  survival       3.1-12     2020-04-10 [2] CRAN (R 4.0.1)
    ##  testthat       3.0.1      2020-12-17 [2] CRAN (R 4.0.1)
    ##  tibble       * 3.0.4      2020-10-12 [2] CRAN (R 4.0.1)
    ##  tidymodels   * 0.1.2      2020-11-22 [2] CRAN (R 4.0.1)
    ##  tidyr        * 1.1.2      2020-08-27 [2] CRAN (R 4.0.1)
    ##  tidyselect     1.1.0      2020-05-11 [2] CRAN (R 4.0.1)
    ##  tidytext     * 0.2.6      2020-09-20 [2] CRAN (R 4.0.1)
    ##  tidyverse    * 1.3.0      2019-11-21 [2] CRAN (R 4.0.1)
    ##  timeDate       3043.102   2018-02-21 [2] CRAN (R 4.0.1)
    ##  tokenizers     0.2.1      2018-03-29 [2] CRAN (R 4.0.1)
    ##  tune         * 0.1.2      2020-11-17 [2] CRAN (R 4.0.1)
    ##  usethis        2.0.0      2020-12-10 [2] CRAN (R 4.0.1)
    ##  utf8           1.1.4      2018-05-24 [2] CRAN (R 4.0.1)
    ##  vctrs          0.3.6      2020-12-17 [2] CRAN (R 4.0.1)
    ##  withr          2.3.0      2020-09-22 [2] CRAN (R 4.0.1)
    ##  workflows    * 0.2.1      2020-10-08 [2] CRAN (R 4.0.1)
    ##  xfun           0.19       2020-10-30 [2] CRAN (R 4.0.1)
    ##  xml2           1.3.2      2020-04-23 [2] CRAN (R 4.0.1)
    ##  yaml           2.2.1      2020-02-01 [2] CRAN (R 4.0.1)
    ##  yardstick    * 0.0.7      2020-07-13 [2] CRAN (R 4.0.1)
    ## 
    ## [1] /home/cmfong/R/x86_64-pc-linux-gnu-library/4.0
    ## [2] /opt/R/4.0.1/lib/R/library

\`\`\`
