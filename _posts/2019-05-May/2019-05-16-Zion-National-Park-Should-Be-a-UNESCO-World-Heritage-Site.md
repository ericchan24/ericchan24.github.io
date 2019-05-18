---
title: Zion National Park Should be a UNESCO World Heritage Site
layout: post
categories:
- classification
- support vector machines
---
![Montage](\assets\img\2019-05-May\2019-05-16-Zion-National-Park-Should-be-a-UNESCO-World-Heritage-Site\montage.png){:width='650px'}
## Introduction
Zion National Park is a large and beautiful park in Utah. Millions of visitors visit annually to hike its scenic terrain. Why is Zion not on the list of UNESCO World Heritage Sites?

## What is UNESCO?

UNESCO is an agency of the United Nations that promotes science, education, and culture. One of the ways they do this is with a list of World Heritage Sites. To make it on the list, a site must be judged of significance to the collective interests of humanity.

I built a model using machine learning to classify whether a site would make it on the UNESCO list.

## Approach
UNESCO sites are divided into two types, one is cultural. These are man-made sites such as The Statue of Liberty or The Great Wall of China. The other type is natural. Natural sites are parks, marine areas, and landforms that exhibit outstanding examples plant and animal life. I focused on the natural sites, specifically the parks. The parks have more consistent features that can be used to build a model.

## Getting the Data
I gathered data from UNESCO, the US National Park Service, the Canadian National Park Service, and TripAdvisor for reviews about each park. The US and Canadian Park Service gave me observations of parks not on the UNESCO list.

## Features
The features that I looked at were size of the park, age of the park, TripAdvisor review score and Number of TripAdvisor reviews. The number of reviews was important because some sites had a very high rating.

![High Rating](/assets/img/2019-05-May/2019-05-16-Zion-National-Park-Should-be-a-UNESCO-World-Heritage-Site/torngat_trip_advisor.PNG){:width='650px'}

But only one review.

![One Review](/assets/img/2019-05-May/2019-05-16-Zion-National-Park-Should-be-a-UNESCO-World-Heritage-Site/torngat_trip_advisor1.PNG){:width='650px'}

So I combined review score and number of reviews into a single metric "weighted reviews" that accounts for the number of reviews. This also serves as proxy as number of visitors to a park that is important in the model.

## Testing out Machine Learning Algorithms
I used F1 score as my scoring metric. A baseline model that always predicts a park is a UNESCO site had an F1 score of 49%. I trained six different machine learning models with my data and here are the results:

![F1 Scores by Model](/assets/img/2019-05-May/2019-05-16-Zion-National-Park-Should-be-a-UNESCO-World-Heritage-Site/f1_score_by_model.PNG){:width='650px'}

A support vector machine (SVM) model was the best performer with a F1 score of 62%.

## What Exactly Can This Model Do?
Given a set of park features, I can input them into the model will look at the data I used to train it and output a prediction whether a park is a UNESCO World Heritage Site.

![Prediction](/assets/img/2019-05-May/2019-05-16-Zion-National-Park-Should-be-a-UNESCO-World-Heritage-Site/prediction.png){:width='650px'}

The model predicts that Zion National Park should be classified as a UNESCO World Heritage Site.  

## Going Forward
The model currently has a F1 score of 62% on 140 parks, 70 that were UNESCO sites and 70 that were non-UNESCO sites. I would like to add more parks from all around the world to see if I can increase the F1 score.

With a strong enough score, the model would be used to find those parks that are not currently UNESCO sites that should be UNESCO sites.

The code for the project can be found on my [GitHub](https://github.com/ericchan24/unesco_classification){:target="_blank"}.
