Hourly Electricity Price Forecasting using Long Short-Term Memory Neural Networks
===============================================

##### *Advanced Analytics have always been an incredibly important topic in trading strategies. This article will focus on electricity price forecasting and a possible state-of-the-art approach to solve it.*

---

_Author: Matteo Bonanomi_

_Source: Medium_  

_Publication Date: Feb 10, 2019_

_Link: https://medium.com/@mbonanomi/hourly-electricity-price-forecasting-using-long-short-term-memory-neural-networks-814ceac517b0_

---


**Technical background**

European electricity spot market (also called day-ahead market) have become more and more fluctuating as the renewable energy share became larger. Renewable energy, often bidded at very competitive prices in the market, have a huge impact on regional electricity price and depends on non-deterministic factors, such as weather conditions and their (often unreliable) forecasts. For these reasons, renewable energy forecasting and electricity price forecasting are a complex matter, commonly addressed in scientific literature.

**Aim of the Work**

The analysis I performed compares a trivial baseline model (called _persistence model_) to an advanced time series forecasting technique: _Long Short-Term Memory_ Neural Networks. This complex algorithm is widely studied when time series forecasting is concerned. Its black-box behavior and difficulty to interpret are two main drawbacks, yet its performance can beat traditional _ARIMA_ models when complex problems are addressed and large datasets are available. The aim of my work is to develop a simple and modular code to train an LSTM using an open electricity price dataset. Performance of this model are estimated varying the time forecast horizon, so that its benefits compared to a simple baseline model can be clarified.

**Dataset and baseline model**

