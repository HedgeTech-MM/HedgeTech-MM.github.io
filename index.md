# HTClient Documentation

## Introduction

HTClient is a suite of powerful algorithms designed by our experienced algorithmic traders. 
It can be operated from a simple User Interface in order to perform various complex tasks such as market making and many more. HTClient is particularly suitable for token issuers, exchanges, broker/dealers and market makers. 
Token issuers can operate the systems within their company, on all exchanges they are listed on. 
Exchanges can operate the systems in-house for their main pairs as well as all other altcoins listed on their platform. 
Broker/dealers can add to the services they offer with an all-in-one liquidity provision system. 
Market makers can chose to either use the algorithms designed by HedgeTech or to design their own custom scripts, benefiting from our integration capabilities and server infrastructure.
The scripts are suitable for any market environment: major coins, altcoins, security tokens, perpetual contracts and all other digital assets.

The documentation helps you navigate through the infrastructure of the HTClient User Interface (UI). We recommend to read this document while having the HTClient UI opened. 
There are two key components of HTClient: Accounts and Scripts. Accounts refer to your exchange accounts, Scripts refers the algorithms (or strategies) you run through the Software as a Service (SaaS). 
Please make sure to follow accurately the syntax (such as upper and lower cases) and instructions as listed below:

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

## Windows Installation Note

After downloading HTClient at www.hedgetech.io, the first time you run HTClient.exe, a message might show up saying that Windows Defender Smartscreen has protected your PC. If that is the case, please click `More info` and if the developer shows `HedgeTech LLC`, you can safely select `Run Anyway`. The next time you open HTClient.exe, you do not have to take this extra step.

## IPs

For security reasons, our servers use non-static IPs. In order to use the SaaS, make sure to whitelist all IPs by not filling any IP under the UI whitelist field on your exchange accounts. If the exchanges do not provide this feature on their UI, please get in touch with them directly.

## Strategy Catalog

#### Script Class: Maker Strategy

symbol: `maker`

The `maker` script class maintains a number of orders on both the buy and sell sides for a currency pair. The `maker` script class lets the price behave according to the supply and demand in the market and adjusts the orders maintained automatically: when people sell, the price will move down; when people buy, the price will move up. Thus, the `maker` script class provides liquidity to the market, it narrows the market spread and it makes profits from the spread.

This script class only uses main account.

| Parameters | Description | Type |
|-------------------|-------------|------|
| base | Market symbol of the base currency | `str`, e.g. `eth` for the market TokenName/ETH|
| quote | Market symbol of the quote currency | `str`, e.g. `usdt` for the market TokenName/ETH|
| baseName (Optional)| Full name of the base currency, shall be specified if multiple currencies listed as the same symbol on CoinGecko, case sensitive | `str` |
| quoteName (Optional)| Full name of the quote currency, shall be specified if multiple currencies listed as the same symbol on CoinGecko, case sensitive | `str` |
| levels | Number of orders maintained on each side of the order book | `int` |
| aveVolume | Average order volume (or amount) in quote currency to be placed on each order | `float` |
| randomness | The desired degree of randomness for the volume (or amount) of your orders | `float`, between 0 and 1, 0 being no randomess and 1 being maximum randomness |
| spread | The distance between the first ask order and the first bid order maintained. It is recommended to set a spread of at least 3 x density and that (spread - density) exceeds 2 x trading fees. | `float`, percentage, e.g. `0.01` means 1% |
| density | The distance between the orders placed within the same side of the order book. It is recommended to set a spread of at least 3 x density and that (spread - density) exceeds 2 x trading fees. | `float`, percentage, e.g. `0.002` means 0.2% |
| cutlossBase (Optional)| The cutloss threshold for base currency: if the total balance for the base currency is lower than the threshold, the script will not run | `float` |
| cutlossQuote (Optional)| The cutloss threshold for quote currency: if the total balance for the quote currency is lower than the threshold, the script will not run | `float` |

Note: after putting maker online, if you need to change the following parameters: levels, aveVolume, randomness, spread, density, you will need to first turn the script offline, clear storage, clear orders and restart with the desired parameters.

#### Script Class: Stable Strategy

symbol: `stable`

The `stable` script class maintains a number of orders on both the buy and sell sides for a currency pair around a given price. Such a price is defined by the base and quote values. You can define the base value and/or the quote value to be a set value, or use the market price that is reported by CoinGecko: an external price reporting platform. Thus, the `stable` script class provides liquidity to the market and narrows the market spread, while matching the price to a desired index.
Warning: this script may fill orders to match the desired price index and this behavior can be exploited if not set-up carefully (i.e. by inputing cutlossBase and cutlossQuote)

