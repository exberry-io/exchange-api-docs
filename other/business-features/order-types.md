# Order Types

## Stop and Stop Limit Orders

### General

Stop & StopLimit are not allowed to be placed during an auction.&#x20;

Replacement of Stop & StopLimit is allowed only before injection.&#x20;

Stop and StopLimit are not being published in the MD APIs and only shown in Private Data APIs:

* Mass Order Status (WS) / Order Mass Status Request (FIX)
* Execution Reports (Both WS and FIX)
* Order History (WS & Admin UI)

Once Stop / StopLimit orders are triggered and injected to the order book it will published in the MD like any regular order.



### Stop & StopLimit Trigger Rules:

The Stop and StopLimit orders will be injected into the order book by those rules:

* For buy orders → In case the stopPrice is lower or equal to the market price
* For sell orders → In case the stopPrice is higher or equal to the market price
* An incoming Stop / StopLimit Order will be injected immediately  if the stop price is already met.
  * Order will be immediately injected only in case there was an order book execution since last time market opened. (so on 24/7 it will always take last execution and non 24 hours calendars it will only use executed trade since today market open.)
* If taker order is executed against multiple resting orders, The Stop / StopLimit orders must be injected into order book only after place (taker) order operation is completed. If during any execution, the stop price is triggered, the order must be injected into order book. Even in case that after place (taker) order operation is completed the price has changed and it no longer meets the rules of the trigger, the order will still be injected into the book.
* System will inject orders that were triggered only when the marketStatus = "Opened" & tradingStatus = "Trade".
  * When it might happen:
    * on auction ends and it changes the market status from `AuctionCrossing` to `Opened`
    * on auto calendar event (start time) and the market status changes to `Opened`
    * on manual calendar changes and the market status changes to `Opened`
    * on manual resume and the trading status changes to `TRADE`
    * on auto resume and the trading status changes to `TRADE`
* After auction system will triggered Stop and Stop Limit orders according to the auction price (for Equilibrium Price Auctions) or according to the highest and lowest execution prices (for Price & Time Auction)



## (NEW v1.34.0) Reserve Orders

Reserve orders support a display(visible) and a hidden quantity. Only the display quantity is visible to MPs via public market data feeds. Each time the display quantity is fully executed, the next portion from the hidden quantity is moved to the display quantity. This is called replenishment of the order.&#x20;

The exchange supports below `displayMethods`:

* **DisplayQuantity** - displays the displayQuantity initially and replenishment
* **Hidden** - Fully hidden order (`displayQuantity=0`)
* <mark style="color:blue;">(NEW v1.35.0)</mark> **Random** - Displays the quantity in displayQuantity initially, then a quantity between `displayLowQuantity` and `displayHighQuantity` when replenished

### **General Rules:**

* The full quantity(both display and hidden quantities) of the reserve order is eligible to trade with resting orders at entry.
* After executing up to the maximum possible at the entry, the order rests on the book subjected to the maximum of `displayQuantity`
* System replenishes the display quantity when it is fully executed.
* Reserve orders are supported with
  * orderType : Limit and StopLimit
  * timeInForce : GTC/ GTD/ DAY
  * during continues trading <mark style="color:blue;">(NEW v1.36.0)</mark> and auctions <mark style="color:red;">(REMOVED v1.36.0)</mark> ~~only (system mass cancels reserve orders during auctions)~~
* <mark style="color:red;">(REMOVED v1.35.0)</mark> ~~Reserve orders are not supported when implied orders are configured for an instrument or its legs.~~
* <mark style="color:blue;">(NEW v1.35.0)</mark> Refer [here](new-v1.28.0-implied-orders/) for the behavior in case of implied orders.
* order priority,
  * when multiple orders at the same price, display quantity has a higher priority than the hidden quantity.&#x20;
  * within the display and hidden quantities, time priority of display and hidden is considered separately.
  * the display quantity loses time priority each time it replenishes.
* Replenishment is accomplished by canceling the existing order and generating a new one (system carries forward the mpOrderId)
* <mark style="color:blue;">(NEW v1.39.0)</mark> Modification of orders:
  * Reduction of quantities is allowed for all displayMethods with some limitations.&#x20;
  * Delta.quantity must be ≥ Delta.displayQuantity
  * Reducing DisplayQuantity is optional and available only with displayMethods=DisplayQuantity.&#x20;
  * displayLowQuantity and displayHighQuantity can’t be changed.
* <mark style="color:blue;">(NEW v1.39.0)</mark> Replacement of orders:
  * displayMethod can’t be changed during replacement.&#x20;
  * replaceOrder behavior is similar to the behavior of other order types.

### (NEW v1.36.0) Auctions

* Both display and hidden quantities of reserve orders are eligible to be traded during auctions.
* The total order quantity is visible via [AuctionIndicativeEP](../../ws/market-data.md#auction-indicative-equilibrium-price-message) market data messages.
* If applicable, replenishment of orders occurs as the final step of the auction crossing.
* The display quantity of an order could be a taker since all orders will be rested on the book prior to the crossing.



## (NEW v1.38) Market to Limit Orders

* Order that executes with the best available price of the contra side of the order book at entry.
* After executing up to the maximum quantity possible, any remaining quantity rests on the order book as a limit order, with a limit price equal to the executed price at entry.
* Supported timeInForce : GTC/ GTD/ DAY/ IOC/ FOK
* Supported trading Mode:&#x20;
  1. continues trading&#x20;
  2. auction (only already resting orders are supported)
* Replacement of the order is not allowed. Modification is allowed.
