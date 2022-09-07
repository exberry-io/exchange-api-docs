# Market Data API

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
* **InstrumentStatus**&#x20;
* **TradeReport**&#x20;
* **TradeCancel**&#x20;

There is 1 message types used to get indicative prices during auctions:

* **AuctionIndicativeEP**

**Anonymous vs Pseudonymous**\
Exchange can configure this stream to be anonymous or pseudonymous (mp`Id` is published but not any other market participant details).\
For anonymous configured streams all the `mpId` and `mpOrderId` data will not be published, except to the market participant that placed the order which will see only its own `mpId` and `mpOrderId` data.

{% hint style="info" %}
`endpoint: v2/exchange.market/orderBookDepth`
{% endhint %}

### **Request**

| Parameter                 | Type | Description                                                                                                                                                                                                                                                                                                                  |
| ------------------------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| trackingNumber `optional` | Long | <p>Determines the starting point of stream.<br>• When set to 0 - Stream will start from first event ever</p><p>• When empty - Stream will start from the next upcoming event</p><p>• When set to specific <code>trackingNumber</code>- Stream will start from the next event after the given <code>trackingNumber</code></p> |

### **Response**

#### **Add Order Message**

Add Order Message indicates that a new order has been accepted by the exchange and was added to the book.

| Field          | Description                                      |
| -------------- | ------------------------------------------------ |
| eventId        | Sequence identifier per instrument for the event |
| messageType    | **Add**                                          |
| eventTimestamp | Event timestamp (in nanoseconds) in GMT          |
| instrument     | Instrument symbol                                |
| orderId        | Exchange order ID                                |
| mpId           | Market participant ID                            |
| mpOrderId      | Market participant order ID                      |
| side           | Buy / Sell                                       |
| quantity       | Order quantity                                   |
| price          | Order price                                      |
| trackingNumber | Event tracking number                            |

#### **Order Executed Message**

Order Executed Message indicates that an order on the book is matched with a new coming order in whole or in part. It is possible to receive several Order Executed Messages for a single orderId.

| Field            | Description                                     |
| ---------------- | ----------------------------------------------- |
| eventId          | Identifier for the event, unique per instrument |
| messageType      | **Executed**                                    |
| eventTimestamp   | Event timestamp (in nanoseconds) in GMT         |
| instrument       | Instrument symbol                               |
| makerMpId        | Resting order market participant ID             |
| makerMpOrderId   | Resting order market participant order ID       |
| makerOrderId     | Resting `orderId`                               |
| takerMpId        | Aggressive order market participant ID          |
| takerMpOrderId   | Aggressive order market participant order ID    |
| takerOrderId     | Aggressive order ID                             |
| matchId          | Unique ID for the match                         |
| executedQuantity | Matched quantity                                |
| executedPrice    | Matched price (maker order price).              |
| trackingNumber   | Event tracking number                           |

#### **Order Cancel Message**

Order Cancel Message indicates that an order on the book is being cancelled. This message also sent in case of market order that was not fully filled.

| Field             | Description                                     |
| ----------------- | ----------------------------------------------- |
| messageType       | **Cancelled**                                   |
| eventId           | Identifier for the event, unique per instrument |
| eventTimestamp    | Event timestamp (in nanoseconds) in GMT         |
| instrument        | Instrument symbol                               |
| side              | Buy/ Sell                                       |
| orderId           | Exchange order ID                               |
| mpId              | Market participant ID                           |
| mpOrderId         | Market participant order ID                     |
| cancelledQuantity | Order cancelled quantity                        |
| trackingNumber    | Event tracking number                           |

#### **Order Modify Message**

Order Modify Message indicates that an order on the book is being modified and order quantity was reduced.

| Field           | Description                                     |
| --------------- | ----------------------------------------------- |
| messageType     | **Modified**                                    |
| eventId         | Identifier for the event, unique per instrument |
| eventTimestamp  | Event timestamp (in nanoseconds) in GMT         |
| instrument      | Instrument symbol                               |
| orderId         | Exchange order ID                               |
| mpId            | Market participant ID                           |
| mpOrderId       | Market participant order ID                     |
| removedQuantity | Order quantity that was removed                 |
| newQuantity     | Remaining open quantity                         |
| trackingNumber  | Event tracking number                           |

#### **Instrument Status Message**

Instrument Status Message indicates that instrument status was changed.\
This message specify only the data that was actually changed, to get the snapshot of current instruments status you should use `orderBookState`\
Note: when creating new instrument the first message will contain `tradingStatus` and status, those will be followed with separate event for `marketStatus`

| Field          | Description                                  |
| -------------- | -------------------------------------------- |
| messageType    | **InstrumentStatus**                         |
| eventTimestamp | Event timestamp (in nanoseconds) in GMT      |
| instrument     | Instrument symbol                            |
| tradingStatus  | Trade/ Halt                                  |
| marketStatus   | Opened/ Closed/ AuctionCall/ AuctionCrossing |
| status         | Active/ Disabled                             |
| trackingNumber | Event tracking number                        |

#### **Auction Indicative Equilibrium Price Message**

Auction Indicative Equilibrium Price Message publish the indicative equilibrium price during an auction.\
This message is sent only on real time during auctions and will not be sent if subscribing after the auction.

