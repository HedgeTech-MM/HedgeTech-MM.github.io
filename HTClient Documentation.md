# HTClient Documentation

## Introduction

HTClient is the HedgeTech app. It can be operated in order to customize, run and monitor trading algorithms.
This documentation helps naviguating through the infrastructure of HTClient and its User Interface (UI). We recommend reading this document while having HTClient opened.

### Target Audience and Target Markets

HTClient is particularly suitable for token issuers, exchanges, market makers and broker/dealers. 
Token issuers can operate the systems within their company, on all exchanges they are listed on. 
Exchanges can operate the systems in-house for their main pairs as well as all other altcoins listed on their platform.
Market makers can chose to either use the algorithms designed by HedgeTech or to design their own custom scripts, benefiting from our integration capabilities and server infrastructure.
Broker/dealers can add to the services they offer with an all-in-one liquidity provision system.

The scripts are suitable for any market environment: major coins, altcoins, security tokens, perpetual contracts and all other digital assets.

### Installation

After registering your HedgeTech account, a `username`, `password` and `secretkey` will be given to access HTClient, each of these should be carefully saved and securely stored.

#### Windows

After downloading HTClient at https://www.hedgetech.io, the first time HTClient.exe is opened, a message might show up saying that Windows Defender Smartscreen has protected the PC. If that is the case, please click `More info` and if the developer shows `HedgeTech LLC`, safely select `Run Anyway`. The next times HTClient.exe is opened, this extra step is not required.

#### Mac OS X

After downloading HTClient at https://www.hedgetech.io, and installing it at the desired location (by dragging the app to that location), the first time HTClient.app is opened, an error message may appear saying that this app cannot be opened. If that is the case, simply run the following code in your Mac Terminal:

```
sudo xattr -rd com.apple.quarantine 
```

Followed by a space, and then drag the app file in the Terminal. Hit `Enter`, enter the password of the device (note: the password will not be displayed on the screen) and hit `Enter`. The next times HTClient.app is opened, this extra step is not required.

### Syntax

Please make sure to follow accurately the syntax such as upper and lower cases.

`str`: text, e.g. `some text`

`int`: integer number, e.g. `1` 

`float`: decimal number, e.g. `1.1`

`str`: text, e.g. `some text`

`int`: integer number, e.g. `1` 

`float`: decimal number, e.g. `1.1`

`bool`: `True` or `False`

`expression`: the expression of a function that is calculated with the following operators: `+`, `-`, `*`, `/`, `(`, `)` and the following components:

+ `ask`: The lowest ask price
+ `bid`: The highest bid price
+ `mid`: The middle price between the lowest ask and the highest bid
+ `unit`: The base price increment 

e.g. `bid+unit` will return the price that is one base price increment higher than the highest bid

### IP Adresses

For security reasons, our servers use non-static IPs. In order to use HTClient, make sure to whitelist all IPs on your exchange accounts by not filling any IP under the exchange website IP whitelist field. If the exchange does not provide this feature on their website, please get in touch with the exchange directly.

### API Permissions

For your own security, please make sure to manage permissions if the exchange offers this feature. In order to use HTClient read/write permissions for everything except withdrawal/deposit should be enabled.

## Strategy Catalog

### Market Making

Script class symbol: `maker`

Purpose: The `maker` script class provides liquidity to the market, it narrows the market spread and it makes profits from the spread when the price fluctuates.
Details: It maintains a number of orders on both the buy and sell sides for a currency pair. It lets the price behave according to supply and demand in the market, it adjusts the maintained orders automatically: when people sell, the price will move down; when people buy, the price will move up.
Use cases: provide liquidity on any market and offer a competitive spread in price to achieve an efficient market environment where traders can enter and exit at fair prices.

Account(s): main account only.

