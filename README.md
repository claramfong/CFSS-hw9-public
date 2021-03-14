# Homework 9: Analyzing text data

The purpose of this assignment was to practice performing a text analysis, either through sentiment analysis, classification, or topic modeling. I chose to do this using the data I had scraped from the web in [homework 8](https://github.com/claramfong/hw08). For that assignment, I had built a data frame from New York Times' API but did not use text analysis to look at the articles I was interested in. Instead, I looked at continuous variables (e.g., word count, publication date etc.). For this assignment, I used the same data but looked at the content of the leading paragraphs of each article and the "snippets", which are the one-liners that load on social media (I think).

More details about the assignment can be found [here](https://cfss.uchicago.edu/homework/text-analysis/).

Note: you will need to use the New York Times API to access their article data.

## Files in Repository

In this repository, you will find two relevant files:

1. The text analysis [Rmarkdown file](nyt_text_analysis.Rmd), and
2. The corresponding markdown [output](nyt_text_analysis.md).

## Information about the New York Times API
For this assignment, I used data on articles about migration from the [New York Times Developers API](https://developer.nytimes.com/apis). To access the API, you will need to create an account on their website, and there will be instructions on how to access the articles from a GUI built into their website. My code builds the request from scratch by reading the documentation on how to submit a query on the website.

I was able to filter for all stories in the past 10 years (2010-2020) relating to migrants. I wanted to specifically look at the Foreign "newsdesk", which is another way to filter for stories because I was curious to see what was being addressed internationally. Further details are explained within the text analysis [Rmarkdown file](nyt_text_analysis.Rmd). Note, however, I have also saved a local version of the data frame I created based on when I built the request (3/11/21). You can find the local file in this repository as [nyt_articles.csv](nyt_articles.csv).
  
## Packages Used
```{r}
library(tidyverse)
library(stringr)
library(httr)
library(jsonlite)
library(lubridate)
library(ggplot2)
library(tidyr)
library(tidymodels)
library(tidytext)
library(wordcloud)
library(ggwordcloud)
library(reshape2)
```
