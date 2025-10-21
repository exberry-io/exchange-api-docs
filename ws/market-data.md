# Market Data

## Building Book State

**Using Raw Data**\
In order to build book state that include all the orders since first order, need to consume all `orderBookDepth` events per instrument and apply it on the right sequence.\
Consumer has the option if to subscribe to the first event (in case that need to build book state from scratch) or to subscribe from the last consumed event (in case that some events were already processed).\
\
**Using Snapshot**\
In order to build current book state without all the orders but only the current resting orders, need to consume `orderBookState` stream the provides list of all the current resting orders. By using returned `lastTrackingNumber`to subscribe to `orderBookDepth` consumer can no catch the real time order book state.

## orderBookDepth

The `orderBookDepth` stream provides the full order book depth data. This stream is public and all consumers receive the messages simultaneously.\
\
The `orderBookDepth` publish only raw data without any aggregation, hence in order to determine the current state of an order, the subscriber needs to maintain the updated order state as per all the messages for that order.

There are few message types used to get real time market data:

* **Add**&#x20;
* **Cancelled**&#x20;
* **Executed**&#x20;
* **Modified**&#x20;
* **NonDisplayTrade**
* **TradeReport**&#x20;
* **TradeCancel**&#x20;
* **InstrumentStatus**&#x20;
* **calendarEndOfDay**

There is 1 message types used to get indicative prices during auctions:

* **AuctionIndicativeEP**

**eventId**

eventId of the orderBookDepth messages are not sequential. There can be missing eventId values due to activities related to reserve orders, and suspended orders.&#x20;

**Anonymous vs Pseudonymous**\
Exchange can configure this stream to be anonymous or pseudonymous (mp`Id` is published but not any other market participant details).\
For anonymous configured streams all the `mpId` and `mpOrderId` data will not be published, except to the market participant that placed the order which will see only its own `mpId` and `mpOrderId` data.

Below messages are generated only for orders with display quantities.

* #### **Add**
* #### **Executed**
* **Cancelled**



