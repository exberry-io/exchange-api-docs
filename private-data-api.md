# Private Data API

Private data API enables market participants to receive additional copies of the trading activity. This interface may also be used by participants to download the current status of all their active orders.

**Data Eligibility**&#x20;

Private data API can be consumed for single market participant (MP) or for group of MPs that are pre-configured by exchange operations team.

Notes:

* Empty field will not be included in the message

## massOrderStatus

Any participant can use the `massOrderStatus` API to retrieve the current status of all its own active orders.

{% hint style="info" %}
qualifier: `v1/exchange.market/massOrderStatus`
{% endhint %}

### **Response**

`orderMassStatus` response provides close to real time list of all current active orders with the entire details of those orders.\
Each `orderMassStatus`response includes`lastTrackingNumber`, the `trackingNumber`of the last event used to generate the orders list, this can be used as input in `executionReports`to be able start consume events from that point.\
\
Note: There are no request parameters.

| Field                 | Description                                                                                                                                                                                                       |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| messageType           | **OrderStatus**                                                                                                                                                                                                   |
| orderId               | Exchange order ID                                                                                                                                                                                                 |
| mpOrderId             | Same as in `placeOrder` request                                                                                                                                                                                   |
| orderType             | Same as in `placeOrder` request                                                                                                                                                                                   |
| side                  | Same as in `placeOrder` request                                                                                                                                                                                   |
| instrument            | Same as in `placeOrder` request                                                                                                                                                                                   |
| quantity              | Same as in `placeOrder` request                                                                                                                                                                                   |
| price                 | Same as in `placeOrder` request                                                                                                                                                                                   |
| timeInForce           | Same as in `placeOrder` request                                                                                                                                                                                   |
| expiryDate            | Same as in `placeOrder` request                                                                                                                                                                                   |
| orderTimestamp        | Order creation timestamp (in nanoseconds) in GMT                                                                                                                                                                  |
| marketModel           | <p>A - (Auction) when order was placed during auction</p><p>T - (Trading) when order was placed on continues trading mode<br>RA- (Resume Auction) when order was placed during auction after halt or autoHalt</p> |
| userId                | Same as in `placeOrder` request                                                                                                                                                                                   |
| accountId             | Same as in `placeOrder` request                                                                                                                                                                                   |
| parties               | Same as in `placeOrder` request                                                                                                                                                                                   |
| accountType           | Same as in `placeOrder` request                                                                                                                                                                                   |
| filledQuantity        | Total filled quantity                                                                                                                                                                                             |
| filledPrice           | Weighted average filled price for all fills on that order $$Sum(event.executedQuantity * event.executedPrice)/Sum(event.executedQuantity)$$                                                                       |
| remainingOpenQuantity | <p>Remaining open quantity.</p><p><span class="math">quantity - filledQuantity - removedQuantity</span></p>                                                                                                       |
| removedQuantity       | Quantity that was removed with modifyOrder request                                                                                                                                                                |
| lastEventTimestamp    | Last order event timestamp (in nanoseconds) in GMT                                                                                                                                                                |
| lastEventId           | Last event that was used to calculate order state                                                                                                                                                                 |
| mpId                  | MP Id                                                                                                                                                                                                             |
| mpName                | MP name                                                                                                                                                                                                           |

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
  "q": "v1/exchange.market/massOrderStatus",
  "sid": 100,
  "d": {}
}
```
{% endtab %}

{% tab title="Active Order" %}
```javascript
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
    "removedQuantity": 0,
    "filledPrice": 100.33,
    "marketModel": "T",
    "userId": "UATUserTest10",
    "lastEventTimestamp": 1617875864297023000,
    "lastEventId": 33,
    "mpId": 1958681073,
    "mpName": "Participant1"
  }
}
```
{% endtab %}

{% tab title="lastTrackingNumber" %}
```javascript
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

Any participant can use the `executionReports`API to subscribe to its own orders and trades events.\
There are few message types used in this API:

* **Add**
* **Cancelled**
* **Executed**
* **Modified**
* **TradeReport**
* **TradeCancel**

{% hint style="info" %}
qualifier: `v1/exchange.market/executionReports`
{% endhint %}

### **Request**

