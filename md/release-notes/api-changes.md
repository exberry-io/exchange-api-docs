# API Changes

## v1.57.0 (2026-06-04)

* Allow Negative Decimals grouping in [market-data.md](../market-data-apis/market-data/market-data.md "mention")

## v1.55.0 (2026-04-08)

* Added the behavior of `lightTickers/priceLimits`  in case applicable price bands defined at instrument level

## v1.54.0 (2026-03-17)

* Changed the formula of `lightTickers/quoteVolume`  to support price multiplier&#x20;

## v1.52.0 (2026-02-03)

* Generic error message was added for all long living APIs `Stream disconnected`

## v1.50.0 (2025-11-20)

* Added `lastPriceTimestamp` field to the `lightTickers`

## v1.47.0 (2025-09-25)

* Consider leg trades on candles generation
* Added new values to `volumeTypes` values of `lightTickers` - Reversal

## v1.45.0 (2025-06-26)

* Allow to configure all APIs to require authentication
* Improve [master-1.md](../master-1.md "mention") section to support 2 access modes  &#x20;

## v1.43.0 (2025-05-14)️

* Removed  `groupIds` from `instrumentList` and `instrumentListWithPagination` APIs
* Changed the stringified number fields of below APIs to non-stringified numbers
  * `/exchange.marketdata/instrumentList`
  * `/exchange.marketdata/instrumentListWithPagination`
  * `/exchange.marketdata/calendarList`
  * `/exchange.marketdata/tickSizeList`

## v1.41.0 (2025-03-11)️

* Added new values and changes in some existing `volumeTypes` values of `lightTickers`&#x20;

## v1.37.0 (2024-12-03)️

* Adding an empty message to calendarList , tickSizeList , instrumentList  to handle cases where there are no entities
* Documenting that numbers are stringified in: instrument list, calendar list, tick size list APIs

## v1.36.0 (2024-11-12)️

* Adding new API to publish auction indicative price&#x20;

## v1.34.0 (2024-09-25)️

* The sorting in the instrument list is changing to be case-insensitive.
* Added supporing Candles data adjusted for Corporate Actions

## v1.33.0 (2024-08-21)️

* Fields in `lightTickers` get adjusted when corporate actions are applied to the active orders.
* `lightTicker` will not include data of market order during auction.
* Adding to the [partialOrderBook](../market-data-apis/market-data/market-data.md) new objects for market orders during auction
* Adding to the[ instrument list](../market-data-apis/market-data/instrument-list.md) filters and new API with pagination.

## v1.31.0 (2024-07-16)️

* Adding new fields to the light ticker - `tickReferencePrice` and its `timestamp`

## v1.30.0 (2024-06-19)️

* Adding new fields to the light ticker to publish price limits of the instrument
* Change empty message of the light ticker to not include the volume fields.

## v1.28.0 (2024-04-16)️

Candles API:

* Change `v1/exchange.marketdata/aggregates` qualifier:&#x20;
  * Real time updates qualifier: v1/exchange.marketdata/candles
  * Historical data qualifier: v1/exchange.marketdata.history/candles
* Add  `timespan` value: HOUR
* Add `multiplier` parameter to support  below intervals:
  * 5 MINUTE
  * 15 MINUTE
  * 4 HOUR

## v1.27.0 (2024-04-01)️

* New API for OHLCV data `v1/exchange.marketdata/aggregates`
* New API Heartbeats (Pings) `v1/heartbeat/ping` for browser API clients
* `tradingModels` field will always be returned by `exchange.marketdata/instrumentList`

## v1.26.0 (2024-02-28)️

* Bugfix related to the Market Data Service. `referencePrice` was not returned after restarting the service before the fix.

## v1.25.0 (2024-02-07)️

* Bugfix related to the end of the snapshot message of `v1/exchange.marketdata/liveTrades`. `multiLegReportingType` field was not returned prior to the fix.
* Bugfix related to the required fields of `v1/exchange.marketdata/lightTickers`. The error message returned was incorrect before the fix.

## v1.24.0 (2024-01-17)

