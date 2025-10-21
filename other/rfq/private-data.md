# Private Data

Refer [here](../../ws/private-data-api.md) for mde details on Private Data APIs.

## massOrderStatus

Any participant can use the `massOrderStatus` API to retrieve the current status of all its own active RFQs and Quotes.

{% hint style="info" %}
qualifier: v1/exchange.market/massOrderStatus
{% endhint %}

### **Response**

`massOrderStatus` response provides close to real time list of all current active RFQs(to initiators) and Quotes(to initiators and dealers) with all the details.\
Each `massOrderStatus` response includes a`lastTrackingNumber`, the `trackingNumber`of the last event used to generate the response, this can be used as input in `executionReports`to start consuming the events from that point.\
\
Note: There are no request parameters.



**RFQStatus message**

<table><thead><tr><th width="234">Field</th><th>Description</th></tr></thead><tbody><tr><td>messageType</td><td><strong>RFQStatus</strong></td></tr><tr><td>side</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>instrument</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>quantity</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>mpId</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>mpName</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>accountType</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>parties</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>mpRfqId</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>expireTime</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>rfqId</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>lastEventId</td><td>eventId of the last event of the RFQ</td></tr><tr><td>lastEventTimestamp</td><td>eventTimestamp of the last event of the RFQ</td></tr><tr><td>status</td><td><p>Valid values:</p><ul><li>Active = RFQ is awaiting incoming Quotes</li></ul></td></tr></tbody></table>



### **Error Codes**

