# Reporting

Reporting API enables market participants to easily retrieve orders and executions data.\
This API is close to real time API, allows to access historical data as well up to date data.

**Data Eligibility**&#x20;

Reporting API can be consumed for single market participant (MP) or for group of MPs that are pre-configured by exchange operations team.

Note: This API is accessible via a separated websocket endpoint, not the one used for Trading & Market Data API.

## Orders

Any participant can use the `orders` API to retrieve the full list of all its own orders .

{% hint style="info" %}
qualifier: `v1/exchange.reporting/mp/orders`
{% endhint %}

### Request

<table><thead><tr><th width="164.33333333333331">Parameter</th><th width="193">Type</th><th>Description</th></tr></thead><tbody><tr><td>dateFrom</td><td>DateTime (GMT)</td><td>Search for orders where <em>Created At ≥ dateFrom</em><br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</td></tr><tr><td>dateTo</td><td>DateTime (GMT)</td><td>Search for orders where <em>Created At &#x3C; dateTo</em><br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</td></tr><tr><td>status</td><td>eNum</td><td><p>Order status (Active/ Executed/Cancelled/<mark style="color:blue;">NEW</mark> Suspended )</p><p>Empty Status = All statuses</p></td></tr><tr><td>instruments </td><td>[]String</td><td>Search for orders by symbol</td></tr><tr><td>mpId </td><td>int</td><td>Search for orders by MP ID.</td></tr><tr><td>orderId </td><td>int</td><td>Search for orders by order ID.</td></tr></tbody></table>

### **Response**

`orders` response provides close to real time list of all current orders for the requested period with the entire details of those orders.

<table><thead><tr><th width="252">Field</th><th>Description</th></tr></thead><tbody><tr><td>orderId</td><td>Exchange order ID</td></tr><tr><td>mpOrderId</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>instrumentId</td><td>Instrument identifier</td></tr><tr><td>instrument</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>mpId</td><td>Market participant identifier</td></tr><tr><td>mpName</td><td>Market participant name</td></tr><tr><td>status</td><td>Order status: Active/ Executed/Cancelled</td></tr><tr><td>orderType</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>side</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>price</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>quantity</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>timeInForce</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>expiryDate</td><td>Optional<br>Order expiry date (GMT)<br>Format: YYYY-MM-DDThh:mm:ss</td></tr><tr><td>expiryDay</td><td>Optional<br>Order expiry day<br>format: YYYY-MM-DD</td></tr><tr><td>createdAt</td><td>Order creation timestamp (in microseconds) in GMT<br>Format: YYYY-MM-DDThh:mm:ss.SSSSSS</td></tr><tr><td>filledPrice</td><td>Weighted average filled price for all fills on that order <span class="math">Sum(event.executedQuantity * event.executedPrice)/Sum(event.executedQuantity)</span></td></tr><tr><td>filledQuantity</td><td>Total filled quantity</td></tr><tr><td>remainingOpenQuantity</td><td><p>Remaining open quantity.</p><p><span class="math">quantity - filledQuantity - removedQuantity</span></p></td></tr><tr><td>removedQuantity</td><td>Quantity that was removed with modifyOrder request</td></tr><tr><td>marketModel</td><td><p>A - (Auction) when order was placed during auction</p><p>T - (Trading) when order was placed on continues trading mode<br>UA- (Unscheduled Auction) when order was placed during auction after halt or autoHalt</p></td></tr><tr><td>parties</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>accountType</td><td>Same as in <code>placeOrder</code> request</td></tr><tr><td>lastEventTimestamp</td><td>Last order event timestamp (in microseconds) in GMT<br>Format: YYYY-MM-DDThh:mm:ss.SSSSSS</td></tr><tr><td>lastEventId</td><td>Last event that was used to calculate order state</td></tr><tr><td>stopPrice</td><td>The price at which the order will be injected to the market. </td></tr><tr><td>minQuantity <code>optional</code></td><td>Minimum quantity of an IOC order to be executed.</td></tr><tr><td>cancelReason</td><td><p>Optional</p><p>The reason of the cancellation. Refer <a href="private-data-api.md#cancel-reason">here</a> for values.</p></td></tr><tr><td>stpAction</td><td><p>Optional</p><p>From place order request </p></td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="152">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Temporary failure to retrieve this data</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1001</td><td><code>Wrong [FieldName] format</code> or<br><code>Wrong Status</code> or<br><code>dateTo must be greater than dateFrom</code></td></tr><tr><td>1001 </td><td><code>Wrong [FieldName] format</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```javascript
{
  "q": "v1/exchange.reporting/mp/orders",
  "sid": 12,
  "d": {
    "dateFrom": "2022-12-01T00:00:01",
    "dateTo": "2022-12-12T00:00:01",
    "status": "Active",
    "instruments": ["123"],
    "mpId": 123456,
    "orderId": 15764
  }
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
    "lastUpdate": "2021-12-15T13:30:42.687025",
    "lastEventId": 911
  }
}
```
{% endtab %}
{% endtabs %}