* Changed `quoteVolume` formula of `LightTicker` to `Total(trade amount * absolute value of trade price * contract size)`. The previous formula didn’t have the `contract size`.
* Added `deliveryStartDate` and `deliveryEndDate` to the `v1/exchange.marketdata/instrumentList`
* Added `makerSide` to the RFQ Trades of `v1/exchange.marketdata/liveTrades`
* Bugfix related to `quantity` of `liveTrades` messages of legs instruments of strategies. `quantity` of trades has incorrectly had the `quantity` of current trade + `quantity` of previous trade when trade prices of current and previous trades were the same.

## v1.23.0 (2023-12-26)️

* Added `scope` to the `lightTickers` API.
* Added `multiLegReportingType` to the `liveTrades` API.

## v1.22.0 (2023-12-05)️

* Added RFQ trades to the liveTrades and lightTickers APIs
* Bugfix related to ​​`openingPrice` calculation of `lightTickers`. The system has incorrectly taken trade entries for the calculation before the fix.
* Bugfix related to the `volumeTypes` object of `lightTickers` API. `volume` field was incorrectly returned as `value` before the fix.

## v1.20.0 (2023-10-25)

* New API to disseminate Instrument details `v1/exchange.marketdata/instrumentList`
* New API to disseminate Calendar details `v1/exchange.marketdata/calendarList`
* New API to disseminate tickSize details `v1/exchange.marketdata/tickSizeList`
* Changed `token` to be a required field in all Market Data APIs
* Deprecated `environments.exchangeNum` field in all Market Data APIs
* Bug fix on  [#livetrades](../market-data-apis/market-data/market-data-2.md#livetrades "mention") API on strategy allocated leg trades to properly display `makerSide`

## 2023-07-12

* Fixed the bug related to timeStamp format in `v1/exchange.marketdata/liveTrades`

## 2023-06-21

* volumeTypes object in the light ticker will be sent only if there is data that is not 0&#x20;

## 2023-06-06

* Adding new fields to the light ticker
* Taking the trade entry data considered in some fields in the light ticker (volume, quoteVolume, high, low, lastPrice, lastQuantity)
* Adding the trade entry and trade cancellation to the live trades.
  * new value in quantity in case of cancellation
  * makerSide will be in this case: -1

## 2023-05-02

* Adding the reference price and timestamp of the instrument to the light ticker response

## 2023-03-22

* Adding the trading status of the instrument to the light ticker response

## 2023-02-08

There are new fields in the light ticker API:

* settlementPrice: Settlement price
* settlementPriceTimestamp: Settlement price last update timestamp
* lastQuantity: Last executed trade quantity



Until now in case one of the symbols is wrong, the entire stream was disconnected.

From now on, in case of wrong symbol stream will continue working for the valid symbols and for the invalid symbols the message returned will be:

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

This error might occur in case that stream is already working (in case symbol is no longer active instrument).

## 2023-01-18

[market-data.md](../market-data-apis/market-data/market-data.md "mention") will publish events only in case there is change in data. Once there is change- it will be sent only when interval time elapsed.

## 2023-01-03&#x20;

All API has a new optional header parameter - we strongly recommend to use it instead of the previous `environments.exchangeNum`  (which is still supported), the new parameter provides more accurate and efficient instrument discovery.&#x20;

The parameter is called `token` and can be retrieved from Admin application under home page:

<img src="../.gitbook/assets/image (5).png" alt="" data-size="original">  &#x20;



Sample API using new parameter:

```json
{
  "q": "v1/exchange.marketdata/lightTickers",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 10,
  "d": {
    "symbols": [
      "ABC",
      "XYZ",
      "INS1"
    ],
    "interval": 1000
  }
}
```

## 2022-**11**-21

`getSettlementPrices` API  will response with the latest results updated in system. &#x20;

## 2022-09-13

`lightTickers` API will have new properties, that are referring to the current trading day :

* lastClosingPrice - last execution prive before market close&#x20;
* closingPriceTimestamp - closing price timestamp
* openingPrice - first execution price after market open&#x20;
* low - lowest execution price since market open&#x20;
* high -  highest execution price since market open
* volume - total volume since market open (total orders amount)&#x20;
* quoteVolume -  total volume since market open in quote currency terms (amount \* price)&#x20;



## 2022-09-13

New API added [market-data-3.md](../market-data-apis/market-data/market-data-3.md "mention")
