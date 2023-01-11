# API Changes

## Coming Soon... :hammer\_pick:

## 2022-01-03

* New Filter in   [Trades Reports](../reporting-api.md#trades-v2)&#x20;

```
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
     "dateFrom": "2020-10-01T00:00:01",
     "dateTo": "2022-10-01T00:00:01",
     "mpId": 2087505425,
     "tradeId": 42,
     "instruments":["Sp1"],
     "actionTypes": [ "TradeReport","TradeCancel", "MatchedTrade"],
     "multiLegReportingTypes": ["SingleSecurity", "IndividualLeg", "None"]
  }
}
```

* New `errorCode` on `Trades V2` API - 1001
* New filters in [orders](../reporting-api.md#orders)

```
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

* New `errorCode` on `Orders` API - 1001
* New trading mode and market model.
* On rejected ExecutionReport _(MsgType = 8) :_  Side (54) = 1 (Buy)

##

## 2022-12-06✔️

* New FIX Drop Copy Client initiated message : [Mass Order Status Request ](../fix-trading/trading-messages-1.md#ordermassstatusrequest-af)
* Place Order validation that price must ne positive was removed
* New Filters in   [Trades Reports](../reporting-api.md#trades-v2)  &#x20;

```
  "q": "v2/exchange.reporting/mp/trades",
  "sid": 10,
  "d": {
     "dateFrom": "2020-10-01T00:00:01",
     "dateTo": "2022-10-01T00:00:01",
     "mpId": 2087505425,
     "tradeId": 42,
     "instruments":["Sp1"],
      "actionTypes": [ "TradeReport","TradeCancel", "MatchedTrade"]
  }
}
```

* New errorCode on `createSession` API - 6003&#x20;

## 2022-11-21✔️

Trading API

* From now on, negative prices are allowed, if you want to make sure that no negative process will be sent for a specific instrument you should configure that minimum price to be 0.0001 or so.&#x20;
*   FIX API:

    * On Order Cancel Reject (msgType=9) in case of failure: OrdStatus(39) was missing, now will have the value 8 = Rejected
    * On Execution Report (msgType=8) in case of rejection OrderID(37) was missing, now will have the value -1

    \


    ### Orders&#x20;

    On [#massorderstatus](../private-data-api.md#massorderstatus "mention")and [#orders](../reporting-api.md#orders "mention") new fields are added

    * &#x20;parties&#x20;
    * accountType

**Strategy Trades**&#x20;

Strategy trades are sent with 2 models:

* Single Security: reports the strategy trade as is
* Individual leg of a multi-leg security: reports the underlying legs of the strategy&#x20;

Each strategy related trade will be sent with the following new parameters:

* multiLegReportingType:
  * SingleSecurity for parent trade&#x20;
  * IndividualLeg for legs trades&#x20;
* tradeLegRefId: leg index
* multiLegDifferentialPrice: Parent trade price
* multiLegStrategyInstrumentId: Parent trade instrument Id
* multiLegStrategyTradeId: Parent trade Id

Strategy Trade Sample

```json
{
  "q": "v1/exchange.market/trades",
  "sid": 155,
  "d": {
    "actionType": "TradeReport",
    "timestamp": 1668524835153741000,
    "trackingNumber": 69920,
    "eventId": 1,
    "mpId": 2087505415,
    "mpName": "Test1",
    "instrumentId": 17,
    "instrument": "Test1Feb-Mar23",
    "side": "Buy",
    "price": 3,
    "quantity": 10,
    "tradeId": 1,
    "tradingMode": "ON",
    "accountType": "Client",
    "parties": [
      {
        "id": "33",
        "source": "D",
        "role": 38
      }
    ],
    "tradeType": "EFRP",
    "tradeDate": "2022-11-15",
    "multiLegReportingType": "SingleSecurity"
  }
}
```

Strategy Leg Trade Sample

```json
{
  "q": "v1/exchange.market/trades",
  "sid": 155,
  "d": {
    "actionType": "TradeReport",
    "timestamp": 1668524835153741000,
    "trackingNumber": 70176,
    "mpId": 2087505415,
    "mpName": "Test1",
    "instrumentId": 14,
    "instrument": "Test1Feb23",
    "side": "Buy",
    "price": 1.5,
    "quantity": 10,
    "tradeId": 1,
    "tradingMode": "ON",
    "accountType": "Client",
    "parties": [
      {
        "id": "33",
        "source": "D",
        "role": 38
      }
    ],
    "tradeType": "EFRP",
    "tradeDate": "2022-11-15",
    "multiLegReportingType": "IndividualLeg",
    "tradeLegRefId": 1,
    "multiLegDifferentialPrice": 3,
    "multiLegStrategyInstrumentId": 17,
    "multiLegStrategyTradeId": 1
  }
}
```

On FIX Drop Copy - subscriber will define the reposting type by sending MultiLegReportingType (442) :

* 1 = Single security (default if not specified): only the parent trade is being sent
* 2 = Individual leg of a multi-leg security: only the underlying legs trades are being sent

Strategy Trade Sample

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=273|35=AE|49=EXBERRY|56=mastergroup|34=11|52=20221115-15:14:40.120488|568=1668525280273|912=N|487=0|828=2|55=Test1Feb-Mar23|32=10.0000|31=3.000000|75=20221115|60=20221115-15:07:15.153740|552=1|54=1|581=1|453=2|448=33|447=D|452=38|448=Test1|447=D|452=7|797=Y|570=N|1003=1|442=1|10=050|
```
{% endcode %}

Strategy Leg Trade Sample

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=290|35=AE|49=EXBERRY|56=mastergroup|34=6|52=20221115-15:13:05.331013|568=1668525185446|912=Y|487=0|828=2|55=Test1FMar23|32=10.0000|31=-1.500000|75=20221115|60=20221115-15:07:15.153740|552=1|54=2|581=1|453=2|448=33|447=D|452=38|448=Test1|447=D|452=7|797=Y|570=N|1003=1|442=2|824=2|1522=3.000000|10=223|2022-11-01|
```
{% endcode %}

##

## 2022-11-01✔️

* [#tradecapturereport-ae](../fix-trading/trading-messages-1.md#tradecapturereport-ae "mention") now support Snapshot + Updates (Subscribe)
* EOD event is now being published on 2 APIs:

orderBookDepth

```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 10,
  "d": {
    "messageType": "CalendarEndOfDay",
    "eventTimestamp": 1667248263443774000,
    "calendarId": 2082487149,
    "calendarName": "Test1",
    "eodDate": "2022-10-31",
    "trackingNumber": 234773792
  }
}
```

trades:

```json
{
  "q": "v1/exchange.market/trades",
  "sid": 16,
  "d": {
    "actionType": "CalendarEndOfDay",
    "timestamp": 1667248263443774000,
    "trackingNumber": 234773792,
    "calendarId": 2082487149,
    "calendarName": "Test1",
    "eodDate": "2022-10-31"
  }
}
```

## 2022-10-03✔️

Drop Copy for FIX GW - see [here](../fix-trading/trading-messages-1.md)&#x20;



New `trades` API that is a real time stream for getting all the trades data&#x20;

{% hint style="info" %}
`qualifier: v1/exchange.market/trades`
{% endhint %}

Available messages in that stream :&#x20;

* MatchedTrade: for order book trade&#x20;
* TradeReport: for trade entry trade&#x20;
* TradeCancel: for trade cancellation (separate records will be returned for cancellation)

| Field          | Description                                                                                                                                                                        | Order Book | Trade Entry | Trade Cancel |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------: | :---------: | :----------: |
| eventId        |  Event Id                                                                                                                                                                          |      V     |      V      |       V      |
| timestamp      | Event timestamp (in nanoseconds) in GMT                                                                                                                                            |      V     |      V      |       V      |
| actionType     | <p>MatchedTrade </p><p>TradeReport </p><p>TradeCancel</p>                                                                                                                          |      V     |      V      |       V      |
| orderId        | Order id initiated the trade                                                                                                                                                       |      V     |             |              |
| mpOrderId      | From source order                                                                                                                                                                  |      V     |             |              |
| mpId           |  MP Id                                                                                                                                                                             |      V     |      V      |       V      |
| mpName         | MP Name                                                                                                                                                                            |      V     |      V      |       V      |
| instrumentId   |  Instrument id                                                                                                                                                                     |      V     |      V      |       V      |
| Instrument     | instrument symbol                                                                                                                                                                  |      V     |      V      |       V      |
| side           |  Buy/ Sell                                                                                                                                                                         |      V     |      V      |       V      |
| price          |  Trade price                                                                                                                                                                       |      V     |      V      |       V      |
| quantity       |  Trade quantity                                                                                                                                                                    |      V     |      V      |       V      |
| tradeId        | matchId                                                                                                                                                                            |      V     |      V      |       V      |
| tradingMode    | IA - (Scheduled Intraday Auction) -When execution was as part of auction CT (Continuous Trading) - When execution was done on a regular trading ON - Trade Reporting (On Exchange) |      V     |      V      |       V      |
| accountType    | Optional, From source order                                                                                                                                                        |      V     |      V      |       V      |
| parties        | Optional, From source order                                                                                                                                                        |      V     |      V      |     `opt`    |
| tradeType      | EFRP/Block/Other                                                                                                                                                                   |            |      V      |     `opt`    |
| makerTaker     | <p><strong>Taker</strong> if order was never resting on the book for that trade <br><strong>Maker</strong> if order was resting on the book for that trade</p>                     |      V     |             |              |
| tradeDate      | <p>Date of the business day of that trade<br>Format: YYY-MM-DD</p>                                                                                                                 |            |             |              |
| trackingNumber |  Tracking number                                                                                                                                                                   |            |             |              |

Samples:

{% tabs %}
{% tab title="Subscription" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 16,
  "d": {
    "trackingNumber": 12345
  }
}
```
{% endtab %}