For an edge case in CBR - see more [here](https://documenter.getpostman.com/view/6229811/TzCV3jcq#46843413-c6ad-44c0-8d67-e18ef719b365)

{% hint style="info" %}
qualifier: v2/exchange.market/orderBookDepth
{% endhint %}

### **Request**

<table><thead><tr><th width="146.33333333333331">Parameter</th><th width="75">Type</th><th width="483.66666666666674">Description</th></tr></thead><tbody><tr><td>trackingNumber <code>optional</code></td><td>Long</td><td>Same as defined in executionReports<a data-mention href="market-data.md#request">#request</a></td></tr></tbody></table>

### **Response**

#### **Add Order Message**

Add Order Message indicates that a new order has been accepted by the exchange and was added to the book.

<table><thead><tr><th width="223">Field</th><th>Description</th></tr></thead><tbody><tr><td>eventId</td><td>Identifier for the event, unique per instrument</td></tr><tr><td>messageType</td><td><strong>Add</strong></td></tr><tr><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>instrument</td><td>Instrument symbol</td></tr><tr><td>orderId</td><td>Exchange order ID</td></tr><tr><td>mpId</td><td>Market participant ID</td></tr><tr><td>mpOrderId</td><td>Market participant order ID</td></tr><tr><td>side</td><td>Buy / Sell</td></tr><tr><td>quantity</td><td>Order quantity</td></tr><tr><td>price</td><td>Order price<br>will be empty for market order only during auction</td></tr><tr><td>trackingNumber</td><td>Event tracking number</td></tr></tbody></table>

#### **Order Cancel Message**

Order Cancel Message indicates that an order on the book is being cancelled. This message also sent in case of market order that was not fully filled.

<table><thead><tr><th width="245">Field</th><th>Description</th></tr></thead><tbody><tr><td>messageType</td><td><strong>Cancelled</strong></td></tr><tr><td>eventId</td><td>Identifier for the event, unique per instrument</td></tr><tr><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>instrument</td><td>Instrument symbol</td></tr><tr><td>side</td><td>Buy/ Sell</td></tr><tr><td>orderId</td><td>Exchange order ID</td></tr><tr><td>mpId</td><td>Market participant ID</td></tr><tr><td>mpOrderId</td><td>Market participant order ID</td></tr><tr><td>cancelledQuantity</td><td>Order cancelled quantity</td></tr><tr><td>trackingNumber</td><td>Event tracking number</td></tr></tbody></table>



#### **Order Executed Message**

Order Executed Message indicates that an order on the book is matched with a new coming order in whole or in part. It is possible to receive several Order Executed Messages for a single orderId.

<table><thead><tr><th width="234">Field</th><th>Description</th></tr></thead><tbody><tr><td>eventId</td><td>Identifier for the event, unique per instrument</td></tr><tr><td>messageType</td><td><strong>Executed</strong></td></tr><tr><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>instrument</td><td>Instrument symbol</td></tr><tr><td>makerMpId</td><td>Resting order market participant ID</td></tr><tr><td>makerMpOrderId</td><td>Resting order market participant order ID</td></tr><tr><td>makerOrderId</td><td>Resting <code>orderId</code></td></tr><tr><td>takerMpId</td><td>Aggressive order market participant ID</td></tr><tr><td>takerMpOrderId</td><td>Aggressive order market participant order ID</td></tr><tr><td>takerOrderId</td><td>Aggressive order ID</td></tr><tr><td>matchId</td><td>Unique ID for the match</td></tr><tr><td>executedQuantity</td><td>Matched quantity</td></tr><tr><td>executedPrice</td><td>Matched price (maker order price).</td></tr><tr><td>trackingNumber</td><td>Event tracking number</td></tr></tbody></table>

#### **Order Modify Message**

Order Modify Message indicates that an order on the book is being modified <mark style="color:blue;">(NEW v1.48.0)</mark> (without losing priority) or replaced (losing priority) <mark style="color:red;">(REMOVED 1.48.0)</mark> ~~and order quantity was reduced.~~

<table><thead><tr><th width="256">Field</th><th>Description</th></tr></thead><tbody><tr><td>messageType</td><td><strong>Modified</strong></td></tr><tr><td>eventId</td><td>Identifier for the event, unique per instrument</td></tr><tr><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>instrument</td><td>Instrument symbol</td></tr><tr><td><mark style="color:blue;">(NEW v1.48.0)</mark> side</td><td>Buy/ Sell</td></tr><tr><td>orderId</td><td>Exchange order ID</td></tr><tr><td>mpId</td><td>Market participant ID</td></tr><tr><td>mpOrderId</td><td>Market participant order ID</td></tr><tr><td><mark style="color:red;">(REMOVED v1.48.0)</mark><br><del>removedQuantity</del></td><td><del>Order quantity that was removed</del></td></tr><tr><td>newQuantity</td><td>Remaining open quantity</td></tr><tr><td><mark style="color:blue;">(NEW v1.48.0)</mark><br>price</td><td>Order price</td></tr><tr><td><mark style="color:blue;">(NEW v1.48.0)</mark><br>lostPriority</td><td><p>Indicates if replaced/modified order has caused an order to lose priority.</p><p>Allowed values:</p><ul><li>true - order has lost time priority </li><li>false - order has not lost time priority </li></ul></td></tr><tr><td>trackingNumber</td><td>Event tracking number</td></tr></tbody></table>

#### **NonDisplayTrade Message**

NonDisplayTrade message indicates a match with a resting hidden quantity of an order. This message is the same as the “Executed” message and must be treated identically except that there is no impact on visible book state.&#x20;

Fields are similar to the order [Executed](market-data.md#order-executed-message) message except `messageType` is **NonDisplayTrade**



#### Trade Report **Message**

Trade report Message indicates that Trade Entry️ was captured, it means that trade was done between market participants out of the order book.&#x20;

<table><thead><tr><th width="229">Field</th><th>Description</th></tr></thead><tbody><tr><td>messageType</td><td><strong>TradeReport</strong></td></tr><tr><td>eventId</td><td>Identifier for the event, unique per instrument</td></tr><tr><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>instrument</td><td>Instrument symbol</td></tr><tr><td>tradeType</td><td>(<mark style="color:blue;">CHANGED v1.41.0)</mark> <del>EFRP/Block/Other</del> Refer <a href="https://documenter.getpostman.com/view/6229811/TzCV3jcq#909a5a9c-3385-43af-9569-ea3168a331e9">Admin API Trade Entry</a>/type field for values; all values are supported.</td></tr><tr><td>matchId</td><td>Trade Id<br>This will use the same sequence as order book trades</td></tr><tr><td>quantity</td><td>Trade quantity</td></tr><tr><td>price</td><td>Trade price</td></tr><tr><td>buyMpId</td><td>Buy MP Id</td></tr><tr><td>sellMpId</td><td>Sell MP Id</td></tr><tr><td>trackingNumber</td><td>Event tracking number</td></tr></tbody></table>



**Trade Cancel Message**

Trade Cancel Message indicates that a Trade was canceled.&#x20;

<table><thead><tr><th width="229">Field</th><th>Description</th></tr></thead><tbody><tr><td>messageType</td><td><strong>TradeCancel</strong></td></tr><tr><td>eventId</td><td>Identifier for the event, unique per instrument</td></tr><tr><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>instrument</td><td>Instrument symbol</td></tr><tr><td>matchId</td><td>Trade Id<br>This will use the same sequence as order book trades</td></tr><tr><td>quantity</td><td>Quantity of cancelled Trade</td></tr><tr><td>price</td><td>Price of cancelled Trade</td></tr><tr><td>buyMpId</td><td>Buy MP Id cancelled Trade</td></tr><tr><td>sellMpId</td><td>Sell MP Id cancelled Trade</td></tr><tr><td>trackingNumber</td><td>Event tracking number</td></tr></tbody></table>



#### **Instrument Status Message**

Instrument Status Message indicates that instrument status was changed.\
This message specify only the data that was actually changed, to get the snapshot of current instruments status you should use `orderBookState`\


Notes:&#x20;

* When creating new instrument the first message will contain `tradingStatus` and status, those will be followed with separate event for `marketStatus`
* When changing instrument status from "Disabled" to "Active",  `tradingStatus` and `marketStatus` will be sent on the same message along with `status`&#x20;

<table><thead><tr><th width="267">Field</th><th>Description</th></tr></thead><tbody><tr><td>messageType</td><td><strong>InstrumentStatus</strong></td></tr><tr><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>instrument</td><td>Instrument symbol</td></tr><tr><td>previousSymbol</td><td>In case symbol was changed, the previous symbol of the instrument</td></tr><tr><td>tradingStatus</td><td>Trade/ Halt</td></tr><tr><td>marketStatus</td><td>Opened/ Closed/ AuctionCall/ AuctionCrossing</td></tr><tr><td>status</td><td>Active/ Disabled</td></tr><tr><td>trackingNumber</td><td>Event tracking number</td></tr></tbody></table>

#### Calendar End Of Day Message

The calendar End Of Day Message indicates that the end-of-day for a specific calendar is triggered.

<table><thead><tr><th width="229">Field</th><th>Description</th></tr></thead><tbody><tr><td>messageType</td><td><strong>calendarEndOfDay</strong></td></tr><tr><td>eventTimestamp</td><td>Event timestamp (in nanoseconds)</td></tr><tr><td>calendarId</td><td>Calendar Id</td></tr><tr><td>calendarName</td><td>Calendar name</td></tr><tr><td>eodDate</td><td><p>The trade date of the day that was closed.</p><p>Format: yyyy-mm-dd</p></td></tr><tr><td>trackingNumber</td><td>Event tracking number</td></tr></tbody></table>



#### **Auction Indicative Equilibrium Price Message**

Auction Indicative Equilibrium Price Message publish the indicative equilibrium price during an auction.\
This message is sent only on real time during auctions and will not be sent if subscribing after the auction.

In case there is no order on one of the side the price and quantity fields will be shown with 0

For market orders during an auction, system will send the `bestBuyQuantity / bestSellQuantity` corresponding to the market order, if it exists, without a price.&#x20;

<table><thead><tr><th width="264">Field</th><th>Description</th></tr></thead><tbody><tr><td>eventId</td><td>Identifier for the event, unique per instrument</td></tr><tr><td>messageType</td><td><strong>AuctionIndicativeEP</strong></td></tr><tr><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>instrument</td><td>Instrument symbol</td></tr><tr><td>IndicativePrice</td><td>The indicative price upon which the paired shares and the imbalance quantity are based</td></tr><tr><td>pairedQuantity</td><td>The quantity to be matched at the current indicative price</td></tr><tr><td>imbalanceQuantity</td><td>The absolute value of quantity that would remain unexecuted at the current indicative price.</td></tr><tr><td>imbalanceSide</td><td><p>Buy - Buy side imbalance</p><p>Sell - Sell side imbalance</p><p>None - No imbalance</p><p>NA - no indicative price</p></td></tr><tr><td>bestBuyPrice</td><td>In case of No match - the highest buy order price</td></tr><tr><td>bestBuyQuantity</td><td>In case of No match - the highest buy order quantity</td></tr><tr><td>bestSellPrice</td><td>In case of No match - the lowest sell order price</td></tr><tr><td>bestSellQuantity</td><td>In case of No match - the lowest sell order quantity</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="193">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>2</td><td><code>Stream disconnected</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1200</td><td><code>General error</code></td></tr><tr><td>1201</td><td><code>Wrong trackingNumber</code></td></tr></tbody></table>

### **Orders Messages Samples**

{% tabs %}
{% tab title="Subscription" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
      "trackingNumber": 100
  }
}
```
{% endtab %}

{% tab title="Add" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
    "eventId": 29969,
    "messageType": "Add",
    "eventTimestamp": 1565790374956123123,
    "instrument": "AMZ",
    "orderId": 20012,
    "mpId": "12",
    "mpOrderId": 1565790360561,
    "side": "Buy",
    "quantity": 1,
    "price": 1.22,
    "trackingNumber": 100,
    "realBookState": true
  }
}
```
{% endtab %}

{% tab title="Cancelled" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
    "eventId": 29969,
    "messageType": "Cancelled",
    "eventTimestamp": 1565790374956123123,
    "instrument": "AMZ",
    "orderId": 20012,
    "side": "Sell",
    "mpId": 2,
    "mpOrderId": 1569161878394,
    "cancelledQuantity": 2.2,
    "trackingNumber": 200,
    "realBookState": true
  }
}
```
{% endtab %}

{% tab title="Executed" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
    "eventId": 29969,
    "messageType": "Executed",
    "eventTimestamp": 1565790374956123123,
    "instrument": "AMZ",
    "makerMpId": 2,
    "makerMpOrderId": 1564403702076,
    "makerOrderId": 1,
    "takerMpId": 2,
    "takerMpOrderId": 1569161798700,
    "takerOrderId": 2,
    "matchId": 1,
    "executedQuantity": 1,
    "executedPrice": 10.25,
    "trackingNumber": 300,
    "realBookState": true
  }
}
```
{% endtab %}