| Parameters | Description | Type |
|------------|-------------|------|
| base | Market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT|
| quote | Market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT|
| levels | Number of orders maintained on each side of the order book | `int` |
| density | The distance between orders placed within the same side of the order book | `float`, percentage, e.g. `0.002` means 0.2% |
| aveVolume | Average amount in quote currency of each order | `float` |
| randomness | Desired degree of randomness of the order amount around aveVolume | `float`, between 0 and 1: 0 is no randomess; 1 is maximum randomness |
| spread | Distance between the first ask order and the first bid order to be placed. It is recommended to set a spread of at least (3 x density) and that (spread - density) > (2 x trading fees) | `float`, percentage, e.g. `0.01` means 1% |
| baseName (Optional) | Full name of the base currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| quoteName (Optional) | Full name of the quote currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| cutlossBase (Optional) | The cutloss threshold for base currency: if the total base currency balance is lower than the threshold, the script will pause | `float` |
| cutlossQuote (Optional) | The cutloss threshold for quote currency: if the total quote currency balance is lower than the threshold, the script will pause | `float` |

Note: after putting maker online, if you need to change the following parameters: levels, aveVolume, randomness, spread, density, you will need to first turn the script offline, clear storage, clear orders and restart with the desired parameters.

### Stable Market Making (or Market Making with Price Matching)

Script class symbol: `stable`

Purpose: The `stable` script class provides liquidity to the market and narrows the market spread, while matching the price to a desired index.
Details: It maintains a number of orders on both the buy and sell sides for a currency pair around a given price. Such a price is defined by the base value and/or quote value. You can define the base value and the quote value to be set values, or use the market price that is reported by CoinGecko.
Warning: It can place taker orders to match the price to the index value. As a result, there are situations in which external orders can be actively filled. Please reach out if more information is necessary.
Use cases: provide liquidity and narrow spread on stablecoin markets that are pegged to a specific value, major pairs on small exchanges that want to reduce adverse arbitrage opportunities, pairs with price dicrepancies for a given token in order to reduce adverse arbitrage opportunities.

Account(s): main account only.

| Parameters | Description | Type |
|------------|-------------|------|
| base | Market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT|
| quote | Market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT|
| baseValue | The USD value of the base currency | Pass `int` or `float` to define a stable value; pass `market` for the value to be automatically adjusted to the price reported by CoinGecko |
| quoteValue | The USD value of the quote currency | Pass `int` or `float` to define a stable value; pass `market` for the value to be automatically adjusted to the price reported by CoinGecko |
| askNum | Number of ask orders to be maintained | `int` |
| bidNum | Number of bid orders to be maintained | `int` |
| density | The distance between orders placed within the same side of the order book | `float`, percentage, e.g. `0.002` means 0.2% |
| aveVolume | Average amount in quote currency of each order | `float` |
| randomness | Desired degree of randomness of the order amount around aveVolume | `float`, between 0 and 1: 0 is no randomess; 1 is maximum randomness |
| spread | Distance between the first ask order and the first bid order to be placed. It is recommended to set a spread of at least (3 x density) and that (spread - density) > (2 x trading fees) | `float`, percentage, e.g. `0.01` means 1% |
| baseName (Optional) | Full name of the base currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| quoteName (Optional) | Full name of the quote currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| cutlossBase (Optional) | The cutloss threshold for base currency: if the total base currency balance is lower than the threshold, the script will pause | `float` |
| cutlossQuote (Optional) | The cutloss threshold for quote currency: if the total quote currency balance is lower than the threshold, the script will pause | `float` |

### Orderbook Replication

Script class symbol: `replication`

Purpose: The orderbook `replication` strategy replicates the liquidity from an exchange with more liquidity to an exchange with less liquidity, in order to provide liquidity on the exchange with less liquidity. This strategy makes profits from strictly hedging the risk.
Details: Whenever the orders maintained on the exchange with less liquidity are filled, it will hedge the position by placing market orders and take the liquidity from the  exchange with more liquidity.
Warning: This is a relatively advanced script class, please reach out if more information is necessary before implementing it.
Use cases: provide liquidity and narrow spread on major pairs on exchanges with little liquidity, pairs with little liquidity for a given token.