Refer [here](../../ws/private-data-api.md#error-codes) for the error codes.



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

{% tab title="Active RFQ" %}
```javascript
{
  "q": "v1/exchange.market/massOrderStatus",
  "sid": 13,
  "d": {
    "messageType": "RFQStatus",
    "rfqId": 11,
    "mpQuoteReqId": 1012,
    "side": "Buy",
    "instrument": "PQRS",
    "quantity": 5800,
    "marketModel": "RQ",
    "accountType": "House",
    "expireTime": 1689758604
    "parties": [
      {
        "id": "veryCoolStringId",
        "source": "D",
        "role": 12
      },
      {
        "id": "user123",
        "source": "D",
        "role": 13
      },
      {
        "id": "user1234",
        "source": "D",
        "role": 13
      }
    ],
    "lastEventTimestamp": 1695721347635332900,
    "lastEventId": 20,
    "mpId": 14,
    "mpName": "JPM-1",
    "status": "Active"
  }
}
```
{% endtab %}

{% tab title="Active Quote" %}
```json
{
  "q": "v1/exchange.market/massOrderStatus",
  "sid": 13,
  "d": {
    "messageType": "QuoteStatus",
    "rfqId": 11,
    "quoteId": 11,
    "mpQuoteId": 1012,
    "quoteDetails": [
        {
        "side":"Buy",
        "price":13.2,
        "quantity":100
        }
        ],
    "instrument": "PQRS",
    "marketModel": "RQ",
    "accountType": "House",
    "parties": [
      {
        "id": "veryCoolStringId",
        "source": "D",
        "role": 12
      },
      {
        "id": "user123",
        "source": "D",
        "role": 13
      },
      {
        "id": "user1234",
        "source": "D",
        "role": 13
      }
    ],
    "lastEventTimestamp": 1695721347635332900,
    "lastEventId": 20,
    "mpId": 14,
    "mpName": "JPM-1",
    "status": "Active"
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

Any participant can use the `executionReports`API to subscribe to events of,

* its own RFQs and Quotes
* Quotes received for the RFQs initated by them

There are few message types used in this API:

* **messages received only by the RFQ initiator**
  * RFQCreated
  * RFQCancelled
  * RFQEnded
* **messages received by the RFQ initiator and dealer**
  * QuoteCreated
  * QuoteCancelled
  * QuoteExecuted

{% hint style="info" %}
qualifier: v1/exchange.market/executionReports
{% endhint %}

### **Request** <a href="#request" id="request"></a>

<table><thead><tr><th width="180">Parameter</th><th width="110.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>trackingNumber <code>optional</code></td><td>Long</td><td>Same as defined in executionReports <a data-mention href="private-data.md#request">#request</a></td></tr></tbody></table>

### **Response** <a href="#response-1" id="response-1"></a>

#### Messages received only by the RFQ initiator

<table><thead><tr><th width="159">Message Type</th><th width="177.33333333333331">Field</th><th>Description</th></tr></thead><tbody><tr><td>All</td><td>messageType</td><td><p>One of the below values:</p><ul><li><strong>RFQCreated</strong></li><li><strong>RFQCancelled</strong></li><li><strong>RFQEnded</strong></li></ul></td></tr><tr><td>All</td><td>side</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>All</td><td>instrument</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>All</td><td>quantity</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>All</td><td>mpId</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>All</td><td>mpName</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>All</td><td>accountType</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>All</td><td>parties</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>All</td><td>mpRfqId</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>All</td><td>rfqId</td><td>Unique id generated by the system for the RFQ</td></tr><tr><td>All</td><td>expireTime</td><td>Same as in <code>submitRFQ</code> request</td></tr><tr><td>All</td><td>eventId</td><td>Sequence identifier per instrument for the event</td></tr><tr><td>All</td><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>All</td><td>trackingNumber</td><td>Event tracking number</td></tr><tr><td>RFQCancelled</td><td>cancelReason</td><td><p>One of the below options:</p><ul><li>Expiration</li><li>CancelRequest</li></ul><p>Refer <a href="private-data.md#cancel-reason">here</a> for the descriptions.</p></td></tr></tbody></table>

#### Messages received by the RFQ Initiator and Quote Dealer

<table><thead><tr><th width="170.33333333333331">Message Type</th><th width="164">Field</th><th width="286">Description</th><th width="155">Sent to</th></tr></thead><tbody><tr><td>All</td><td>messageType</td><td><p>One of the below values:</p><ul><li><strong>QuoteCreated</strong></li><li><strong>QuoteExecuted</strong></li><li><strong>QuoteCancelled</strong></li></ul></td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>All</td><td>instrument</td><td>Same as in <code>submitRFQ</code> request</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>All</td><td>mpQuoteId</td><td>Same as in <code>submitQuote</code> request</td><td>Quote Dealer</td></tr><tr><td>All</td><td>quoteId</td><td>Unique id generated by the system for the Quote.</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>All</td><td>mpRfqId</td><td>Same as in <code>submitRFQ</code> request</td><td>RFQ Initiator</td></tr><tr><td>All</td><td>rfqId</td><td>Unique id generated by the system for the RFQ.</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>All</td><td>eventId</td><td>Sequence identifier per instrument for the event</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>All</td><td>eventTimestamp</td><td>Event timestamp (in nanoseconds) in GMT</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>All</td><td>trackingNumber</td><td>Event tracking number</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteCreated<br>QuoteCancelled</td><td>quoteDetails</td><td>Same as in <code>submitQuote</code> request</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteCreated<br>QuoteCancelled</td><td>mpId</td><td>Same as in <code>submitQuote</code> request</td><td>Quote Dealer</td></tr><tr><td>QuoteCreated<br>QuoteCancelled</td><td>mpName</td><td>Same as in <code>submitQuote</code> request</td><td>Quote Dealer</td></tr><tr><td>QuoteCreated<br>QuoteCancelled</td><td>parties</td><td>Same as in <code>submitQuote</code> request</td><td>Quote Dealer</td></tr><tr><td>QuoteCreated<br>QuoteCancelled</td><td>accountType</td><td>Same as in <code>submitQuote</code> request</td><td>Quote Dealer</td></tr><tr><td>QuoteExecuted</td><td>side</td><td>MP’s side of the trade</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteExecuted</td><td>price</td><td>Price of this trade</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteExecuted</td><td>quantity</td><td>Quantity of this trade</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteExecuted</td><td>mpId</td><td>MP Id relevant to side of trade</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteExecuted</td><td>mpName</td><td>MP Name relevant to side of trade</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteExecuted</td><td>tradeId</td><td>Unique ID for the match</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteExecuted</td><td>tradingMode</td><td>RQ - Request for Quotes</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteExecuted</td><td>parties</td><td>For Quote Dealer<br>Same as in <code>submitQuote</code> request<br><br>For RFQ Initiator<br>Same as in <code>submitRFQ</code> request</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteExecuted</td><td>accountType</td><td>For Quote Dealer<br>Same as in <code>submitQuote</code> request<br><br>For RFQ Initiator<br>Same as in <code>submitRFQ</code> request</td><td>Quote Dealer<br>RFQ Initiator</td></tr><tr><td>QuoteCancelled</td><td>cancelReason</td><td><p>One of the below options:</p><ul><li>RFQEnded</li><li>Expiration</li><li>RFQCancelled</li><li>CancelRequest</li></ul><p>Refer <a href="private-data.md#cancel-reason">here</a> for the descriptions.</p></td><td>Quote Dealer<br>RFQ Initiator</td></tr></tbody></table>



### Cancel Reason

<table><thead><tr><th width="205">Cancel Reason</th><th>Description</th></tr></thead><tbody><tr><td>Expiration</td><td>When the RFQ is cancelled due to elapsing expireTime OR,<br>When the related RFQ of a Quote is cancelled due to expiration.</td></tr><tr><td>CancelRequest</td><td>When the RFQ is cancelled by the initiator OR,<br>When the Quote is cancelled by the dealer.</td></tr><tr><td>RFQEnded</td><td>When the related RFQ is cancelled due to expiration.</td></tr><tr><td>RFQCancelled</td><td>When the related RFQ is cancelled by the initiator</td></tr></tbody></table>



## trades

Refer [here](../../ws/private-data-api.md#trades) for more details on Trades API

`RFQTrade` messages are disseminated in addition to other types of messages.

### **Request**

<table><thead><tr><th width="170.33333333333331">Parameter</th><th width="84">Type</th><th width="483.66666666666674">Description</th></tr></thead><tbody><tr><td>trackingNumber <code>optional</code></td><td>Long</td><td>Same as defined in executionReports <a data-mention href="private-data.md#request">#request</a></td></tr></tbody></table>



### **Response**

<table><thead><tr><th width="231">Field</th><th width="521">Description</th></tr></thead><tbody><tr><td>eventId</td><td>Event Id </td></tr><tr><td>timestamp</td><td>Event timestamp (in nanoseconds) in GMT</td></tr><tr><td>actionType</td><td>RFQTrade</td></tr><tr><td>orderId</td><td><p>Initiator side - rfqId</p><p>Dealer side - quoteId</p></td></tr><tr><td>mpOrderId</td><td><p>Initiator side - mpRfqId</p><p>Dealer side - mpQuoteId</p></td></tr><tr><td>mpId</td><td>MP Id </td></tr><tr><td>mpName</td><td>MP Name </td></tr><tr><td>instrumentId</td><td>Instrument id </td></tr><tr><td>Instrument</td><td>Instrument symbol</td></tr><tr><td>side</td><td>Buy/Sell </td></tr><tr><td>price</td><td>Trade price </td></tr><tr><td>quantity</td><td>Trade quantity </td></tr><tr><td>tradeId</td><td>matchId </td></tr><tr><td>tradingMode</td><td><strong>RQ</strong> - RFQ Trade</td></tr><tr><td>accountType</td><td>Optional, From source Quote </td></tr><tr><td>parties</td><td>Optional, From source Quote </td></tr><tr><td>makerTaker</td><td><p>Initiator side - Taker</p><p>Dealer side - Maker</p></td></tr><tr><td>tradeDate</td><td>Date of the business day of that trade<br>Format: YYY-MM-DD</td></tr><tr><td><mark style="color:blue;">(NEW v1.48.0)</mark><br>tickReferencePrice</td><td>Tick Reference Price at the time trade is captured in system<br>If not exists, empty</td></tr><tr><td>trackingNumber</td><td>Tracking number</td></tr><tr><td>multiLegReportingType</td><td><ul><li>SingleSecurity: non strategy</li><li>MultiLegSecurity: strategy </li><li>IndividualLeg: strategy leg </li></ul><p>For more details see <a href="../../ws/private-data-api.md#strategies-multi-legs-handling">here</a></p></td></tr></tbody></table>

### **Error Codes**

Refer [here](../../ws/private-data-api.md#error-codes-2) for the error codes

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```javascript
{
  "q": "v1/exchange.market/trades",
  "sid": 16,
  "d": {
    "trackingNumber": 12345
  }
}
```
{% endtab %}

{% tab title="RFQTrade" %}
```javascript
{
  "q": "v1/exchange.market/trades",
  "sid": 3,
  "d": {
    "actionType": "RFQTrade",
    "timestamp": 1698930160328787700,
    "trackingNumber": 104765920,
    "eventId": 21,
    "mpId": 14,
    "mpName": "JJJ-1",
    "instrumentId": 4136,
    "instrument": "RFQINST2",
    "side": "Buy",
    "price": 15,
    "quantity": 1300,
    "tradeId": 4,
    "tradingMode": "RQ",
    "accountType": "House",
    "parties": [
      {
        "id": "StringId",
        "source": "D",
        "role": 12
      },
      {
        "id": "user123",
        "source": "D",
        "role": 13
      }
    ],
    "tradeDate": "2023-11-02",
    "multiLegReportingType": "SingleSecurity"
  }
}
```
{% endtab %}
{% endtabs %}