{% tab title="Modified" %}
<pre class="language-json"><code class="lang-json"><strong>//NEW v1.48.0
</strong>{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 15,
  "d": {
    "messageType": "Modified",
    "eventTimestamp": 1760967019875993000,
    "eventId": 59,
    "trackingNumber": 507888608,
    "instrument": "AMZ",
    "orderId": 27,
    "mpId": 20,
    "mpOrderId": 1760966630572,
    "side": "Buy",
    "newQuantity": 80,
    "price": 30,
    "lostPriority": false
  }
}

<strong>//REMOVED v1.48.0
</strong>{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
    "eventId": 279,
    "messageType": "Modified",
    "eventTimestamp": 1615374689625629000,
    "instrument": "INS1",
    "trackingNumber": 27024768,
    "mpId": 1958681073,
    "mpOrderId": 2003,
    "orderId": 192,
    "removedQuantity": 0.8, REMOVED v1.48.0
    "newQuantity": 0.5,
    "realBookState": true
  }
}

</code></pre>
{% endtab %}

{% tab title="NonDisplayTrade" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 15,
  "d": {
    "messageType": "NonDisplayTrade",
    "eventTimestamp": 1726742610562105600,
    "eventId": 3,
    "trackingNumber": 3311663360,
    "instrument": "ABC",
    "makerOrderId": 1,
    "makerMpId": 14,
    "makerMpOrderId": 156441381102411,
    "takerOrderId": 2,
    "takerMpId": 14,
    "takerMpOrderId": 1726742610373,
    "matchId": 2,
    "executedQuantity": 20,
    "executedPrice": 56
  }
}
```
{% endtab %}
{% endtabs %}

### **Other Messages Samples**

{% tabs %}
{% tab title="InstrumentStatus" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 101,
  "d": {
    "messageType": "InstrumentStatus",
    "eventTimestamp": 1652333371407456300,
    "instrument": "INS1",
    "tradingStatus": "Halt",
    "trackingNumber": 3159035424
  }
}
```
{% endtab %}

