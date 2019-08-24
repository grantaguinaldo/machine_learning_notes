### Notes on Time-Series Forecasting 

General Resources 

* [Tamara Louie: Applying Statistical Modeling & Machine Learning to Perform Time-Series Forecasting](https://youtu.be/JntA9XaTebs)
* [A comprehensive beginner’s guide to create a Time Series Forecast (with Codes in Python and R)](https://www.analyticsvidhya.com/blog/2016/02/time-series-forecasting-codes-python/)
* [Why does a time series have to be stationary?](https://stats.stackexchange.com/questions/19715/why-does-a-time-series-have-to-be-stationary)
* [A Gentle Introduction to Handling a Non-Stationary Time Series in Python](https://www.analyticsvidhya.com/blog/2018/09/non-stationary-time-series-python/)

As noted on the ["Time Series" entry on Wikipedia](https://en.wikipedia.org/wiki/Time_series):

>A time series is a series of data points indexed (or listed or graphed) in time order. Most commonly, a time series is a sequence taken at successive equally spaced points in time. Thus, it is a sequence of discrete-time data. Examples of time series are heights of ocean tides, counts of sunspots, and the daily closing value of the Dow Jones Industrial Average.

![](https://upload.wikimedia.org/wikipedia/commons/7/77/Random-data-plus-trend-r2.png)

#### Why use Time-Series Forecasting?

When deciding to use time-series forecasting, it is important to formulate a question that can be answered with data, and more importantly, a question that can only be answered with _time-series data_.

One way to know if your question needs to be answered with time-series data, is to know if there an _inherent relationship_, or _structure_, between a given variable and time. 

These examples include: 

* What will be the stock price of `Google` be in 5 years?
* How much natural gas will Los Angeles consume in the next 10 years?
* What will be the average ground-level methane concentration from the oil field in the next 2 years?

Some of the models that can answer these questions include:

* Traditional regression-based models.
* Univariate time-series models (ARIMA, etc.).
* Modified univariate time-series models.
* Bayesian structural time series models.
* Hierarchical time-series models.


When choosing a model, be sure that you're sensitive to the inherent order and structure of the data. Some models will ignore the order and structure of the data and can lead to less desirable outcomes. If you opt to use a traditional machine learning model, you're ignoring the structure of the data and not extracting the full value from the data that you have. 

Also, when selecting a type of model to work with, you're going to want to plot your data to understand what is happening within the data. For example, when plotting the data, you will maybe see trends of seasonality, or varying variance in the structure. Those observations are also very important when selecting a type of model. 

Most time series models will assume that the data is _stationary_, meaning that the data has constant mean, variance and autocovariance. This allows us to make the assumption that what has happened in the past will allow us to predict the new trends in the future. 

To test for _stationarity_, you can run the _Dickey-Fuller Test_. For this test, you are conducting a hypothesis test where the H0: is that the time-series data _is not_ stationary. 

You can use `from statsmodels.tsa.stattools import adfuller` 

(more on this [here](https://www.analyticsvidhya.com/blog/2016/02/time-series-forecasting-codes-python/))

If you find that the data is not stationary, then we can call the under lying process that is used to collect the data, into question. 

Also, if you find that your data is not stationary, then you'll need to correct the data accordingly. Two common ways to correct the data are correct for trend and seasonality. 

Ways for removing trends and seasonality include:

* Transformation
* Smoothing
* Differencing
* Polynomial fitting
* Decomposition

(more on this [here](https://www.analyticsvidhya.com/blog/2016/02/time-series-forecasting-codes-python/))

Sometimes, you'll need to run multiple corrections and see which one fits the data best. 

#### ARIMA (Auto-Regressive Integrated Moving Average) Model

The model is broken down into three parts and assumes that the data is stationary.

* `p` Number of Auto-Regressive terms. (Plot the line between time `t` and the previous time `t-1` )
* `d` Number of Integrated or Difference terms.
* `q` Number of Moving Average terms. Is there any remaining error in the residuals? 


We determine the values for `p`,`d`, and `q` by looking at the Autocorrelation Function (`ACF`) and Partial Autocorrelation Function (`PACF`).

`ACF` is the correlation between the time series with a lagged version of itself (correlation of `Y(t)` with `Y(t-1)`

`PACF` is the additional correlation explained by each successive lagged term.

#### Why is Stationarity Important

>To add a high-level answer to some of the other answers that are good but more detailed, stationarity is important because, in its absence, a model describing the data will vary in accuracy at different time points. As such, stationarity is required for sample statistics such as means, variances, and correlations to accurately describe the data at all time points of interest. [Stackexchange](https://stats.stackexchange.com/questions/19715/why-does-a-time-series-have-to-be-stationary)

<br>

> Weak stationarity means that the first and second moments (so the mean, variance, and autocovariance) don't change with time. A strongly stationary process has the same distribution no matter the time, but let's focus on the weakly stationary process, as any strongly stationary process is weakly stationary.
[Quora](https://www.quora.com/Why-should-you-stationalize-a-time-series-before-using-ARMA-model-in-time-series-forecasting)

#### Choosing `p`,`d`, and `q` for an `ARIMA` model.

> **ACF and PACF plots:** After a time series has been stationarized by differencing, the next step in fitting an ARIMA model is to determine whether AR or MA terms are needed to correct any autocorrelation that remains in the differenced series. Of course, with software like Statgraphics, you could just try some different combinations of terms and see what works best. But there is a more systematic way to do this. By looking at the autocorrelation function (ACF) and partial autocorrelation (PACF) plots of the differenced series, you can tentatively identify the numbers of AR and/or MA terms that are needed. You are already familiar with the ACF plot: it is merely a bar chart of the coefficients of correlation between a time series and lags of itself. The PACF plot is a plot of the partial correlation coefficients between the series and lags of itself. [Duke University](http://people.duke.edu/~rnau/411arim3.htm)
