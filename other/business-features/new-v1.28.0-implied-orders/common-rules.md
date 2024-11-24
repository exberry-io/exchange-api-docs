# Common Rules

* Implied orders are generated using all non-implied orders at best <mark style="color:blue;">(NEW v1.35.0)</mark> visible bid and best <mark style="color:blue;">(NEW v1.35.0)</mark> visible ask prices. (aka BBO = Best Bid Offer)
* <mark style="color:blue;">(NEW v1.35.0)</mark> In case of reserve orders, only the visible quantity is used to generate implied orders.
* Implied Orders trade only with non-implied orders, therefore, Implied Orders are not generated when they are tradable with resting implied orders
* Implied Orders are generated only when the parent instrument and all its leg instruments:
  * Market is Open for continues trading&#x20;
  * Trading is not halted&#x20;
  * Instrument Status is active
* When one of the above pre-conditions are being changed:
  * The system cancels any existing implied orders if these conditions are not met.
  * The system places the implied orders if these conditions are met.
* Implied orders are reevaluated when base orders go through changes such as execution, modification, cancellation, etc. or the best order price changes.
* Implied Orders are generated under the ownership of the system(mpId=-1). Therefore, no MP will receive execution reports of Implied Orders.
* `executionReports` and `trades` of the system can be retrieved via MP Groups of [Private Data](../../../ws/private-data-api.md) and [Reporting](../../../ws/reporting-api.md) APIs.
* Implied Orders and their changes are visible to all MPs via Market Data subscriptions and will be distributed as a regular market data entry.
* <mark style="color:blue;">NEW v1.37</mark> When there is a CBR of potential execution:
  * Implied orders are generated only when the order does not lead to an execution that would cause a breach.
  * Resting implied orders are canceled if they become executable with an incoming order that causes a breach or leads to a breach in a base instrument.

Note: The orders used to generate implied orders are referred to as “Base Orders”.
