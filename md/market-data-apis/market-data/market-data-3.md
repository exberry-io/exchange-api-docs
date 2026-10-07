# Get Settlement Prices

## getSettlementPrices

This API allows  retrieve the settlement prices for all instruments or for a specific list of instruments. \
Stream is bounded stream, after sending the relvant responses, stream will be closed.&#x20;

{% hint style="info" %}
qualifier: v1/exchange.marketdata/getSettlementPrices
{% endhint %}

### **Request**

<table><thead><tr><th width="146.17225950782998">Parameter</th><th width="94">Type</th><th width="446.2">Description</th></tr></thead><tbody><tr><td>symbols</td><td>[]String</td><td>List of symbols to retrieve the settlement prices for.<br>Keep empty to get settlement prices for all instruments<br><br>Disabled instruments with settlement price are also returned.  </td></tr></tbody></table>

### **Response**

<table><thead><tr><th width="147.6710763680096">Parameter</th><th width="132">Type</th><th width="395.2">Description</th></tr></thead><tbody><tr><td>symbol</td><td>String</td><td>Instrument symbol </td></tr><tr><td>price</td><td>Decimal</td><td>Settlement price</td></tr><tr><td>lastUpdate</td><td>Unix timestamp</td><td>Settlement price update timestamp<br>In milliseconds </td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="169.57142857142856">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td>2</td><td>Missing fields: [Fieldname]</td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v1/exchange.marketdata/getSettlementPrices",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 10,
  "d": {
    "symbols": [
      "INS1",
      "INS2"
    ]
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "q": "v1/exchange.marketdata/getSettlementPrices",
  "sid": 10,
  "d": {
    "symbol": "INS1",
    "price": 1.235,
    "lastUpdate": 1662638980158
  }
}
```
{% endtab %}

{% tab title="Stream Closure" %}
```json
{
  "sig": 1,
  "q": "v1/exchange.marketdata/getSettlementPrices",
  "sid": 10
}
```
{% endtab %}
{% endtabs %}
