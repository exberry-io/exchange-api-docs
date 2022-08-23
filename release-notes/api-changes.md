# API Changes

## Coming Soon... :hammer\_pick:

New Fields In Place Order & Execution Reports & massOrderStatus

* Market Participant Group - The ability to access all private data API for a group of market participants, or all market participants.&#x20;
* [New fields in placeOrder , exeutionReports and massOrderStatus](api-changes.md#place-order-and-execution-reports-and-massorderstatus)
* [Trade Entry](api-changes.md#trade-entry)
* [Trade Cancellation](api-changes.md#trades-cancellation)&#x20;
* [Reporting - New version of Trades API](api-changes.md#reporting-trades-api)&#x20;



### Place Order & Execution Reports & massOrderStatus&#x20;

On `placeOrder` and `exeutionReports` new optional fields will be added:&#x20;

* accountType: House/ Client&#x20;
* parties: array of parties, the concept is identical the the parties element in [FIX protocol ](https://fiximate.fixtrading.org/en/FIX.Latest/cmp1012.html?find=PartyIDSource)

Party specification:&#x20;



| Name   |        | Description                         |
| ------ | ------ | ----------------------------------- |
| id     | String | <p>Party id<br>Max length = 20 </p> |
| source | Char   | Party source                        |
| role   | Int    | Party role                          |

on `executionReports` and `massOrderStatus` those fields were added:

* mpId: market participant ID
* mpName: market participant name&#x20;

{% tabs %}
{% tab title="placeOrder" %}
```javascript
{
  "q": "v1/exchange.market/placeOrder",
  "sid": 1,
  "d": {
    "orderType": "Limit",
    "side": "Buy",
    "quantity": 1.3,
    "price": 100.33,
    "instrument": "INS1",
    "mpOrderId": 1001,
    "timeInForce": "GTC",
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
      }
    ]
  }
}
```
{% endtab %}

{% tab title="executionReports" %}
{% code overflow="wrap" %}
```json
{
  "q": "v1/exchange.market/executionReports",
  "sid": 103,
  "d": {
    "messageType": "Add",
    "orderId": 43,
    "mpOrderId": 13,
    "orderType": "Limit",
    "side": "Buy",
    "instrument": "TSTUSD",
    "quantity": 1.3,
    "price": 100.33,
    "timeInForce": "GTC",
    "orderTimestamp": 1660572732860184300,
    "filledQuantity": 0,
    "remainingOpenQuantity": 1.3,
    "removedQuantity": 0,
    "marketModel": "T",
    "userId": "UserTest1",
    "eventTimestamp": 1660572732860184300,
    "eventId": 68,
    "trackingNumber": 50401152,
    "mpId": 12345,
    "mpName": "Name1",
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
      }
    ]
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Trade Entry

Trade Entry️ enable to report about trades that were done between market participants out of the order book.&#x20;

This is going to impact [#orderbookdepth](../market-data.md#orderbookdepth "mention") and [#executionreports](../private-data-api.md#executionreports "mention")

{% tabs %}
{% tab title="orderBookDepth" %}
<pre class="language-json"><code class="lang-json">{
  "q": "v1/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
    "eventId": 29969,
    "messageType": "TradeReport",
    "tradeType": "Block",
    "matchId": 12345,
    "eventTimestamp": 1565790374956123123,
    "instrument": "AMZ",
    "buyMpId": "12",
    "sellMpId": "13",
<strong>    "quantity": 1,
</strong>    "price": 1.22,
    "trackingNumber": 100
  }
}</code></pre>
{% endtab %}

{% tab title="executionReport" %}
```javascript
{
  "q": "v1/exchange.market/executionReports",
  "sid": 104,
  "d": {
    "messageType": "TradeReport",
    "tradeType": "Block",
    "side": "Buy",
    "instrument": "INS1",
    "quantity": 1,
    "price": 1,
    "eventTimestamp": 1620913727891980000,
    "eventId": 454,
    "matchId": 12345,
    "tradingMode": "ON",
    "accountType": "Client",
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
    "trackingNumber": 34272768
  }
}
```
{% endtab %}
{% endtabs %}

### Trades Cancellation&#x20;

Trade cancellation enable to cancel trades that were done on the last business day .&#x20;

This is going to impact [#orderbookdepth](../market-data.md#orderbookdepth "mention") and [#executionreports](../private-data-api.md#executionreports "mention")



{% tabs %}
{% tab title="orderBookDepth" %}
<pre class="language-json"><code class="lang-json">{
  "q": "v1/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
    "eventId": 29969,
    "messageType": "TradeCancel",
    "matchId": 12345,
    "eventTimestamp": 1565790374956123123,
    "instrument": "AMZ",
    "buyMpId": "12",
    "sellMpId": "13",
<strong>    "quantity": 1,
</strong>    "price": 1.22,
    "trackingNumber": 100
  }
}</code></pre>
{% endtab %}

{% tab title="executionReport" %}
```javascript
{
  "q": "v1/exchange.market/executionReports",
  "sid": 104,
  "d": {
    "messageType": "TradeCancel",
    "tradeType": "Block",    //only in case of trade entry
    "side": "Buy",
    "instrument": "INS1",
    "quantity": 1,
    "price": 1,
    "eventTimestamp": 1620913727891980000,
    "eventId": 454,
    "matchId": 12345,
    "tradingMode": "ON",
    "accountType": "Client",
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
    "trackingNumber": 34272768
  }
}
```
{% endtab %}
{% endtabs %}



### Reporting - Trades API&#x20;

New version is going to be release to [#trades](../reporting-api.md#trades "mention") API.\
Previous version is still supported but will not include new developments, we encourage you to migrate to the new version.  &#x20;

{% hint style="info" %}
endpoint: v2/exchange.reporting/mp/trades
{% endhint %}

In the new version each record will be one of the following `actionType`:

* MatchedTrade for order book trade&#x20;
* TradeReport for trade entry trade
* TradeCancel for trade cancellation (separate records will be returned for cancellation)

Additional changes:

* `matchId` renamed to `tradeId`
* `executedPrice` renamed to  `price`
* `executedQuantity` renamed to `quantity`
* New fields&#x20;
  * `tradeType` - EFRP/Block/Other (only for TradeReport trades)
  * `makerTaker`
  * `accountType`&#x20;
  * `parties`
*

{% tabs %}
{% tab title="MatchedTrade " %}
```json
{
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
    "eventId": 925,
    "timestamp": "2021-12-15T21:02:45.189982",
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
    "makerTaker": "Taker" 
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
    "timestamp": "2021-12-15T21:02:45.189982",
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
    "timestamp": "2021-12-15T21:02:45.189982",
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
  }
}
```
{% endtab %}
{% endtabs %}



## 2022-05-16✔️

`replaceOrder` API is now available, it allows cancel and add new order in a single command.

See [#replaceorder](../trading-api.md#replaceorder "mention") for more details.

## 2022-05-11✔️

Introduce new version of `orderBookDepth` and `orderBookState`

{% hint style="info" %}
The new versions are available on:

* `v2/exchange.market/orderBookState`
* `v2/exchange.market/orderBookDepth`
{% endhint %}

This version includes market events, now you can get real time messages when trading is being halted / resumed, when market is open / closed and more.\
See [here ](../market-data.md#instrument-status-message-1)for more details.

{% hint style="warning" %}
The old endpoints (v1) are still up and running, but will be deprecated in one of the next releases.\
New features/ fixes will be introduced only for the new version.

\
Please update your systems to work with the new endpoints.
{% endhint %}

## 2021-12-14✔️

Reporting API is now available allow easy retrieval of orders and trades

## 2021-10-04✔️

`orderBookDepth` and `executionReports` :

* Now have a dedicated `errorCode:2` for case the active stream is being disconnected.
* Bug fix regarding sorting

## 2021-06-29 ✔️

Additions:

* [FIX Gateway](../fix-trading/introduction.md) is now available, contact us if you would like to try it\\

Changes:

* `userId` is now optional field within `placeOrder` command
* Error message for errorCode =1 was changed for `orderBookState` and `massOrderStatus`
* Admin API [changes](https://documenter.getpostman.com/view/6229811/TzCV3jcq#api-changes):
  * Instrument symbol can now contains .(dot) and -(hyphen)
  * Calendar.tradingDays is now optional to allow auctions only trading schedule

## 2021-05-13 ✔️

Additions:

* [Private API](../private-data-api.md) to get your own detailed orders data on a simple way.
* [Admin API ](../admin-api.md)for the entire administration activity can now be done via an API.

## 2021-03-10 ✔️

Additions:

* New trading API `modifyOrder` that allows to reduce order quantity without losing the order priority on book.
* New messageType `Modified` in`orderBookDepth` for modified orders
  * Note: this require a new `apiKey`
* New `placeOrder` validations (tick size, price collar, max order value and max book depth).
* Add `accountId` as optional parameter to `placeOrder`

## 2020-11-12 ✔️

Websocket endpoint changed to be `wss://exchange-gateway-uat.exchange.uat.exberry.io`

## 2020-11-11 ✔️

**Additions:**

* Auctions support:
  * New TIF (GAA)
  * New placeOrder validations

⚠️ **API Changes:**

New API version released for Trading & Market Data API.\
All endpoints should now include v1 ass follow:

* `v1/exchange.market/placeOrder`
* `v1/exchange.market/cancelOrder`
* `v1/exchange.market/massCancel`
* `v1/exchange.market/orderBookDepth`
* `v1/exchange.market/orderBookState`

{% hint style="warning" %}
The old endpoint are still up and running, but will be deprecated in one of the next releases.\
Please update your systems to work with the new endpoints.
{% endhint %}

Renaming Trading & Market Data API `Broker` to `MP`(Market participant)

* **placeOrder** and **cancelOrder**
  * `brokerOrderId` :soon:`mpOrderId`
* **orderBookDepth**
  * Add & Cancel Message-
    * `brokerId` :soon:`mpId`
    * `brokerOrderId` :soon:`mpOrderId`
  * Execute Message-
    * `takerBrokerId` :soon: `takerMpId`
    * `takerBrokerOrderId` :soon: `takerMpOrderId`
    * `makerBrokerId` :soon:`makerMpId`
    * `makerBrokerOrderId` :soon: `makerMpOrderId`

{% hint style="info" %}
The new names are available on new version endpoints only:

* `v1/exchange.market/orderBookDepth`
* `v1/exchange.market/placeOrder`
* `v1/exchange.market/cancelOrder`
{% endhint %}

{% hint style="warning" %}
The old fields names are available on old version endpoints only (it is required to migrate to new endpoints):

* `exchange.market/orderBookDepth`
* `exchange.market/placeOrder`
* `exchange.market/cancelOrder`
{% endhint %}

**Other Changes:**

* Generic error message errorCode 1 `Exchange is unavailable`

## 2020-08-26 ✔️

**Added**

* New Trading API method `massCancel`to cancel all resting orders for the relevant market participant.
* IOC & FOK are now supported in limit order (and not only market order).

## 2020-06-28 ✔️

**Added**

* New Market Data API method `orderBookState`to retrieve snapshot of order book state.

## 2020-06-24 ✔️

**Added**

* New `trackingNumber`as input and output parameter to `orderBookState`to allow stream consumption from specific event.

**Changed**

* On `orderBookState`for "Cancelled" events, `orderSide`changed to be `side`
* `q`field was changed in all messages, in addition it is now returned as part of stream closure message.