[Nord Pool](https://www.nordpoolgroup.com/) website is a huge open data source when Northern European Electricity Market is concerned. You can download the dataset I used [HERE](https://www.nordpoolgroup.com/historical-market-data/). This analysis is performed on hourly price data, but the code I developed is quite flexible and modular and can be reused and tuned for any kind of analysis.

The baseline to beat for the LSTM model is quite simple: a persistence model. This means that the forecasted price at a certain hour is going to be the last price I observed. If I have access to real-time data, the persistence model will predict at _hour t_ the same price I observed at _hour t-1_. In many cases, one might be interested in predicting electricity price with a few hours in advance. For instance, given the last known price at time t, I would like to know the price at _t+2h_, _t+3h_, _t+4h_ and so on. For this reason, the persistence model might be quite effective with forecast quite close to the last available data, especially with time series that are not so fluctuating (electricity price is not cypto!). Later on, I will show the performance of this kind of model as a function of the last available hourly price data. It is easy to understand model performance are going to be decent only when we are very close to the last known hour and they will decay quite suddenly. A new LSTM must be able to beat this baseline, especially for a daily forecast: 1–24h from the actual time.

**Algorithm and Code Development**

The algorithm chosen for this kind of analysis is a Long Short-Term Memory (LSTM) Neural Networks. The details about this kind of Recurrent Neural Networks are out of the purpose of this article, yet a brief bullet list may clarify some relevant points:

1.  LSTM Neural Network, like an other Recurrent Neural Network (RNN), differs from a standard feed-forward approach by the use of previous input sources within the calculation.
2.  During the training of RNN one may have troubles due to the explosion of gradient errors as during iterations loop. This may lead to a very unstable neural network.
3.  LSTM approach solves this issue by adding the ability of memorizing and update new information or just deleting them. This is possible thanks to a more complex and complete architecture, showed in picture below.

_A simple introduction to LSTM Neural Networks that I find very useful can be found on YouTube right [HERE](https://www.youtube.com/watch?v=WCUNPb-5EYI)_

There are of course drawbacks in using this state-of-the-art technique. As far as Time Series Forecasting is concerned, we need to deal with:

1.  **Dataset size**. LSTM model requires very large datasets to learn. In case you don’t have it, stick to classic approach like ARIMA models to get more reliable predictions.
2.  **Black Box**. As any neural network, a LSTM model may be hard to analyze and interpret, especially when results can be surprisingly good!
3.  **Computational Cost**. ANN are often hard to train and their hyperparameters have often a weak connection with the real problem you want to solve. A grid search approach can be computationally unsustainable and it is up to the data scienstist experience to limit its search and tuning model parameters to find a trade off between performance and training time.

In this simplified example, the model was developed in _Keras_, a Python library that uses _Tensorflow_ as a backend. It is extremely easy to implement any kind of Neural Network with Keras, so it’s a good starting point to quickly analyse a new dataset, like to one I use for this example. You can find my code and explanatory Jupyter Notebooks in my [GitHub repo](https://github.com/matteobonanomi/dsnd-capstone).

**Model Predictions**

Firstly, let’s look at the baseline model selected for this Time Series Forecasting problem: a persistence model. The figure below shows the best case for a persistence model: we have access to the last available data (at _hour t_) and we make predictions about the next hour ( _t+1h_). Since hourly price is a very good resolution to analyze the electricity price beahvior and electricity price has rarely dramatic fluctuations on hourly basis, this trivial approach may lead to very good performance. As you may imagine, the larger is the gap between the last available data and the time of forecast (let’s say _t+2h_, _t+3h_, _t+4h_ and so on) the worse is going to be our persistent prediction.

Persistence model: forecasted values (red points) for the next market hour are equal to actual values (blue points).

Below a sample LSTM output is summarized. One-week forecasts with hourly resolutions are shown. The assumption behind the model was the availability of data at hour t and the need to forecast the electricity price at hour t+2h, hence we had 1-hour gap of unknown price. This can be a very common situation when dealing with time series. One may not have the last updated input features, or maybe it is not useful to make predictions so close to the actual time. For market trading purposes, one may want to know the price behavior with a consistent time margin.

Example of 1-week LSTM price forecasts with a forecast horizon of two hours. For instance, if the last available hourly price is 8.00am we want to forecast the electricity price at 10.am, and so on.

Notice that our LSTM model seems to predict quite well the real price trend with 2 hour advance. It is quite different from the persistence model behavior, since our prediction does not seem to have a bias or lag. Sometimes price peaks are underestimated or overestimated, but the general trend is well-caught by the neural network. For this particular model, input features were 48 hourly lagged features. The hidden layer was made by 5 LSTM neurons and 10 epochs were enough to reach a reasonably low loss without and limit overfitting issues.

**Model Performance**

LSTM model was trained on a reasonably large dataset, containing hourly electricity prices from 2013 to 2016. Both persistence and LSTM model have been tested on a never-seen year (2017). Since electricity price is affected by various seasonal effects, it is important to train any model on several years and to test on (at least) one year. The diagrams below refer to the test performance on 2017 and uses two common metrics (MAE and RMSE) to evaluate model predictions.

Average model performance on validation set (year 2017): LSTM model (red) is always better than persistence model (blue) showing lower error metrics, especially in a short-term price forecasting (1-6h ahead of actual time)

Some important conclusions can be drawn after the analysis of model performance:

1.  Persistence and LSTM model are comparable only when predicting the next hour with respect to the last data available. This is a reasonable outcome, since electricity price usually does not dramatically change from hour to the next one.
2.  The previous statement does not mean that persistence model is somewhat a good option to address. Usually it is not useful to predict only the next-hour price for trading purposes. Sometimes, last our data may not be available at all!
3.  From 2h to 7h ahead of current time, hourly price predictions for the LSTM model are remarkably better than persistence model. This can have a realistic importance, since one may be interested in predicting the electricity price about the peak hours of the market day. Hence forecasting afternoon price during the morning (or evening price in the early afternoon) can be important for traders.
4.  From 7 to 18h hours ahead both MAE and RMSE increases for the LSTM model and reach a sort of plateau. In any case, both performance metrics are always better than persistence baseline.
5.  From 18h to 23h in advance, error tends to decrease in both models. This is due to daily seasonality of the price, having day/night fluctuations. Knowing the price of today morning, it is easier to predict the electricity price for tomorrow morning than the price for this evening.

In conclusion, we can state that LSTM may be a feasible option for electricity price forecasting. Nevertheless, this article is just a preliminary introduction to the problem and must be further developed. The next steps can be:

1.  Further investigation of LSTM model tuning to improve performance in the 6h-24h time span with respect to the actual time.
2.  Application of the model with other market data or for larger dataset, to exploit the benefits of a Neural Network algorithm.
3.  Build a more competitive baseline, such as an ARIMA model, to understand if LSTM may be better than another common approach to time series forecasting.

**Documentation and Code Repository**

Please have a look at my [GitHub repository](https://github.com/matteobonanomi/dsnd-capstone) for the complete code explained in a set of Jupyter Notebooks. It is build in a modular fashion, to be shared and re-used. Please comment and contact me in case you have any suggestion to improve my analysis.

Dataset is open and available in the [Market Data](https://www.nordpoolgroup.com/historical-market-data/) historical archive webpage of the [Nord Pool](https://www.nordpoolgroup.com/) website.

If you are new to LSTM and their implementation on Keras, please follow some of this introductory guides on [MachineLearningMastery.com](https://machinelearningmastery.com): [Link1](https://machinelearningmastery.com/time-series-prediction-lstm-recurrent-neural-networks-python-keras/), [Link2](https://machinelearningmastery.com/time-series-forecasting-long-short-term-memory-network-python/), [Link3](https://machinelearningmastery.com/multi-step-time-series-forecasting-long-short-term-memory-networks-python/).