| Field             | Description                                                                                                               |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------- |
| eventId           | Sequence identifier per instrument for the event                                                                          |
| messageType       | **AuctionIndicativeEP**                                                                                                   |
| eventTimestamp    | Event timestamp (in nanoseconds) in GMT                                                                                   |
| instrument        | Instrument symbol                                                                                                         |
| IndicativePrice   | The indicative price upon which the paired shares and the imbalance quantity are based                                    |
| pairedQuantity    | The quantity to be matched at the current indicative price                                                                |
| imbalanceQuantity | The absolute value of quantity that would remain unexecuted at the current indicative price.                              |
| imbalanceSide     | <p>Buy - Buy side imbalance</p><p>Sell - Sell side imbalance</p><p>None - No imbalance</p><p>NA - no indicative price</p> |
| bestBuyPrice      | In case of No match - the highest buy order price                                                                         |
| bestBuyQuantity   | In case of No match - the highest buy order quantity                                                                      |
| bestSellPrice     | In case of No match - the lowest sell order price                                                                         |
| bestSellQuantity  | In case of No match - the lowest sell order quantity                                                                      |

#### Trade Report **Message**

Trade report Message indicates that Trade Entry️ was captured, it means that trade was done between market participants out of the order book.&#x20;

| Field          | Description                                                             |
| -------------- | ----------------------------------------------------------------------- |
| messageType    | **TradeReport**                                                         |
| eventId        | Identifier for the event, unique per instrument                         |
| eventTimestamp | Event timestamp (in nanoseconds) in GMT                                 |
| instrument     | Instrument symbol                                                       |
| tradeType      | EFRP/Block/Other                                                        |
| matchId        | <p>Trade Id<br>This will use the same sequence as order book trades</p> |
| quantity       | Trade quantity                                                          |
| price          | Trade price                                                             |
| buyMpId        | Buy MP Id                                                               |
| sellMpId       | Sell MP Id                                                              |
| trackingNumber | Event tracking number                                                   |

### **Error Codes**

| Code | Message                                         |
| ---- | ----------------------------------------------- |
| 1    | `Exchange is unavailable`                       |
| 2    | `Stream disconnected`                           |
| 1007 | `Invalid session`                               |
| 1008 | `This apiKey doesn’t have the right permission` |
| 1200 | `General error`                                 |
| 1201 | `Wrong trackingNumber`                          |

### **Orders Messages Samples**

{% tabs %}
{% tab title="Subscription" %}
```javascript
{
  "q": "v1/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
      "trackingNumber": 100
  }
}
```
{% endtab %}

{% tab title="Add" %}
```javascript
{
  "q": "v1/exchange.market/orderBookDepth",
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
    "trackingNumber": 100
  }
}
```
{% endtab %}

{% tab title="Cancelled" %}
```javascript
{
  "q": "v1/exchange.market/orderBookDepth",
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
    "trackingNumber": 200
  }
}
```
{% endtab %}

{% tab title="Executed" %}
```javascript
{
  "q": "v1/exchange.market/orderBookDepth",
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
    "trackingNumber": 300
  }
}
```
{% endtab %}

{% tab title="Modified" %}
```javascript
{
  "q": "v1/exchange.market/orderBookDepth",
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
    "removedQuantity": 0.8,
    "newQuantity": 0.5
  }
}
```
{% endtab %}
{% endtabs %}

### **Orders Messages Samples**

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
```javascript
{
  "q": "v1/exchange.market/orderBookDepth",
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

{% tab title="Executed" %}
```javascript
{
  "q": "v1/exchange.market/orderBookDepth",
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
```javascript
{
  "q": "v1/exchange.market/orderBookDepth",
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
{% endtabs %}

## orderBookState

This stream provides close to real time snapshot of order book state for all traded instruments.\
The result are the list of:

* All instruments and their current status
* All orders currently resting on the book, it is not aggregated by price level.

Each `orderBookState`stream will be closed with `lastTrackingNumber`, the `trackingNumber`of the last event used as part of returned book state, this can be used as input in `orderBookDepth`to be able start consume from that point.\
\
Note: There are no request parameters.

{% hint style="info" %}
`endpoint: v2/exchange.market/orderBookState`
{% endhint %}

### **Response**

#### **Order Message**

| Parameter          | Type    | Description               |
| ------------------ | ------- | ------------------------- |
| messageType        | String  | **Order**                 |
| orderId            | Long    | Exchange Order ID         |
| side               | String  | Buy or Sell               |
| instrument         | String  | Instrument identifier     |
| quantity           | Decimal | Order quantity            |
| price              | Decimal | Order price               |
| lastTrackingNumber | Long    | Last event trackingNumber |

#### Instrument Status Message

| Field          | Description                                  |
| -------------- | -------------------------------------------- |
| messageType    | **InstrumentStatus**                         |
| eventTimestamp | Event timestamp (in nanoseconds) in GMT      |
| instrument     | Instrument symbol                            |
| tradingStatus  | Trade/ Halt                                  |
| marketStatus   | Opened/ Closed/ AuctionCall/ AuctionCrossing |
| status         | Active/ Disabled                             |
| trackingNumber | Event tracking number                        |

### **Error Codes**

| Code | Message                                         |
| ---- | ----------------------------------------------- |
| 1    | `Temporary failure to retrieve this data`       |
| 1007 | `Invalid session`                               |
| 1008 | `This apiKey doesn’t have the right permission` |

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```javascript
{
  "q": "v1/exchange.market/orderBookState",
  "sid": 100,
  "d": {}
}
```
{% endtab %}

{% tab title="Resting Orders" %}
```javascript
{
  "q": "v1/exchange.market/orderBookState",
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
```javascript
{
  "q": "v1/exchange.market/orderBookState",
  "sid": 100,
  "d": {
    "lastTrackingNumber": 161888
  }
}
```
{% endtab %}
{% endtabs %}