Account(s): both main account and secondary account. Main account is the account on the exchange on which liquidity will be added (the one with less liquidity), secondary account is the account on the exchange on which liquidity will be taken from (the one with more liquidity).

| Parameters | Description | Type |
|------------|-------------|------|
| base | Market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT|
| quote | Market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT|
| levels | Number of orders maintained on each side of the order book on the exchange with less liquidity | `int` |
| divergence | The price distance between the orders placed on the exchange with less liquidity and the existing orders on the exchange with more liquidity. The larger divergence is, the higher the maintained spread and the lower the maintained order density will be on the exchange with less liquidity. However density has to be large enough to cover the trading fees on both exchanges combined and the transaction fee when transfering balance from one exchange to the other. Divergence must be greater than the maximum between (2 x base price unit) and (2 x trading fees + withdrawal fee) | `float`, percentage, e.g. `0.01` means 1% |
| percentage | The percentage of liquidity to be replicated from the exchange with more liquidity to the exchange with less liquidity | `float`, percentage, e.g. `0.01` means 1%. In order to hedge the position effectively, it is recommended to limit this parameter to a maximum of 0.2 |
| volumeCap | A hard maximum on the amount of each order in quote currency on the exchange with less liquidity | `float` |
| randomness | When the order amount exceeds the `volumeCap` for a given order, instead of placing an order exactly at the `volumeCap`, it will place an order around `volumeCap` with a degree of randomness. | `float`, between 0 and 1: 0 is no randomess; 1 is maximum randomnesss |
| baseName (Optional) | Full name of the base currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| quoteName (Optional) | Full name of the quote currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| cutlossBase (Optional) | The cutloss threshold for base currency: if the total base currency balance for the two accounts combined is lower than the threshold, the script will pause | `float` |
| cutlossQuote (Optional) | The cutloss threshold for quote currency: if the total quote currency balance for the two accounts combined is lower than the threshold, the script will pause | `float` |

### Market Entry (or Buy-Back, or Accumulate)

Script class symbol: `enter`

Purpose: The `enter` strategy is designed to efficiently purchase a large amount of tokens on the secondary market without influencing the price and suffering from a large slippage.
Details: The user specifies a certain price threshold and the script will purchase as much as possible at or under that threshold.
Use cases: efficiently accumulate tokens at the lowest price possible, build a price support (that can be moved up gradually).

Account(s): main account only.

| Parameters | Description | Type |
|------------|-------------|------|
| base | Market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT|
| quote | Market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT|
| maxPrice | The price threshold: the script will actively puchase only at a price lower than this threshold | `float` for price in quote currency or `str` in the format of `{price}-{symbol}` for price in a specific currency, e.g. `0.00000135-btc` to set the price to 0.00000135 in BTC |
| threshold | The volume threshold: the script will actively fill an order that has an amount in base currency that is higher than this threshold only | `float` |
| leftOver | In order to not impact the price, the script will always fill part of an order and leave a leftover amount, in base currency | `float` |
| bidVolume | When the price is lower than the price threshold, the script will always maintain a bid order on top of the bid side of the orderbook. This parameter specifies the average amount in base currency of the maintained bid order | `float` |
| randomness | Desired degree of randomness of the order amount around bidVolume | `float`, between 0 and 1: 0 is no randomess; 1 is maximum randomness |
| baseName (Optional) | Full name of the base currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| quoteName (Optional) | Full name of the quote currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| cutlossBase (Optional) | The cutloss threshold for base currency: if the total base currency balance is lower than the threshold, the script will pause | `float` |
| cutlossQuote (Optional) | The cutloss threshold for quote currency: if the total quote currency balance is lower than the threshold, the script will pause | `float` |

### Market Exit (or Sell-Off, or Liquidate)

Script class symbol: `exit`

