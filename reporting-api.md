# Reporting API

Reporting API enables market participants to easily retrieve orders and executions data.\
This API is close to real time API, allows to access historical data as well up to date data.

**Data Eligibility**&#x20;

Reporting API can be consumed for single market participant (MP) or for group of MPs that are pre-configured by exchange operations team.

Note: This API is accessible via a separated websocket endpoint, not the one used for Trading & Market Data API.

## Orders

Any participant can use the `orders` API to retrieve the full list of all its own orders .

{% hint style="info" %}
qualifier: `v1/`exchange.reporting/mp/orders
{% endhint %}

### Request

| Parameter | Type           | Description                                                                                           |
| --------- | -------------- | ----------------------------------------------------------------------------------------------------- |
| dateFrom  | DateTime (GMT) | <p>Search for orders where <em>Created At ≥ dateFrom</em><br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</p>    |
| dateTo    | DateTime (GMT) | <p>Search for orders where <em>Created At &#x3C; dateTo</em><br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</p> |
| status    | eNum           | <p>Order status (Active/ Executed/Cancelled )</p><p>Empty Status = All statuses</p>                   |

### **Response**

`orders` response provides close to real time list of all current orders for the requested period with the entire details of those orders.

| Field                                            | Description                                                                                                                                 |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| orderId                                          | Exchange order ID                                                                                                                           |
| mpOrderId                                        | Same as in `placeOrder` request                                                                                                             |
| instrumentId                                     | Instrument identifier                                                                                                                       |
| instrument                                       | Same as in `placeOrder` request                                                                                                             |
| mpId                                             | Market participant identifier                                                                                                               |
| mpName                                           | Market participant name                                                                                                                     |
| status                                           | Order status: Active/ Executed/Cancelled                                                                                                    |
| orderType                                        | Same as in `placeOrder` request                                                                                                             |
| side                                             | Same as in `placeOrder` request                                                                                                             |
| price                                            | Same as in `placeOrder` request                                                                                                             |
| quantity                                         | Same as in `placeOrder` request                                                                                                             |
| timeInForce                                      | Same as in `placeOrder` request                                                                                                             |
| expiryDate                                       | <p>Optional<br>Order expiry date (GMT)<br>Format: YYYY-MM-DDThh:mm:ss</p>                                                                   |
| createdAt                                        | <p>Order creation timestamp (in microseconds) in GMT<br>Format: YYYY-MM-DDThh:mm:ss.SSSSSS</p>                                              |
| filledPrice                                      | Weighted average filled price for all fills on that order $$Sum(event.executedQuantity * event.executedPrice)/Sum(event.executedQuantity)$$ |
| filledQuantity                                   | Total filled quantity                                                                                                                       |
| remainingOpenQuantity                            | <p>Remaining open quantity.</p><p><span class="math">quantity - filledQuantity - removedQuantity</span></p>                                 |
| removedQuantity                                  | Quantity that was removed with modifyOrder request                                                                                          |
| marketModel                                      | <p>A - (Auction) when order was placed during auction</p><p>T - (Trading) when order was placed on continues trading mode</p>               |
| userId                                           | Same as in `placeOrder` request                                                                                                             |
| accountId                                        | Same as in `placeOrder` request                                                                                                             |
| <mark style="color:blue;">NEW</mark> parties     | Same as in `placeOrder` request                                                                                                             |
| <mark style="color:blue;">NEW</mark> accountType | Same as in `placeOrder` request                                                                                                             |
| lastEventTimestamp                               | <p>Last order event timestamp (in microseconds) in GMT<br>Format: YYYY-MM-DDThh:mm:ss.SSSSSS</p>                                            |
| lastEventId                                      | Last event that was used to calculate order state                                                                                           |

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

## Trades V2&#x20;

Any participant can use the `trades` API to retrieve the full list of all its own trades data.\
Each execution will be represented as 2 trades.

{% hint style="info" %}
qualifier: `v2/`exchange.reporting/mp/trades
{% endhint %}

### Request

| Parameter | Type           | Description                                                                                           |
| --------- | -------------- | ----------------------------------------------------------------------------------------------------- |
| dateFrom  | DateTime (GMT) | <p>Search for orders where <em>Created At ≥ dateFrom</em><br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</p>    |
| dateTo    | DateTime (GMT) | <p>Search for orders where <em>Created At &#x3C; dateTo</em><br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</p> |

