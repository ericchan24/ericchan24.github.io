---
title: How Expensive is Your City?
layout: post
categories:
- linear regression
---
<head>
<style>
.wrap {
    width: 300px;
    position: relative;
}

.wrap img {
    float: left;
    height: 20px;
}

.wrap h2 {
    line-height: 20px;
    <!-- top: 33px;
    left: 50px; -->
    <!-- display: inline; -->
}
tr.dark {
    background-color: #141866;
    color: #ffffff;
}
</style>
</head>

## Introduction
Suppose you receive a job offer in a new city. Your new job will give you a raise by 10%. But will you be better off financially after moving to that city after taking into consideration its cost of living?

The website [Expatistan](https://www.expatistan.com/cost-of-living){:target="_blank"} has information about prices from over 320 cities around the world. Each city has a list of over 50 items and their corresponding prices. The website takes those items and prices and gives us a value for the price index of each city.

## Objective
I decided to take the idea of this website and extend it by building a model to predict the cost of living index of any city in the world.

In order train my model, I needed to get data. I used Python's BeautifulSoup library to build a web scraper, to scrape 320 individual city pages from Expatistan's website. I extracted the item names and item prices from each page and loaded the data into a Pandas data frame.

I cleaned up the data and built my model. I used scikit-learn to perform a linear regression to calculate the coefficients for my model. Here are the results of my model.

![All Features Model Results](/assets/img/2019-05-May/2019-05-15-How-Expensive-is-Your-City/all_features.png){:width='650px'}

I reverse engineered the website's model. I was able to perfectly predict a city's price index!

However, my goal was to be able to use this model on all cities, not only the cities included on the website. The problem with this is that I need 50 prices of all sorts of items from in categories from food, housing, clothing, transportation, health, and entertainment. For some of these items would be nearly impossible to get their prices. There is no reliable to place to find the price of **1 pair of men's leather business shoes** or **Short visit to private Doctor**. I went item-by-item and kept only the items for which I could easily find their prices.

I was left with only six items: gasoline, rental housing, fast food, public transportation, internet, and automobiles. I had to build a new model with the combination of these items. I also created a new feature called "region" that was based on the location of the city because I thought location would affect a city's price index.

I was skeptical whether a new model using only seven items would be very predictive. I noticed that rent was highly correlated to price index and I built a test model using only this single feature. It turns out that rent by itself is highly predictive of price index! From here, I added the price of fast food, the price of a monthly bus pass, and the region in which the city was located as my final model.

Here are the results of my final model:
![Final Model](/assets/img/2019-05-May/2019-05-15-How-Expensive-is-Your-City/final_predictions.png){:width='650px'}

## Chicago vs South Bend
Here's an example of the model in action. I'll calculate the price indices for Chicago and compare it to South Bend.

{% highlight python %}
# chicago
Intercept = 12.76
Rent = 2000 * 0.03
Bus_ticket = 105 * 0.42
Fast_Food = 8 * 6.57
Region = 14.43 * 1
cost_chi = Intercept + Rent + Bus_ticket + Fast_Food + Region
cost_chi = 184

# south bend
Intercept_sb = 12.76
Rent_sb = 900 * 0.03
Bus_ticket_sb = 36 * 0.42
Fast_Food_sb = 6 * 6.57
Region_sb = 14.43 * 1
cost_sb = Intercept_sb + Rent_sb + Bus_ticket_sb + Fast_Food_sb + Region_sb
cost_sb = 109

cost_chi / cost_sb - 1 = 0.688
{% endhighlight %}

My model calculates that Chicago's cost of living is about 69% more expensive than South Bend's.

The code for the project can be found on my [GitHub](https://github.com/ericchan24/price_indices){:target="_blank"}.

Here's a link to some [visualizations](https://price-indices.herokuapp.com/){:target="_blank"} that I made for this project.
