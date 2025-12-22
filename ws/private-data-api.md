# Private Data

Private data API enables market participants to receive additional copies of the trading activity. This interface may also be used by participants to download the current status of all their active orders.

## Data **Eligibility**&#x20;

Private data API can be consumed for single market participant (MP) or for group of MPs that are pre-configured by exchange operations team.&#x20;

<mark style="color:blue;">(NEW v1.51.0)</mark> In case of ThirdPartyLockedIn MP trade reporting, TradeReports are returned to their Reporting MP in addition to Buy MP and Sell MP.

### Account Assignment

An account can be assigned at API Key level. If assigned,&#x20;

* System adds the relevant party/targetParty <mark style="color:blue;">(CHANGED v1.46.0)</mark> ~~(id=accountId, role=1001, source=D)~~ (id=accountId, role=24, source=P) (if not already exist), to the below requests of the API Key.
  * [placeOrder](trading-api.md#placeorder)
  * [massCancel](trading-api.md#masscancel)
  * [createTradeReport](../other/trade-reporting/trade-report-api.md#createtradereport)
  * [submitRFQ](../other/rfq/initiator-trading.md#submitrfq)
  * [submitQuote](../other/rfq/dealer-trading.md#submitquote)
* Only the data relevant to the account is visible for the API Key.



Technical Note:

* Empty field will not be included in the message

## massOrderStatus

`massOrderStatus` API is used to retrieve the current status of all its own active orders.

{% hint style="info" %}
qualifier: v1/exchange.market/massOrderStatus
{% endhint %}

### **Response**

The response provides close to real time list of all current active orders with the entire details of those orders.\
\
In addition the response includes`lastTrackingNumber`, this the `trackingNumber` of the last event used to generate the orders list, this can be used as input in `executionReports` to be able start consume events from that point.

The response is sorted by instrument and orderID.\
\
Note: There are no request parameters.

<table><thead><tr><th width="205">Field</th><th width="564">Description</th></tr></thead><tbody><tr><td>messageType</td><td><strong>OrderStatus</strong></td></tr><tr><td>orderId</td><td>Exchange order ID</td></tr><tr><td>mpOrderId</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>orderType</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>side</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>instrument</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>quantity</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>price</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>stopPrice</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>timeInForce</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>expiryDate</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>orderTimestamp</td><td>Order creation timestamp (in nanoseconds) in GMT</td></tr><tr><td>marketModel</td><td><p>A - (Auction) when order was placed during auction</p><p>T - (Trading) when order was placed on continues trading mode<br>UA- (Unscheduled Auction) when order was placed during non scheduled auction </p></td></tr><tr><td>parties</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>accountType</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>filledQuantity</td><td>Total filled quantity</td></tr><tr><td>filledPrice</td><td>Weighted average filled price for all fills on that order <span class="math">Sum(event.executedQuantity * event.executedPrice)/Sum(event.executedQuantity)</span></td></tr><tr><td>remainingOpenQuantity</td><td><p>Remaining open quantity.</p><p><span class="math">quantity - filledQuantity - removedQuantity</span></p></td></tr><tr><td><p><mark style="color:red;">REMOVED v1.48.0</mark></p><p><del>removedQuantity</del></p></td><td><del>Quantity that was removed with modifyOrder request</del></td></tr><tr><td>displayMethod</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>displayQuantity</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>workingDisplayQuantity</td><td>Current workingDisplayQuantity</td></tr><tr><td>workingHiddenQuantity</td><td>Current workingHiddenQuantity</td></tr><tr><td>displayLowQuantity</td><td>​​The lower quantity limit to randomise the display quantity at replenishment</td></tr><tr><td>displayHighQuantity</td><td>​​The upper quantity limit to randomise the display quantity at replenishment</td></tr><tr><td>lastEventTimestamp</td><td>Last order event timestamp (in nanoseconds) in GMT</td></tr><tr><td>lastEventId</td><td>Last event that was used to calculate order state</td></tr><tr><td>mpId</td><td>MP Id</td></tr><tr><td>mpName</td><td>MP name </td></tr><tr><td>status</td><td><p>the status of the order.</p><ul><li>Active - as long as still on the book </li><li>Suspended - an order not yet injected into the order book.</li></ul></td></tr><tr><td><mark style="color:blue;">NEW 1.50</mark><br>metadata</td><td>Same as in <code>placeOrder</code> request</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="175">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Temporary failure to retrieve this data</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```json
{
  "q": "v1/exchange.market/massOrderStatus",
  "sid": 100,
  "d": {}
}
```
{% endtab %}

{% tab title="Active Order" %}
```json
{
  "q": "v1/exchange.market/massOrderStatus",
  "sid": 102,
  "d": {
    "messageType": "OrderStatus",
    "orderId": 19,
    "mpOrderId": 6900,
    "orderType": "Limit",
    "side": "Sell",
    "instrument": "INS2",
    "quantity": 100,
    "price": 100.33,
    "timeInForce": "GTC",
    "orderTimestamp": 1617559600457506000,
    "filledQuantity": 53,
    "remainingOpenQuantity": 47,
   // "removedQuantity": 0, //REMOVED v1.48.0
    "filledPrice": 100.33,
    "marketModel": "T",
    "lastEventTimestamp": 1617875864297023000,
    "lastEventId": 33,
    "mpId": 1958681073,
    "mpName": "Participant1",
    "status": "Active"
  }
}
```
{% endtab %}

{% tab title="lastTrackingNumber" %}
```json
{
  "q": "v1/exchange.market/massOrderStatus",
  "sid": 102,
  "d": {
    "lastTrackingNumber": 34256480
  }
}
```
{% endtab %}
{% endtabs %}

## **executionReports**

`executionReports` API is used to subscribe to real time orders and trades events.\
There are few message types used in this API:

* **Add**
* **Cancelled**
* **Executed**
* **Modified**
* **TradeReport**
* **TradeCancel**
* **Suspended**
* <mark style="color:blue;">NEW 1.50</mark> **News** - for more details see [#news-message-new-v1.50](market-data.md#news-message-new-v1.50 "mention")

{% hint style="info" %}
qualifier: v1/exchange.market/executionReports
{% endhint %}

### **Request**

<table><thead><tr><th width="150.33333333333331">Parameter</th><th width="77">Type</th><th>Description</th></tr></thead><tbody><tr><td>trackingNumber <code>optional</code></td><td>Long</td><td><p>Defines the starting point of the stream.</p><p>Supported values:</p><ul><li><code>0</code> – Starts the stream from the very first event in the system</li><li><em>Empty</em> – Starts from the next upcoming event</li><li>Specific <code>trackingNumber</code> – Starts from the event immediately following the provided tracking number</li></ul><p>Notes:</p><ul><li>In certain scenarios, multiple events may share the same tracking number, for example when the same market participant (MP) is involved on both sides of a trade.</li><li><p>For reliable recovery (e.g., after a disconnection), it is recommended to:</p><ul><li>Subscribe using the second-most-recent <code>trackingNumber</code></li><li>Implement duplicate event h<strong>andling</strong> logic</li></ul></li></ul></td></tr></tbody></table>

### **Response**

<table><thead><tr><th width="155.33333333333331">Message Type</th><th width="218">Field</th><th>Description</th></tr></thead><tbody><tr><td>All</td><td>messageType</td><td><p>One of the below values:</p><ul><li><strong>Add</strong></li><li><strong>Cancelled</strong></li><li><strong>Executed</strong></li><li><strong>Modified</strong></li><li><strong>TradeReport</strong></li><li><strong>TradeCancel</strong></li><li><strong>Suspended</strong></li></ul></td></tr><tr><td>All</td><td>side</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>All</td><td>instrument</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>All</td><td>quantity</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>All</td><td>price</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>All</td><td>mpId</td><td>MP Id</td></tr><tr><td>All</td><td>mpName</td><td>MP name </td></tr><tr><td>All</td><td>accountType</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>All</td><td>parties</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>All</td><td>eventId</td><td>Sequence identifier per instrument for the event</td></tr><tr><td>All</td><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>All</td><td>trackingNumber</td><td>Event tracking number</td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended<br><mark style="color:blue;">(NEW v1.50.0)</mark> TradeReport<br><mark style="color:blue;">(NEW v1.50.0)</mark> TradeCancel</td><td>orderId</td><td>Exchange order ID</td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended<br><mark style="color:blue;">(NEW v1.50.0)</mark> TradeReport<br><mark style="color:blue;">(NEW v1.50.0)</mark> TradeCancel</td><td>mpOrderId</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended</td><td>orderType</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended</td><td>timeInForce</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended</td><td>expiryDate</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended</td><td>expiryDay</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended</td><td>orderTimestamp</td><td>Order creation timestamp (in nanoseconds) in GMT</td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended</td><td>marketModel</td><td><p>A - (Auction) when order was placed during auction</p><p>T - (Trading) when order was placed on continues trading mode<br>UA- (Unscheduled Auction) when order was placed during auction after halt or autoHalt</p></td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended</td><td>filledQuantity</td><td>Total filled quantity</td></tr><tr><td>Add <br>Executed Modified Cancelled  Suspended</td><td>filledPrice</td><td>Weighted average filled price for all fills on that order<br><span class="math">Sum(event.executedQuantity * event.executedPrice)/Sum(event.executedQuantity)</span></td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended</td><td>remainingOpenQuantity</td><td><p>Remaining open quantity.</p><p><span class="math">quantity - filledQuantity - removedQuantity</span></p></td></tr><tr><td><mark style="color:red;">(REMOVED v1.48.0)</mark><br><del>Add</del> <br><del>Cancelled Executed Modified</del><br><del>Suspended</del></td><td><del>removedQuantity</del></td><td><del>Quantity that was removed with modifyOrder request</del></td></tr><tr><td>Add <br>Cancelled Executed Modified<br>Suspended</td><td>stopPrice</td><td>The price at which the order will be injected to the market. </td></tr><tr><td><p>Add</p><p>Executed</p><p>Modified</p><p>Cancelled</p><p>Suspended</p></td><td>displayMethod</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td><p>Add</p><p>Executed</p><p>Modified</p><p>Cancelled</p><p>Suspended</p></td><td>displayQuantity</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td><p>Add</p><p>Executed</p><p>Modified</p><p>Cancelled</p></td><td>workingDisplayQuantity</td><td>workingDisplayQuantity at the end of event</td></tr><tr><td><p>Add</p><p>Executed</p><p>Modified</p><p>Cancelled</p></td><td>workingHiddenQuantity</td><td>workingHiddenQuantity at the end of event</td></tr><tr><td><p>Add</p><p>Executed</p><p>Modified</p><p>Cancelled<br>Suspended</p></td><td>displayLowQuantity</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td><p>Add</p><p>Executed</p><p>Modified</p><p>Cancelled<br>Suspended</p></td><td>displayHighQuantity</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>Executed</td><td>lastFilledQuantity</td><td>Matched quantity</td></tr><tr><td>Executed</td><td>lastFilledPrice</td><td>Matched price (maker order price).</td></tr><tr><td><mark style="color:blue;">(NEW v1.43.0)</mark><br>Executed</td><td>makerTaker</td><td>Same as<a href="private-data-api.md#trades"> trades API</a></td></tr><tr><td>Executed<br>TradeReport<br>TradeCancel</td><td>mathchId</td><td>Unique ID for the match</td></tr><tr><td>Executed<br>TradeReport<br>TradeCancel</td><td>tradingMode</td><td>IA - (Scheduled Intraday Auction) -When execution was as part of auction<br>CT -  (Continuous Trading) - When execution was done on a regular trading<br>ON -  Trade Reporting (On Exchange)<br>UA- (Unscheduled Auction) - When execution was as part of auction after halt or autoHalt</td></tr><tr><td><mark style="color:blue;">(NEW v1.48.0)</mark><br>TradeReport<br>TradeCancel</td><td>linkId</td><td>Same as<a href="private-data-api.md#trades"> trades API</a></td></tr><tr><td>Cancelled</td><td>cancelledQuantity</td><td>The cancelled quantity in the current cancelled event</td></tr><tr><td>Cancelled</td><td>cancelReason</td><td><p>Reason of the cancellation</p><p>Refer <a href="private-data-api.md#cancel-reason">here</a> for values.</p></td></tr><tr><td><mark style="color:red;">(REMOVED v1.48.0)</mark><br><del>Modified</del></td><td><del>lastRemovedQuantity</del></td><td><del>Removed quantity on current event</del></td></tr><tr><td><mark style="color:red;">(REMOVED v1.48.0)</mark><br><del>Modified</del></td><td><del>lastRemovedWorkingDisplayQuantity</del></td><td><del>Removed Working Display Quantity on current event</del></td></tr><tr><td><mark style="color:red;">(REMOVED v1.48.0)</mark><br><del>Modified</del></td><td><del>lastRemovedWorkingHiddenQuantity</del></td><td><del>Removed Working Hidden Quantity on current event</del></td></tr><tr><td><mark style="color:blue;">(NEW v1.48.0)</mark><br>Modified</td><td>lostPriority</td><td><p>Indicates if Modified order has caused an order to lose priority.</p><p>Allowed values:</p><ul><li>true - order has lost time priority (replace) </li><li>false - order has not lost time priority (modify)</li></ul></td></tr><tr><td>TradeReport<br>TradeCancel</td><td>tradeType</td><td><mark style="color:blue;">(CHANGED v1.41.0)</mark> <del>EFRP/Block/Other</del> Refer <a href="https://documenter.getpostman.com/view/6229811/TzCV3jcq#909a5a9c-3385-43af-9569-ea3168a331e9">Admin API Trade Entry</a>/type field for values; all values are supported.<br>On TradeCancel it will be shown only for TradeReport cancellation </td></tr><tr><td><p>Add </p><p>Executed Modified Cancelled OrderStatus Suspended</p></td><td>minQuantity</td><td>Minimum Quantity for executed</td></tr><tr><td><p>Add </p><p>Executed Modified Cancelled OrderStatus Suspended</p></td><td>stpAction</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td><p><mark style="color:blue;">(NEW v1.50.0)</mark></p><p>Add </p><p>Executed Modified Cancelled OrderStatus Suspended</p></td><td>metadata</td><td>Same as in <code>placeOrder</code> request</td></tr></tbody></table>

### **Cancel Reason**

<table><thead><tr><th width="307">Cancel Reason</th><th>Description</th></tr></thead><tbody><tr><td>CancelRequest</td><td>As a result of cancelOrder request</td></tr><tr><td>ReplaceRequest</td><td>As a result of replaceOrder request</td></tr><tr><td>MassCancelRequest</td><td>As a result of massCancel request</td></tr><tr><td>Expiration</td><td>As a result of GTD, DAY &#x26; GAA order expiration</td></tr><tr><td>ImmediateOrder</td><td>As a result of order with TIF IOC or FOK that can not be immediately executed according to exact condition (e.g. IOC that can be partially executed, yet, the residual quantity will be canceled) </td></tr><tr><td>DailyPriceBandBreachMassCancel</td><td>As a result of cancel/reject order according to Daily Price Band</td></tr><tr><td>TickPriceBandBreachMassCancel</td><td>As a result of cancel/reject order according to Tick Price Band</td></tr><tr><td>LegsPriceBandBreachMassCancel </td><td>As a result of cancel/reject order according to Leg Price Band</td></tr><tr><td>CancelOnDisconnect</td><td>As a result of cancellation according to COD</td></tr><tr><td>InjectionRejected</td><td>As a result of cancel/reject order according to validation during injection</td></tr><tr><td>InstrumentExpiration</td><td>As a result of a stop date that has passed its time</td></tr><tr><td>BlockedMP</td><td>As a result of blocking the MP</td></tr><tr><td><mark style="color:blue;">(NEW v1.50.0)</mark><br>BlockedAccount</td><td>as a result of blocking the Account</td></tr><tr><td>STPIncomingOrder</td><td>As a result of cancel/reject order according to STP action of cancel incoming order</td></tr><tr><td>STPRestingOrder</td><td>As a result of cancel/reject order according to STP action of cancel resting order</td></tr><tr><td>STPBothOrders</td><td>As a result of cancel/reject order according to STP action of cancel both orders</td></tr><tr><td>CancelOrderOnBehalf </td><td>As a result of CancelOrder request from admin user</td></tr><tr><td>MassCancelOnBehalf</td><td>As a result of a MassCancel request from admin user</td></tr><tr><td>ReplaceOrderOnBehalf</td><td>As a result of replaceOrder request from admin user</td></tr><tr><td>BaseOrderChanged</td><td>When an implied order is canceled due to change in base orders (for example, base order canceled, filled, etc.)</td></tr><tr><td>CorporateAction</td><td>Canceled due to a corporate action</td></tr><tr><td>ReserveOrderReplenishment</td><td>Reserve order canceled to replenish the display quantity</td></tr><tr><td>NotSupported</td><td>Used for cancellations related to scenarios to be handled in the future.</td></tr><tr><td>CBRBreach</td><td>As a result of cancel/reject order according of CBR trigger</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="186">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>2</td><td><code>Stream disconnected</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1200</td><td><code>General error</code></td></tr><tr><td>1201</td><td><code>Wrong trackingNumber</code></td></tr></tbody></table>

### **Orders Messages Samples**

{% tabs %}
{% tab title="Subscription" %}
```json
{
  "q": "v1/exchange.market/executionReports",
  "sid": 104,
  "d": {
    "trackingNumber":34256480
  }
}
```
{% endtab %}

{% tab title="Add" %}
```json
{
  "q": "v1/exchange.market/executionReports",
  "sid": 104,
  "d": {
    "messageType": "Add",
    "orderId": 301,
    "mpOrderId": 1620913719739,
    "orderType": "Limit",
    "side": "Buy",
    "instrument": "INS1",
    "quantity": 1,
    "price": 1,
    "timeInForce": "GTC",
    "orderTimestamp": 1620913727891980000,
    "filledQuantity": 0,
    "remainingOpenQuantity": 1,
    "removedQuantity": 0,
    "filledPrice": 0,
    "marketModel": "T",
    "eventTimestamp": 1620913727891980000,
    "eventId": 454,
    "trackingNumber": 34272768,
    "mpId": 1958681073,
    "mpName": "Participant1"
  }
}
```
{% endtab %}

{% tab title="Cancelled" %}
```json
{
  "q": "v1/exchange.market/executionReports",
  "sid": 104,
  "d": {
    "messageType": "Cancelled",
    "orderId": 300,
    "mpOrderId": 10019999,
    "orderType": "Limit",
    "side": "Buy",
    "instrument": "INS1",
    "quantity": 1.3,
    "price": 100.33,
    "timeInForce": "GTC",
    "orderTimestamp": 1620913494330769000,
    "filledQuantity": 0,
    "remainingOpenQuantity": 0,
    "removedQuantity": 0.8,
    "filledPrice": 0,
    "marketModel": "T",
    "eventTimestamp": 1620913521839426000,
    "eventId": 453,
    "cancelledQuantity": 0.5,
    "trackingNumber": 34271264,
    "mpId": 1958681073,
    "mpName": "Participant1"
  }
}
```
{% endtab %}

{% tab title="Executed" %}
```json
{
  "q": "v1/exchange.market/executionReports",
  "sid": 104,
  "d": {
    "messageType": "Executed",
    "orderId": 19,
    "mpOrderId": 6900,
    "orderType": "Limit",
    "side": "Sell",
    "instrument": "INS2",
    "quantity": 0,
    "price": 100.33,
    "timeInForce": "GTC",
    "orderTimestamp": 1617559600457506000,
    "filledQuantity": 1.25,
    "remainingOpenQuantity": 45.75,
    "removedQuantity": 0,
    "filledPrice": 100.33,
    "marketModel": "T",
    "eventTimestamp": 1620913666099642000,
    "eventId": 44,
    "lastFilledQuantity": 1.25,
    "lastFilledPrice": 100.33,
    "matchId": 24,
    "tradingMode": "CT",
    "trackingNumber": 34272544,
    "mpId": 1958681073,
    "mpName": "Participant1",
    "makerTaker": "Taker"
  }
}
```
{% endtab %}

{% tab title="Modified" %}
```json
{
  "q": "v1/exchange.market/executionReports",
  "sid": 104,
  "d": {
    "messageType": "Modified",
    "orderId": 300,
    "mpOrderId": 10019999,
    "orderType": "Limit",
    "side": "Buy",
    "instrument": "INS1",
    "quantity": 1.3,
    "price": 100.33,
    "timeInForce": "GTC",
    "orderTimestamp": 1620913494330769000,
    "filledQuantity": 0,
    "remainingOpenQuantity": 0.5,
    "removedQuantity": 0.8,
    "filledPrice": 0,
    "marketModel": "T",
    "eventTimestamp": 1620913512564134000,
    "eventId": 452,
    "lastRemovedQuantity": 0.8,
    "trackingNumber": 34271040,
    "mpId": 1958681073,
    "mpName": "Participant1"
  }
}
```
{% endtab %}

{% tab title="Suspended" %}
```json
 {
      "messageType": "Suspended",      
      "orderId": 1,
      "mpOrderId": "123",
      "orderType": "StopLimit",
      "side": "Buy",
      "instrument": "Appl",
      "quantity": 2,
      "price": 1200,
      "StopPrice": 1500
      "timeInForce": "GTD",
      "expiryDate": 1572560725,
      "orderTimestamp": 1572560625123456,
      "eventTimestamp": 1572560625123456,
      "eventId": 10,
      "filledQuantity": 0,
      "remainingOpenQuantity": 2,
      "removedQuantity": 0,
      "filledPrice": 0,
      "marketModel": "CT",
      "mpId": 1237745,
     "mpName": "MP",
}
```
{% endtab %}
{% endtabs %}

### **Other Messages Samples**

{% tabs %}
{% tab title="TradeReport" %}
```json
{
  "q": "v1/exchange.market/executionReports",
  "sid": 121,
  "d": {
    "messageType": "TradeReport",
    "side": "Buy",
    "instrument": "ABC1",
    "quantity": 10,
    "price": 1.1234,
    "accountType": "Client",
    "parties": [
      {
        "id": "1234",
        "source": "D",
        "role": 38
      }
    ],
    "eventTimestamp": 1663490089515144400,
    "eventId": 31,
    "matchId": 8,
    "tradeType": "Block",
    "tradingMode": "ON",
    "trackingNumber": 219954560,
    "mpId": 2087505339,
    "mpName": "MP1"
  }
}
```
{% endtab %}

{% tab title="TradeCancel" %}
```json
{
  "q": "v1/exchange.market/executionReports",
  "sid": 121,
  "d": {
    "messageType": "Cancelled",
    "orderId": 2,
    "mpOrderId": 1,
    "orderType": "Limit",
    "side": "Sell",
    "instrument": "ABC4",
    "quantity": 0.3,
    "price": 100.48,
    "timeInForce": "GTC",
    "orderTimestamp": 1662997067465939500,
    "filledQuantity": 0,
    "remainingOpenQuantity": 0,
    "removedQuantity": 0,
    "marketModel": "T",
    "accountId": 10,
    "eventTimestamp": 1662998446000199200,
    "eventId": 4,
    "cancelledQuantity": 0.3,
    "trackingNumber": 218169568,
    "mpId": 2087505339,
    "mpName": "MP1"
  }
}
```
{% endtab %}

{% tab title="(NEW v1.48.0) Trade Adjustment" %}
```json
{
  "q": "v1/exchange.market/executionReports",
  "sid": 13,
  "d": {
    "messageType": "TradeCancel",
    "side": "Buy",
    "instrument": "AMZ",
    "quantity": 145,
    "price": 225.44,
    "eventTimestamp": 1760348836187466800,
    "eventId": 31,
    "matchId": 8,
    "tradeType": "EFS",
    "tradingMode": "ON",
    "trackingNumber": 484234688,
    "mpId": 20,
    "mpName": "MP1",
    "linkId": 807
  }
}

{
  "q": "v1/exchange.market/executionReports",
  "sid": 13,
  "d": {
    "messageType": "TradeReport",
    "side": "Buy",
    "instrument": "AMZ",
    "quantity": 145,
    "price": 230,
    "eventTimestamp": 1760348836187466800,
    "eventId": 32,
    "matchId": 9,
    "tradeType": "EFS",
    "tradingMode": "ON",
    "trackingNumber": 484235008,
    "mpId": 20,
    "mpName": "MP1",
    "linkId": 807
  }
}
```
{% endtab %}

{% tab title="News" %}
```json
{
  "q": "v1/exchange.market/executionReports",
  "sid": 10,
  "d": {
    "messageType": "News",
    "eventTimestamp": 1763050048890147800,
    "trackingNumber": 580050560,
    "id": 456,
    "message": "Sample message"
  }
}
```
{% endtab %}
{% endtabs %}

## Trades&#x20;

`trades` API allows to get a real time stream for all the trades data&#x20;

Available messages in that API:&#x20;

* MatchedTrade: for order book trade&#x20;
* TradeReport: for trade entry trade&#x20;
* TradeCancel: for trade cancellation (separate records will be returned for cancellation)
* calendarEndOfDay: for End Of Day event

{% hint style="info" %}
qualifier: v1/exchange.market/trades
{% endhint %}

### **Request**

<table><thead><tr><th width="173.33333333333331">Parameter</th><th width="98">Type</th><th>Description</th></tr></thead><tbody><tr><td>trackingNumber <code>optional</code></td><td>Long</td><td>Same as defined in executionReports<a data-mention href="private-data-api.md#request">#request</a></td></tr></tbody></table>

### **Response**

<table><thead><tr><th width="149">Field</th><th width="250">Description</th><th width="112.62841796875" align="center">Order Book</th><th width="137.37841796875" align="center">Trade Entry</th><th width="140" align="center">Trade Cancel</th></tr></thead><tbody><tr><td>eventId</td><td> Event Id </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>timestamp</td><td>Event timestamp (in nanoseconds) in GMT</td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>actionType</td><td><p>MatchedTrade/ </p><p>TradeReport /</p><p>TradeCancel</p></td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>orderId</td><td>Order id initiated the trade  </td><td align="center">V</td><td align="center"><mark style="color:blue;">(NEW v1.50.0)</mark> <br>V (Trade Adj. Only)</td><td align="center"><mark style="color:blue;">(NEW v1.50.0)</mark> <br>V </td></tr><tr><td>mpOrderId</td><td>From source order </td><td align="center">V</td><td align="center"><mark style="color:blue;">(NEW v1.50.0)</mark> <br>V (Trade Adj. Only)</td><td align="center"> <mark style="color:blue;">(NEW v1.50.0)</mark> <br>V </td></tr><tr><td>mpId</td><td> MP Id </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>mpName</td><td>MP Name </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>instrumentId</td><td> Instrument id </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>Instrument</td><td>instrument symbol</td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>side</td><td> Buy/ Sell </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>price</td><td> Trade price </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>quantity</td><td> Trade quantity </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>tradeId</td><td>matchId </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>tradingMode</td><td>IA - (Scheduled Intraday Auction) -When execution was as part of auction CT (Continuous Trading) - When execution was done on a regular trading ON - Trade Reporting (On Exchange)<br>UA- (Unscheduled Auction) - When execution was as part of auction after halt or autoHalt</td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>accountType</td><td>Optional, From source order </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>parties</td><td>Optional, From source order </td><td align="center">V</td><td align="center">V</td><td align="center"><code>opt</code></td></tr><tr><td>tradeType</td><td><mark style="color:blue;">(CHANGED v1.41.0)</mark> <del>EFRP/Block/Other</del> Refer <a href="https://documenter.getpostman.com/view/6229811/TzCV3jcq#909a5a9c-3385-43af-9569-ea3168a331e9">Admin API Trade Entry</a>/type field for values; all values are supported.</td><td align="center"> </td><td align="center">V</td><td align="center"><code>opt</code></td></tr><tr><td>makerTaker</td><td><strong>Taker</strong> if order was never resting on the book for that trade <br><strong>Maker</strong> if order was resting on the book for that trade</td><td align="center">V</td><td align="center"> </td><td align="center"> </td></tr><tr><td>tradeDate</td><td>Date of the business day of that trade<br>Format: YYY-MM-DD</td><td align="center"> V</td><td align="center"> V</td><td align="center">V </td></tr><tr><td><mark style="color:blue;">(NEW v1.48.0)</mark><br>tickReferencePrice</td><td>Tick Reference Price at the time trade is captured in system<br>If not exists, empty</td><td align="center">V</td><td align="center">V</td><td align="center"></td></tr><tr><td><mark style="color:blue;">(NEW v1.48.0)</mark><br>linkId</td><td>System generated ID to group all events related to a trade adjustment</td><td align="center"></td><td align="center">V<br>(Trade Adj. Only)</td><td align="center">V<br>(Trade Adj. Only)</td></tr><tr><td>trackingNumber</td><td> Tracking number </td><td align="center"> V</td><td align="center"> V</td><td align="center"> V</td></tr><tr><td>multiLegReportingType</td><td><ul><li>SingleSecurity: non strategy</li><li>MultiLegSecurity: strategy </li><li>IndividualLeg: strategy leg </li></ul><p>For more details see <a href="https://docs.exberry.io/private-data-api#strategies-multi-legs-handling">here</a></p></td><td align="center">V</td><td align="center">V</td><td align="center"><code>opt</code></td></tr></tbody></table>

### **End Of Day - Response**

<table><thead><tr><th width="229">Field</th><th>Description</th></tr></thead><tbody><tr><td>actionType</td><td>calendarEndOfDay</td></tr><tr><td>timestamp</td><td>Event timestamp (in nanoseconds)</td></tr><tr><td>calendarId</td><td>Calendar Id</td></tr><tr><td>calendarName</td><td>Calendar name</td></tr><tr><td>eodDate</td><td><p>The trade date of the day that was closed.</p><p>Format: yyyy-mm-dd</p></td></tr><tr><td>trackingNumber</td><td>Event tracking number</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="186">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>2</td><td><code>Stream disconnected</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1200</td><td><code>General error</code></td></tr><tr><td>1201</td><td><code>Wrong trackingNumber</code></td></tr></tbody></table>



### Samples

{% tabs %}
{% tab title="Subscription" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 16,
  "d": {
    "trackingNumber": 12345
  }
}
```
{% endtab %}

{% tab title="MatchedTrade " %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 16,
  "d": {
    "eventId": 23,
    "timestamp": 1662905104172275200,
    "actionType": "MatchedTrade",
    "orderId": 18,
    "mpOrderId": 46,
    "mpId": 2087505339,
    "mpName": "mp1",
    "instrumentId": 963,
    "instrument": "ABC1",
    "side": "Sell",
    "price": 100.48,
    "quantity": 0.3,
    "tradeId": 3,
    "tradingMode": "CT",
    "accountType": "Client",
    "parties": [
      {
        "id": "TST1",
        "source": "D",
        "role": 12
      },
      {
        "id": "1234",
        "source": "D",
        "role": 38
      }
    ],
    "makerTaker": "Maker",
    "tradeDate": "2022-09-11",
    "trackingNumber": 217814816
  }
}
```
{% endtab %}

{% tab title="TradeReport " %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 14,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1690096334129503000,
    "trackingNumber": 1127207840,
    "mpId": 2087505414,
    "mpName": "yael",
    "instrumentId": 5,
    "instrument": "yael",
    "side": "Buy",
    "price": 3602879701896397,
    "quantity": 10,
    "tradeId": 6,
    "tradingMode": "CT",
    "makerTaker": "Maker",
    "tradeDate": "2023-07-23",
    "multiLegReportingType": "IndividualLeg",
    "tradeLegRefId": 1,
    "multiLegDifferentialPrice": 10000,
    "multiLegStrategyInstrumentId": 14837,
    "multiLegStrategyInstrument": "Spread",
    "multiLegStrategyTradeId": 11,
    "multiLegStrategyOrderId": 21,
    "multiLegStrategyMpOrderId": 1564403702311
  }
}
```
{% endtab %}

{% tab title="TradeCancel " %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 16,
  "d": {
    "eventId": 26,
    "timestamp": 1662906282293290000,
    "actionType": "TradeCancel",
    "mpId": 2087505339,
    "mpName": "mp1",
    "instrumentId": 963,
    "instrument": "ABC1",
    "side": "Buy",
    "price": 100.48,
    "quantity": 0.3,
    "tradeId": 4,
    "tradingMode": "CT",
    "tradeDate": "2022-09-11",
    "trackingNumber": 217818208
  }
}
```
{% endtab %}

{% tab title="EOD" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 14,
  "d": {
    "actionType": "CalendarEndOfDay",
    "timestamp": 1681731969637411800,
    "trackingNumber": 407833440,
    "calendarId": 123,
    "calendarName": "NewCalendar",
    "eodDate": "2023-04-17"
  }
}
```
{% endtab %}

{% tab title="(NEW v1.48.0) Trade Adjustment" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 3,
  "d": {
    "actionType": "TradeCancel",
    "timestamp": 1760348836187466800,
    "trackingNumber": 484234688,
    "eventId": 31,
    "mpId": 20,
    "mpName": "MP1",
    "instrumentId": 28,
    "instrument": "AMZ",
    "side": "Buy",
    "price": 225.44,
    "quantity": 145,
    "tradeId": 8,
    "tradingMode": "ON",
    "tradeType": "EFS",
    "tradeDate": "2025-10-13",
    "linkId": 807,
    "multiLegReportingType": "SingleSecurity"
  }
}

{
  "q": "v1/exchange.market/trades",
  "sid": 3,
  "d": {
    "actionType": "TradeReport",
    "timestamp": 1760348836187466800,
    "trackingNumber": 484235008,
    "eventId": 32,
    "mpId": 20,
    "mpName": "MP1",
    "instrumentId": 28,
    "instrument": "AMZ",
    "side": "Buy",
    "price": 230,
    "quantity": 145,
    "tradeId": 9,
    "tradingMode": "ON",
    "tradeType": "EFS",
    "tradeDate": "2025-10-13",
    "tickReferencePrice": 50,
    "linkId": 807,
    "multiLegReportingType": "SingleSecurity"
  }
}
```
{% endtab %}
{% endtabs %}

### **Strategies/ Multi Legs Handling**&#x20;

Strategies trades are sent on `trades` API by 2 models:

* Individual leg of a multi-leg security: reports the underlying legs of the strategy
* Multi-leg security - reports the strategy trade as is

Consumers can decide how to consume strategies trades.

\
On Individual legs model, each trade will be sent with the following parameters:

<table><thead><tr><th width="356.3333333333333">Parameter</th><th>Description</th></tr></thead><tbody><tr><td>tradeLegRefId</td><td>leg index</td></tr><tr><td>multiLegDifferentialPrice</td><td>Parent trade price</td></tr><tr><td>multiLegStrategyInstrumentId</td><td>Parent trade instrument Id</td></tr><tr><td>multiLegStrategyTradeId</td><td>Parent trade Id</td></tr><tr><td>multiLegStrategyInstrument</td><td>Parent instrument symbol</td></tr><tr><td>multiLegStrategyOrderId</td><td>Parent orderId</td></tr><tr><td>multiLegStrategyMpOrderId</td><td>Parent mpOrderId</td></tr></tbody></table>

#### Samples

{% tabs %}
{% tab title="SingleSecurity" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 13,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1686554476838564000,
    "trackingNumber": 845932288,
    "eventId": 93,
    "orderId": 43,
    "mpOrderId": 1564403702131,
    "mpId": 2087505414,
    "mpName": "user",
    "instrumentId": 12208,
    "instrument": "INS1",
    "side": "Buy",
    "price": 180,
    "quantity": 10,
    "tradeId": 25,
    "tradingMode": "CT",
    "makerTaker": "Maker",
    "tradeDate": "2023-06-12",
    "multiLegReportingType": "SingleSecurity"
  }
}
```
{% endtab %}

{% tab title="IndividualLeg" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 13,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1686554527628924000,
    "trackingNumber": 845932896,
    "mpId": 2087505414,
    "mpName": "user",
    "instrumentId": 14836,
    "instrument": "future",
    "side": "Buy",
    "price": 10,
    "quantity": 7,
    "tradeId": 8,
    "tradingMode": "CT",
    "makerTaker": "Maker",
    "tradeDate": "2023-06-12",
    "multiLegReportingType": "IndividualLeg",
    "tradeLegRefId": 1,
    "multiLegDifferentialPrice": 10,
    "multiLegStrategyInstrumentId": 14837,
    "multiLegStrategyTradeId": 7
  }
}
```
{% endtab %}

{% tab title="MultiLegSecurity" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 13,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1686554527628924000,
    "trackingNumber": 845932704,
    "eventId": 24,
    "orderId": 7,
    "mpOrderId": 15644037020778,
    "mpId": 2087505414,
    "mpName": "user",
    "instrumentId": 14837,
    "instrument": "Spread",
    "side": "Buy",
    "price": 10,
    "quantity": 7,
    "tradeId": 7,
    "tradingMode": "CT",
    "makerTaker": "Maker",
    "tradeDate": "2023-06-12",
    "multiLegReportingType": "MultiLegSecurity"
  }
}
```
{% endtab %}

{% tab title="MultiLegSecurity" %}
<pre class="language-json"><code class="lang-json">//MultiLegSecurity trade for the side without an order 
//(in case of implied out trade)

<strong>{
</strong>  "q": "v1/exchange.market/trades",
  "sid": 3,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1711625611194461400,
    "trackingNumber": 1075309280,
    "eventId": 64,
    "mpId": -1,
    "mpName": "System",
    "instrumentId": 22730,
    "instrument": "1SP1",
    "side": "Sell",
    "price": 1,
    "quantity": 5,
    "tradeId": 12,
    "tradingMode": "CT",
    "makerTaker": "Maker",
    "tradeDate": "2024-03-28",
    "multiLegReportingType": "MultiLegSecurity"
  }
}




</code></pre>
{% endtab %}
{% endtabs %}