### **Response**

`trades` response provides close to real time list of all trades data for the requested period with the entire details of those trades.

Each record will be one of the following `actionType`:

* MatchedTrade for order book trade&#x20;
* TradeReport for trade entry trade
* TradeCancel for trade cancellation (separate records will be returned for cancellation)

| Field                                                     | Description                                                                                                                                                                        | Order Book | Trade Entry | Trade Cancel |
| --------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------: | :---------: | :----------: |
| eventId                                                   |  Event Id                                                                                                                                                                          |      V     |      V      |       V      |
| timestamp                                                 | <p>Trade event timestamp (in microseconds) in GMT<br>Format: YYYY-MM-DDThh:mm:ss.SSSSSS</p>                                                                                        |      V     |      V      |       V      |
| actionType                                                | <p>MatchedTrade </p><p>TradeReport </p><p>TradeCancel</p>                                                                                                                          |      V     |      V      |       V      |
| orderId                                                   | Order id initiated the trade                                                                                                                                                       |      V     |             |              |
| mpOrderId                                                 | From source order                                                                                                                                                                  |      V     |             |              |
| mpId                                                      |  MP Id                                                                                                                                                                             |      V     |      V      |       V      |
| mpName                                                    | MP Name                                                                                                                                                                            |      V     |      V      |       V      |
| instrumentId                                              |  Instrument id                                                                                                                                                                     |      V     |      V      |       V      |
| Instrument                                                | instrument symbol                                                                                                                                                                  |      V     |      V      |       V      |
| side                                                      |  Buy/ Sell                                                                                                                                                                         |      V     |      V      |       V      |
| price                                                     |  Trade price                                                                                                                                                                       |      V     |      V      |       V      |
| quantity                                                  |  Trade quantity                                                                                                                                                                    |      V     |      V      |       V      |
| tradeId                                                   | matchId                                                                                                                                                                            |      V     |      V      |       V      |
| tradingMode                                               | IA - (Scheduled Intraday Auction) -When execution was as part of auction CT (Continuous Trading) - When execution was done on a regular trading ON - Trade Reporting (On Exchange) |      V     |      V      |       V      |
| accountType                                               | Optional, From source order                                                                                                                                                        |      V     |      V      |       V      |
| parties                                                   | Optional, From source order                                                                                                                                                        |      V     |      V      |     `opt`    |
| tradeType                                                 | EFRP/Block/Other                                                                                                                                                                   |            |      V      |     `opt`    |
| makerTaker                                                | <p><strong>Taker</strong> if order was never resting on the book for that trade <br><strong>Maker</strong> if order was resting on the book for that trade</p>                     |      V     |             |              |
| <p><mark style="color:blue;">NEW</mark><br>tradeDate </p> | <p>Date of the business day of that trade<br>Format: YYY-MM-DD</p>                                                                                                                 |      V     |      V      |       V      |

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
```json
{
  "q": "v1/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
    "dateFrom": "2021-12-15T00:00:01",
    "dateTo": "2021-12-16T00:00:01"
  }
}
```
{% endtab %}

{% tab title="MatchedTrade " %}
```json
{
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
    "eventId": 925,
    "timestamp": "2022-10-03T21:02:45.189982",
    "actionType": "MatchedTrade"
    "orderId": 643,
    "mpOrderId": 1631632596945,
    "mpId": 1958681073,
    "mpName": "Participant1",
    "instrumentId": 14,
    "instrument": "INS1",
    "side": "Buy",
    "price": 100.5,
    "quantity": 1.25,
    "tradeId": 413,
    "tradingMode": "CT",
    "userId": "UATUserTest1"
    "accountId": 555,
    "accountType" : "Client",
    "parties": [
      {
        "id": "abc456",
        "source": "D",
        "role": 38
      },
      {
        "id": "4455",
        "source": "D",
        "role": 12
      }
    ],
    "makerTaker": "Taker" ,
    "tradeDate": "2022-10-03"
  }
}
```
{% endtab %}