This script class only uses main account.

| Config Parameters | description | type |
|-------------------|-------------|------|
| base | market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT|
| quote | market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT|
| baseName | (Optional) Full name of the base currency, specify if multiple currencies listed as the same symbol, case sensitive | `str` |
| quoteName | (Optional) Full name of the quote currency, specify if multiple currencies listed as the same symbol, case sensitive | `str` |
| baseValue | The USD value of the base currency | Pass `int` or `float` if you want to define a stable value to it; pass `market` if you want it to be automatically adjusted to the price reported by the data reporting platforms |
| quoteValue | The USD value of the quote currency | Pass `int` or `float` if you want to define a stable value to it; pass `market` if you want it to be automatically adjusted to the price reported by the data reporting platforms |
| askNum | Number of ask orders you want to maintain at all times | `int` |
| bidNum | Number of bid orders you want to maintain at all times | `int` |
| aveVolume | Average order volume in quote currency to be placed on each order | `float` |
| randomness | The degree of randomness you want for the volume of your orders | `float`, between 0 and 1, 0 being no randomess and 1 being maximum randomness |
| spread | The distance between the first ask order and the first bid order you want to place | `float`, percentage, e.g. `0.01` means 1% |
| density | The distance between the orders you place within the same side of the order book | `float`, percentage, e.g. `0.002` means 0.2% |
| cutlossBase | (Optional) The cutloss threshold for base currency: if the total balance for the base currency is lower than the threshold, the script will not run | `float` |
| cutlossQuote | (Optional) The cutloss threshold for quote currency: if the total balance for the quote currency is lower than the threshold, the script will not run | `float` |

#### Script Class: Orderbook Replication

symbol: `replication`

The orderbook replication strategy replicates the liquidity from an exchange with larger liquidity to an exchange with smaller liquidity, in order to provide liquidity on the smaller exchange. Whenever the orders maintained on the smaller exchange are filled, it will hedge the position by placing market orders and take the liquidity from the larger exchange. This strategy makes profits from strictly hedging the risk.

This script class uses both a main account and a secondary account. The main account is the account on the exchange on which you want to add liquidity (the one with smaller liquidity), the secondary account is the account on the exchange on which you want to take liquidity (the one with larger liquidity).

This is a relatively advanced script class, feel free to consult our tech team before implementing it.

| Config Parameters | description | type |
|-------------------|-------------|------|
| base | market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT |
| quote | market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT |
| baseName | (Optional) Full name of the base currency, specify if multiple currencies listed as the same symbol, case sensitive | `str` |
| quoteName | (Optional) Full name of the quote currency, specify if multiple currencies listed as the same symbol, case sensitive | `str` |
| divergence | The price distance between the orders you place on the smaller exchange and the existing orders on the larger exchange. The larger it is, the higher the maintained spread and the lower the maintained order density will be on the smaller exchange. However it has to be large enough to cover the trading fees on both of the exchanges combined and the transaction fee when you send balance from one exchange to the other. Divergence has to be greater than the maximum between (2 x base price unit) and (2 x trading fees + withdrawal fee) | `float`, percentage, e.g. `0.01` means 1% |
| levels | Number of orders maintained on each side on the smaller exchange | `int` |
| percentage | The percentage of liquidity you want to replicate from the larger exchange to the smaller one | `float`, percentage, e.g. `0.01` means 1%. In order to hedge the position effectively, we recommend limiting this parameter to a maximum of 0.2 |
| volumeCap | A hard maximum on the each order volume on the smaller exchange, in quote currency | `float` |
| randomness | When the order volume exceeds the `volumeCap` for a given order, instead of placing an order exactly at the `volumeCap`, it will place an order around `volumeCap` with a degree of randomness. | `float`, between 0 and 1, 0 being no randomess and 1 being maximum randomness |
| cutlossBase | (Optional) The cutloss threshold for base currency: if the total balance of the two accounts combined for the base currency is lower than the threshold, the script will not run | `float` |
| cutlossQuote | (Optional) The cutloss threshold for quote currency: if the total balance of the two accounts combined for the quote currency is lower than the threshold, the script will not run | `float` |

#### Script Class: Enter Strategy

symbol: `enter`

The `enter` strategy is designed to purchase a large amount of tokens without influencing the price and suffer a large slippage on the secondary market. The user specifies a certain price threshold and the script will puchase as much as possible at or under that threshold. This strategy can be used to build a price support.

