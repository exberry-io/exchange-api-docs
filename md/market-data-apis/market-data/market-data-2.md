# Live Trades

## liveTrades

This API allows to get close to real time trades details happen on the market, you can retrieve limited amount last trades the were already happen and right after you will get real time event for new trades.\
Trade will come sorted by time, old trades will come first. \
When snapshot is completed, the last trade will be sent again, but quantity field will be set to 0.&#x20;

This stream includes trade entry, trade cancellation and RFQ trades.&#x20;

{% hint style="info" %}
qualifier: v1/exchange.marketdata/liveTrades
{% endhint %}

### **Request**

<table><thead><tr><th width="149.6710763680096">Parameter</th><th width="99">Type</th><th width="482.2">Description</th></tr></thead><tbody><tr><td>symbol</td><td>String</td><td>Symbol to retrieve the light tickers for </td></tr><tr><td>limit</td><td>eNum</td><td><p>Number of past trades (sorted by time descending). </p><p>0 ≤ limit ≤ 10,000 </p></td></tr></tbody></table>

### **Response**

<table><thead><tr><th width="100" data-type="number">Index</th><th width="129">Parameter</th><th width="123">Type</th><th width="390.2">Description</th></tr></thead><tbody><tr><td>1</td><td>price</td><td>Decimal</td><td>Trade price</td></tr><tr><td>2</td><td>quantity</td><td>Decimal</td><td>Trade quantity, for snapshot end message this will be set to 0.  <br>In case of cancellation the quantity will be negative  → quantity * (-1)</td></tr><tr><td>3</td><td>makerSide</td><td>Decimal</td><td>If resting order or dealer side of RFQ is Buy→ 1 else 0<br>for trade cancellation, trade entry  -1</td></tr><tr><td>4</td><td>timeStamp</td><td>Unix timestamp</td><td>Trade timestamp (milliseconds)</td></tr><tr><td>5</td><td>multiLegReportingType</td><td>eNum</td><td><ul><li>For non strategy instruments: 1</li><li>For strategy leg instruments: 2</li><li>For strategy parent instruments: 3</li></ul></td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="93.27803690934905">Code</th><th width="554.4285714285713">Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td>2</td><td>Missing fields: [Fieldname] |<br><mark style="color:blue;">(NEW v1.52.0)</mark> Stream disconnected</td></tr><tr><td>3</td><td>Wrong limit |<br>Wrong symbol [symbol] |</td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```json
{
  "q": "v1/exchange.marketdata/liveTrades",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 153,
  "d": {
    "symbol": "AMZ",
    "limit": 100
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "q": "v1/exchange.marketdata/liveTrades",
  "sid": 153,
  "d": [
    3.2,           //price
    1.21,          //quantity
    1,             //makerSide
    1648969398501, //timestamp
    1              //multiLegReportingType
  ]
}
```
{% endtab %}

{% tab title="End Of Snapshot" %}
```javascript
{
  "q": "v1/exchange.marketdata/liveTrades",
  "sid": 153,
  "d": [
    3.2,
    0, //This means snapshot is completed
    1,
    1648969398501,
    1
  ]
}
```
{% endtab %}

{% tab title="Empty Instrument" %}
```json
{
  "q": "v1/exchange.marketdata/liveTrades",
  "sid": 153,
  "d": [
    0,
    0, //This means snapshot is completed
    0,
    0,
    0
  ]
}
```
{% endtab %}
{% endtabs %}

