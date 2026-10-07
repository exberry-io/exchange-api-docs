# Auction Indicative Equilibrium Price

## auctionIndicativeEP

This API provides close to real-time updates for auction indicative equilibrium price.&#x20;

{% hint style="info" %}
qualifier: v1/exchange.marketdata/auctionIndicativeEP
{% endhint %}

### **Request**

<table><thead><tr><th width="163.17225950782998">Parameter</th><th width="124">Type</th><th width="446.2">Description</th></tr></thead><tbody><tr><td>symbol</td><td>String</td><td>Symbol to subscribe </td></tr><tr><td>interval</td><td>eNum</td><td>Response interval in milliseconds, allowed values: 100, 1000, 2000</td></tr></tbody></table>

### **Response**

Similar to [this](https://docs.exberry.io/ws/market-data#auction-indicative-equilibrium-price-message), with the below changes:

* Removed  `messageType`
* Removed `eventId`
* Change `eventTimestamp` to be in milliseconds&#x20;



### **Error Codes**

<table><thead><tr><th width="155.111083984375">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td>2</td><td>Missing fields: [Fieldname]<br><mark style="color:blue;">(NEW v1.52.0)</mark> Stream disconnected</td></tr><tr><td>3</td><td>Wrong interval | Wrong symbol [symbol] </td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```json
{
  "q": "v1/exchange.marketdata/auctionIndicativeEP",
  "sid": 1032,
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoxM123",
  "d": {
    "symbol": "OIL",
    "interval": "400"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "q": "v1/exchange.marketdata/auctionIndicativeEP",
  "sid": 1032,
  "d": {
    "instrument": "OIL",
    "indicativePrice": 1.2,
    "pairedQuantity": 1000,
    "imbalanceQuantity": 2333,
    "imbalanceSide": "Buy",
    "bestBuyPrice": 0,
    "bestSellPrice": 0,
    "bestBuyQuantity": 0,
    "bestSellQuantity": 0,
    "eventTimestamp": 1730988285213
  }
}
```
{% endtab %}
{% endtabs %}
