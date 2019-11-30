---
title: Keep up with the news using Python
layout: post
categories:
- automation
- cron
- email
- web scraping
---
## Introduction and Goal
I try to keep up with all the articles on
[Fangraphs](https://fangraphs.com){:target="_blank"} every day. But some days,
I don't have time to read read everything.

For these days, I created an automated script to scrape all the articles on
Fangraphs and send an email attachment containing the articles.

## Tools used
Cron and BeautifulSoup

## Building the scraper
To scrape the articles, I used the package BeautifulSoup.

![Thumbnails](/assets/img/2019-11-November/30/beautiful_soup.jpg){:width='250px'}

This is the workflow:
- Find all the recent Fangraphs article links
- Parse the article dates to find today's articles
- Write the articles to a text file

One of the challenges with parsing and writing the articles was handling
non-utf-8 symbols in the articles. To handle this, I used the string function
```python
.encode('utf-8')
```
to get rid of all the extra symbols.

## Sending the email
To send the email, I used the email library.

I had to enable [Less secure app access](https://support.google.com/accounts/answer/6010255?hl=en){:target="_blank"}
on my gmail account for the script to work.

## Automating the script
I used the software utility [cron](https://en.wikipedia.org/wiki/Cron){:target="_blank"}
to run the script daily.

```bash
# scrape fangraphs articles daily at 11 pm
0 23 * * * . $HOME/.bash_profile; /Users/eric/anaconda/envs/python3.6/bin/python /Users/eric/ds/fangraphs/scripts/c_run_all.py >> /Users/eric/ds/fangraphs/scripts/fangraphs.log 2>&1
```

I since I used environment variables and a conda virtual enviornment in the
script, I had to prepend the following the following commands to my Python
script call:
```bash
# activate enviornment variables
. $HOME/.bash_profile
# use a conda virtual environment to run the script
/Users/eric/anaconda/envs/python3.6/bin/python
```

## Summary
I created an automated process to run daily that scrapes all Fangraphs articles,
write them to a file, and send an email attachement with the articles.

The code for the project can be found on my [Github](https://github.com/ericchan24/fangraphs_scrape){:target="_blank"}.
