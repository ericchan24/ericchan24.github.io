---
title: What Are People Saying About Airlines on Twitter?
layout: post
categories:
- topic modeling
---
![Thumbnails](/assets/img/2019-05-May/2019-05-18-What-Are-People-Saying-About-Airlines-on-Twitter/thumbnails.png){:width='650px'}

## Introduction and Goal
Given tweets that mention an airline company, can we understand what people are saying about that company and their experiences with them? I pulled two weeks worth of tweets mentioning airlines for analysis using machine learning Natural Language Processing (NLP) techniques.

The final goal is a list of topics that people are tweeting about. This is an unsupervised learning problem because we don't have labels for the topics we are finding, nor do we even know the number of topics to look for. These are things that will be determined during the modeling process.

## Modeling Process and Pipeline
Let me guide you through my process in going from tens of thousands of tweets to a list of topics. First I cleaned my tweets. Twitter data is messy and filled with non-standard English words. There are slang, misspellings, abbreviations, hashtags, Twitter handles, links, emojis, and other things that make it difficult to analyze. A thorough cleaning is imperative.

I made my own custom cleaning function to handle Twitter specific issues like correcting common misspellings, slang, eliminating emojis, Twitter handles etc. . .  

<p style="float: left; font-size: 9pt; text-align: center; width: 99%;
margin-right: 1%; margin-bottom: 0.5em;">
<img src="/assets/img/2019-05-May/2019-05-18-What-Are-People-Saying-About-Airlines-on-Twitter/raw_tweet.png"
style="width: 100%">
Raw tweet, words with red squares will be cleaned</p>

<p style="float: left; font-size: 9pt; text-align: center; width: 99%;
margin-right: 1%; margin-bottom: 0.5em;">
<img src="/assets/img/2019-05-May/2019-05-18-What-Are-People-Saying-About-Airlines-on-Twitter/cleaned_tweet.png"
style="width: 100%">
After the first iteration of cleaning the tweet</p>

This is the first of many iterations in the cleaning process. From here I would convert all words to lower case, remove "$" symbols, remove numbers, and other things to make tweets easier for a computer to work with.

Next I took the cleaned tweets and converted them to a numerical format.

![Count Vectorizer](/assets/img/2019-05-May/2019-05-18-What-Are-People-Saying-About-Airlines-on-Twitter/count_vectorizer.png){:width='650px'}

Each row is a tweet and the words in the tweet would be tallied in the corresponding columns.

After counting, I had a really, really long list of words and counts. I had a matrix that was

61711 rows x 24583 columns

The rows represent the number of tweets, the columns represent the number of unique words. How do we work with this data?

We need to do dimensionality reduction. I like to think of dimensionality reduction as taking a lot of data that doesn't mean much on its own and compressing it down such that the compressed data conveys much more information. Take movies for example. There are tens of thousands of movies, each one having its own unique features. We can go from a space where we think about every movie individually to one where we think in genres. Suddenly, we go from tens of thousands of movies to 30 genres.

Similarly with words, we can take tens of thousands of words and compress them into "topics."

It turns out, it is much easier to analyze tweets in this reduced dimension "topic" space. In the new "topic" space, I can group our tweets into clusters.

After clustering, I examine the tweets in each cluster and try to understand the "topic" of the cluster and if it makes sense.

This process is repeated until the topics make sense. It is an iterative process. There is no right answer or score.

![Workflow](/assets/img/2019-05-May/2019-05-18-What-Are-People-Saying-About-Airlines-on-Twitter/workflow.png){:width='650px'}

Above is the flow chart for working the project. Start at the upper left. The process is iterative, which means it is repeated until the data scientist is satisfied with the results.

## Summary and App
One particularly interesting topic that I found was topic number 9 which was about racism. On October 25, American Airlines was flagged for racist policies. There were a huge spike in tweets directed towards them from angry tweeters. However, the controversy quickly died down and the angry tweets returned to normal within a few days.

Another notable topic was topic number 3 which was about love of flying. It showed people's enthusiasm for traveling and their positive experiences with airlines. Topic number 7 is a topic nearly every traveler is familiar with: waiting at the gate, worrying about time, and flight delays.

Here is a link to an [Interactive App](https://airline-tweets.herokuapp.com/){:target="_blank"} that summarizes my results.

The code for the project can be found on my [Github](https://github.com/ericchan24/airline_twitter){:target="_blank"}.
