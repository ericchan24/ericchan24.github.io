---
title: Time Series Forecasting in R
layout: post
categories:
- time_series
- forecasting
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

## Introduction: World Bank Data &#38; Malaysia GNI
This is a tutorial on how to create a time series forecast using R.  

A time series forecast predicts future values of a variable based on past
variables. I will predict future values of GNI (Gross National Income) based
 past values.  

I used economic data from the [World Bank](https://data.worldbank.org/) to
build my forecast. I chose Malaysia as the country to forecast.

<div class="wrap">
<h2>&#160;Malaysia GNI <img src="\assets\img\10 October\02\malaysia_flag.png" alt="malaysia flag"></h2>
</div>
## Step 0: Examine the data  

## Step 1: Create a naive model as a baseline  
I'll build a baseline model to compare to assess the performance of my model.

A naive model predicts that this year's GNI is last year's GNI.

```
# create model & get predictions
fcast <- naive(y_train, h = num_test)
y_pred <- fcast$mean
```

Let's plot this model to visualize how well it predicts GNI.

```
gni <- df$GNI
# plotting prep
ylim <- c(min(gni) * .9, max(gni) * 1.1)
xlab <- 'Year'
ylab <- 'GNI'
main <- 'Malaysia GNI Actual VS Forecast'
location = 'bottomright'
legend = c('train', 'test', 'forecast')
col = c('black', 'blue', 'red')
lty = 1
cex = 0.8
# plot actual vs forecast  
plot(df$Year, y1, type = 'l', col = 'black', main = main,
    xlab = xlab, ylab = ylab, ylim = ylim)
lines(df$Year, y2, type = 'l', col = 'blue')
lines(df$Year, y3, type = 'l', col = 'red')
legend(location, col = col, legend = legend, lty = lty, cex = cex)
```
![Naive Model](\assets\img\10 October\02\naive_graph.PNG){:width='650px'}  

The naive model's last training observation was in 2008. After 2008, it
forecasts that Malaysia will have the same GNI that it had in 2008.  

## Step 2: Calculate MAPE for naive model  
To quantify the model, I'll calculate MAPE (mean absolute percent error) of the
forecast.
```
# create data frame with actual and predicted values
mape_naive_df <- data.frame('Actual' = y_test, 'Predicted' = y_pred)
mape_naive_df['error'] <-  mape_naive_df['Actual'] - mape_naive_df['Predicted']
mape_naive_df['abs_pct_error'] <- abs(mape_naive_df['error'] / mape_naive_df['Actual'])
mape_naive <- mean(mape_naive_df[, 'abs_pct_error']) * 100
```

<strong>Naive Model MAPE: 9.04%</strong>

## Step 3: Create an ARIMA model  
```
# create ARIMA model & generate predictions  
arima_fit <- auto.arima(y_train)
fcast <- forecast(arima_fit, h = num_test)  
```

Plot the ARIMA model's predictions to visual how well it predicts GNI.  

```
# plot preparation
y_pred <- fcast$mean
y3 <- append(rep(NA, num_train), y_pred)
# plot actual vs forecast  
plot(df$Year, y1, type = 'l', col = 'black', main = main,
     xlab = xlab, ylab = ylab, ylim = ylim)
lines(df$Year, y2, type = 'l', col = 'blue')
lines(df$Year, y3, type = 'l', col = 'red')
legend(location, col = col, legend = legend, lty = lty, cex = cex)
```

![ARIMA graph](/assets\img\10 October\02\arima_graph.PNG){:width='650px'}  

## Step 4: Calculate MAPE for ARIMA model  
I'll calculate the MAPE for the ARIMA model and compare it with the naive
model.  

```
arima_df <- data.frame('Actual' = y_test, 'Predicted' = y_pred)
arima_df['error'] <-  arima_df['Actual'] - arima_df['Predicted']
arima_df['abs_pct_error'] <- abs(arima_df['error'] / arima_df['Actual'])
mape_arima_df <- mean(arima_df[, 'abs_pct_error']) * 100
```
<strong>ARIMA model MAPE: 3.01%</strong>  

## Step 5: Compare model performance  
The ARIMA model outperformed the naive model.  
<table>
  <tr class="dark">
    <th>model</th>
    <th>MAPE</th>
  </tr>
  <tr>
    <th>naive</th>
    <th>9.04%</th>
  </tr>
  <tr>
    <th>ARIMA</th>
    <th>3.01%</th>
  </tr>
</table>
