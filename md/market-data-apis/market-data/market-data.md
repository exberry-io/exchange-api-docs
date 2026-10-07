# Partial Order Book

## partialOrderBook

This API allows easy refresh of the book state without the need to calculate the entire book, it allows consumers to subscribe to some price levels in the book.

Upon subscription current book state will sent,  afterward, updated book state will be sent only in case it was changed (it will not not be sent on real time but only when interval time reached).

{% hint style="info" %}
qualifier: v1/exchange.marketdata/partialOrderBook
{% endhint %}

### **Request**

<table><thead><tr><th width="130.6710763680096">Parameter</th><th width="83">Type</th><th width="469.2">Description</th></tr></thead><tbody><tr><td>symbol</td><td>String</td><td>Symbol to subscribe</td></tr><tr><td>levels</td><td>eNum</td><td>Price level to be returned , values can be : 1,5,10,20,100</td></tr><tr><td>interval</td><td>eNum</td><td>Response interval in milliseconds, allowed values: 100,1000,2000</td></tr><tr><td>decimals</td><td>Int</td><td><p>Define the grouping decimal places (empty means no grouping)</p><p><br><mark style="color:blue;">(NEW v1.57.0)</mark>  Negative decimals apply grouping to the left side of the decimal point.</p><p>For example, -2 means values are grouped in increments of 100.</p></td></tr></tbody></table>

### **Response**

<table><thead><tr><th width="196.8239997035043">Parameter</th><th width="135">Type</th><th width="403.2">Description</th></tr></thead><tbody><tr><td>symbol</td><td>String</td><td>Instrument symbol </td></tr><tr><td>timeStamp</td><td>Unix timestamp</td><td>Timestamp where message was sent </td></tr><tr><td>bids</td><td>[]priceLevel</td><td>Array of bids, sorted by price level descending</td></tr><tr><td>asks</td><td>[]priceLevel</td><td>Array of asks, sorted by price level ascending</td></tr><tr><td>priceLevel.price</td><td>Decimal</td><td>Price level according to the decimal in request</td></tr><tr><td>priceLevel.quantity</td><td>Decimal</td><td>Aggregated quantity for that price level</td></tr><tr><td>priceLevel.numberOfOrders</td><td>Int</td><td>Aggregated number of orders for that price level</td></tr><tr><td><mark style="color:blue;">NEW v1.33.0</mark> marketBids</td><td>marketOrders Object</td><td><p><code>optional</code></p><p>Market orders bids (only during auction)</p></td></tr><tr><td><mark style="color:blue;">NEW v1.33.0</mark> marketAsks</td><td>marketOrders Object</td><td><p><code>optional</code></p><p>Market orders asks (only during auction)</p></td></tr></tbody></table>

### <mark style="color:blue;">NEW v1.33.0</mark> marketOrders **object**

<table><thead><tr><th width="187">Name</th><th width="149">Type</th><th>Description</th></tr></thead><tbody><tr><td>quantity</td><td>Decimal</td><td>Aggregated quantity for market orders</td></tr><tr><td>numberOfOrders</td><td>int</td><td>Aggregated number of market orders</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="155.111083984375">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td>2</td><td>Missing fields: [Fieldname]<br><mark style="color:blue;">(NEW v1.52.0)</mark> Stream disconnected</td></tr><tr><td>3</td><td>Wrong levels | <br>Wrong interval | <br>Wrong symbol [symbol] | <br>Wrong decimals</td></tr><tr><td>100</td><td>Your connection is slow, please reduce data consumed</td></tr></tbody></table>

​

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```javascript
{
  "q": "v1/exchange.marketdata/partialOrderBook",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 10,
  "d": {
    "symbol": "INS1",
    "levels": 5,
    "interval": 1000,
    "decimals": 2
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "q": "v1/exchange.marketdata/partialOrderBook",
  "sid": 10,
  "d": {
    "symbol": "AMZ",
    "timeStamp": 1646549579452,
    "bids": [
      [
        100.5,  //Price level
        10.25,  //Total quantity 
        1       //number of orders 
      ],
      [
        100.33,
        6.5,
        5
      ],
      [
        100,
        93.0049,
        1
      ],
      [
        97,
        99.0038,
        2
      ],
      [
        96,
        123.0193,
        3
      ]
    ],
    "asks": [
      [
        100.6,
        1.3,
        1
      ],
      [
        101,
        1.3,
        1
      ]
    ]
  }
}
```
{% endtab %}

{% tab title="Response with Makret orders During Auction" %}
```javascript
{
  "q": "v1/exchange.marketdata/partialOrderBook",
  "sid": 101,
  "d": {
    "symbol": "yael1",
    "timeStamp": 1723107804960,
    "bids": [
      [
        100,
        5,
        1
      ],
      [
        95,
        5,
        1
      ]
    ],
    "asks": [
      [
        110,
        1,
        1
      ],
      [
        150,
        5,
        1
      ]
    ],
    "marketBids": [
      6,
      2
    ],
    "marketAsks": [
      3,
      1
    ]
  }
}
```
{% endtab %}

{% tab title="Empty Response" %}
```javascript
{
  "q": "v1/exchange.marketdata/partialOrderBook",
  "sid": 13,
  "d": {
    "symbol": "INS3",
    "timeStamp": 1646667453613,
    "bids": [],
    "asks": []
  }
}
```
{% endtab %}
{% endtabs %}

