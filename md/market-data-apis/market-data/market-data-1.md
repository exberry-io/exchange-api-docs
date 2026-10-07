# Light Tickers

## lightTickers

This API allows easy refresh of the instruments market data.\
Upon subscription data for all instruments will sent, afterward, data for specific instrument will be returned only in case one of the values was changed (it will not not be sent on real time but only when interval time reached)

This API is not considering trade cancellations AND does not include market orders during auction.

Some fields of the response are adjusted when corporate actions are applied to the active orders of an instrument

{% hint style="info" %}
qualifier: v1/exchange.marketdata/lightTickers
{% endhint %}

### **Request**

<table><thead><tr><th width="146.17225950782998">Parameter</th><th width="94">Type</th><th width="446.2">Description</th></tr></thead><tbody><tr><td>symbols</td><td>[]String</td><td>List of symbols to retrieve the light tickers for </td></tr><tr><td>interval</td><td>eNum</td><td>Response interval in milliseconds, allowed values: 100,1000,2000</td></tr><tr><td>scope</td><td>eNum</td><td><p>Optional</p><p>Specifies the type of data used to calculate: <code>lastPrice</code>, <code>lastQuantity</code>, <code>high</code>, <code>low</code>, <code>volume</code>, <code>quoteVolume</code> fields.</p><p></p><p>Supported Values:</p><ul><li>CLOB - data is derived from order book executions only</li><li>All - data is derived from all trades (CLOB, trade entry and RFQ) </li></ul><p>If not sent - CLOB is the default </p></td></tr></tbody></table>

### **Response**

<table><thead><tr><th width="175.6710763680096">Parameter</th><th width="132">Type</th><th width="423.2">Description</th></tr></thead><tbody><tr><td>symbol</td><td>String</td><td>Instrument symbol </td></tr><tr><td>lastPrice</td><td>Decimal</td><td>last execution price as per the <code>scope</code></td></tr><tr><td><mark style="color:blue;">(NEW v1.50.0)</mark><br>lastPriceTimestamp</td><td>Unix timestamp</td><td>lastPrice timestamp (reset with lastPrice) as per <code>scope</code></td></tr><tr><td>lastQuantity </td><td>Decimal</td><td>Last executed trade quantity as per the <code>scope</code></td></tr><tr><td>bidPrice</td><td>Decimal</td><td>Highest bid price</td></tr><tr><td>bidQuantity</td><td>Decimal</td><td>Sum of quantity of all orders with <code>bidPrice</code></td></tr><tr><td>askPrice</td><td>Decimal</td><td>Lowest ask price</td></tr><tr><td>askQuantity</td><td>Decimal</td><td>Sum of quantity of all orders with the <code>askPrice</code></td></tr><tr><td>timeStamp</td><td>Unix timestamp</td><td>The last timestamp (milliseconds) when one of the fields was updated.</td></tr><tr><td>openingPrice</td><td>Decimal</td><td>First order book trade on the day, will be empty until first execution is happening</td></tr><tr><td>low</td><td>Decimal</td><td>Lowest order book executed price of the day as per the <code>scope</code></td></tr><tr><td>high </td><td>Decimal</td><td>Highest order book executed price of the day as per the <code>scope</code></td></tr><tr><td>volume</td><td>Decimal</td><td>Total trade volume (in base asset) as per the <code>scope</code></td></tr><tr><td>quoteVolume </td><td>Decimal</td><td><p>Total trade volume in quote asset as per the <code>scope</code></p><p><mark style="color:blue;">BEFORE v1.54</mark></p><p><span class="math"> \xi (Trade Amount * Absolute Value Of Trade Price * Contract Size ) </span> <br><br><mark style="color:blue;">CHANGED v1.54</mark></p><p><span class="math"> \xi (Trade Amount * Absolute Value Of Trade Price * Contract Size * priceMultiplier) </span> </p></td></tr><tr><td>closingPrice </td><td>Decimal</td><td>Last day closing price (=last order book trade on the day)</td></tr><tr><td>closingPriceTimestamp  </td><td>Unix timestamp</td><td>The time where closing price was determined</td></tr><tr><td>settlementPrice</td><td>Decimal</td><td>Settlement price</td></tr><tr><td>settlementPriceTimestamp </td><td>Unix timestamp</td><td>Settlement price last update timestamp</td></tr><tr><td>status</td><td>Enum</td><td>The trading status of the instrument.<br>Trading/ Closed/ Halted/ AuctionCall/ AuctionCrossing</td></tr><tr><td> referencePrice </td><td>Decimal</td><td>Reference Price to be considered on the daily price bands and market rate circuit breakers. <br><br>It will be equal to the latest between Settlement Price and Closing Price. <br>Optionally auction price can override it. </td></tr><tr><td>referencePriceTimestamp</td><td>Unix timestamp</td><td>Reference Price last update timestamp</td></tr><tr><td>tickReferencePrice</td><td>Decimal</td><td><p>Tick Reference Price to be considered on the tick price bands and dynamic market rate circuit breakers. <br></p><p>It will be equal to the last order book execution price on current day, if not exits, it will be equal to <code>referencePrice</code></p></td></tr><tr><td> tickReferencePriceTimestamp</td><td>Unix timestamp</td><td>Tick Reference Price last update timestamp</td></tr><tr><td>volumeTypes</td><td>[] Object</td><td>Array of all relevant volumes</td></tr><tr><td>priceLimits</td><td>Object</td><td>object of all relevant price limits</td></tr></tbody></table>