{% tab title="MatchedTrade " %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 16,
  "d": {
    "eventId": 23,
    "timestamp": 1662905104172275200,
    "actionType": "MatchedTrade",
    "orderId": 18,
    "mpOrderId": 46,
    "mpId": 2087505339,
    "mpName": "mp1",
    "instrumentId": 963,
    "instrument": "ABC1",
    "side": "Sell",
    "price": 100.48,
    "quantity": 0.3,
    "tradeId": 3,
    "tradingMode": "CT",
    "accountType": "Client",
    "parties": [
      {
        "id": "TST1",
        "source": "D",
        "role": 12
      },
      {
        "id": "1234",
        "source": "D",
        "role": 38
      }
    ],
    "makerTaker": "Maker",
    "tradeDate": "2022-09-11",
    "trackingNumber": 217814816
  }
}
```
{% endtab %}

{% tab title="TradeReport " %}
```javascript
{
  "q": "v1/exchange.market/trades",
  "sid": 16,
  "d": {
    "eventId": 27,
    "timestamp": 1662906852364973300,
    "actionType": "TradeReport",
    "mpId": 2087505339,
    "mpName": "mp1",
    "instrumentId": 963,
    "instrument": "ABC1",
    "side": "Buy",
    "price": 1.1234,
    "quantity": 10,
    "tradeId": 5,
    "tradingMode": "ON",
    "accountType": "Client",
    "parties": [
      {
        "id": "1234",
        "source": "D",
        "role": 38
      }
    ],
    "tradeType": "Block",
    "tradeDate": "2022-09-11",
    "trackingNumber": 217819392
  }
}
```
{% endtab %}

{% tab title="TradeCancel " %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 16,
  "d": {
    "eventId": 26,
    "timestamp": 1662906282293290000,
    "actionType": "TradeCancel",
    "mpId": 2087505339,
    "mpName": "mp1",
    "instrumentId": 963,
    "instrument": "ABC1",
    "side": "Buy",
    "price": 100.48,
    "quantity": 0.3,
    "tradeId": 4,
    "tradingMode": "CT",
    "tradeDate": "2022-09-11",
    "trackingNumber": 217818208
  }
}
```
{% endtab %}
{% endtabs %}