{% tab title="TradeReport " %}
```javascript
{
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
    "eventId": 925,
    "timestamp": "2022-10-03T21:02:45.189982",
    "actionType": "TradeReport"
    "mpId": 1958681073,
    "mpName": "Participant1",
    "instrumentId": 14,
    "instrument": "INS1",
    "side": "Buy",
    "price": 100.5,
    "quantity": 1.25,
    "tradeId": 413,
    "tradingMode": "CT",
    "accountType" : "Client",
    "parties": [
      {
        "id": "abc456",
        "source": "D",
        "role": 38
      },
      {
        "id": "4455",
        "source": "D",
        "role": 12
      }
    ],
      "tradeType" : "Block",
      "tradeDate": "2022-10-03"
  }
}
```
{% endtab %}

{% tab title="TradeCancel " %}
```json
{
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
    "eventId": 925,
    "timestamp": "2022-10-03T21:02:45.189982",
    "actionType": "TradeCancel"
    "mpId": 1958681073,
    "mpName": "Participant1",
    "instrumentId": 14,
    "instrument": "INS1",
    "side": "Buy",
    "price": 100.5,
    "quantity": 1.25,
    "tradeId": 413,
    "tradingMode": "CT",
    "accountType" : "Client",
    "parties": [
      {
        "id": "abc456",
        "source": "D",
        "role": 38
      },
      {
        "id": "4455",
        "source": "D",
        "role": 12
      }
    ],
    "tradeType" : "Block",
    "tradeDate": "2022-10-03"
  }
}
```
{% endtab %}
{% endtabs %}

### <mark style="color:blue;">**NEW**</mark>** Strategies/ Multi Legs Handling**&#x20;

See description and field details on Private Data API [#new-strategies-multi-legs-handling](private-data-api.md#new-strategies-multi-legs-handling "mention")

#### **Samples**

{% tabs %}
{% tab title="SingleSecurity" %}
```json
{
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
    "eventId": 4,
    "timestamp": "2022-11-21T16:20:13.865026",
    "actionType": "MatchedTrade",
    "orderId": 1,
    "mpOrderId": 1669047601666,
    "mpId": 2087505415,
    "mpName": "Test1",
    "instrumentId": 17,
    "instrument": "Test1Feb-Mar23",
    "side": "Sell",
    "price": 1.3333,
    "quantity": 1.33,
    "tradeId": 3,
    "tradingMode": "CT",
    "parties": [
      {
        "id": "123456",
        "source": "D",
        "role": 38
      },
      {
        "id": "ABCD",
        "source": "D",
        "role": 12
      }
    ],
    "makerTaker": "Maker",
    "tradeDate": "2022-11-21",
    "multiLegReportingType": "SingleSecurity"
  }
}
```
{% endtab %}

{% tab title="IndividualLeg" %}
```json
{
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
    "timestamp": "2022-11-21T16:20:13.865026",
    "actionType": "MatchedTrade",
    "mpId": 2087505415,
    "mpName": "Test1",
    "instrumentId": 14,
    "instrument": "Test1Feb23",
    "side": "Sell",
    "price": 1.5,
    "quantity": 1.33,
    "tradeId": 120,
    "tradingMode": "CT",
    "parties": [
      {
        "id": "123456",
        "source": "D",
        "role": 38
      },
      {
        "id": "ABCD",
        "source": "D",
        "role": 12
      }
    ],
    "makerTaker": "Maker",
    "tradeDate": "2022-11-21",
    "multiLegReportingType": "IndividualLeg",
    "tradeLegRefId": 1,
    "multiLegDifferentialPrice": 1.3333,
    "multiLegStrategyInstrumentId": 17,
    "multiLegStrategyTradeId": 3
  }
}
```
{% endtab %}
{% endtabs %}



## Trades V1 - To be Deprecated&#x20;

Any participant can use the `trades` API to retrieve the full list of all its own trades.\
Each execution will be represented as 2 trades.

Note: `userId` and `accountId` will be populated with values only for orders captured after Dec 14 2021

{% hint style="info" %}
qualifier: `v1/`exchange.reporting/mp/trades
{% endhint %}

### Request

| Parameter | Type           | Description                                                                                           |
| --------- | -------------- | ----------------------------------------------------------------------------------------------------- |
| dateFrom  | DateTime (GMT) | <p>Search for orders where <em>Created At ≥ dateFrom</em><br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</p>    |
| dateTo    | DateTime (GMT) | <p>Search for orders where <em>Created At &#x3C; dateTo</em><br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</p> |

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
