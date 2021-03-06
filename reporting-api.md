# Reporting API

Reporting API enables market participants to easily retrieve orders and executions data.\
This API is close to real time API, allows to access historical data as well up to date data.

Note: This API is accessible via a separated websocket endpoint, not the one used for Trading & Market Data API.

## Orders

Any participant can use the `orders` API to retrieve the full list of all its own orders .

{% hint style="info" %}
`endpoint: v1/`exchange.reporting/mp/orders
{% endhint %}

### Request

| Parameter | Type           | Description                                                                                              |
| --------- | -------------- | -------------------------------------------------------------------------------------------------------- |
| dateFrom  | DateTime (GMT) | <p>Search for orders where <em>Created At ≥ dateFrom</em><br>Format:<br>YYYY-MM-DDThh:mm:ss[.SSS]</p>    |
| dateTo    | DateTime (GMT) | <p>Search for orders where <em>Created At &#x3C; dateTo</em><br>Format:<br>YYYY-MM-DDThh:mm:ss[.SSS]</p> |
| status    | eNum           | <p>Order status (Active/ Executed/Cancelled )</p><p>Empty Status = All statuses</p>                      |

### **Response**

`orders` response provides close to real time list of all current orders for the requested period with the entire details of those orders.

| Field                 | Description                                                                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| orderId               | Exchange order ID                                                                                                                           |
| mpOrderId             | Same as in `placeOrder` request                                                                                                             |
| instrumentId          | Instrument identifier                                                                                                                       |
| instrument            | Same as in `placeOrder` request                                                                                                             |
| mpId                  | Market participant identifier                                                                                                               |
| mpName                | Market participant name                                                                                                                     |
| status                | Order status: Active/ Executed/Cancelled                                                                                                    |
| orderType             | Same as in `placeOrder` request                                                                                                             |
| side                  | Same as in `placeOrder` request                                                                                                             |
| price                 | Same as in `placeOrder` request                                                                                                             |
| quantity              | Same as in `placeOrder` request                                                                                                             |
| timeInForce           | Same as in `placeOrder` request                                                                                                             |
| expiryDate            | <p>Optional<br>Order expiry date (GMT)<br>Format: YYYY-MM-DDThh:mm:ss</p>                                                                   |
| createdAt             | <p>Order creation timestamp (in microseconds) in GMT<br>Format: YYYY-MM-DDThh:mm:ss.SSSSSS</p>                                              |
| filledPrice           | Weighted average filled price for all fills on that order $$Sum(event.executedQuantity * event.executedPrice)/Sum(event.executedQuantity)$$ |
| filledQuantity        | Total filled quantity                                                                                                                       |
| remainingOpenQuantity | <p>Remaining open quantity.</p><p><span class="math">quantity - filledQuantity - removedQuantity</span></p>                                 |
| removedQuantity       | Quantity that was removed with modifyOrder request                                                                                          |
| marketModel           | <p>A - (Auction) when order was placed during auction</p><p>T - (Trading) when order was placed on continues trading mode</p>               |
| userId                | Same as in `placeOrder` request                                                                                                             |
| accountId             | Same as in `placeOrder` request                                                                                                             |
| lastEventTimestamp    | <p>Last order event timestamp (in microseconds) in GMT<br>Format: YYYY-MM-DDThh:mm:ss.SSSSSS</p>                                            |
| lastEventId           | Last event that was used to calculate order state                                                                                           |

### **Error Codes**

| Code | Message                                                                                                                              |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------ |
| 1    | `Temporary failure to retrieve this data`                                                                                            |
| 1007 | `Invalid session`                                                                                                                    |
| 1008 | `This apiKey doesn’t have the right permission`                                                                                      |
| 1001 | <p><code>Wrong [FieldName] format</code> or<br><code>Wrong Status</code> or<br><code>dateTo must be greater than dateFrom</code></p> |

### **Samples**