## Trades v2&#x20;

Any participant can use the `trades` API to retrieve the full list of all its own trades data.\
Each execution will be represented as 2 trades.

{% hint style="info" %}
qualifier: `v2/`exchange.reporting/mp/trades
{% endhint %}

### Request

<table><thead><tr><th width="152.33333333333331">Parameter</th><th width="170">Type</th><th>Description</th></tr></thead><tbody><tr><td>dateFrom</td><td>DateTime (GMT)</td><td>Search for trades where <em>Created At ≥ dateFrom</em><br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</td></tr><tr><td>dateTo</td><td>DateTime (GMT)</td><td>Search for trades where <em>Created At &#x3C; dateTo</em><br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</td></tr><tr><td>instruments </td><td>[]String</td><td>Search for trades by symbol</td></tr><tr><td>mpId </td><td>Int</td><td>Search for trades by MP ID.</td></tr><tr><td>tradeId </td><td>Int</td><td>Search for trades by tradeId. </td></tr><tr><td>actionTypes</td><td>[]eNum</td><td><p>Search for trades by actionType.<br>Any combinations of the below values:</p><ul><li>“MatchedTrade”</li><li>“TradeReport”</li><li>“TradeCancel”</li></ul><p>Empty = All</p></td></tr><tr><td><p></p><p>multiLegReportingTypes </p></td><td>[]eNum</td><td><p>Search for trades by multiLegReportingTypes.<br>Any combinations of the below values:</p><ul><li>"MultiLegSecurity"</li><li>“IndividualLeg”</li><li>“SingleSecurity”</li></ul><p>Empty = All</p></td></tr><tr><td>tradeDate</td><td>Date</td><td><p>Search for the executions by the trade date of the reports.</p><p><br>Format: YYYY-MM-DD</p></td></tr></tbody></table>

### **Response**

`trades` response provides close to real time list of all trades data for the requested period with the entire details of those trades.

Each record will be one of the following `actionType`:

* MatchedTrade for order book trade&#x20;
* TradeReport for trade entry trade
* TradeCancel for trade cancellation (separate records will be returned for cancellation)