Purpose: The `exit` strategy is designed to efficiently sell a large amount of tokens on the secondary market without influencing the price and suffering from a large slippage.
Details: The user specifies a certain price threshold and the script will sell as much as possible at or above that threshold.
Use cases: efficiently liquidate tokens at the highest price possible.

Account(s): main account only.

| Parameters | Description | Type |
|------------|-------------|------|
| base | Market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT|
| quote | Market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT|
| minPrice | The price threshold: the script will actively sell only at a price higher than this threshold | `float` for price in quote currency or `str` in the format of `{price}-{symbol}` for price in a specific currency, e.g. `0.00000135-btc` to set the price to 0.00000135 in BTC |
| threshold | The volume threshold: the script will actively fill an order that has an amount in base currency that is higher than this threshold only | `float` |
| leftOver | In order to not impact the price, the will always fill part of an order and leave a leftover amount, in base currency | `float` |
| askVolume | When the price is higher than the price threshold, the script will always maintain an ask order at the bottom of the sell side of the orderbook. This parameter specifies the average amount of the maintained ask order in base currency | `float` |
| randomness | Desired degree of randomness of the order amount around askVolume | `float`, between 0 and 1: 0 is no randomess; 1 is maximum randomness |
| baseName (Optional) | Full name of the base currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| quoteName (Optional) | Full name of the quote currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| cutlossBase (Optional) | The cutloss threshold for base currency: if the total base currency balance is lower than the threshold, the script will pause | `float` |
| cutlossQuote (Optional) | The cutloss threshold for quote currency: if the total quote currency balance is lower than the threshold, the script will pause | `float` |

### Scheduled Orders

Script class symbol: `schedule`

Purpose: `schedule` recursive orders over time, spaced out by a given time interval.
Details: Every time it places a buy order, it will use the account that has more quote currency; everytime it places a sell order, it will use the account that has less quote currency.
Use cases: customize a trading strategy without any coding background.

Account(s): main account, secondary account is optional.

| Parameters | Description | Type |
|------------|-------------|------|
| base | Market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT|
| quote | Market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT|
| interval | The number of seconds between each time the order(s) is/are placed | `float` |
| randomness | Desired degree of randomness of the order amount for each order | `float`, between 0 and 1: 0 is no randomess; 1 is maximum randomness |
| buyAmount (Optional) | The order amount of the buy orders to be placed | `float` |
| buyPrice (Optional) | The price of the buy orders to be placed  | `float` tp place the orders at a set price, `expression` to place the orders according to the market conditions |
| sellAmount (Optional) | The order amount of the sell orders to be placed | `float` |
| sellPrice (Optional) | The price of the sell orders to be placed | `float` to place the orders at a set price, `expression` to place the order according to the market conditions |
| async (Optional) | In order to place orders simultaneously | `bool` |
| makerOnly (Optional) | In order to prevent the orders placed to fill existing external orders | `bool` |
| baseName (Optional) | Full name of the base currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| quoteName (Optional) | Full name of the quote currency. Must specify if multiple currencies listed as the same symbol on CoinGecko. Case sensitive | `str` |
| cutlossBase (Optional) | The cutloss threshold for base currency: if the total base currency balance is lower than the threshold, the script will pause | `float` |
| cutlossQuote (Optional) | The cutloss threshold for quote currency: if the total quote currency balance is lower than the threshold, the script will pause | `float` |

Note: at least one pair (`buyAmount`, `buyPrice`) and/or (`sellAmount`, `sellPrice`) has to be specified.

## HTClient Components

### Accounts

Accounts are the exchanges accounts. 
An account is defined by: the exchange it is on and its API keys.

