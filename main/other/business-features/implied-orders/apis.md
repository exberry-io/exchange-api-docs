# APIs

## Market Data

Refer [here](../../../ws/market-data.md) for more details on Market Data API.

### Order Book Depth

Below exceptions are applicable to `Executed` message of [orderBookDepth](../../../ws/market-data.md#orderbookdepth) API.&#x20;

* In case of implied order trades, when the maker or taker side doesn’t have an order, value of below fields relevant to the side without an order are equal to `-1`. (For example, in case of an implied in order getting executed with a regular order, instruments of base orders don't have contra orders at instrument level)
  * makerMpId
  * makerMpOrderId
  * makerOrderId
  * takerMpId
  * takerMpOrderId
  * takerOrderId



## Private Data

The MP Group All contains the implied order events generated under the ownership of the system(mpId=-1).

Refer [here](../../../ws/private-data-api.md) for mde details on Private Data APIs.

### Trades

Below exceptions are applicable to the `makerTaker` field of [Trades](../../../ws/private-data-api.md#trades) API

#### Maker/Taker logic used for implied orders

* **Implied in:**
  * **Implied order (Parent)** trade, and **Base Orders (legs)** trades:&#x20;
    * **Taker:** if implied order is taker&#x20;
    * **Maker:** if implied order is maker
  * **Regular order (Parent)** trade, and its **legs allocated** trades:&#x20;
    * The opposite of the contra side
* **Implied out:**
  * **Base order (Parent)** trade, and its **legs allocated** trades:&#x20;
    * Always **Taker**
  * **(Parent)** trade **without an order**, and **Base Order (leg)** trade, and **Regular order (leg)** traded with implied order:
    * Always **Maker**

## Reporting

The MP Group All contains the implied order events generated under the ownership of the system(mpId=-1).

Refer [here](../../../ws/reporting-api.md) for mde details on Reporting APIs.

## Example

Base orders: ordid1, ordid2, Implied out order: ordid3

**InstA(Leg1)**

<table><thead><tr><th width="139">mpId/orderId</th><th>Bid Quantity</th><th>Bid Price</th><th>mpId/orderId</th><th>Ask Quantity</th><th>Price</th></tr></thead><tbody><tr><td></td><td></td><td></td><td>mpid1/ordid1</td><td>5</td><td><a data-footnote-ref href="#user-content-fn-1">10.05</a></td></tr></tbody></table>



**InstB(Leg2)**

| mpId/orderId | Bid Quantity | Bid Price | mpId/orderId | Ask Quantity | Price |
| ------------ | ------------ | --------- | ------------ | ------------ | ----- |
|              |              |           | SYS/ordid3   | 4            | 10.03 |

\
**InstAB(Buy 1 Leg1, Sell 1 Leg2)**

<table><thead><tr><th width="111">mpId/orderId</th><th width="124">Bid Quantity</th><th width="138">Bid Price</th><th width="124">mpId/orderId</th><th width="128">Ask Quantity</th><th>Price</th></tr></thead><tbody><tr><td>mpid2/order2</td><td>4</td><td>0.02</td><td></td><td></td><td></td></tr></tbody></table>

When: InstB gets a new Buy order (quantity=4, price=10.03, ordid5, mpid5). System generates 6 `TradeReports` (2 per instrument). System generates a trade for the side of the strategy instrument without an order(sample below).



### Samples: Order Book Depth

{% tabs %}
{% tab title="InstAB" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 14,
  "d": {
    "messageType": "Executed",
    "eventTimestamp": 1713780680893168400,
    "eventId": 91,
    "trackingNumber": 1331411104,
    "instrument": "AB",
    "makerOrderId": -1,
    "takerOrderId": 55,
    "takerMpId": 14,
    "takerMpOrderId": 1713780484,
    "matchId": 21,
    "executedQuantity": 4,
    "executedPrice": 0.02
  }
}
```
{% endtab %}

{% tab title="InstA" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 14,
  "d": {
    "messageType": "Executed",
    "eventTimestamp": 1713780680893168400,
    "eventId": 75,
    "trackingNumber": 1331412160,
    "instrument": "A",
    "makerOrderId": 34,
    "makerMpId": 14,
    "makerMpOrderId": 1713780543,
    "takerOrderId": -1,
    "matchId": 30,
    "executedQuantity": 4,
    "executedPrice": 10.05
  }
}
```
{% endtab %}

{% tab title="InstB" %}
```json
{
  "q": "v2/exchange.market/orderBookDepth",
  "sid": 14,
  "d": {
    "messageType": "Executed",
    "eventTimestamp": 1713780680893168400,
    "eventId": 58,
    "trackingNumber": 1331413568,
    "instrument": "B",
    "makerOrderId": 25,
    "makerMpId": 14,
    "makerMpOrderId": 1713780680,
    "takerOrderId": 24,
    "matchId": 25,
    "executedQuantity": 4,
    "executedPrice": 10.03
  }
}
```
{% endtab %}
{% endtabs %}



### Samples: Trades

{% tabs %}
{% tab title="InstAB" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 3,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1713780680893168400,
    "trackingNumber": 1331411104,
    "eventId": 91,
    "mpId": -1,
    "mpName": "System",
    "instrumentId": 22730,
    "instrument": "InstAB",
    "side": "Sell",
    "price": 0.02,
    "quantity": 4,
    "tradeId": 21,
    "tradingMode": "CT",
    "makerTaker": "Maker",
    "tradeDate": "2024-04-22",
    "multiLegReportingType": "MultiLegSecurity"
  }
}
```
{% endtab %}

{% tab title="InstAB" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 3,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1713780680893168400,
    "trackingNumber": 1331411104,
    "eventId": 91,
    "orderId": 2,
    "mpOrderId": 1713780484,
    "mpId": 14,
    "mpName": "JPM-1",
    "instrumentId": 22730,
    "instrument": "InstAB",
    "side": "Buy",
    "price": 0.02,
    "quantity": 4,
    "tradeId": 21,
    "tradingMode": "CT",
    "parties": [
      {
        "id": "39",
        "source": "D",
        "role": 1001
      },
      {
        "id": "Acc1",
        "source": "D",
        "role": 1003
      },
      {
        "id": "ccc",
        "source": "D",
        "role": 1004
      },
      {
        "id": "28",
        "source": "D",
        "role": 1002
      }
    ],
    "makerTaker": "Taker",
    "tradeDate": "2024-04-22",
    "multiLegReportingType": "MultiLegSecurity"
  }
}
```
{% endtab %}

{% tab title="InstA" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 3,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1713780680893168400,
    "trackingNumber": 1331412160,
    "eventId": 75,
    "orderId": 1,
    "mpOrderId": 1713780543,
    "mpId": 14,
    "mpName": "JPM-1",
    "instrumentId": 22726,
    "instrument": "InstA",
    "side": "Sell",
    "price": 10.05,
    "quantity": 4,
    "tradeId": 30,
    "tradingMode": "CT",
    "parties": [
      {
        "id": "39",
        "source": "D",
        "role": 1001
      },
      {
        "id": "Acc1",
        "source": "D",
        "role": 1003
      },
      {
        "id": "ccc",
        "source": "D",
        "role": 1004
      },
      {
        "id": "28",
        "source": "D",
        "role": 1002
      }
    ],
    "makerTaker": "Maker",
    "tradeDate": "2024-04-22",
    "multiLegReportingType": "SingleSecurity"
  }
}
```
{% endtab %}

{% tab title="InstA" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 3,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1713780680893168400,
    "trackingNumber": 1331413088,
    "mpId": 14,
    "mpName": "JPM-1",
    "instrumentId": 22726,
    "instrument": "InstA",
    "side": "Buy",
    "price": 10.05,
    "quantity": 4,
    "tradeId": 30,
    "tradingMode": "CT",
    "parties": [
      {
        "id": "39",
        "source": "D",
        "role": 1001
      },
      {
        "id": "Acc1",
        "source": "D",
        "role": 1003
      },
      {
        "id": "ccc",
        "source": "D",
        "role": 1004
      },
      {
        "id": "28",
        "source": "D",
        "role": 1002
      }
    ],
    "makerTaker": "Taker",
    "tradeDate": "2024-04-22",
    "multiLegReportingType": "IndividualLeg",
    "tradeLegRefId": 1,
    "multiLegDifferentialPrice": 0.02,
    "multiLegStrategyInstrumentId": 22730,
    "multiLegStrategyInstrument": "1SP1",
    "multiLegStrategyTradeId": 21,
    "multiLegStrategyOrderId": 55,
    "multiLegStrategyMpOrderId": 1713780484
  }
}
```
{% endtab %}

{% tab title="InstB" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 3,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1713780680893168400,
    "trackingNumber": 1331413568,
    "eventId": 58,
    "orderId": 5,
    "mpOrderId": 1713780680,
    "mpId": 14,
    "mpName": "JPM-1",
    "instrumentId": 22727,
    "instrument": "InstB",
    "side": "Buy",
    "price": 10.03,
    "quantity": 4,
    "tradeId": 25,
    "tradingMode": "CT",
    "parties": [
      {
        "id": "39",
        "source": "D",
        "role": 1001
      },
      {
        "id": "Acc1",
        "source": "D",
        "role": 1003
      },
      {
        "id": "ccc",
        "source": "D",
        "role": 1004
      },
      {
        "id": "28",
        "source": "D",
        "role": 1002
      }
    ],
    "makerTaker": "Maker",
    "tradeDate": "2024-04-22",
    "multiLegReportingType": "SingleSecurity"
  }
}
```
{% endtab %}

{% tab title="InstB" %}
```json
{
  "q": "v1/exchange.market/trades",
  "sid": 3,
  "d": {
    "actionType": "MatchedTrade",
    "timestamp": 1713780680893168400,
    "trackingNumber": 1331414496,
    "mpId": 14,
    "mpName": "JPM-1",
    "instrumentId": 22727,
    "instrument": "InstB",
    "side": "Sell",
    "price": 10.03,
    "quantity": 4,
    "tradeId": 25,
    "tradingMode": "CT",
    "parties": [
      {
        "id": "39",
        "source": "D",
        "role": 1001
      },
      {
        "id": "Acc1",
        "source": "D",
        "role": 1003
      },
      {
        "id": "ccc",
        "source": "D",
        "role": 1004
      },
      {
        "id": "28",
        "source": "D",
        "role": 1002
      }
    ],
    "makerTaker": "Taker",
    "tradeDate": "2024-04-22",
    "multiLegReportingType": "IndividualLeg",
    "tradeLegRefId": 2,
    "multiLegDifferentialPrice": 0.02,
    "multiLegStrategyInstrumentId": 22730,
    "multiLegStrategyInstrument": "1SP1",
    "multiLegStrategyTradeId": 21,
    "multiLegStrategyOrderId": 55,
    "multiLegStrategyMpOrderId": 1713780484
  }
}
```
{% endtab %}
{% endtabs %}





[^1]: 