| Parameter                 | Type | Description                                                                                                                                                                                                                                                                                                                  |
| ------------------------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| trackingNumber `optional` | Long | <p>Determines the starting point of stream.<br>• When set to 0 - Stream will start from first event ever</p><p>• When empty - Stream will start from the next upcoming event</p><p>• When set to specific <code>trackingNumber</code>- Stream will start from the next event after the given <code>trackingNumber</code></p> |

### **Response**

| Message Type                                              | Field                 | Description                                                                                                                                                                                                                                                                                     |
| --------------------------------------------------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| All                                                       | messageType           | <p>One of the below values:</p><ul><li><strong>Add</strong></li><li><strong>Cancelled</strong></li><li><strong>Executed</strong></li><li><strong>Modified</strong></li><li><strong>TradeReport</strong></li><li><strong>TradeCancel</strong></li></ul>                                          |
| All                                                       | side                  | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| All                                                       | instrument            | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| All                                                       | quantity              | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| All                                                       | price                 | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| All                                                       | mpId                  | MP Id                                                                                                                                                                                                                                                                                           |
| All                                                       | mpName                | MP name                                                                                                                                                                                                                                                                                         |
| All                                                       | accountType           | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| All                                                       | parties               | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| All                                                       | eventId               | Sequence identifier per instrument for the event                                                                                                                                                                                                                                                |
| All                                                       | eventTimestamp        | Event timestamp (in nanoseconds) in GMT                                                                                                                                                                                                                                                         |
| All                                                       | trackingNumber        | Event tracking number                                                                                                                                                                                                                                                                           |
| <p>Add <br>Cancelled Executed Modified</p>                | orderId               | Exchange order ID                                                                                                                                                                                                                                                                               |
| <p>Add <br>Cancelled Executed Modified</p>                | mpOrderId             | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| <p>Add <br>Cancelled Executed Modified</p>                | orderType             | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| <p>Add <br>Cancelled Executed Modified</p>                | timeInForce           | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| <p>Add <br>Cancelled Executed Modified</p>                | expiryDate            | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| <p>Add <br>Cancelled Executed Modified</p>                | orderTimestamp        | Order creation timestamp (in nanoseconds) in GMT                                                                                                                                                                                                                                                |
| <p>Add <br>Cancelled Executed Modified</p>                | marketModel           | <p>A - (Auction) when order was placed during auction</p><p>T - (Trading) when order was placed on continues trading mode<br>RA- (Resume Auction) when order was placed during auction after halt or autoHalt</p>                                                                               |
| <p>Add <br>Cancelled Executed Modified<br>TradeCancel</p> | userId                | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| <p>Add <br>Cancelled Executed Modified<br>TradeCancel</p> | accountId             | Same as in `placeOrder` request                                                                                                                                                                                                                                                                 |
| <p>Add <br>Cancelled Executed Modified</p>                | filledQuantity        | Total filled quantity                                                                                                                                                                                                                                                                           |
| <p>Add <br>Cancelled Executed Modified</p>                | remainingOpenQuantity | <p>Remaining open quantity.</p><p><span class="math">quantity - filledQuantity - removedQuantity</span></p>                                                                                                                                                                                     |
| <p>Add <br>Cancelled Executed Modified</p>                | removedQuantity       | Quantity that was removed with modifyOrder request                                                                                                                                                                                                                                              |
| Executed                                                  | lastFilledQuantity    | Matched quantity                                                                                                                                                                                                                                                                                |
| Executed                                                  | lastFilledPrice       | Matched price (maker order price).                                                                                                                                                                                                                                                              |
| <p>Executed<br>TradeReport<br>TradeCancel</p>             | mathchId              | Unique ID for the match                                                                                                                                                                                                                                                                         |
| <p>Executed<br>TradeReport<br>TradeCancel</p>             | tradingMode           | <p>IA - (Scheduled Intraday Auction) -When execution was as part of auction<br>CT -  (Continuous Trading) - When execution was done on a regular trading<br>ON -  Trade Reporting (On Exchange)<br>UA- (Unscheduled Auction) - When execution was as part of auction after halt or autoHalt</p> |
| Cancelled                                                 | cancelledQuantity     | The cancelled quantity in the current cancelled event                                                                                                                                                                                                                                           |
| Modified                                                  | lastRemovedQuantity   | Removed quantity on current event                                                                                                                                                                                                                                                               |
| <p>TradeReport<br>TradeCancel</p>                         | tradeType             | <p>EFRP/Block/Other<br>On TradeCancel it will be shown only for TradeReport cancellation </p>                                                                                                                                                                                                   |

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
  "q": "v1/exchange.market/executionReports",
  "sid": 104,
  "d": {
    "trackingNumber":34256480
  }
}
```
{% endtab %}

{% tab title="Add" %}
```javascript
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
    "marketModel": "T",
    "userId": "UATUserTest1",
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
```javascript
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
    "marketModel": "T",
    "userId": "UATUserTest10",
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
```javascript
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
    "marketModel": "T",
    "userId": "UATUserTest10",
    "eventTimestamp": 1620913666099642000,
    "eventId": 44,
    "lastFilledQuantity": 1.25,
    "lastFilledPrice": 100.33,
    "matchId": 24,
    "tradingMode": "CT",
    "trackingNumber": 34272544,
    "mpId": 1958681073,
    "mpName": "Participant1"
  }
}
```
{% endtab %}

{% tab title="Modified" %}
```javascript
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
    "marketModel": "T",
    "userId": "UATUserTest10",
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
{% endtabs %}