{% tab title="AuctionIndicativeEP" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 11,
  "d": {
    "eventId": 433,
    "messageType": "AuctionIndicativeEP",
    "eventTimestamp": 1605110670005410000,
    "instrument": "DUD",
    "indicativePrice": 101,
    "pairedQuantity": 5.3,
    "imbalanceQuantity": 6,
    "imbalanceSide": "Buy",
    "bestBuyPrice": 0,
    "bestBuyQuantity": 0,
    "bestSellPrice": 0,
    "bestSellQuantity": 0
  }
}
```
{% endtab %}

{% tab title="TradeReport" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
    "eventId": 29969,
    "messageType": "TradeReport",
    "tradeType": "Block",
    "matchId": 12345,
    "eventTimestamp": 1565790374956123123,
    "instrument": "AMZ",
    "buyMpId": "12",
    "sellMpId": "13",
    "quantity": 1,
    "price": 1.22,
    "trackingNumber": 100
  }
}
```
{% endtab %}

{% tab title="TradeCancel" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
    "eventId": 29969,
    "messageType": "TradeCancel",
    "matchId": 12345,
    "eventTimestamp": 1565790374956123123,
    "instrument": "AMZ",
    "buyMpId": "12",
    "sellMpId": "13",
    "quantity": 1,
    "price": 1.22,
    "trackingNumber": 100
  }
}
```
{% endtab %}

{% tab title="EOD " %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 18,
  "d": {
    "messageType": "CalendarEndOfDay",
    "eventTimestamp": 1681726468141994200,
    "calendarId": 123,
    "calendarName": "NewCalendar",
    "eodDate": "2023-04-17",
    "trackingNumber": 407807712
  }
}
```
{% endtab %}
{% endtabs %}