This script class only uses main account.

| Config Parameters | description | type |
|-------------------|-------------|------|
| base | market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT|
| quote | market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT|
| baseName | (Optional) Full name of the base currency, specify if multiple currencies listed as the same symbol, case sensitive | `str` |
| quoteName | (Optional) Full name of the quote currency, specify if multiple currencies listed as the same symbol, case sensitive | `str` |
| maxPrice | The price threshold: the script will only puchase at a price lower than this threshold | Put `float` for price in quote currency, or put `str` in the format of `{price}-{symbol}` for price in a specific currency, e.g. `0.00000135-btc` to set the price to 0.00000135 in BTC |
| threshold | The volume threshold: the script will only actively fill an order that has a volume in base currency that is higher than this threshold | `float` |
| leftOver | In order to not impact the price, when the script fills an order, it will always fill part of that order and leave a leftover volume in base currency | `float` |
| bidVolume | When the price is lower than the price threshold, the script will always maintain a bid order on top of the bid side of the orderbook. This parameter specifies the average volume in base currency of the maintained bid order | `float` |
| randomness | The degree of randomness you want for the volume of the maintained bid order | `float`, between 0 and 1, 0 being no randomess and 1 being maximum randomness |
| cutlossBase | (Optional) The cutloss threshold for base currency: if the total balance for the base currency is lower than the threshold, the script will not run | `float` |
| cutlossQuote | (Optional) The cutloss threshold for quote currency: if the total balance for the quote currency is lower than the threshold, the script will not run | `float` |

#### Script Class: Exit Strategy

symbol: `exit`

The `exit` strategy is designed to sell a large amount of tokens without influencing the price and suffer a large slippage on the secondary market. The user specifies a certain price threshold and the script will try to sell as much as possible above that threshold.

This script class only uses main account.

| Config Parameters | description | type |
|-------------------|-------------|------|
| base | market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT|
| quote | market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT|
| baseName | (Optional) Full name of the base currency, specify if multiple currencies listed as the same symbol, case sensitive | `str` |
| quoteName | (Optional) Full name of the quote currency, specify if multiple currencies listed as the same symbol, case sensitive | `str` |
| minPrice | The price threshold: the script will only sell at a price higher than this threshold | Put `float` for price in quote currency, or put `str` in the format of `{price}-{symbol}` for price in a specific currency, e.g. `0.00000135-btc` to set the price to 0.00000135 in BTC |
| threshold | The volume threshold: the script will only actively fill an order that has a volume in base currency that is higher than this threshold | `float` |
| leftOver | In order to not impact the price, when the script fills an order, it will always fill part of that order and leave a leftover volume in base currency | `float` |
| askVolume | When the price is higher than the price threshold, the script will always maintain an ask order at the bottom of the sell side of the orderbook. This parameter specifies the average volume of the maintained ask order in base currency | `float` |
| randomness | The degree of randomness you want for the volume of the maintained ask order | `float`, between 0 and 1, 0 being no randomess and 1 being maximum randomness |
| cutlossBase | (Optional) The cutloss threshold for base currency: if the total balance for the base currency is lower than the threshold, the script will not run | `float` |
| cutlossQuote | (Optional) The cutloss threshold for quote currency: if the total balance for the quote currency is lower than the threshold, the script will not run | `float` |

#### Script Class: Scheduled Orders

symbol: `schedule`

You can schedule recursive orders over time, with a given time interval, using the `schedule` script class. You can use up to two accounts. Every time it places a buy order, it will use the account that has more quote currency, everytime it places a sell order, it will use the account that has less quote currency.

This script class uses main account; using secondary account is optional.

| Config Parameters | description | type |
|-------------------|-------------|------|
| base | market symbol of the base currency | `str`, e.g. `eth` for the market ETH/USDT|
| quote | market symbol of the quote currency | `str`, e.g. `usdt` for the market ETH/USDT|
| baseName | (Optional) Full name of the base currency, specify if multiple currencies listed as the same symbol, case sensitive | `str` |
| quoteName | (Optional) Full name of the quote currency, specify if multiple currencies listed as the same symbol, case sensitive | `str` |
| interval | The number of seconds between each time the order(s) is/are placed | `float` |
| buyAmount | The amount of the buy orders to be placed (Optional) | `float` |
| buyPrice | The price of the buy orders to be placed (Optional) | pass `float` if you want place the orders at a set price; pass `expression` if you want to place the order according to the market conditions |
| sellAmount | The amount of the sell orders to be placed (Optional) | `float` |
| sellPrice | The price of the sel orders to be placed (Optional) | pass `float` if you want place the orders at a set price; pass `expression` if you want to place the order according to the market conditions |
| randomness | The degree of randomness you want for the volume of your orders | `float`, between 0 and 1, 0 is being no randomess and 1 being maximum randomness |
| async | Whether you want to place orders asynchronously (Optional) | `bool` |
| makerOnly | Whether you want to prevent your order to fill existing orders | `bool` |
| cutlossBase | (Optional) The cutloss threshold for base currency: if the total balance for the base currency is lower than the threshold, the script will not run | `float` |
| cutlossQuote | (Optional) The cutloss threshold for quote currency: if the total balance for the quote currency is lower than the threshold, the script will not run | `float` |