### **Other Messages Samples**

{% tabs %}
{% tab title="TradeReport" %}
```javascript
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
```javascript
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
{% endtabs %}

## Trades&#x20;

`trades` API allows to get a real time stream for all the trades data&#x20;

Available messages in that API:&#x20;

* MatchedTrade: for order book trade&#x20;
* TradeReport: for trade entry trade&#x20;
* TradeCancel: for trade cancellation (separate records will be returned for cancellation)

{% hint style="info" %}
`qualifier: v1/exchange.market/trades`
{% endhint %}

### **Request**

| Parameter                 | Type | Description                                                                                                                                                                                                                                                                                                                  |
| ------------------------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| trackingNumber `optional` | Long | <p>Determines the starting point of stream.<br>• When set to 0 - Stream will start from first event ever</p><p>• When empty - Stream will start from the next upcoming event</p><p>• When set to specific <code>trackingNumber</code>- Stream will start from the next event after the given <code>trackingNumber</code></p> |

### **Response**

| Field          | Description                                                                                                                                                                                                                                                                           | Order Book | Trade Entry | Trade Cancel |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------: | :---------: | :----------: |
| eventId        |  Event Id                                                                                                                                                                                                                                                                             |      V     |      V      |       V      |
| timestamp      | Event timestamp (in nanoseconds) in GMT                                                                                                                                                                                                                                               |      V     |      V      |       V      |
| actionType     | <p>MatchedTrade </p><p>TradeReport </p><p>TradeCancel</p>                                                                                                                                                                                                                             |      V     |      V      |       V      |
| orderId        | Order id initiated the trade                                                                                                                                                                                                                                                          |      V     |             |              |
| mpOrderId      | From source order                                                                                                                                                                                                                                                                     |      V     |             |              |
| mpId           |  MP Id                                                                                                                                                                                                                                                                                |      V     |      V      |       V      |
| mpName         | MP Name                                                                                                                                                                                                                                                                               |      V     |      V      |       V      |
| instrumentId   |  Instrument id                                                                                                                                                                                                                                                                        |      V     |      V      |       V      |
| Instrument     | instrument symbol                                                                                                                                                                                                                                                                     |      V     |      V      |       V      |
| side           |  Buy/ Sell                                                                                                                                                                                                                                                                            |      V     |      V      |       V      |
| price          |  Trade price                                                                                                                                                                                                                                                                          |      V     |      V      |       V      |
| quantity       |  Trade quantity                                                                                                                                                                                                                                                                       |      V     |      V      |       V      |
| tradeId        | matchId                                                                                                                                                                                                                                                                               |      V     |      V      |       V      |
| tradingMode    | <p>IA - (Scheduled Intraday Auction) -When execution was as part of auction CT (Continuous Trading) - When execution was done on a regular trading ON - Trade Reporting (On Exchange)<br>UA- (Unscheduled Auction) - When execution was as part of auction after halt or autoHalt</p> |      V     |      V      |       V      |
| accountType    | Optional, From source order                                                                                                                                                                                                                                                           |      V     |      V      |       V      |
| parties        | Optional, From source order                                                                                                                                                                                                                                                           |      V     |      V      |     `opt`    |
| tradeType      | EFRP/Block/Other                                                                                                                                                                                                                                                                      |            |      V      |     `opt`    |
| makerTaker     | <p><strong>Taker</strong> if order was never resting on the book for that trade <br><strong>Maker</strong> if order was resting on the book for that trade</p>                                                                                                                        |      V     |             |              |
| tradeDate      | <p>Date of the business day of that trade<br>Format: YYY-MM-DD</p>                                                                                                                                                                                                                    |            |             |              |
| trackingNumber |  Tracking number                                                                                                                                                                                                                                                                      |            |             |              |