volumeTypes object:

<table><thead><tr><th width="124.33333333333331">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>Enum</td><td><p>The type of the volume:</p><ul><li>Matched: Order book trades </li><li>RFQ: RFQ trades</li><li><mark style="color:blue;">(NEW v1.41.0)</mark> Refer <a href="https://documenter.getpostman.com/view/6229811/TzCV3jcq#909a5a9c-3385-43af-9569-ea3168a331e9">Admin API Trade Entry</a>/type field for additional values; all values are supported.</li></ul></td></tr><tr><td>volume</td><td>Decimal</td><td>Total trade volume (in base asset) for the specific volume type</td></tr></tbody></table>

priceLimits object:

<mark style="color:blue;">(NEW v1.55.0)</mark> API replies with all the price limits regardless of the scope and applicable price bands configuration at instrument level, consumers need to calculate the real price bands per trade type using the instrument details.

<table><thead><tr><th width="169">Name</th><th width="116">Type</th><th>Description</th></tr></thead><tbody><tr><td>dailyMinPrice</td><td>Decimal</td><td>The min price allowed based on the daily price bands</td></tr><tr><td>dailyMaxPrice</td><td>Decimal</td><td>The max price allowed based on the daily price bands</td></tr><tr><td>tickMinPrice</td><td>Decimal</td><td>The min price allowed based on the tick price bands</td></tr><tr><td>tickMaxPrice</td><td>Decimal</td><td>The max price allowed based on the tick price bands</td></tr><tr><td>minPrice</td><td>Decimal</td><td>The Instrument min price</td></tr><tr><td>maxPrice</td><td>Decimal</td><td>The Instrument max price</td></tr><tr><td>finalMaxPrice</td><td>Decimal</td><td><a data-footnote-ref href="#user-content-fn-1"><em>Min ([dailyMaxPrice], [tickMaxPrice], [maxPrice],[legsPriceBandMaxPrice])</em></a></td></tr><tr><td>finalMinPrice</td><td>Decimal</td><td><em>Max ([dailyMinPrice], [tickMinPrice], [minPrice], [legsPriceBandMinPrice])</em></td></tr><tr><td>legs</td><td>[] object</td><td>All the above fields for the legs with the below field</td></tr></tbody></table>

legs object:

All the above fields except `finalMaxPrice` & `finalMinPrice` and the below field:

| Name | Type | Description       |
| ---- | ---- | ----------------- |
| id   | int  | The id of the leg |

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```javascript
{
  "q": "v1/exchange.marketdata/lightTickers",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 10,
  "d": {
    "symbols": ["AMZN","INS1"],
    "interval": 2000
  }
}

```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "q": "v1/exchange.marketdata/lightTickers",
  "sid": 10,
  "d": {
    "status": "Trading",
    "symbol": "INS10",
    "lastPrice": 0.3333,
    "lastQuantity": 0.23,
    "bidPrice": 0.3333,
    "bidQuantity": 1.1,
    "askPrice": 0.4455,
    "askQuantity": 3.77,
    "timeStamp": 1675869327183,
    "openingPrice": 1.3333,
    "low": 0.3333,
    "high": 1.3333,
    "volume": 12.09,
    "quoteVolume": 10.083385,
    "closingPrice": 1,
    "closingPriceTimestamp": 1675807200002,
    "settlementPrice": 1.12,
    "settlementPriceTimestamp": 1673793853931,
    "volumeTypes": [
    {
      "type": "EFRP",
      "volume": 1234
    },
    {
      "type": "Block",
      "volume": 5555
    },
   {
      "type": "Other",
      "volume": 6666
    },
    {
      "type": "Matched",
      "volume": 7878
    },
    {
      "type": "RFQ",
      "volume": 44
    }
      ],
    "referencePrice": 1.68,
    "referencePriceTimestamp": 1686002400000,
    "tickReferencePrice": 2,
    "tickReferencePriceTimestamp": 1696002400000,
    "priceLimits": {
      "dailyMinPrice": 9.5,
      "dailyMaxPrice": 11.5,
      "tickMinPrice": 9.7,
      "tickMaxPrice": 12,
      "finalMinPrice": 9.7,
      "finalMaxPrice": 11.5,
      "legs": [
        {
          "id": 1,
          "minPrice": 1,
          "maxPrice": 1000
        },
        {
          "id": 2,
          "minPrice": 1.25,
          "maxPrice": 500
        }
      ]
    }
}
```
{% endtab %}

{% tab title="Empty Response" %}
<pre class="language-javascript"><code class="lang-javascript">//After v1.30.0
<strong>{
</strong>  "q": "v1/exchange.marketdata/lightTickers",
  "sid": 108,
  "d": {
    "symbol": "Test",
    "status": "Halted",
    "bidQuantity": 0,
    "askQuantity": 0,
    "timeStamp": 1717909200001
  }
}

<strong>//Before v1.30.0
</strong>{
  "q": "v1/exchange.marketdata/lightTickers",
  "sid": 12,
  "d": {
    "symbol": "INS10",
    "bidQuantity": 0,
    "askQuantity": 0,
    "status": "Trading",
    "timeStamp": 1674115200000,
    "volume": 0,
    "quoteVolume": 0
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

### **Error Codes**

<table><thead><tr><th width="169.57142857142856">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td>2</td><td>Missing fields: [Fieldname] |<br><mark style="color:blue;">(NEW v1.52.0)</mark> Stream disconnected</td></tr><tr><td>3</td><td>Wrong interval |<br>Wrong symbol</td></tr></tbody></table>

Note: unlike other errors, in case of `Wrong symbol` , stream will continue working for the valid symbols.&#x20;

This error might returned on the subscription but also in case that stream is already working but instrument was updated so that symbol is no longer active instrument <mark style="color:blue;">NEW</mark> or the symbol was changed.

### **Error Samples**

{% tabs %}
{% tab title="Error" %}
```json
{
  "sig": 2,
  "q": "v1/exchange.marketdata/lightTickers",
  "errorType": "500",
  "sid": 10,
  "d": {
    "errorCode": 3,
    "errorMessage": "Wrong interval"
  }
}
```
{% endtab %}

{% tab title="Wrong Symbol" %}
```json
{
  "q": "v1/exchange.marketdata/lightTickers",
  "sid": 10,
  "d": {
    "symbol": "Ins1",
    "bidQuantity": 0,
    "askQuantity": 0,
    "timeStamp":1675769165802,
    "errorCode": 3,
    "errorMessage": "Wrong symbol" 
  }
}
```
{% endtab %}
{% endtabs %}

[^1]: 
