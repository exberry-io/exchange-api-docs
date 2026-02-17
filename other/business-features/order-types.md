# Order Types

## **Limit**&#x20;

A Limit order is an order to trade at a defined price(limit price) or better. Buy orders execute at or below the limit price. Sell orders execute at or above the limit price.

## Market

A market order is an order to buy or sell immediately at the best available prices. In case of a partial fill, the remaining order quantity is cancelled without resting on the book.

## Stop, Stop Limit, <mark style="color:blue;">(NEW v1.53.0)</mark> MIT and LIT

### Stop and MIT

Stop/ Market If Touched is an order that stays in Suspended state (not eligible to trade) until the stop price is reached. Once triggered, it will behave similar to a Market Order.&#x20;

### Stop Limit and LIT

Stop Limi/ Limit If Touched is an order that stays in Suspended state (not eligible to trade) until the stop price is reached. Once triggered, it will behave similar to a Limit Order.

### Usage

| Usage                         | Order Types           |
| ----------------------------- | --------------------- |
| Stop loss (long positions)    | Sell Stop, StopLimit  |
| Stop loss (short positions)   | Buy MIT, LIT          |
| Take Profit (long positions)  | Sell MIT, LIT         |
| Take Profit (short positions) | Buy Stop, StopLimit   |

### Common Rules

* Not allowed to be placed during an auction.&#x20;
* Replacement is allowed only before triggering.&#x20;
* Not published in the MD APIs before triggering and only shown in Private Data APIs
  * Mass Order Status (WS) / Order Mass Status Request (FIX)
  * Execution Reports (Both WS and FIX)
  * Order History (WS & Admin UI)

When triggered and injected to the order book it will published in MD like any regular order.

#### Trigger Rules

| Order Types                                 | Trigger Condition         |
| ------------------------------------------- | ------------------------- |
| <p>Buy Stop, StopLimit<br>Sell MIT, LIT</p> | Stop Price ≤ Market Price |
| <p>Sell Stop, StopLimit<br>Buy MIT, LIT</p> | Stop Price ≥ Market Price |

* An incoming order will be injected immediately if the trigger condition is already met.
  * Only in case there was an order book execution since last market opened time. (On 24/7 it will always take last execution and non 24 hours calendars it will only use executed trade since today market open.)
* If taker order is executed against multiple resting orders, the Stop / StopLimit / MIT / LIT orders are injected into order book only after place (taker) order operation is complete. In case of multiple execution at multiple price levels, all orders met the trigger conditions are injected irrespective of whether the last execution price meets the trigger condition or not.&#x20;
* System injects orders that were triggered only when the marketStatus = "Opened" & tradingStatus = "Trade".
  * When it occurs:
    * on auction ends and it changes the market status from `AuctionCrossing` to `Opened`
    * on auto calendar event (start time) and the market status changes to `Opened`
    * on manual calendar changes and the market status changes to `Opened`
    * on manual resume and the trading status changes to `TRADE`
    * on auto resume and the trading status changes to `TRADE`
* After auction, the system triggers Stop, StopLimit, MIT, LIT orders according to the auction price (for Equilibrium Price Auctions) or according to the highest and lowest execution prices (for Price & Time Auction)

## Market to Limit Orders

* Order that executes with the best available price of the contra side of the order book at entry.
* After executing up to the maximum quantity possible, any remaining quantity rests on the order book as a limit order, with a limit price equal to the executed price at entry.
* Supported timeInForce : GTC/ GTD/ DAY/ IOC/ FOK
* Supported trading Mode:&#x20;
  1. continues trading&#x20;
  2. auction (only already resting orders are supported)
* Replacement of the order is not allowed. Modification is allowed.

## Reserve Orders

Reserve orders support a display(visible) and a hidden quantity. Only the display quantity is visible to MPs via public market data feeds. Each time the display quantity is fully executed, the next portion from the hidden quantity is moved to the display quantity. This is called replenishment of the order.&#x20;

The exchange supports below `displayMethods`:

* **DisplayQuantity** - displays the displayQuantity initially and replenishment
* **Hidden** - Fully hidden order (`displayQuantity=0`)
* **Random** - Displays the quantity in displayQuantity initially, then a quantity between `displayLowQuantity` and `displayHighQuantity` when replenished

### **General**

* The full quantity(both display and hidden quantities) of the reserve order is eligible to trade with resting orders at entry.
* After executing up to the maximum possible at the entry, the order rests on the book subjected to the maximum of `displayQuantity`
* System replenishes the display quantity when it is fully executed.
* Reserve orders are supported with
  * orderType : Limit, StopLimit and LIT
  * timeInForce : GTC/ GTD/ DAY
  * during continues trading and auctions
* Refer [here](implied-orders/) for the behavior in case of implied orders.
* order priority,
  * when multiple orders at the same price, display quantity has a higher priority than the hidden quantity.&#x20;
  * within the display and hidden quantities, time priority of display and hidden is considered separately.
  * the display quantity loses time priority each time it replenishes.
* Replenishment is accomplished by canceling the existing order and generating a new one (system carries forward the mpOrderId)
* Modification of orders:
  * Reduction of quantities is allowed for all displayMethods with some limitations.&#x20;
  * Delta.quantity must be ≥ Delta.displayQuantity
  * Reducing DisplayQuantity is optional and available only with displayMethods=DisplayQuantity.&#x20;
  * displayLowQuantity and displayHighQuantity can’t be changed.
  * In case the displayQuantity is not changed,
    * Quantity is reduced from workingHiddenQuantity first, any remaining reductions are reduced from workingDisplayQuantity
  * In case the displayQuantity is changed:
    * if workingDisplayQuantity <= new displayQuantity:
      * new display quantity will apply only at the next replenishment(if applicable).
      * system reduces the workingHiddenQuantity
    * if workingDisplayQuantity > new displayQuantity:
      * system reduces the workingDisplayQuantity first, any remaining reduction is reduced from the workingHiddenQuantity
* Replacement of orders:
  * displayMethod can’t be changed during replacement.&#x20;
  * replaceOrder behavior is similar to the behavior of other order types.

### Auctions

* Both display and hidden quantities of reserve orders are eligible to be traded during auctions.
* The total order quantity is visible via [AuctionIndicativeEP](../../ws/market-data.md#auction-indicative-equilibrium-price-message) market data messages.
* If applicable, replenishment of orders occurs as the final step of the auction crossing.
* The display quantity of an order could be a taker since all orders will be rested on the book prior to the crossing.