### **Error Codes**

| Code | Message                                         |
| ---- | ----------------------------------------------- |
| 1    | `Exchange is unavailable`                       |
| 2    | `Stream disconnected`                           |
| 1007 | `Invalid session`                               |
| 1008 | `This apiKey doesn’t have the right permission` |
| 1200 | `General error`                                 |
| 1201 | `Wrong trackingNumber`                          |

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
```javascript
{
  "q": "v1/exchange.market/trades",
  "sid": 16,
  "d": {
    "eventId": 27,
    "timestamp": 1662906852364973300,
    "actionType": "TradeReport",
    "mpId": 2087505339,
    "mpName": "mp1",
    "instrumentId": 963,
    "instrument": "ABC1",
    "side": "Buy",
    "price": 1.1234,
    "quantity": 10,
    "tradeId": 5,
    "tradingMode": "ON",
    "accountType": "Client",
    "parties": [
      {
        "id": "1234",
        "source": "D",
        "role": 38
      }
    ],
    "tradeType": "Block",
    "tradeDate": "2022-09-11",
    "trackingNumber": 217819392
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
{% endtabs %}

### **Strategies/ Multi Legs Handling**&#x20;

Strategies trades are sent on `trades` API by 2 models:

* Single Security: reports the strategy trade as is
* Individual leg of a multi-leg security: reports the underlying legs of the strategy&#x20;

Consumers can decide how to consume stratesies trades.

\
Each strategy related trade will be sent with the following new parameters:

| Parameter                    | Description                                                                              |
| ---------------------------- | ---------------------------------------------------------------------------------------- |
| multiLegReportingType        | <ul><li>SingleSecurity for parent trade</li><li>IndividualLeg for legs trades </li></ul> |
| tradeLegRefId                | leg index                                                                                |
| multiLegDifferentialPrice    | Parent trade price                                                                       |
| multiLegStrategyInstrumentId | Parent trade instrument Id                                                               |
| multiLegStrategyTradeId      | Parent trade Id                                                                          |

#### Samples

{% tabs %}
{% tab title="SingleSecurity" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 155,
  "d": {
    "actionType": "TradeReport",
    "timestamp": 1668524835153741000,
    "trackingNumber": 69920,
    "eventId": 1,
    "mpId": 2087505415,
    "mpName": "Test1",
    "instrumentId": 17,
    "instrument": "Test1Feb-Mar23",
    "side": "Buy",
    "price": 3,
    "quantity": 10,
    "tradeId": 1,
    "tradingMode": "ON",
    "accountType": "Client",
    "parties": [
      {
        "id": "33",
        "source": "D",
        "role": 38
      }
    ],
    "tradeType": "EFRP",
    "tradeDate": "2022-11-15",
    "multiLegReportingType": "SingleSecurity"
  }
}
```
{% endtab %}

{% tab title="IndividualLeg" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 155,
  "d": {
    "actionType": "TradeReport",
    "timestamp": 1668524835153741000,
    "trackingNumber": 70176,
    "mpId": 2087505415,
    "mpName": "Test1",
    "instrumentId": 14,
    "instrument": "Test1Feb23",
    "side": "Buy",
    "price": 1.5,
    "quantity": 10,
    "tradeId": 1,
    "tradingMode": "ON",
    "accountType": "Client",
    "parties": [
      {
        "id": "33",
        "source": "D",
        "role": 38
      }
    ],
    "tradeType": "EFRP",
    "tradeDate": "2022-11-15",
    "multiLegReportingType": "IndividualLeg",
    "tradeLegRefId": 1,
    "multiLegDifferentialPrice": 3,
    "multiLegStrategyInstrumentId": 17,
    "multiLegStrategyTradeId": 1
  }
}
```
{% endtab %}
{% endtabs %}

****
