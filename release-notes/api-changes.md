# API Changes

## Coming Soon... :hammer\_pick:

* 2022-05-16✔️

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