The exchanges currently supported are: [binance](https://www.binance.com/), [huobi](https://www.huobi.com/), [kucoin](https://www.kucoin.com/), [okex](https://www.okex.com/), [bitfinex](https://www.bitfinex.com/), [bithumb](https://www.bithumb.pro/), [digifinex](https://www.digifinex.com/), [liquid](https://www.liquid.com/), [hitbtc](https://hitbtc.com/), [bittrex](https://bittrex.com/), [bitrue](https://www.bitrue.com/), [cointiger](https://www.cointiger.com/), [latoken](https://latoken.com/), [lbank](https://www.lbank.info/), [idex](https://idex.market/), [coinall](https://www.coinall.com/), [dragonex](https://dragonex.io/), [bitforex](https://www.bitforex.com/), [bw](https://www.bw.com/), [bispex](https://bispex.com/), [coineal](https://www.coineal.com/), [smartvalor](https://smartvalor.com/), [vitex](https://vitex.net/), [stex](https://www.stex.com/).

### Scripts

Scripts are the algorithms that run on our servers. 
A script is defined by: script class, parameters and account(s) it uses.

### Classes

A script class is the strategy that a script uses. There are public script classes that are available for all users, and there are also customized script classes that are defined by each individual user. 
The public script classes are: [maker]("#maker"), [stable]("#stable"), [replication]("#replication"), [enter]("#enter"), [exit]("#exit") and [schedule]("#schedule").

## HTClient User Interface

### Accounts Page

The account page is used to add new accounts, manage the existing accounts, and balances.

### Scripts Page

The scripts page is used to add new scripts, manage and monitor existing scripts, clear orders and storage, and check the server logs of a script. For some script classes, the recommended funding can be checked based on the parameters it uses. 

### Classes Page

The classes page is used to [create and manage customized script classes]("#customized-scripts").

### Monitor Page

The monitor page displays all the scripts that may require attention on one screen.

In each script monitor, `Monitor Metrics` is displayed on the left and `Server Logs` is displayed on the right. 
The `Monitor Metrics` field displays some metrics of the script, account balances in the format: available/total. If the script monitor of a script is displayed on the monitor page, it shows that some of the script metrics are different from the config parameters settings. If that happens, check the `Server Logs` field to see if there is any error.

If there is no error in logs, the script will normally correct itself and maintain the right metrics. 
If there are errors in logs, some action will have to be taken according to the error. For example, if the error says insufficient balance, you will have to refill the account accordingly. 
In case the error returned by the exchange is not clear, i.e. the exchange returns an error followed by a number, please refer to the error section of the API documentation of that exchange for more information on the error encountered. A general error encountered happens when an exchange performs maintenance: in that case the error will have an HTML format (involving characters such as <>), at that time the script is not running and no new order is being placed.

### Search Bars

Several terms separated by commas can be entered when searching. Spaces are ignored.
If all these terms are contained in an account name or script name, the appropriate results will be shown; otherwise nothing will be displayed.
When using the search bar within the monitor page, the results will be displayed among the items present on the last update of the monitor. Searching something will not update the monitor.

## Customized Scripts

Coding background is required passed this point.

Customized scripts allow to define your own script class and run it on our backend, thus benefiting from our centralized script management, exchange integration and servers stability and scalability.

### Components

To define a customized script class, the following components need to be defined: Meta, Code Block and Monitor Metrics.

#### Meta

Meta is a `json` object that defines the required and optional config parameters and accounts. It has to following attributes:

| Attributes | Description |
|------------|-------------|
| paramRequired | An `array` of required config parameter names |
| paramOptional | An `array` of optional config parameter names |
| accRequired | An `array` of required account types |
| accOptional | An `array` of optional account types |

In addition to what is defined in Meta, there will be two public config parameters that apply to all customized script classes:

| Parameters | Type | Description |
|------------|------|-------------|
| _sleep | `float` | The number of seconds between each iteration of the script |
| _clear | `string` | The pair symbol of the orders that will be cleared if the "Clear Orders" button is used on the script page |

Note: if the account type `main` is defined, the exchange logo of that account will be displayed within the script on the monitor page.

#### Code Block

Code block is the code that defines what the script will do within each iteration. It follows [python3.6](https://www.python.org) runtime, and it supports the following libraries: [requests](https://pypi.org/project/requests/), [websocket_client](https://pypi.org/project/websocket_client/), [numpy](https://pypi.org/project/numpy/), [scipy](https://pypi.org/project/scipy/), [pandas](https://pypi.org/project/pandas/) and [Crypto](https://pypi.org/project/pycryptodome/). In addition [predefined functions and objects]("#predefined-functions-and-objects") can be used, and it is possible to interact with the server storage, server logs and the exchanges APIs.

#### Monitor Metrics

Monitor metrics determine what will be displayed within the script monitor on the script page and monitor page. As many monitor metrics as needed can be defined. Each monitor metric has the following three attributes:

| Attributes | Description |
|------------|-------------|
| Name | The name of the monitor metric |
| Value Expression | A [python3.6](https://www.python.org) expression that returns the value of the monitor metric. [predefined functions and objects]("#predefined-functions-and-objects") can be used. |
| Range Expression | A [python3.6](https://www.python.org) expression that returns a `bool` type value, in which the name `value` returns the value of the metric that is defined by the value expression. If the value of the expression is `True`, it indicates that this monitor metric is within its normal range. [predefined functions and objects]("#predefined-functions-and-objects") can be used. In the monitor page, if the range expression returns `False` for one or more monitor metric(s), it indicates that the script may require attention, and it will appear on the monitor screen. |

### Predefined Functions and Objects

The following objects and functions are predefined, they can be used in the code block and the expressions of the monitor metrics. Warning: these names shall not be overwritten when working on the code block.

#### Get the config parameter value

```python
get(key)
```

| Argument | description | type |
|-------------------|-------------|------|
| key | the name of the config parameter | `str` |

It returns the value of the config parameter. If the config parameter is not defined, it returns `None`.

#### Interact with the script storage

> Each script has a designated storage that can store key/value pairs. The following functions are used to interact with them.

```python
read(key)
```

| Argument | description | type |
|-------------------|-------------|------|
| key | the key in the storage | `str` |

It returns the value associated with the key in the storage. If the key is not defined, it returns `None`.

```python
write(key, value)
```

| Arguments | description | type |
|-------------------|-------------|------|
| key | the key of the key/value pair | `str` |
| value | the value of the key/value pair | `str`, `float`, `int`, `bool`, `None` |

This function writes a key/value pair in the storage.

#### Output to logs

> Use the following function to output a message to logs instead of the print statement:

```python
log(message)
```

| Argument | description | type |
|-------------------|-------------|------|
| message | the message to be outputed to logs | `str` |

#### Check CoinGecko price

```python
market_price(symbol, fullName=None)
```

| Argument | description | type |
|-------------------|-------------|------|
| symbol | symbol of the currency on CoinGecko (lowercase) | `str` |
| fullName (Optional) | Full name of the currency, must specify if multiple currencies listed as the same symbol on CoinGecko, case sensitive | `str` |

It returns the reported price of the token/coin in USD.

#### Get Exchange API Client

```python
HT(accType)
```

| Argument | description | type |
|-------------------|-------------|------|
| accType | The account type of the account that is usded by the script | `str` |

It returns a `Client` object that can be used to interact with the exchange API.

#### The Exchange API Client

The `Client` object represents an account that is used by the script. It is used to interact with the exchange API. Note that the proper way to get an instance of this class is to call the `HT` function. The following are the methods defined:

##### Get the orderbook

```python
Client.orderbook(self, base, quote)
```

| Arguments | description | type |
|-------------------|-------------|------|
| base | the symbol of the base currency of the trading pair (lowercase) | `str` |
| quote | the symbol of the quote currency of the trading pair (lowercase) | `str` |

It returns a nested `dict`, i.e.

``` python
{
	'asks': [
		{
			'price': 9186,
			'volume': 0.32
		},
		...
	],
	'bids': [
		{
			'price': 9185,
			'volume': 1.05
		},
		...
	]
}
```

##### Get the rolling 24 hours volume in USD

```python
Client.volume24(self, baes, quote)
```

| Arguments | description | type |
|-------------------|-------------|------|
| base | the symbol of the base currency of the trading pair (lowercase) | `str` |
| quote | the symbol of the quote currency of the trading pair (lowercase) | `str` |

It returns the rolling 24 hours volume in USD as a `float`.

##### Get the last traded price

```python
Client.last(self, base, quote)
```

| Arguments | description | type |
|-------------------|-------------|------|
| base | the symbol of the base currency of the trading pair (lowercase) | `str` |
| quote | the symbol of the quote currency of the trading pair (lowercase) | `str` |

It returns the last traded price as a `float`.

##### Get the pair meta

```python
Client.meta(self, base, quote)
```

| Arguments | description | type |
|-------------------|-------------|------|
| base | the symbol of the base currency of the trading pair (lowercase) | `str` |
| quote | the symbol of the quote currency of the trading pair (lowercase) | `str` |

It returns a `dict` that is defined as follows:

| keys | description | type |
|-------------------|-------------|------|
| "priceDecimal" | The price precision | `int` |
| "volumeDecimal" | The volume precision | `int` |
| "minPrice" | The minimum price allowed to place order | `float` |
| "maxPrice" | The maximum price allowed to place order | `float` |
| "minVolume" | The minimum volume allowed to place order | `float` |
| "maxVolume" | The maximum volume allowed to place order | `float` |

##### Check balance of the account

```python
Client.balance(self, *args)
```

| Argument(s) | description | type |
|-------------------|-------------|------|
| *args | the symbol of the currencies (lowercase) | `str` |

It returns a nested `dict`, i.e.:

```python
{
	'btc': {
		'available': 3.5,
		'total': 3.7
	},
	...
}
```

##### Place an order

```python
Client.place_order(self, base, quote, side, volume, price)
```

| Arguments | description | type |
|-------------------|-------------|------|
| base | the symbol of the base currency of the trading pair (lowercase) | `str` |
| quote | the symbol of the quote currency of the trading pair (lowercase) | `str` |
| side | trading side | `'buy'`/`'sell'` |
| volume | the volume of the order in base currency | `float` |
| price | the price of the order | `float` |

It returns `{'success': True}` if the order goes through; it raises an `Exception` with the exchange error message if it does not go through.

##### Get all open orders

```python
Client.get_orders(self, base, quote)
```

| Arguments | description | type |
|-------------------|-------------|------|
| base | the symbol of the base currency of the trading pair (lowercase) | `str` |
| quote | the symbol of the quote currency of the trading pair (lowercase) | `str` |

It returns a nested `dict`, i.e.:

```python
{
	'asks': [
		{
			'symbol': 'btc_usdt',
			'_type': 'ask',
			'price': 9186,
			'volume': 0.58,
			'order_id': 49830927
		},
		...
	],
	'bids': []
}
```

##### Cancel an order

```python
Client.cancel(self, order)
```

The argument `order` is a `dict` that has the same format as the return of the `get_orders` function, `order` can be referred to the element of the `'asks'` or `'bids'` field from the return of the `get_orders` function, i.e.:

```python
{
	'symbol': 'btc_usdt',
	'_type': 'ask',
	'price': 9186,
	'volume': 0.58,
	'order_id': 49830927
}
```

It returns `{'success': True}` if the order is cancelled successfully; it raises an `Exception` with the exchange error message if cancelling does not go through.

##### Cancel all orders for a certain pair

```python
Client.cancel_all(self, base, quote)
```

| Arguments | description | type |
|-------------------|-------------|------|
| base | the symbol of the base currency of the trading pair (lowercase) | `str` |
| quote | the symbol of the quote currency of the trading pair (lowercase) | `str` |

It returns `{'success': True}` if all orders are cancelled successfully; it raises an `Exception` with the exchange error message if one or more order(s) did not get cancelled.