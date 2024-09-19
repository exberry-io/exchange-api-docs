# (NEW v1.34.0) Reserve Orders

Reserve orders support a display(visible) and a hidden quantity. Only the display quantity is visible to MPs via public market data feeds. Each time the display quantity is fully executed, the next portion from the hidden quantity is moved to the display quantity. This is called replenishment of the order.&#x20;

The exchange supports below `displayMethods`:

* **DisplayQuantity** - displays the displayQuantity initially and replenishment
* **Hidden** - Fully hidden order (`displayQuantity=0`)



## **General Rules:**

* The full quantity(both display and hidden quantities) of the reserve order is eligible to trade with resting orders at entry.
* After executing up to the maximum possible at the entry, the order rests on the book subjected to the maximum of `displayQuantity`
* System replenishes the display quantity when it is fully executed.
* Reserve orders are supported with
  * orderType : Limit and StopLimit
  * timeInForce : GTC/ GTD/ DAY
  * during continues trading only (system mass cancels reserve orders during auctions)
* Reserve orders are not supported when implied orders are configured for an instrument or its legs.
* order priority,
  * when multiple orders at the same price, display quantity has a higher priority than the hidden quantity.&#x20;
  * within the display and hidden quantities, time priority of display and hidden is considered separately.
  * the display quantity loses time priority each time it replenishes.
* Replenishment is accomplished by canceling the existing order and generating a new one (system carries forward the mpOrderId)
