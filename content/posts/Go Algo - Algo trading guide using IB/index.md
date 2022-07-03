---
title: "Go Algo - Algo trading guide using IB"
date: 2021-10-01T07:57:25+06:00
description: Algo Trading Guide Part I
menu:
  sidebar:
    name: Go Algo - Algo trading guide using IB
    identifier: Go Algo - Algo trading guide using IB
    weight: 10
tags: ["Basic", "Multi-lingual"]
categories: ["Basic"]
---

Hey readers, I am Prajual, currently working at a high-frequency trading firm (better known as HFT’s) and prior to this, I have also worked in a hedge fund. I am starting this series, Go Algo to share whatever I know to help you get started with Algo trading.

Here I am using Interactive brokers Python API. But the basic idea remains the same across brokers and geographies. You will easily be able to modify these functions and use them for your respective brokers.

This series assumes that you already have a brokerage account with API services. If you don’t have check this out for IB API. This series is strictly for educational purposes and doesn’t endorse any broker.

Two important imports to be used for the complete series (To know more about ib_insync, check this out):

	from ib_insync import IB, Stock, Option
	from ib_insync import util

Part I: Establish connection to the broker

To get started with algo-trading, the very first step always involves connecting to your broker through API. Once connected, we can use all the in-built API functions to perform our trading operations. So, let’s connect using the following code:

	class ConnectAPI(): def __init__(self, *args, **kwargs):
   		self.host = # Your host
   		self.port = port  ##{'Pseudo testing':7496, 'Live trading': 7497}
   		self.client_id = # Your client id   self.__ib = None
   		super().__init__(*args, **kwargs) def connect_to_ib(self):
   		self.ib.connect(self.host, self.port, clientId=self.client_id)
 
 
	@property
	def ib(self):
		if not self.__ib:
		self.__ib = IB()
		return self.__ib

Here I have created a ConnectAPI class with basic implementation to connect to IB API.

Further you will need to create an object of this class and call the corresponding function as:

	if __name__ == ‘__main__’:
    		API = ConnectAPI()
    		API.connect_to_ib()

Now we are connected to IB API and can easily steer our trading experience algorithmically.

Part II: Fetch some historical data

Let’s start with a very important aspect of algo-trading, fetching historical data to develop our strategies. This simple piece of code, can be used for the above purpose:

	symbol = ‘SPX’         ## Collect data for SPX index
	contract = Index(symbol, exchange = ‘CBOE’,currency = ‘USD’)
	bars = self.ib.reqHistoricalData(
	contract, endDateTime=””, durationStr = “5 Y”,
	barSizeSetting = “1 day”, whatToShow=’TRADES’, useRTH=True)
	self.ib.sleep(25)
	data = util.df(bars)

If we want to fetch the data for some equity, we should create a contract as (rest remains the same):

	contract = Stock(symbol, exchange = ‘SMART’,currency = ‘USD’)

Here using endDateTime = “”, implies data till the current candle, durationStr specifies the whole time period for data and barSizeSetting gives the size for a candlestick. More details about the function parameter can be found here.

Part III: Placing an Order

Now, as you have all the data needed and you have also tested your strategy, it is time to trade and place some orders for the same. I’ll deliberately keep this part simple and just talk about placing market and limit orders.

	contract = Option(symbol = symbol, 
	lastTradeDateOrContractMonth = expiry_date, strike = strike, 
	right = right, exchange = ‘SMART’)
	self.ib.reqMktData(contract, ‘’, False, False)
	self.ib.sleep(10)
	self.ib.qualifyContracts(contract)
	mkt_order = self.ib.marketOrder('BUY', quantity)  ## market order
	self.ib.placeOrder(contract, mkt_order)option_lmt = round((option_mkt_data.bid + option_mkt_data.ask)/2, 1)
	limit_order = self.ib.limitOrder('BUY', quantity, option_lmt)                                         ## limit order
	self.ib.placeOrder(contract, limit_order)

Here I have only described market and limit orders for Options but it is fairly simple to place multiple types of orders for various instruments, as described here.

For some complicated orders like Bracket orders (they require both stoploss and target on your position) you can refer to the following code:

	option_lmt = round((option_mkt_data.bid + option_mkt_data.ask)/2, 1)
	option_trgt = round(option_lmt + (abs(stock_trgt — stock_lmt)/2),1)
	option_stp_lss = round(option_lmt — (abs(stock_lmt — stock_stp_lss)/2),1)
	self.ib.qualifyContracts(contract)
	bracket = self.ib.bracketOrder(‘BUY’, quantity, option_lmt, option_trgt, option_stp_lss)           ### Bracket order
	for order in [bracket.parent, bracket.takeProfit,bracket.stopLoss]:
 		self.ib.placeOrder(contract, order)

Part IV: Cancelling a live order

Ability to cancel and modify your orders is central to any strategy. You will need a specific order identifier (order Id in our case) to tell the exchange that you want to modify a particular order. This piece can help you cancel the order:

	cancel_order_id = order_id   ### give the order id to be cancelled.
	for orders in self.ib.openTrades():    ## get all open orders
  		if(orders.order.orderId==cancel_order_id or orders.order.parentId==cancel_order_id):
     		self.ib.cancelOrder(orders.order)

The above piece can cancel a simple market or limit order as well as a bracket order.

Conclusion

Apart from the basic functions I demonstrated, IB API can literally do everything one needs ranging from streaming live data to getting realized or unrealized PnL. So, you can use these API’s as your canvas and soon you will realize that sky is the limit.

_________________________________________________________________

Do share your feedback with me and if you liked this blog post, do give it a clap. Follow me on social media (Instagram, Facebook, and LinkedIn)
