# Stop and Stop Limit Orders

#### General

Stop & StopLimit are not allowed to be placed during an auction.&#x20;

Replacement of Stop & StopLimit is allowed only before injection.&#x20;

Stop and StopLimit are not being published in the MD APIs and only shown in Private Data APIs:

* Mass Order Status (WS) / Order Mass Status Request (FIX)
* Execution Reports (Both WS and FIX)
* Order History (WS & Admin UI)

Once Stop / StopLimit orders are triggered and injected to the order book it will published in the MD like any regular order.



#### Stop & StopLimit Trigger Rules:

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