<table><thead><tr><th width="150">Field</th><th width="337">Description</th><th width="100" align="center">Order Book</th><th width="80" align="center">Trade Entry</th><th width="92" align="center">Trade Cancel</th></tr></thead><tbody><tr><td>eventId</td><td> Event Id </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>timestamp</td><td>Trade event timestamp (in microseconds) in GMT<br>Format: YYYY-MM-DDThh:mm:ss.SSSSSS</td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>actionType</td><td><p>MatchedTrade </p><p>TradeReport </p><p>TradeCancel</p></td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>orderId</td><td>Order id initiated the trade </td><td align="center">V</td><td align="center"> </td><td align="center"> </td></tr><tr><td>mpOrderId</td><td>From source order </td><td align="center">V</td><td align="center"> </td><td align="center"> </td></tr><tr><td>mpId</td><td> MP Id </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>mpName</td><td>MP Name </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>instrumentId</td><td> Instrument id </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>Instrument</td><td>instrument symbol</td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>side</td><td> Buy/ Sell </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>price</td><td> Trade price </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>quantity</td><td> Trade quantity </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>tradeId</td><td>matchId </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>tradingMode</td><td>IA - (Scheduled Intraday Auction) -When execution was as part of auction CT (Continuous Trading) - When execution was done on a regular trading ON - Trade Reporting (On Exchange)<br>UA- (Unscheduled Auction) - When execution was as part of auction after halt or autoHalt</td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>accountType</td><td>Optional, From source order </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>parties</td><td>Optional, From source order </td><td align="center">V</td><td align="center">V</td><td align="center"><code>opt</code></td></tr><tr><td>tradeType</td><td>EFRP/Block/Other</td><td align="center"> </td><td align="center">V</td><td align="center"><code>opt</code></td></tr><tr><td>makerTaker</td><td><strong>Taker</strong> if order was never resting on the book for that trade <br><strong>Maker</strong> if order was resting on the book for that trade</td><td align="center">V</td><td align="center"> </td><td align="center"> </td></tr><tr><td>tradeDate </td><td>Date of the business day of that trade<br>Format: YYY-MM-DD</td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>multiLegReportingType </td><td><ul><li>SingleSecurity: non strategy</li><li>MultiLegSecurity: strategy </li><li>IndividualLeg: strategy leg </li></ul><p>For more details see <a href="https://docs.exberry.io/private-data-api#strategies-multi-legs-handling">here</a></p></td><td align="center">V</td><td align="center">V</td><td align="center">Per trade</td></tr><tr><td>multiLegStrategyInstrument</td><td><p>For strategy allocated trades only</p><p>Parent instrument symbol</p></td><td align="center">V</td><td align="center">V</td><td align="center">Per trade</td></tr><tr><td>multiLegStrategyOrderId</td><td><p>For strategy allocated trades only</p><p>Parent orderId</p></td><td align="center">V</td><td align="center">V</td><td align="center">Per trade</td></tr><tr><td>multiLegStrategyMpOrderId</td><td><p>For strategy allocated trades only</p><p>Parent mpOrderId</p></td><td align="center">V</td><td align="center">V</td><td align="center">Per trade</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="126">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Temporary failure to retrieve this data</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1001</td><td><code>Wrong [FieldName] format</code> or<br><code>dateTo must be greater than dateFrom</code></td></tr><tr><td>1001</td><td><code>Invalid parameter:[FieldName]</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
     "dateFrom": "2020-10-01T00:00:01",
     "dateTo": "2022-10-01T00:00:01",
     "mpId": 2087505425,
     "tradeId": 42,
     "instruments":["INS1", "INS2"],
     "actionTypes": [ "TradeReport","TradeCancel", "MatchedTrade"],
     "multiLegReportingTypes": ["SingleSecurity", "IndividualLeg", "MultiLegSecurity"]

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
    "tradeDate": "2022-10-03",
    "multiLegReportingType": "SingleSecurity"

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
      "tradeDate": "2022-10-03",
      "multiLegReportingType": "SingleSecurity"

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

### **Strategies/ Multi Legs Handling**&#x20;

See description and field details on Private Data API [**#Strategies/ Multi Legs Handling**](private-data-api.md#strategies-multi-legs-handling)

#### **Samples**

{% tabs %}
{% tab title="SingleSecurity" %}
```json
{
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 12,
  "d": {
    "eventId": 2,
    "timestamp": "2022-11-15T13:11:42.812834",
    "actionType": "MatchedTrade",
    "orderId": 2,
    "mpOrderId": 1663245087042,
    "mpId": 2087505414,
    "mpName": "user",
    "instrumentId": 5,
    "instrument": "INS1",
    "side": "Sell",
    "price": 20,
    "quantity": 3,
    "tradeId": 1,
    "tradingMode": "CT",
    "makerTaker": "Taker",
    "tradeDate": "2022-11-15",
    "multiLegReportingType": "SingleSecurity"
  }
}
```
{% endtab %}

{% tab title="IndividualLeg" %}
```json
{
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 12,
  "d": {
    "timestamp": "2023-07-23T07:12:14.129503",
    "actionType": "MatchedTrade",
    "mpId": 2087505414,
    "mpName": "USER",
    "instrumentId": 5,
    "instrument": "INS1",
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

{% tab title="MultiLegSecurity" %}
```json
{
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 12,
  "d": {
    "eventId": 8,
    "timestamp": "2022-11-17T09:13:01.086217",
    "actionType": "MatchedTrade",
    "orderId": 7,
    "mpOrderId": 1663245087048,
    "mpId": 2087505414,
    "mpName": "user",
    "instrumentId": 5,
    "instrument": "INS1",
    "side": "Sell",
    "price": 30,
    "quantity": 3,
    "tradeId": 3,
    "tradingMode": "CT",
    "userId": "UserTest1",
    "makerTaker": "Taker",
    "tradeDate": "2022-11-17",
    "multiLegReportingType": "MultiLegSecurity"
  }
}
```
{% endtab %}
{% endtabs %}



