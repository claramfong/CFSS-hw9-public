Analyzing Text from New York Times Articles
================
Clara Fong
2021-03-13

  - [Summary of Report](#summary-of-report)
  - [Accessing API and Creating Data
    Frame](#accessing-api-and-creating-data-frame)
      - [Data Source](#data-source)
      - [Tidying the Text](#tidying-the-text)
  - [Text Analysis](#text-analysis)
      - [Most Common Words in Leading
        Paragraphs](#most-common-words-in-leading-paragraphs)
      - [Most Common Words used for Each Continent by `tf-idf`
        Score](#most-common-words-used-for-each-continent-by-tf-idf-score)
      - [Article Sentiment Analysis](#article-sentiment-analysis)
      - [Wordclouds](#wordclouds)
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

### Data Source

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

Because each row in the new data frame consists of the details of one
articles, I needed to select only necessary information from the cells
with text in them. This meant I could select from the title, the lead
paragraph, or the “snippet,” which I think is used for social
media/advertising purposes. I chose to analyze the lead paragraphs for
the overall sentiment among all the articles in the past 10 years. To
tidy the data, I started by making each word in the leading paragraph a
row, then filtered the `stop_words` (ones that hold little semantic
meaning) from the text.

## Text Analysis

### Most Common Words in Leading Paragraphs

Starting off, it would be interesting to look at what are the most
common words used in these articles in the past 10 years before looking
at any kind of sentiment analysis. I’m curious to see if we will capture
any meaningful verbs or adjectives used to describe migrants/migrant
story.

![](nyt_text_analysis_files/figure-gfm/common%20words%20viz-1.png)<!-- -->

Expectedly, the most commonly used word is Europe/European, which is
interesting considering the migrant European crisis didn’t happen until
2015-2016. This suggests that there was really a spike in articles
talking about migrants during this period, much more so than in the
previous 5 years (2010-2015) and perhaps even after.

It should also be noted that there are a few semantically less
meaningful words included in this chart (e.g. friday, people, thousand).
Given that this analysis will only take the top used word, we will need
to do more of an analysis of sentiment and not just the text itself to
further evaluate what kind of language is being used to describe
migrants generally and around the world.

### Most Common Words used for Each Continent by `tf-idf` Score

As we conducted above, one way to quantify the language of these text
documents is to look a term’s frequency (how often it is mentioned).
However, as we also saw above, this doesn’t always allow us to take the
most important words even after filtering for stop words. So, we can use
a inverse document frequency to decrease the weight of commonly used
words and increase the weight of words not used as much.

![](nyt_text_analysis_files/figure-gfm/common%20words%20cont-1.png)<!-- -->

After creating a `tf-idf` score, the plots identify word that are
important to a text but not used *too* commonly. I have further gone in
and grouped these by continents. As we can see, the “important words” by
`tf-idf` are actually countries or regions within the continents. That
being said, we still see some interesting phrases such as how the Middle
East includes “coast” or “boat”, and Australia includes “contentious”
and “detainees”.

I suppose this analysis tells us more about *where* events relating to
migration are happening most frequently than *what* is being discussed.
Furthermore, segmenting by continent gives us a more nuanced observation
than the general word frequency, since we know that there are a
disproportionate amount of articles about Europe than other regions.

### Article Sentiment Analysis

For this part, I relied on the [Text mining with R
textbook](https://www.tidytextmining.com/tidytext.html) cited in our
class materials.

One thing that was interesting about sentiment analysis is the use of
dictionaries, created by previous researchers. Below, I used the [Bing
et al.](https://www.cs.uic.edu/~liub/FBS/sentiment-analysis.html)
dictionary that dichotomizes words into positive or negative to analyze
the general trends of positive/negative sentiment in articles over time.
Also note, I also made two different plots to compare the article
snippets from their lead paragraphs to see if there might be any obvious
difference between the two. My initial guess would be that snippets
would have more extreme language (more positive/ negative) because it is
used to capture readers’ attention.

<img src="nyt_text_analysis_files/figure-gfm/sentiment plots-1.png" width="50%" /><img src="nyt_text_analysis_files/figure-gfm/sentiment plots-2.png" width="50%" />

Interestingly, there is more variation and extreme values (both positive
and negative) in the lead paragraph’s sentiment than in the “snippet”
introduction lines. We can also see that, on average, the words used to
describe migrants in stories and/or their conditions are overwhelmingly
negative. There are rarely any articles across all the key continents
that score high on this positive sentiment dictionary. For the most
part, when the New York Times is talking about migrants, they are not
talking about them in a positive way. This is likely due to the fact
that migrant working and living conditions are pretty precarious and
also easily exploitable. Migrants also encompass documented and
undocumented migrants (e.g., migrant workers, refugees, asylum seekers,
etc.) as we note with the European migrant crisis.

Furthermore, we can go ahead and look at the most commonly used negative
and positive words in these articles.

<img src="nyt_text_analysis_files/figure-gfm/sentiment words-1.png" width="50%" /><img src="nyt_text_analysis_files/figure-gfm/sentiment words-2.png" width="50%" />

As we can see, the most frequently used terms for both positive and
negative have some faults in this dictionary. Of course, without
context, some of these words seem like they would be a positive or
negative. However, we know that, for example, it is unlikely the term
“trump” as an adjective like the dictionary might assume and rather it
was likely referring to former President Trump. Another limitation of
this analysis is that the positive and negative “sentiment” doesn’t give
us much insight into the agency vs. object of these stories. For
example, “attacks” is one of the most commonly used terms in these
articles, but it is unclear whether this is referring to attacks made
*onto* migrants or attacks *by* migrants. In this basic analysis, we
lose out some of the nuance in these stories by only looking at the
terms.

### Wordclouds

Finally, one thing we did not explicitly cover in class was how to make
these word clouds, which are another representation of our text data (it
was, however, covered in the exercises but we did not have time to go
over it in class). Below, I have created a word cloud that polarizes
positive and negative words based on the Bing et al. dictionary (again).

<img src="nyt_text_analysis_files/figure-gfm/word cloud-1.png" width="50%" /><img src="nyt_text_analysis_files/figure-gfm/word cloud-2.png" width="50%" />

Visually, these two plots more or less do the same thing, the left-hand
side plot was generated in base R’s package `wordcloud`, and the
right-hand side plot was generated in ggplot’s version, `ggwordcloud`.
Both word clouds are another visualization method of the most frequently
used words, positive and negative as determined by Bing et al., in the
New York Times articles on migration in the last 10 years.

Overall, the plots here show that there are far more negative words than
positive ones when reporting on migrants. As mentioned previously, one
possible reason this might be the case is that there are more negative
things happen to migrants than there are positive ones. This kind of
sentiment analysis says less about how reporters talk about migrants and
perhaps more about what actually is happening to migrants around the
world.

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
    ##  date     2021-03-13                          
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
    ##  ggwordcloud  * 0.5.0      2019-06-02 [2] CRAN (R 4.0.1)
    ##  globals        0.14.0     2020-11-22 [2] CRAN (R 4.0.1)
    ##  glue           1.4.2      2020-08-27 [2] CRAN (R 4.0.1)
    ##  gower          0.2.2      2020-06-23 [2] CRAN (R 4.0.1)
    ##  GPfit          1.0-8      2019-02-08 [2] CRAN (R 4.0.1)
    ##  gtable         0.3.0      2019-03-25 [2] CRAN (R 4.0.1)
    ##  haven          2.3.1      2020-06-01 [2] CRAN (R 4.0.1)
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
    ##  modeldata    * 0.1.0      2020-10-22 [2] CRAN (R 4.0.1)
    ##  modelr         0.1.8      2020-05-19 [2] CRAN (R 4.0.1)
    ##  munsell        0.5.0      2018-06-12 [2] CRAN (R 4.0.1)
    ##  nnet           7.3-14     2020-04-26 [2] CRAN (R 4.0.1)
    ##  parallelly     1.22.0     2020-12-13 [2] CRAN (R 4.0.1)
    ##  parsnip      * 0.1.4      2020-10-27 [2] CRAN (R 4.0.1)
    ##  pillar         1.4.7      2020-11-20 [2] CRAN (R 4.0.1)
    ##  pkgbuild       1.2.0      2020-12-15 [2] CRAN (R 4.0.1)
    ##  pkgconfig      2.0.3      2019-09-22 [2] CRAN (R 4.0.1)
    ##  pkgload        1.1.0      2020-05-29 [2] CRAN (R 4.0.1)
    ##  plyr           1.8.6      2020-03-03 [2] CRAN (R 4.0.1)
    ##  png            0.1-7      2013-12-03 [2] CRAN (R 4.0.1)
    ##  prettyunits    1.1.1      2020-01-24 [2] CRAN (R 4.0.1)
    ##  pROC           1.16.2     2020-03-19 [2] CRAN (R 4.0.1)
    ##  processx       3.4.5      2020-11-30 [2] CRAN (R 4.0.1)
    ##  prodlim        2019.11.13 2019-11-17 [2] CRAN (R 4.0.1)
    ##  ps             1.5.0      2020-12-05 [2] CRAN (R 4.0.1)
    ##  purrr        * 0.3.4      2020-04-17 [2] CRAN (R 4.0.1)
    ##  R6             2.5.0      2020-10-28 [2] CRAN (R 4.0.1)
    ##  rappdirs       0.3.1      2016-03-28 [2] CRAN (R 4.0.1)
    ##  RColorBrewer * 1.1-2      2014-12-07 [2] CRAN (R 4.0.1)
    ##  Rcpp           1.0.5      2020-07-06 [2] CRAN (R 4.0.1)
    ##  readr        * 1.4.0      2020-10-05 [2] CRAN (R 4.0.1)
    ##  readxl         1.3.1      2019-03-13 [2] CRAN (R 4.0.1)
    ##  recipes      * 0.1.15     2020-11-11 [2] CRAN (R 4.0.1)
    ##  remotes        2.2.0      2020-07-21 [2] CRAN (R 4.0.1)
    ##  reprex         0.3.0      2019-05-16 [2] CRAN (R 4.0.1)
    ##  reshape2     * 1.4.4      2020-04-09 [2] CRAN (R 4.0.1)
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
    ##  textdata       0.4.1      2020-05-04 [2] CRAN (R 4.0.1)
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
    ##  vctrs          0.3.6      2020-12-17 [2] CRAN (R 4.0.1)
    ##  withr          2.3.0      2020-09-22 [2] CRAN (R 4.0.1)
    ##  wordcloud    * 2.6        2018-08-24 [2] CRAN (R 4.0.1)
    ##  workflows    * 0.2.1      2020-10-08 [2] CRAN (R 4.0.1)
    ##  xfun           0.19       2020-10-30 [2] CRAN (R 4.0.1)
    ##  xml2           1.3.2      2020-04-23 [2] CRAN (R 4.0.1)
    ##  yaml           2.2.1      2020-02-01 [2] CRAN (R 4.0.1)
    ##  yardstick    * 0.0.7      2020-07-13 [2] CRAN (R 4.0.1)
    ## 
    ## [1] /home/cmfong/R/x86_64-pc-linux-gnu-library/4.0
    ## [2] /opt/R/4.0.1/lib/R/library

\`\`\`