{% tabs %}
{% tab title="Request" %}
```javascript
{
  "d": {
    "dateFrom": "2021-12-15T00:00:01",
    "dateTo": "2021-12-16T00:00:01",
    "status": "Active"
  },
  "q": "v1/exchange.reporting/mp/orders",
  "sid": 10
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "q": "v1/exchange.reporting/mp/orders",
  "sid": 10,
  "d": {
    "orderId": 680,
    "mpOrderId": 1639575009009,
    "instrumentId": 14,
    "instrument": "INS1",
    "mpId": 1958681073,
    "mpName": "Participant1",
    "status": "Active",
    "orderType": "Limit",
    "side": "Buy",
    "price": 100.5,
    "quantity": 1.25,
    "timeInForce": "GTC",
    "createdAt": "2021-12-15T13:30:42.687025",
    "filledPrice": 0,
    "filledQuantity": 0,
    "remainingOpenQuantity": 1.25,
    "removedQuantity": 0,
    "marketModel": "T",
    "userId": "UATUserTest1",
    "lastUpdate": "2021-12-15T13:30:42.687025",
    "lastEventId": 911
  }
}
```
{% endtab %}
{% endtabs %}

## Trades

Any participant can use the `trades` API to retrieve the full list of all its own trades.\
Each execution will be represented as 2 trades.

Note: `userId` and `accountId` will be populated with values only for orders captured after Dec 14 2021

{% hint style="info" %}
`endpoint: v1/`exchange.reporting/mp/trades
{% endhint %}

### Request

| Parameter | Type           | Description                                                                                              |
| --------- | -------------- | -------------------------------------------------------------------------------------------------------- |
| dateFrom  | DateTime (GMT) | <p>Search for orders where <em>Created At ≥ dateFrom</em><br>Format:<br>YYYY-MM-DDThh:mm:ss[.SSS]</p>    |
| dateTo    | DateTime (GMT) | <p>Search for orders where <em>Created At &#x3C; dateTo</em><br>Format:<br>YYYY-MM-DDThh:mm:ss[.SSS]</p> |

### **Response**

`trades` response provides close to real time list of all trades for the requested period with the entire details of those trades.

| Field            | Description                                                                                                                                                       |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| eventId          | Event identifier of the execution                                                                                                                                 |
| timestamp        | <p>Execution event timestamp (in microseconds) in GMT<br>Format: YYYY-MM-DDThh:mm:ss.SSSSSS</p>                                                                   |
| orderId          | Exchange order ID                                                                                                                                                 |
| mpOrderId        | Same as in `placeOrder` request                                                                                                                                   |
| mpId             | Market participant identifier                                                                                                                                     |
| mpName           | Market participant name                                                                                                                                           |
| instrumentId     | Instrument identifier                                                                                                                                             |
| instrument       | Same as in `placeOrder` request                                                                                                                                   |
| side             | Buy/ Sell                                                                                                                                                         |
| executedPrice    | Trade price                                                                                                                                                       |
| executedQuantity | Trade amount                                                                                                                                                      |
| matchId          | Match identifier                                                                                                                                                  |
| tradingMode      | <p>IA - (Scheduled Intraday Auction) -When execution was as part of auction</p><p>CT (Continuous Trading) - When execution was done on a regular trading mode</p> |
| userId           | Same as in `placeOrder` request                                                                                                                                   |
| accountId        | Same as in `placeOrder` request                                                                                                                                   |

### **Error Codes**

| Code | Message                                                                                              |
| ---- | ---------------------------------------------------------------------------------------------------- |
| 1    | `Temporary failure to retrieve this data`                                                            |
| 1007 | `Invalid session`                                                                                    |
| 1008 | `This apiKey doesn’t have the right permission`                                                      |
| 1001 | <p><code>Wrong [FieldName] format</code> or<br><code>dateTo must be greater than dateFrom</code></p> |

### **Samples**

{% tabs %}
{% tab title="Request" %}
```javascript
{
  "d": {
    "dateFrom": "2021-12-15T00:00:01",
    "dateTo": "2021-12-16T00:00:01"
  },
  "q": "v1/exchange.reporting/mp/trades",
  "sid": 10
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "q": "v1/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
    "eventId": 927,
    "timestamp": "2021-12-15T21:02:45.189982",
    "orderId": 645,
    "mpOrderId": 1632496974889,
    "mpId": 1958681073,
    "mpName": "Participant1",
    "instrumentId": 14,
    "instrument": "INS1",
    "side": "Buy",
    "executedPrice": 100.5,
    "executedQuantity": 1.25,
    "matchId": 415,
    "tradingMode": "CT",
    "userId": "UATUserTest1"
  }
}
```
{% endtab %}
{% endtabs %}

##
