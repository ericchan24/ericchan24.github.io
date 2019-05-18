---
title: Classifying Animals Using Machine Learning
layout: post
categories:
- classification
- random forrest
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
![Mini Highland Cow](\assets\img\2019-05-May\2019-05-19-Classifying-Animals-Using-Machine-Learning\mini_highland_cow.png){:width='650px'}

This is a mini highland cow. From the image, we see that it has hair, a tail,
and four legs. Some other things that we know about this animal is that it has
teeth and it produces milk.

The mini highland cow's attributes are what are called the features of the
animal. The class it belongs to is the mammal class. My goal is to develop a
model that maps from the features of an animal to its class.

## The Data

The Data
I used a data set from the [UCI Machine Learning repository](http://archive.ics.uci.edu/ml/datasets/zoo){:target="_blank"}.

![Animal Classes](\assets\img\2019-05-May\2019-05-19-Classifying-Animals-Using-Machine-Learning\animal_classes.png){:width='650px'}

The animals are divided into seven classes. A particularly interesting class is
the non-insect invertebrate class. It includes are a variety of animals from
worms, arachnids, crustaceans, mollusks, and others.

![Number of Animals by Class](\assets\img\2019-05-May\2019-05-19-Classifying-Animals-Using-Machine-Learning\number_of_animals_by_class.PNG){:width='650px'}

There are 101 animals in the data set. There were only five reptiles and four
amphibians. This is important to keep in mind during the splitting of the data.
I want at least one example of reptile and amphibian in my testing data.

## Data exploration, preprocessing, and cleaning
This is a clean data set with no missing values or outliers. The only non-binary feature was the number of legs of each animal. I scaled the number of legs to values between 0 and 1 using a max-min scaler.

## Modeling
There are many algorithms to choose for classification. There is a rule of thumb for logistic regression requiring ten training observations per feature to have properly calibrated model probabilities. I decided to not use logistic regression. Naive Bayes has the assumption that the features used are independent. Since I had features such as "Does this animal have fins" and "Does this animal live in the water", my features were not independent. I decided not to use naive bayes.

I built four models and compared the results.

![Four Model Accuracy](\assets\img\2019-05-May\2019-05-19-Classifying-Animals-Using-Machine-Learning\four_model_accuracy_barh.PNG){:width='650px'}

These models were built without any tuning of hyperparameters. Random forest
and decision tree were producing high accuracy and moved forward with these
algorithms.

I used randomized grid search to tune the hyperparameters for a decision tree
and random forest model. Random forest produced the best results.

![Confusion Matrix Random Forest](\assets\img\2019-05-May\2019-05-19-Classifying-Animals-Using-Machine-Learning\confusion_matrix_random_forest.PNG){:width='650px'}

The random forest model correctly classified every observation in the test set.

Conclusion
I made a model that maps from animal features to the class of that animal using
random forest. The model was trained on 81 observations and tested on 20
observation. It had 100% accuracy on the test set.

I made an [interactive app](https://animal-classifier.herokuapp.com/){:target="_blank"} that shows how the model works.

The code for this project can be found on my [Github](https://github.com/ericchan24/Animal-Classification){:target="_blank"}.
