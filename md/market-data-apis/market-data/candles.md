# Candles

This API disseminates aggregated data on different time resolutions. It supports retrieving data for a given period or as a continuous stream. Updates are disseminated every 3 seconds (only in case of a change) or when the candle is closed.&#x20;

Response description&#x20;

For real time updates qualifier:&#x20;

* `to` is ignored&#x20;
* When `from` was not sent:  only new streaming messages will be sent
* When only `from` was sent - candles from that point will be returned, stream will remain open for the new streamed messages to come

For historical data qualifier: from & to must be sent&#x20;

* `from` & `to` must be sent: candles from that period will be returned, stream will be closed after last message.
  * In case there are no candles in range,  system will return 1 candle which is the last candle prior to range (just before the “from”)

This API is non real-time but close to real-time.

below fields of  the response are adjusted for Corporate Actions:

* open
* close
* high
* low
* volume
* vwa

{% hint style="info" %}
Real time updates qualifier: v1/exchange.marketdata/candles

Historical data qualifier: v1/exchange.marketdata.history/candles
{% endhint %}

### **Request**

<table><thead><tr><th width="139.6710763680096">Parameter</th><th width="127">Type</th><th width="482.2">Description</th></tr></thead><tbody><tr><td>symbol</td><td>String</td><td>Symbol to retrieve the candles</td></tr><tr><td>from<br><code>optional</code></td><td>Int or<br>Unix timestamp</td><td><p>Candles start from </p><p>format: if <code>timespan=DAY</code>: yyyyMMdd else epoch time in seconds</p></td></tr><tr><td>to<br><code>optional</code></td><td>Int or<br>Unix timestamp</td><td><p>Candles to</p><p>Same format as <code>from</code>, refer description of <code>from</code> for more details.</p></td></tr><tr><td>timespan</td><td>eNum</td><td><p>One of below values:</p><p>MINUTE -  flat min (from 00 seconds to 59.999 seconds) </p><p>HOUR- based on minute candle and equal to 60 min</p><p>DAY - Based on trade date (starts on calendar time zone midnight)</p></td></tr><tr><td>multiplier <code>optional</code></td><td>Int</td><td><p>Timespan multiplier, if not sent: 1 is default value. <br>Allowed intervals:</p><ul><li>1 MINUTE </li><li>5 MINUTE </li><li>15 MINUTE </li><li>1 HOUR</li><li>4 HOUR</li><li>1 DAY </li></ul></td></tr><tr><td>caAdjustments</td><td>Boolean</td><td><p>Optional</p><p>If not exist, <code>caAdjustments=false</code></p><p></p><p>Whether historical prices should be adjusted for corporate actions. Refer Admin API to create adjustment factors.</p><p></p><p>Applicable only to history API (<code>exchange.marketdata.history/candles</code>)</p><p><br></p><p>Available values:</p><ul><li><code>true</code> - apply caAdjustments on data </li><li><code>false</code> - do not apply caAdjustments on data</li></ul></td></tr></tbody></table>

### **Response**

<table><thead><tr><th width="196.8239997035043">Parameter</th><th width="133">Type</th><th width="403.2">Description</th></tr></thead><tbody><tr><td>symbol</td><td>String</td><td>Instrument symbol </td></tr><tr><td>open</td><td>Decimal</td><td>First order book execution price of the interval</td></tr><tr><td>close</td><td>Decimal</td><td>Last order book execution price of the interval</td></tr><tr><td>high</td><td>Decimal</td><td>Highest order book execution price of the interval</td></tr><tr><td>low</td><td>Decimal</td><td>Lowest order book execution price of the interval</td></tr><tr><td>volume</td><td>Decimal</td><td>Total volume of order book execution price of the interval</td></tr><tr><td>vwa</td><td>Decimal</td><td>Volume weighted average price of order book executions of the interval</td></tr><tr><td>timeStamp</td><td>Int or<br>Unix timestamp</td><td><p>Starting timestamp of the interval </p><p>format: if <code>timespan=DAY</code>: yyyyMMdd else epoch time in sec</p></td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="93.27803690934905">Code</th><th width="554.4285714285713">Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td>2</td><td>Missing or invalid parameter: [FieldName] |<br><mark style="color:blue;">(NEW v1.52.0)</mark> Stream disconnected</td></tr><tr><td>3</td><td>Wrong symbol [symbol]</td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Subscription (Real Time)" %}
```json
{
  "q": "v1/exchange.marketdata/candles",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 14,
  "d": {
    "symbol": "AAPL",
    "timespan": "MINUTE",
    "multiplier": 1,
    "from": 1701159900
  }
}
```
{% endtab %}

{% tab title="Subscription (History)" %}
```json
{
  "sid": 14,
  "q": "v1/exchange.marketdata.history/candles",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoxMTV9",
  "d": {
    "symbol": "Spot",
    "timespan": "MINUTE",
    "from": 1712970598,
    "to": 1713094027,
    "multiplier": 1
  }
}
```
{% endtab %}

{% tab title="Response (MINUTE)" %}
```json
{
  "q": "v1/exchange.marketdata/candles",
  "sid": 14,
  "d": {
    "symbol": "TEST2",
    "open": 110.5,
    "close": 100.5,
    "high": 120.5,
    "low": 90.5,
    "volume": 10000,
    "vwa": 100.5,
    "timeStamp": 1709536080000
  }
}
```
{% endtab %}

{% tab title="Response (DAY)" %}
```json
{
  "q": "v1/exchange.marketdata/candles",
  "sid": 16,
  "d": {
    "symbol": "TEST1",
    "open": 10.5,
    "close": 14.5,
    "high": 14.5,
    "low": 10.5,
    "volume": 450,
    "vwa": 11.922222222222222,
    "timeStamp": 20240304
  }
}
```
{% endtab %}

{% tab title="Error" %}
```json
{
  "sig": 2,
  "q": "v1/exchange.marketdata/candles",
  "errorType": "500",
  "sid": 13,
  "d": {
    "errorCode": 3,
    "errorMessage": "Wrong symbol 0Test"
  }
}
```
{% endtab %}
{% endtabs %}
