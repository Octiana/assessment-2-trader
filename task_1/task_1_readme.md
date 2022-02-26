# Task 1: Work with Data
## Gathering, Manipulating, Cleaning and Visualizing Data

The goal and steps of this assessment are the following:
1. [Collect Trading data from multiple sources via API](#1-collect-trading-data-from-multiple-sources-via-api);
2. [Clean and Manipulate data](#2-clean-and-manipulate-data);
3. [Visualize Data](#3-visualize-data)
4. [Bonus: Trading Strategy (not mandatory)](#4-bonus-trading-strategy-not-mandatory)

Imagine your Line Manager ask you for some data due to a Department Request or because She/He has a Trading Idea that wants to test.

### 1. Collect Trading data from multiple sources via API
At this step, with the usage of API you have get trading data directly from the exchanges.
In specific, you have to gather data from [FTX](https://ftx.com/) and from [BITTREX](https://global.bittrex.com/).

As a reference: 
1. [FTX API documentation](https://docs.ftx.com/#overview)
2. [BITTREX API documentation](https://bittrex.github.io/api/v3)

You have to download the following Data:
- OHLC and Volumes 15-minutes data from November 1st, 2021 00.00 until February 1st, 2021 00.00 for the following markets:
   1. [DAWN/USD](https://ftx.com/trade/DAWN/USD) on FTX.
   2. [DAWN/BTC](https://global.bittrex.com/Market/Index?MarketName=BTC-DAWN) on Bittrex.
   3. [BTC/USD](https://global.bittrex.com/Market/Index?MarketName=USD-BTC) on Bittrex.

- OHLC and Funding Rate at 1h interval from June 1st, 2021 00.00 CET until February 1st, 2021 00.00 CET for the following market:
   1. [DAWN-PERP](https://ftx.com/trade/DAWN-PERP) on FTX.

**P.S.** if you are unable to collect the data, in the folder [data](/task_1/data) are the csv file.
So that, if you are not able to get them on your own, you can still go through the next steps. Nevertheless, use this file as the last chance (pay attention to timestamps).

### 2. Clean and Manipulate data:
1. Evaluate the synthetic ``DAWN/USD`` price in Bittrex;
2. Resample the 15min ``FTX:DAWNUSD`` and 15min ``BITTREX:DAWNUSD`` to 1h OHLC and volumes data. Are the two timeseries cointegrated?
3. Evaluate the spread between FTX:DAWNUSD and BITTREX:DAWNUSD in the following way: ``(FTX:DAWNUSD-BITTREX:DAWNUSD)/(BITTREX:DAWNUSD)``. Is the spread first order and second order stationary? 
4. Set as index of your spread dataset the ``timestamp`` column and convert it to a datetime object with CET timezone.
5. Write the spread dataset into a ``.csv`` file named ``dawnusd_ftx_bittrex_spread.csv`` that contains OHLC spread data and the Bittrex Volumes.
6. Evaluate at least 5 statistics/key measures that you think are useful to study this time series. (i.e. Moving Averages, Distribution, Z-score, etc.)
7. Are the funding rates and the spread cointegrated? Is there a spillover effect?
8. Create a model that defines the Funding rates as linear combination of the Spread: ``y=b0 + b1*x+e`` using as a rolling window to estimate the parameters: ``24h``, ``30-days``, ``the whole dataset``. with:
   - ``y``: being the funding rates
   - ``x``: being the spread
   - ``b0``: being the intercept
   - ``b1``: being the slope
   -``e``: being the error term
   How are these parameters changing? Are they statistically significant?

### 3. Visualize data
At this step we want to see how good you are in showing and presenting your findings.
We want to see from you at least the following plots, **but** we encourage you to show us everything you think is worth visualizing.
Also feel free to use the software/programing language you like the most to make data visualizations!

1. In a single plot show the close price of ``FTX:DAWNUSD`` and ``BITTREX:DAWNUSD`` at 1h interval.
   As a reference have a look at the following chart: 
![](/task_1/data/DAWNUSD_price_comparison.png)
2. A plot with:
   - the Spread ``(FTX:DAWNUSD-BITTREX:DAWNUSD)/(BITTREX:DAWNUSD)`` 1h OHLC time-series;
   - The Simple Moving Average of the Close with 24h rolling window;
   - A Horizontal Line with intercept equal to 0;
   - The ``BITTREX:DAWNUSD`` trading volumes
   As a reference have a look at the following chart: 
![](/task_1/data/DAWNUSD_spread_plot.png)
3. At least 2 plots of your choice showing the Statistics you have evaluated (histograms, timeseries, tables, etc.)
4. A Plot with the timeseries of the of the coefficients ``b0 and b1`` for both the 24h model and the 30-days model.

### 4. Bonus: Trading Strategy (not mandatory)
Now that you have dipped into the case let's see if we can profit from it!
- As a trading scenario you can assume 10bps as trading fees and 30$ as fixed costs for withdrawals.

1. First consider an arbitrage trading strategy "Buy Low, Sell High" without transactions fees (no trading fees and withdrawal costs). 
Here we want to see an in-sample backtest of a trading strategy that buys on the exchange with a lower price and then sell on the exchange where the price is higher. Describe the performance (i.e. return per trade, drawdown, cumulative return, number of trades, etc.) 

2. Now, let's make it more real. Consider the transactions costs (trading fees and withdrawal costs) as well as the trading volumes on Bittrex. 
As a hypothesis: consider that you are able to use 50% of the hourly trading volumes on Bittrex and 50% of the hourly volumes on FTX. Provide an in-sample backtest of the strategy described at the previous step.
Describe the performance.
