---
title: Predicting Fake News With 99 Percent Accuracy
layout: post
categories:
- classification
- Naive Bayes
---
## tl;dr
I built a classifier using AI and natural language processing that was 99% accurate in detecting whether news articles were real or fake news. I obtained the articles by web scraping real and fake news sites to download over 600 articles.

<p style="float: left; font-size: 9pt; text-align: center; width: 49%;
margin-right: 1%; margin-bottom: 0.5em;">
<img src="/assets/img/2019-05-May/2019-05-17-Predicting-Fake-News-With-99-Percent-Accuracy/real_news_wordcloud.png"
style="width: 100%">
Real News Words</p>

<p style="float: left; font-size: 9pt; text-align: center; width: 49%;
margin-right: 1%; margin-bottom: 0.5em;">
<img src="/assets/img/2019-05-May/2019-05-17-Predicting-Fake-News-With-99-Percent-Accuracy/fake_news_wordcloud.png"
style="width: 100%">
Fake News Words</p>

## Introduction
A large chemical company keeps its employees up to date on news by compiling a list of news articles to send to its staff. Recently, its news feed has been infiltrated by fake news articles. I used AI and Natural Language Processing to build a classifier to separate the genuine articles from the fake news articles.

## The Blacklist
The company provided a list of sites on its "blacklist". These were sites that they flagged as sources of fake news. At first glance, articles on these sites appear to be real news. But closer examination reveals otherwise. Here is an excerpt of an article from one of the sites on the blacklist [nbpostgazette](http://nbpostgazette.com/){:target="_blank"}:

<blockquote cite="http://nbpostgazette.com">
Growing opportunity for startups in the Fuel Dispensers industry<br><br>

Questale released a detailed assessment of trends in Fuel Dispensers market. The research report includes diverse topics like total market size, key market drivers, challenges, growth opportunities, key players etc. We have also covered key market updates, the impact of regulations and technological updates. New startups entering the space of Fuel Dispensers need to carefully pick their niches and genres so that they can compete on an equal footing with global companies who have an end to end development studios, production capabilities and global skills and experience backing them.

You can get free access to samples from the report here: https://questale.com/report/fuel-dispensers-market-report-by-company-regions-types-and-applications-global-status-and-forecast-to-2025/303479

</blockquote><br><br>

## Building a data set
I built a web scraper to download over 600 news articles from real news sources (e.g. NY Times, FiveThirtyEight, Reuters) and fake news sources using the company's blacklist (e.g. NBPostGazette, JournalismDay, SatPRNews).

I prepared the articles for building a classifier by removing stop words (words such as **and**, **the**, and **I** that don't convey much information), converting all words to lowercase, and removing punctuation.

## Counting the words in each article
Counting the words in each article is the key step that quantifies our text data into a numerical format for modeling.

I used a method of quantifying article word counts called term frequency inverse document frequency (TF-IDF).

Term frequency represents how important each word is in an article. It is a rate statistic.

Inverse document frequency penalizes words that appear in many articles. The idea is that words that occur in many articles are less informative than those
that occur only in a small number of the articles.

TF-IDF multiplies term frequency by inverse document frequency to quantify the importance of each word in each article.

## Naive Bayes
Naive Bayes is a fast, accurate, and easy to implement classifier often used in text classification. It is based on Bayes' theorem and the **naive** part comes from the assumption that the features in the dataset are independent.

We are interested in calculating the posterior probabilities: What is the
probability that an article is fake news given the words in the article?

Naive Bayes looks at the TF-IDF score for each article and uses those numbers to identify words correlated with real news and fake news.

## Results
The classifier misclassified one out of 127 articles in the test set. It correctly classified all 27 fake news articles in the test set.

This is a [Github](https://github.com/ericchan24/Fake-News-Classifier){:target="_blank"} link with all the code used in this project.