You have to pass at least one pair of: (`buyAmount`, `buyPrice`) and/or (`sellAmount`, `sellPrice`)

## HTclient UI

HTclient is a desktop app that you can use to manage your accounts and scripts. In this section we will give you instructions regarding each page of the app.

#### Login Page

After your initial subscription, you will be given a `username`, `password` and `secretkey` that you will use to login to your SaaS system. Please make sure that your `password` and `secretkey` are secure. When you launch the app, you will see the login page. If you fill the fields of `username`, `password` and `secretkey`, and hit the `Login` button, you will be logged in. If you do not want to input your `username`, `password` and `secretkey` every time,  you can alternatively set up the environment variables on your local device. It is strongly advised against doing so on a computer that is shared or not secured. Set the environment variable `HTMM_USERNAME` to your `username`, the environment variable `HTMM_PASSWORD` to your `password` and the environment variable `HTMM_SECRET` to your `secretkey`. After the set-up, every time you login, you will not have to input anything, you can just hit the `Envlogin` button to login.

#### Main Menu Page

After you login, you will see the main menu page, from where you can navigate to account search page, script search page, custom script classes page and monitor page.

#### Console

Within some pages, you will see a blue console, where you will see the log messages of the app: it prints the change you made to your accounts and scripts and if it runs into an error, it will print the error.

#### Account Page

Displayed after you navigate to account page from the main menu page. By default, you will see all of your accounts. If you input a search word or string in the search bar and hit the `Search` button, you will filter out all accounts whose names do not contain the search word or string. You can click on an account to navigate to that account detail page, you can click on the `Add Account` button to go to the add account page, or you can click the `back` button to go back to the main menu.

#### Add Account Page

The add account page is where you register your exchange account on the SaaS infrastructure. You input the account name, exchange and account auth on this page. By default, you can input one key for account auth. If the exchange requires multiple auth keys you can click the `Add Auth Key` button to add additional key and value input boxes to input more auth keys. Once you input all the information, click confirm to add the account. If it was a success, it will navigate you to the account detail page. To check the name of the keys required for a given exchange, fill in the exchange field and click on `Check Auth`. The output of the required keys will be shown in the console window.

#### Account Detail Page

The account detail page will display all the account information for a selected account. You can also modify your account information and check account balance here. If you want to modify a piece of information, input the new value in the input box on the right of the information that you want to modify and then click the `Modify` button on the right of the input box. If you want to add additional auth keys, fill the key and value in the New Auth Key field and click `Add` button. If you want to check balance, input the currency you want to check in the search bar and click the `Search` button. The balance will be printed on the console. If you want to check balances for multiple currencies, input multiple currencies and use `,` to seperate them. Click the red `Delete` button if you want to delete this account.

#### Script Page

Displayed after you navigate to script page from the main menu page. By default, you will see all of your scripts. You can input a search word or string in the search bar and hit the search button, you will filter out all scripts whose names do not contain the word or search string. You can click on the script to navigate to the script detail page, you can click on the `Add Script` button to go to the add script page, or you can click the `back` button to go back to the main menu.

#### Add Script Page

The add script page is where you add a script. You input the script name, script class, the account(s) the script will be using and the config parameters on this page. By default, you can input one config parameter. If the script class requires multiple parameters you can click the `Add Parameter` button to add additional parameters input boxes to input more parameters. Once you input all the information, click confirm to add the script. If it was a success, it will navigate you to the script detail page. The newly added script is offline, you can turn it online on the script detail page. The output of the recommended inventory will be shown in the console window.

#### Script Detail Page

