# Private Data API

Private data API enables market participants to receive additional copies of the trading activity. This interface may also be used by clients to download the current status of all their active orders.

Notes:

* Empty field will not be included in the message

## massOrderStatus

Any participant can use the `massOrderStatus` API to retrieve the current status of all its own active orders.

{% hint style="info" %}
`endpoint: v1/exchange.market/massOrderStatus`
{% endhint %}

### **Response**

`orderMassStatus` response provides close to real time list of all current active orders with the entire details of those orders.\
Each `orderMassStatus`response includes`lastTrackingNumber`, the `trackingNumber`of the last event used to generate the orders list, this can be used as input in `executionReports`to be able start consume events from that point.\
\
Note: There are no request parameters.

| Field                 | Description                                                                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| messageType           | **OrderStatus**                                                                                                                             |
| orderId               | Exchange order ID                                                                                                                           |
| mpOrderId             | Same as in `placeOrder` request                                                                                                             |
| orderType             | Same as in `placeOrder` request                                                                                                             |
| side                  | Same as in `placeOrder` request                                                                                                             |
| instrument            | Same as in `placeOrder` request                                                                                                             |
| quantity              | Same as in `placeOrder` request                                                                                                             |
| price                 | Same as in `placeOrder` request                                                                                                             |
| timeInForce           | Same as in `placeOrder` request                                                                                                             |
| expiryDate            | Same as in `placeOrder` request                                                                                                             |
| orderTimestamp        | Order creation timestamp (in nanoseconds) in GMT                                                                                            |
| marketModel           | <p>A - (Auction) when order was placed during auction</p><p>T - (Trading) when order was placed on continues trading mode</p>               |
| userId                | Same as in `placeOrder` request                                                                                                             |
| accountId             | Same as in `placeOrder` request                                                                                                             |
| filledQuantity        | Total filled quantity                                                                                                                       |
| filledPrice           | Weighted average filled price for all fills on that order $$Sum(event.executedQuantity * event.executedPrice)/Sum(event.executedQuantity)$$ |
| remainingOpenQuantity | <p>Remaining open quantity.</p><p><span class="math">quantity - filledQuantity - removedQuantity</span></p>                                 |
| removedQuantity       | Quantity that was removed with modifyOrder request                                                                                          |
| lastEventTimestamp    | Last order event timestamp (in nanoseconds) in GMT                                                                                          |
| lastEventId           | Last event that was used to calculate order state                                                                                           |

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
    "lastEventId": 33
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

Any participant can use the `executionReports`API to subscribe to its own orders events.\
There are few message types used in this API:

* **Add**
* **Cancelled**
* **Executed**
* **Modified**

{% hint style="info" %}
`endpoint: v1/exchange.market/executionReports`
{% endhint %}

### **Request**

| Parameter                 | Type | Description                                                                                                                                                                                                                                                                                                                  |
| ------------------------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| trackingNumber `optional` | Long | <p>Determines the starting point of stream.<br>• When set to 0 - Stream will start from first event ever</p><p>• When empty - Stream will start from the next upcoming event</p><p>• When set to specific <code>trackingNumber</code>- Stream will start from the next event after the given <code>trackingNumber</code></p> |

### **Response**

| Message Type | Field                 | Description                                                                                                                                                                  |
| ------------ | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| All          | messageType           | <p>One of the below values:</p><ul><li><strong>Add</strong></li><li><strong>Cancelled</strong></li><li><strong>Executed</strong></li><li><strong>Modified</strong></li></ul> |
| All          | orderId               | Exchange order ID                                                                                                                                                            |
| All          | mpOrderId             | Same as in `placeOrder` request                                                                                                                                              |
| All          | orderType             | Same as in `placeOrder` request                                                                                                                                              |
| All          | side                  | Same as in `placeOrder` request                                                                                                                                              |
| All          | instrument            | Same as in `placeOrder` request                                                                                                                                              |
| All          | quantity              | Same as in `placeOrder` request                                                                                                                                              |
| All          | price                 | Same as in `placeOrder` request                                                                                                                                              |
| All          | timeInForce           | Same as in `placeOrder` request                                                                                                                                              |
| All          | expiryDate            | Same as in `placeOrder` request                                                                                                                                              |
| All          | orderTimestamp        | Order creation timestamp (in nanoseconds) in GMT                                                                                                                             |
| All          | marketModel           | <p>A - (Auction) when order was placed during auction</p><p>T - (Trading) when order was placed on continues trading mode</p>                                                |
| All          | userId                | Same as in `placeOrder` request                                                                                                                                              |
| All          | accountId             | Same as in `placeOrder` request                                                                                                                                              |
| All          | filledQuantity        | Total filled quantity                                                                                                                                                        |
| All          | remainingOpenQuantity | <p>Remaining open quantity.</p><p><span class="math">quantity - filledQuantity - removedQuantity</span></p>                                                                  |
| All          | removedQuantity       | Quantity that was removed with modifyOrder request                                                                                                                           |
| All          | eventTimestamp        | Event timestamp (in nanoseconds) in GMT                                                                                                                                      |
| All          | eventId               | Sequence identifier per instrument for the event                                                                                                                             |
| All          | trackingNumber        | Event tracking number                                                                                                                                                        |
| Executed     | lastFilledQuantity    | Matched quantity                                                                                                                                                             |
| Executed     | lastFilledPrice       | Matched price (maker order price).                                                                                                                                           |
| Executed     | mathchId              | Unique ID for the match                                                                                                                                                      |
| Executed     | tradingMode           | <p>IA - (Scheduled Intraday Auction) -When execution was as part of auction<br>CT (Continuous Trading) - When execution was done on a regular trading</p>                    |
| Cancelled    | cancelledQuantity     | The cancelled quantity in the current cancelled event                                                                                                                        |
| Modified     | lastRemovedQuantity   | Removed quantity on current event                                                                                                                                            |

### **Error Codes**

| Code | Message                                         |
| ---- | ----------------------------------------------- |
| 1    | `Exchange is unavailable`                       |
| 2    | `Stream disconnected`                           |
| 1007 | `Invalid session`                               |
| 1008 | `This apiKey doesn’t have the right permission` |
| 1200 | `General error`                                 |
| 1201 | `Wrong trackingNumber`                          |

### **Samples**

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
    "trackingNumber": 34272768
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
    "trackingNumber": 34271264
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
    "trackingNumber": 34272544
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
    "trackingNumber": 34271040
  }
}
```
{% endtab %}
{% endtabs %}

##