## orderBookState

This stream provides close to real time snapshot of order book state for all traded instruments.\
The result are the list of:

* All  active instruments&#x20;
* All orders currently resting on the book, it is not aggregated by price level. Messages are not generated for hidden quantites of orders.

Each `orderBookState`stream will be closed with `lastTrackingNumber`, the `trackingNumber`of the last event used as part of returned book state, this can be used as input in `orderBookDepth`to be able start consume from that point.

&#x20;The response is sorted by price and time and side.\
\
Note: There are no request parameters.

{% hint style="info" %}
qualifier:v2/exchange.market/orderBookState
{% endhint %}

### **Response**

#### **Order Message**

<table><thead><tr><th width="207.33333333333331">Parameter</th><th width="135">Type</th><th>Description</th></tr></thead><tbody><tr><td>messageType</td><td>String</td><td><strong>Order</strong></td></tr><tr><td>orderId</td><td>Long</td><td>Exchange Order ID</td></tr><tr><td>side</td><td>String</td><td>Buy or Sell</td></tr><tr><td>instrument</td><td>String</td><td>Instrument identifier</td></tr><tr><td>quantity</td><td>Decimal</td><td>Order quantity<br>In case of Iceberg orders, working display quantity</td></tr><tr><td>price</td><td>Decimal</td><td>Order price<br>will be empty for market order only during auction</td></tr><tr><td>lastTrackingNumber</td><td>Long</td><td>Last event trackingNumber</td></tr></tbody></table>

#### Instrument Status Message

<table><thead><tr><th width="225">Field</th><th>Description</th></tr></thead><tbody><tr><td>messageType</td><td><strong>InstrumentStatus</strong></td></tr><tr><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>instrument</td><td>Instrument symbol</td></tr><tr><td>tradingStatus</td><td>Trade/ Halt</td></tr><tr><td>marketStatus</td><td>Opened/ Closed/ AuctionCall/ AuctionCrossing</td></tr><tr><td>status</td><td>Active</td></tr><tr><td>trackingNumber</td><td>Event tracking number</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="149">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Temporary failure to retrieve this data</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```json
{
  "q": "v2/exchange.market/orderBookState",
  "sid": 100,
  "d": {}
}
```
{% endtab %}

{% tab title="Resting Orders" %}
```json
{
  "q": "v2/exchange.market/orderBookState",
  "sid": 100,
  "d": {
    "orderId": 908,
    "side": "Buy",
    "instrument": "AMZ",
    "quantity": 551.3,
    "price": 155.33
  }
}
```
{% endtab %}

{% tab title="Instrument Status" %}
```json
{
  "q": "v2/exchange.market/orderBookState",
  "sid": 100,
  "d": {
    "messageType": "InstrumentStatus",
    "instrument": "INS1",
    "tradingStatus": "Trade",
    "marketStatus": "Opened",
    "status": "Active"
  }
}
```
{% endtab %}

{% tab title="lastTrackingNumber" %}
```json
{
  "q": "v2/exchange.market/orderBookState",
  "sid": 100,
  "d": {
    "lastTrackingNumber": 161888
  }
}
```
{% endtab %}
{% endtabs %}