The script detail page will display all the script information for a selected script. To check the inventory required for the parameters inputed, click on `Check Inventory` (for the following script classes: `maker`, `stable` and `replication` . You can do so before you turn it online. You can also modify your script information, script monitor and script logs, as well as turning the script online/offline, clearing all related orders and navigate to the account detail page of the account(s) the script is using. If you want to modify a piece of information, input the new value in the input box on the right of the information that you want to modify and then click the `Modify` button on the right of the input box. If you want to add additional parameters, fill the parameter and value in the New Parameter field and click `Add` button. Click the `Turn Online`/`Turn Offline` button to turn the script online/offline. When a script is online, it runs on our servers even if your HTclient UI is closed. Click the green account button to navigate to the account detail page. Click the red `Clear Orders` button to cancel all orders placed by this script. Sometimes if you have too many orders and the exchange does not have a cancel all orders endpoint, it might return a `timeout` error after a 30 seconds delay. If that happens, the cancelling is still in progress, you just need to wait for a couple minutes and click the `Clear Orders` button again until it returns success. Click the red `Delete` button if you want to delete this script.

##### Script Monitoring

In the script detail page, there are a `Monitor` field and a `Logs` field. The `Monitor` field displays some metrics of the script, account balances in the format: available/total and if the script needs attention. If the require attention output shows `True`, it shows that some of the script metrics are different from the config parameters settings. If that happens, check the `Logs` field to see if there is any error. If there is no error in logs, the script will normally correct itself and maintain the right metrics. If you see errors in logs, you will have to do something according to the error. For example, if the error says insufficient balance, you will have to go to the account detail page, check the balance and refill the account accordingly. 
In case the error returned by the exchange is not clear, i.e. the exchange returns an error followed by a number, please refer to the error section of the API documentation of that exchange for more information on the error encountered. A general error encountered happens when an exchange performs maintenance: in that case the error will have an HTML format (involving characters such as <>), at that time the script is not running and no new order is being placed.

#### Custom Script Classes Page

Please refer to the separate documentation for custom script classes.

#### Monitor Page

The monitor page displays all the scripts that potentially require attention on a single window, allowing you to monitor at one glance. Please refer to the script monitoring section above for more information about monitoring.

## Accounts

To register an account on the SaaS infrastructure, you have to define the following attributes:

#### Account Name

Account name is the name you give to your exchange account for reference. For example, if you have an account on the exchange Huobi and you want to use this account to run a script on the pair TokenName/ETH, you could name this account: `huobi-tokenname-eth`

#### Account Exchange

Account exchange is the exchange your account is on. The exchanges that are currently supported are: `binance`, `huobi`, `kucoin`, `okex`, `bitfinex`, `bithumb`, `digifinex`, `liquid`, `hitbtc`, `bittrex`, `bitrue`, `cointiger`, `latoken`, `lbank`, `idex`, `coinall`, `dragonex`, `bitforex`, `bw`, `bispex`, `coineal`, `smartvalor`, `vitex`, `stex`. If you would like to use HTclient on an exchange that is not currently supported, please send a request via the user contact form on the website, specifying "new exchange integration". For current users, we will integrate new exchanges within 24 hours. We may contact you with additional requirements.

#### Account Authentication

In order to run the HTClient strategies on your exchange accounts, you need to authenticate your accounts via the parameters that the exchanges use to verify accounts, known as API keys. Different exchanges have different keys. For example for `huobi`, the two auth keys needed are `publickey` and `secretkey`.
Important: for your own security, please make sure to manage permissions if the exchange offers this feature. You should enable read/write permissions for everything except withdrawal/deposit.

## Scripts

To create a script, you need to define the following attributes:

#### Script Name

Script name is the name you give to the script you want to run for reference. For example, if you want to run the stable script on the pair TokenName/ETH on Huobi, you could name this script: `huobi-stable-tokenname-eth`

#### Script Class

Script class is the type of script, each class representing an algorithm (or a strategy). Currently the script classes that are currently publicly supported are: `maker`, `stable`, `replication`, `enter`, `exit`, `schedule`, `arbitrage`. 

#### Script Status

The status indicates if the script is online or offline. If it shows offline, it means that the script has been added but is not currently running. If it shows online, it means that the script is currently running (closing the UI does not stop the online script from running, it is running on the cloud server). For each SaaS user, their subscription has a limit to the maximum number of scripts that can be put online at the same time. If you need to run more scripts than your current limit, please upgrade your subscription via the settings page or the website.

#### Main Account and Secondary Account

Main account and secondary account are the accounts used to run a given script. To attach an account to a script, the account has to be registered on the SaaS infrastructure, it is referenced by the account name you have given. Some script classes run with only one account while others have the option to run with two accounts.

#### Parameters

Parameters define the behavior of a script. Different script classes use different parameters.