## 2022-09-13✔️

* On FIX API:
  * LOGON message can now be sent with a simple password authentication&#x20;
  * New tags on NewOrderSingle (MsgType = D) and ExecutionReport (MsgType = 8)
    * AccountType (581)&#x20;
    * Parties component
* On [#trades-v2](../reporting-api.md#trades-v2 "mention")new field was added: `tradeDate`
* New API: Get Settlement Prices to retrieve the settlement prices for all instruments or for a specific list of instruments.&#x20;

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v1/exchange.marketdata/getSettlementPrices",
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

{% tab title="Last Message" %}
```json
{
  "sig": 1,
  "q": "v1/exchange.marketdata/getSettlementPrices",
  "sid": 10
}
```
{% endtab %}
{% endtabs %}

## 2022-08-31✔️

* Market Participant Group - The ability to access all private data API for a group of market participants, or all market participants.&#x20;
* [New fields in placeOrder , exeutionReports and massOrderStatus](api-changes.md#place-order-and-execution-reports-and-massorderstatus)
* [Trade Entry](api-changes.md#trade-entry)
* [Trade Cancellation](api-changes.md#trades-cancellation)&#x20;
* [Reporting - New version of Trades API](api-changes.md#reporting-trades-api)&#x20;



**Place Order & Execution Reports & massOrderStatus**&#x20;

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

**Trade Entry**

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
}
</code></pre>
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

**Trades Cancellation**&#x20;

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
}
</code></pre>
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



**Reporting - Trades API**\
New version is going to be release to [#trades](../reporting-api.md#trades "mention") API.\
Previous version is still supported but will not include new developments, we encourage you to migrate to the new version.  &#x20;

{% hint style="info" %}
qualifier: v2/exchange.reporting/mp/trades
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
