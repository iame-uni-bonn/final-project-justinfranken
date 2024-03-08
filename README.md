# TradingStratTester

[![image](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

## Introduction

This project assesses trading strategies across different asset classes and frequencies, offering users ample flexibility to experiment with various combinations of financial assets, frequencies, and strategies. Users can adjust global variables in 'src/tradingstrattester/config.py' to explore different assets, strategies, and simulation parameters such as timing for buying, selling, or holding, as well as initial depot value, starting assets, and transaction costs.

## Table of Content:

- [Introduction](#introduction)
- [Instructions on modifying the 'config.py' file:](#instructions-on-modifying-the--configpy--file-)
  * [Downloading financial data configurations](#downloading-financial-data-configurations)
    + [FREQUENCIES + START_DATE and END_DATE:](#frequencies---start-date-and-end-date-)
    + [ASSETS:](#assets-)
  * [Simulating depot configurations](#simulating-depot-configurations)
    + [STRATEGIES:](#strategies-)
    + [UNIT_STRAT and UNIT_VAR:](#unit-strat-and-unit-var-)
    + [Simulating depot variables (INITIAL_DEPOT_CASH, START_STOCK_PRCT, TAC):](#simulating-depot-variables--initial-depot-cash--start-stock-prct--tac--)
- [Get Started](#get-started)
- [Project template](#project-template)
- [Credits](#credits)

## Instructions on modifying the 'config.py' file:

In the 'config.py' file, users can adjust the results of the trading strategy comparisons. You can locate the 'config.py' file in the following directory:

```
src / tradingstrattester / config.py
```

Subsequently, instructions on modifying the lists and variables will be provided.

### Downloading financial data configurations
We download finanical data directly from [Yahoo Finance](https://de.finance.yahoo.com/) using the python library [yfinance](https://github.com/ranaroussi/yfinance).

#### FREQUENCIES + START_DATE and END_DATE:

The python library [yfinance](https://github.com/ranaroussi/yfinance) is providing the following possible frequencies:

|   Frequency   | Maximum possible history from today | Variable Name
| :---:   | :---: | :---: |
| 1 Minute | 7 days | "1m" |
| 2 Minute | 59 days | "2m" |
| 5 Minute | 59 days | "5m" |
| 15 Minute | 59 days | "15m" |
| 30 Minute | 59 days | "30m" |
| 60 Minute | 729 days | "60m" |
| 1 Day | Infinity | "1d" |
| 5 Days | Infinity | "5d" |
| 1 Week | Infinity | "1wk" |
| 1 Months | Infinity | "1mo" |
| 3 Months | Infinity | "3mo" |

To change FREQUENCIES, START_DATE or END_DATE change the corresponding objects. Initial FREQUENCIES, START_DATE and END_DATE (in the format 'YYYY-MM-DD') configurations are the following

```python
FREQUENCIES = ["2m", "60m", "1d"]
START_DATE = "2014-01-01"
END_DATE = "2024-01-01"
```

#### ASSETS:

Please input valid symbols corresponding to each asset, as listed on [Yahoo Finance](https://de.finance.yahoo.com/). To modify ASSETS, adjust the corresponding object. The initial configuration for ASSETS is as follows

```python
ASSETS = ["MSFT", "DB", "EURUSD=X", "GC=F", "^TNX"]
```

### Simulating depot configurations
Modifying the following objects will result in changes to the simulation's outcomes.

#### STRATEGIES:
Following trading strategies are implemented:

1. **Crossover Strategy** ('_crossover_gen'): The crossover strategy identifies market shifts by generating bearish signals when the current open surpasses its close and the previous open is lower than its close, and bullish signals when the current open is lower than its close and the previous open exceeds its close.
1. **Relative Strength Index (RSI)** ('_RSI_gen'): The [RSI](https://www.investopedia.com/terms/r/rsi.asp) is a momentum indicator and helps to identify overbought (above 70) or oversold (below 30) conditions in an asset, guiding potential buying or selling opportunities accordingly. The RSI has the following form:
$$RSI = 100 - \left[ {100} \over 1 + {{\text{Avg. gain}} \over {\text{Avg. loss}}} \right].$$

1. **Bollinger Bands (BB)** ('_BB_gen'): [BB's](https://www.investopedia.com/articles/technical/102201.asp) are a volatility indicator comprising a moving average and two bands laying above and below it, based on standard deviations. They dynamically adjust to market conditions, expanding during periods of high volatility and contracting during low volatility. When prices intersect with the upper Bollinger Band, it signals a sell opportunity, while intersecting with the lower band suggests a buy opportunity.
1. **Moving Average Convergence Divergence (MACD)** ('_MACD_gen'): The [MACD](https://www.investopedia.com/terms/m/macd.asp) line is generated by $MACD = \text{12-Period EMA} - \text{26-Period EMA}$ where EMA stands for exponential moving average. Bullish signals occurre when the MACD line crosses above the signal line, and bearish signals when it crosses below, where a threshold considering standard deviation of the MACD line is included.
1. **Random** ('_random_gen'): This strategy generates random buy (15%), sell (15%) or do nothing (70%) signals based on a random number generator.

Please input only valid strategy names in STRATEGIES, as listed above. To modify STRATEGIES, adjust the corresponding object. The initial configuration for STRATEGIES is as follows

```python
STRATEGIES = ["_random_gen", "_crossover_gen", "_RSI_gen", "_BB_gen", "_MACD_gen"]
```

#### UNIT_STRAT and UNIT_VAR:
In this project, the following unit trading strategies are available to determine the quantity of units traded for a buy or sell signal:

1. **fixed trade units** ('fixed_trade_units'): Trade a fixed amount of units determined in UNIT_VAR (e.g. 100),
1. **percentage to value trades** ('percentage_to_value_trades'): Calculate the number of units to trade by applying a fixed percentage of the current portfolio value determined by UNIT_VAR (e.g., 0.05).
1. **volatility unit trades** ('volatility_unit_trades'): Calculate the number of units to trade by applying a fixed percentage of the current portfolio value determined by UNIT_VAR (e.g., 0.075), multiplied by the standard deviation of the past 10 trades.

Please input only valid strategy names for UNIT_STRAT as listed above and **floats** or **ints** for UNIT_VAR. To modify UNIT_STRAT or UNIT_VAR, adjust the corresponding object. The initial configuration for UNIT_STRAT and UNIT_VAR is as follows

```python
UNIT_STRAT = "percentage_to_value_trades"
UNIT_VAR = 0.05
```

#### Simulating depot variables (INITIAL_DEPOT_CASH, START_STOCK_PRCT, TAC):
The following objects directly impact the simulated depot used to assess the performance of strategies across various asset/frequency combinations:

1. **INITIAL_DEPOT_CASH** (int / float): This parameter sets the initial total value of the simulated portfolio.
1. **START_STOCK_PRCT** (int / float): This parameter determines the initial number of assets (rounded down to the nearest whole integer) to be placed in the portfolio.
1. **TAC** (int / float): This parameter defines the transaction costs per trade. The calculation is as follows: $\text{tac per period} = \text{trade units} * tac$.

The initial configurations of these simulating depot variables are as follows:

```python
INITIAL_DEPOT_CASH = 10000
START_STOCK_PRCT = 0.25
TAC = 0.001
```

## Get Started

Once you've cloned this repository, you can begin by creating and activating the environment. This can be done by navigating to the directory containing 'environment.yml' and executing the following command.

```console
$ conda/mamba env create -f environment.yml
$ conda activate tst
```

In order to create the project type the following into your console,
while being in the project directory:

```console
$ pytask
```

## Project template

The project which will then be generated is structured as follows:

- **bld**: The build directory contains our analysis plots.
  - **analysis**: The storage consists of pickle files containing the signaling and simulated portfolio outcomes for each individual strategy.
  - **data**: Storage for the downloaded financial data files.
  - **plots**: Directory comprising subdirectories corresponding to distinct output plots: assets_and_depot_value, indicator_bars, and units_and_cash.
- **src**: The source directory contains all of our python files for generating analysis results.
  - **analysis**: Python files containing essential functions, initiating the analysis results.
  - **data_management**: Python file containing essential functions to download data from [Yahoo Finance](https://de.finance.yahoo.com/).
  - **final**: Python files which generate outcomes for the 'pytask' command.
  - **config.py**: Configurations file to change outcomes of that project. See "Instructions on modifying the 'config.py'" file for more information.
- **test**: The test directory contains files to test the functions used for our analysis.
  - **analysis**: Python files for testing the essential analysis functions.
  - **data_management**: Python file which tests the data_management functions.

> [!CAUTION]
> "To ensure the proper functionality of this project, it is necessary to have an **internet connection**. This allows the project template structure to download financial data directly from [Yahoo Finance](https://de.finance.yahoo.com/). Please note that no pre-downloaded financial data will be available on this GitHub page."

## Credits

This project was created with [cookiecutter](https://github.com/audreyr/cookiecutter)
and the
[econ-project-templates](https://github.com/OpenSourceEconomics/econ-project-templates).

We furthmore used the open-source package
[yfinance](https://github.com/ranaroussi/yfinance) to download financial data from
[Yahoo Finance](https://de.finance.yahoo.com/).
