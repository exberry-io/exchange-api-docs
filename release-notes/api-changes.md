# API Changes

## v1.46.0 (2025-08-26)

**General-**

* Multiple smantic changes and cleanups&#x20;
* Changed the account id party definition to `party.role=24` and `party.source=P`

## v1.43.0 (2025-05-14)

**WS API-**

* Added `makerTaker` field to WebSocket `executionReports`

## v1.41.0 (2025-03-11)

**General-**

* Added new [`timeInForce` ](../other/business-features/time-in-force.md)values&#x20;
  * `OPG`(At the Opening)
  * `ATC`(At The Close)
* Documentation change only to elaborate on tag 442 (MultiLegReportingType) on FIX API

**WS API-**

* Changed the mandatoryParties validation of MP trade reporting flow=AllegedSystemMatch.
* Added `filledPrice` to `Add`, `Executed`, `Modified`, `Cancelled` and `Suspended` `executionReport`  messages
* Added new `tradeType` values to `TradeReport` messages of `orderBookDepth` and `exchange.reporting/mp/trades`
* Changed some modifyOrder error messages

**FIX API-**

* Added order created timestamp to the `ExecutionReport(8)` messages.
* Added `AggressorIndicator(1057)` to `TradeCaptureReport(AE)`
* Added `AvgPx(6)` to the `ExecutionReport(8)` messages
* <mark style="color:red;">Deprecated</mark> `MDEntryType(269)` values: `v`, `o`, `f`, `g`, and `q` from `MarketDataRequest(V)` and `MarketDataSnapshot(W)`
* Added new `TrdType(828)` values to `TradeCaptureReport(AE)` messages
* Added new values for tags `20035` and `20039` of `SecurityList(y)` messages (with new trade type values)&#x20;

## v1.40.0 (2025-02-19)

**General-**

* Enhanced STP to support different scopes within MPs.

**FIX  API-**

* Tagged some fields in `MarketDataRequest(V)` and `MarketDataSnapshot(W)` as to be deprecated.
* TransactTime (60) was missing in the documentation of ExecutionReports and was added.

## v1.39.0 (2025-01-09)

**General-**&#x20;

* Added supporting Modify/Replace reserve orders
* Enhance the logic used to populate `cancelReason` in case of Self-Trade Prevention

## v1.38.0 (2024-12-24)

**General-**&#x20;

* Added Market to Limit Orders
* Adding another sorting to the [order book state](../ws/market-data.md#orderbookstate)
* Apply new sorting in the [Mass Order Status](../ws/private-data-api.md#massorderstatus)
* Added supporting ASCII (32 -126) characters for party.id and targetParty.id fields.

## v1.37.0 (2024-12-03)

**General-**&#x20;

* Support new types of [CBRs ](https://documenter.getpostman.com/view/6229811/TzCV3jcq#46843413-c6ad-44c0-8d67-e18ef719b365)with implied orders
* Added `legPriceBands` validation to Strip(Average Price) instruments
* Added implied order generation for Strip(Average Price) instruments

**WS API-**&#x20;

* Adding a new field to the [Instrument Status Message](../ws/market-data.md#instrument-status-message) in [#orderbookdepth](api-changes.md#orderbookdepth "mention")- `previousSymbol`

**FIX API-**

* `ExecutionReport(8)` messages related to order adjustments due to corporate actions are generated under the same `ClOrdID(11)`.

## v1.36.0 (2024-11-12)

**General-**&#x20;

* Added supporting reserve orders during auctions.
* Auction Indicative Price Events are now published on real time when change occur on order book

## v1.35.0 (2024-10-21)

**General-**&#x20;

* Added new cancel reason - [CBRBreach](../ws/private-data-api.md#cancel-reason)
* Added `displayMethod=Random` to the reserve orders.
* Added implied order generation using reserve orders&#x20;

**WS API-**&#x20;

* Added new filters to the Trades report - mpOrderId & orderId

**FIX API-**

* Added a new field to the ticker in the Market Data Snapshot Full Refresh (W) and Market Data Request (V) - `p = Tick Reference price`

## v1.34.0 (2024-09-25)

**General-**&#x20;

* Added Reserve orders (iceberg and hidden)
* Added `tradingCutoffs` for trade reporting
* `indicativePriceEvents`: the message was sent per interval even if nothing was changed. Starting this version it will be published only in case of a change in one of the parameters.&#x20;

**FIX API-**

* Documentation only:
  * Added Order replaced with an immediate partial/full execution to FIX [Order Lifecycle Scenarios](../fix-trading/trading-messages.md#order-lifecycle-scenarios)
  * Added Trade Entry Minimum Quantity array in the security list, which was previously undocumented and introduced in version 1.18.0.

**WS API-**&#x20;

* Data eligibility for Trading and Private Data APIs to support account specific connection&#x20;

## v1.33.0 (2024-08-21)

**Market order during auction is now supported by system-**

* Trading APIs
  * Adding new validations and changing existing in [place order](../ws/trading-api.md#error-codes)&#x20;
  * Adding new validation to the [modify order](../ws/trading-api.md#error-codes-3)
  * Adding new validations and changing existing in [replace order](../ws/trading-api.md#error-codes-4)
* Market Data APIs
  * price will be empty in **`Add`** message for market orders during auction in [orderBookDepth](../ws/market-data.md#add-order-message)
  * New definition of  **`AuctionIndicativeEP`** message with market orders during auction in [orderBookDepth](../ws/market-data.md#auction-indicative-equilibrium-price-message)
  * price will be empty for market orders during auction in [orderBookState](../ws/market-data.md#order-message)
  * FIX API changes
    * Adding 2 new values in MDEntryType(269) to Market Data Incremental Refresh _(MsgType = X) and_ Market Data Snapshot Full Refresh _(MsgType = W)_

**WS API-**

* Apply new sorting in the [OrderBookState](../ws/market-data.md#orderbookstate)
* Apply new filter mpOrderId in [Orders Report](../ws/reporting-api.md#orders)
* Order ID does not depend on a specific instrument in [Orders Report](../ws/reporting-api.md#orders) filters&#x20;
* Change in the instrument status message in [OrderBook](../ws/market-data.md#orderbookstate)[Depth](../ws/market-data.md#orderbookdepth) - in `tradingStatus` and `marketStatus` for instrument becomes active.&#x20;

**FIX API-**

* Sorting snapshot of MBOFD
* Data eligibility for trading and drop copy API to support account specific connection&#x20;

## v1.32.0 (2024-08-07)

**General -**

* New [cancel reasons](../ws/private-data-api.md#cancel-reason) added&#x20;

**WS -**

* Apply new sorting in the[ trades report](../ws/reporting-api.md#trades-v2)&#x20;

## v1.31.0 (2024-07-16)

**General -**

* Added the ability for MPs to report trades. (Currently available only via WS API)
* Allow mass cancellation across all instruments

**WS -**

* Added MP Trade Reporting APIs
  * `exchange.market/createTradeReport`
  * `exchange.market/cancelAllegedTradeReport`
  * `exchange.reporting/mp/allegedTrades`
* Added `targetScope` to the `massCancel` to define the cancel scope
* Added `AllegedTrade` to the `targetEntities` of the `massCancel`

**FIX GW -**&#x20;

* Added `MassCancelRequestType(530)=7(Cancel all orders)` to the `OrderMassCancelRequest(q)`

## v1.30.0 (2024-06-19)

**General -**

* Added `Orders` and `Quotes` with negative prices to the `maxOrderValue` validation.

**WS -**

* Added `targetEntities` to the `massCancel`

## v1.29.0 (2024-04-23)&#x20;

**General -**

* Added `contractSize` of Option, Strategy and Futures instruments to the maxOrderValue validation

## v1.28.0 (2024-04-16)&#x20;

**General -**

* Implied Order generation for `Spread` and `Strip` instruments.&#x20;
* Added a configurable Price Band Validation for `Spread` and `Strip` instruments and a Leg Trade Price Adjustment for `Spread` Instruments.
* Adding new value to cancel reason - `ReplaceOrderOnBehalf` to be used when Admin user replace orders.

## v1.27.0 (2024-04-01)&#x20;

**General -**

* Changed the error message returned when configured mandatory parties are missing (to specify the missing party) in WS `placeOrder`, `submitRFQ`, `submitQuote`, and FIX `NewOrderSingle(D)`.
* Changed the logic used to populate `makerTaker` values of implied order trades.
* Adding new value to cancel reason - `MassCancelOnBehalf` to be used when Admin user mass cancel orders.

**WS -**

* Bugfix related to `InstrumentStatus` messages of `exchange.market/orderBookDepth`. Before the fix, `instrument` field was not sent in the message disseminated when the instrument was created.

## v1.26.0 (2024-02-28)

**General -**

* Adding new value for the cancel reason in the execution report - `CancelOrderOnBehalf`

**FIX GW -**&#x20;

* Added `Order is not allowed` validation for `NewOrderSingle(D)`for RFQ only instrument.&#x20;
* Bugfix, an incorrect `SequenceReset(4)` message was sent by the system when a `ResendRequest(2)` is received with `BeginSeqNo(7)` belonging to an `ExecutionReport(8)`

**WS -**

* Added `Order is not allowed` validation for `placeOrder` for RFQ only instrument

## v1.25.0 (2024-02-07)

**General -**

* Added support for Configurable Mandatory Fields (Account Type and Specific Parties)
* Bugfix related to placing GTD orders. GTD orders placed during auctions with `expiryDay` field were immediately expired by the system before the fix.

**FIX GW -**&#x20;

* Bug fix, MPs were able to temporarily connect when `acceptorCompId` was not set up before the fix.
* Bugfix related to `MarketDataRequest(V)`. The system returned an incorrect error message for MarketDataRequests with duplicate `MDReqID(262)`.

**WS -**

* Bugfix related to validation of TIF values of new orders. This validation returned an incorrect error message before the fix.
* Bugfix related to the `v2/exchange.reporting/mp/trades` of implied order trades. mpName field was not included before the fix.

## v1.24.0 (2024-01-17)

**FIX GW -**&#x20;

* Added support for multiple FIX Sessions for a single MP
* Added `First delivery date` and `Last delivery date` to the `SecurityList(y)`
* Added `AggressorSide(2446)` to the `MarketDataIncrementalRefresh` messages of RFQ Trades
* Bugfix related to Time and Sales updates. `MarketDataIncrementalRefresh(X)` updates were not disseminated when `MarketDepth(264)=0` was on the subscription request.

**WS -**

* Added `orderId`, `mpOrderId`, and `makerTaker` fields to the `RFQTrade` events of `v1/exchange.market/trades` and `v2/exchange.reporting/mp/trades`&#x20;

## v1.23.0 (2023-12-26)

**FIX GW -**&#x20;

* Removed the `ExecType(150)=0(New)`, `ExecutionReport(8)` sent when an accepted or triggered(stop/stop limit) order partially executed at entry to the book and the `LeavesQty(151)` rests on the book.
* Added `Scope(546)` to the `MarketDataRequest(V)` of Ticker data.
* Added `multiLegReportingType(442)` to the `MarketDataIncrementalRefresh(X)` of Time & Sales data.

**WS -**

* Bugfix related to the `quantity` of `executionReports` when the `Quantity Precision` of the instrument was changed. The `quantity` had a value of Scientific Notation, before the fix.

**RFQ API** -&#x20;

* Added parties, accountType to QuoteExecuted events of the RFQ initiator. Values are taken from the RFQ.&#x20;
* Added colon ( : ) and Underscore ( \_ ) to the allowed characters of the party.id field of `submitRFQ`, and `submitQuote`.

**Self-Trade Prevention**

&#x20;Self-Trade Prevention is now available as optional functionality. \
It is available via FIX and WS APIs:

* In WS `stpAction` was added to the relevant message
* In FIX tag 20033 was added to the relevant messages



## v1.22.0 (2023-12-05)

**FIX GW -**&#x20;

* Proper sequencing of replacements and modifications: Changed the behavior of `OrderCancelReplaceRequest(G)`. The system replaces the `ClOrdID(11)` of the order with the value received in `OrderCancelReplaceRequest(G)`
* Changed the `ExecType(150)` of the `ExecutionReport(8)` sent when an order is replaced as `ExecType(150)=5(Replaced)`. It was `0(New)` before the change.
* Added RFQ trades to `Time & Sales` Market Data updates.
* Added MDEntryType(269)=q(RFQ volume) to `Ticker` Market Data updates
* Bugfix to add `ExpireDate(432)` to FIX `ExecType(150)=I(OrderStatus)` `ExecutionReport(8)`.
* Added `TradeCaptureReport(AE)` messages for RFQ trades
* Bugfix to remove the `WorkingIndicator(636)` from `I(OrderStatus)` `ExecutionReport(8)` messages of partially filled Stop/Stop Limit orders. `WorkingIndicator(636) = Y` was populated before the fix.

**WS -**

* Added Underscore`_`  and colon `:` to the allowed characters of the `party.id` field of `placeOrder` and `massCancel`.

## v1.21.0 (2023-11-20)&#x20;

**FIX GW -**&#x20;

* Added `WorkingIndicator(636)`, Changed `ExecType(150)` and `OrdStatus(39)` of `ExecutionReport(8)` messages of suspended Stop/StopLimit orders.   &#x20;
* Added `ExpireDate(432)` to `NewOrderSingle(D)`, `OrderCancelReplaceRequest(G)` and `ExecutionReport(8)`
* Added Leg trades for `ExecutionReport(8)`. The new messages are optional and should be subscribed to via `Logon(A)` Message.
* Increased the total number of tags supported to 256.
* Bugfix related to `OrdStatus (39)` of `ExecutionReport(8)` of partially filled orders. `OrdStatus (39)` was `New(0)` before the fix, it was corrected as `Partially filled(1)`.
* Bugfix related to `OrderQty(38)` of `ExecType(150)=4(Canceled)` `ExecutionReport(8)` , `CumQty(14)` value has been incorrectly reported as `OrderQty(38)` before the fix.
* Changed the error message of minimum and maximum quantity validation messages of Orders



**WS -**

* Added expiryDay to `placeOrder`, `replaceOrder`,   `v1/exchange.market/executionReports` and `v1/exchange.reporting/mp/orders` &#x20;
* Adding validation for party.source field of `placeOrder`, `massCancel`. Allowed values: alphanumeric \[0-9,a-z,A-Z].
* Adding validation for party.id field of `placeOrder`, `massCancel`. Allowed values: alphanumeric \[a-z,A-Z,0-9,”.”,”-”,”^”,”/””=”, “ ”].
* Bugfix related to creating WS sessions.
* Bugfix related to validating minimum order quantity in `modifyOrder`
* Changed the error message of minimum and maximum quantity validation messages of Orders
* Bugfix related to `lastMessage: "Y"` indicator of the instrumentList API

## v1.20.0 (2023-10-25)&#x20;

**FIX GW -**

* Adding to the Execution Reports(8) cancel reason (in tag 58)
* Changed the order expiration `ExecutionReport(8)`, `ExecType(150)` value from `Canceled(4)` to `Expired(C)`
* Bug fix on market data “Time & Sale” API on strategy allocated leg trades to properly display AggressorSide(2446)

**WS -**

* Adding to the Execution Reports cancel reason
* Deprecate below APIs
  * `v1/exchange.reporting/mp/trades`
  * `v1/exchange.market/orderBookDepth`
  * `v1/exchange.market/orderBookState`
* Deprecate `userId` and `accountId` fields from below APIs
  * `v1/exchange.market/placeOrder`
  * `v1/exchange.reporting/mp/orders`
  * `v2/exchange.reporting/mp/trades`
  * `v1/exchange.market/massOrderStatus`
  * `v1/exchange.market/executionReports`

## v1.19.0 (2023-10-04) &#x20;

**FIX GW -**

* Added supporting the replaceOrder behavior of the WebSocket to OrderCancelReplaceRequest _(MsgType = G)_
* _Adding_ tag 110 (MinQty) to -
  * New Order Single (D)
  * Execution Reports (8)
* Added a session level validation for number of repeating groups in FIX messages
  * In case Market Data Request (V) sent with NoRelatedSym(146) is not equal to actual repeating group instances Till now -Market Data Request Reject (Y) was sent,  From now Reject (3) will be sent

**WS -**

* Added supporting the modifyOrder behavior of the WebSocket to replaceOrder. In case replaceOrder is being used only for reducing order remaining open quantity, from now on it will not trigger cancellation and placing new order (with priority lose) but it will result same order with same priority with reduced quantity&#x20;
* Adding the minQuantity field  and new validations to-
  * Place Order
  * Replace Order
  * Execution Reports

## v1.18.0 (2023-09-19) &#x20;

<mark style="color:red;">Deprecation -</mark>

* realBookState field in the orderBookDepth
* Tag 20028 in FIX GW that show the realBookState&#x20;

**FIX GW -**

* Changed the response of Market Data Request _(MsgType = V):_ MDEntryType=A(Imbalance) request.
* Changed the order of the fields in the security list (y)
* Bugfix related to MDEntryTime(273) value in MarketDataIncrementalRefresh(35=X)

**WS -**

* Bugfix related to removedQty of messageType=Add executionReports
* Bugfix related to userId of messageType=Add executionReport of replaceOrder

## v1.17.0 (2023-08-08)

* **FIX GW-**
  * Adding 20029(Custom tag) to Market Data Snapshot Full Refresh _(MsgType = W) Ticker Data_ - identify last message of the snapshot
  * Deprecate 20001 and 20002
  * Removed MDEntrySize(271) from MarketDataIncrementalRefres&#x68;_(MsgType = X) when_ MDUpdateAction(279) = 2 (Delete)
  * Adding TargetParties to OrderMassCancelRequest _(MsgType = q)_
  * Adding TargetParties to OrderMassCancelReport _(MsgType = r)_
  * Adding to the TradeCaptureRepoet(AE) new fields to individual legs reports to allow association on the individual leg trade report to the multi leg strategy execution report.
* **WS -**
  * Adding targetParties to [massCancel](../ws/trading-api.md#masscancel)
  * Adding to [trades ](../ws/private-data-api.md#trades)and [trades history API](../ws/reporting-api.md#trades-v2) new fields to the trade report to allow association on the individual leg trade report to the multi leg strategy execution report.
  * Adding to the [massOrderStatus ](../ws/private-data-api.md#massorderstatus)- status field
* #### Stop & Stop Limit - Auction & CBR-&#x20;
  * Injections rules: till this version, auctions execution didn't trigger Stop & Stop Limit. From now on, auctions execution will trigger Stop & Stop Limit as follow:
    * For E**quilibrium Price** auctions: After the auction, system will trigger the orders according to the auction price
    * For **Price & Time** auctions: System will check the highest and lowest price of executed prices and will trigger accordingly.&#x20;
  * Trigger the Stop & Stop Limit orders even in case the order is about to trigger a CBR that will halt the trading or trigger an auction.&#x20;

## 2023-07-12

* **Stop & Stop Limit**
  * Adding a new field stopPrice to [massOrderStatus ](../ws/private-data-api.md#massorderstatus)message.
  * Allow to replace and modify Stop & Stop Limit orders
    * adding to the replace new field (stopPrice)
    * delete this validation `Not allowed on stop or stop limit orders` from the modify and replace
  * New validation added: stopPrice can sent only for Stop/ StopLimit orders
    * place order
    * New order single(D)
    * replace order
* **FIX Market Data**&#x20;
  * Adding MDEntryType(269)=J(Empty book) to indicate empty order books (for both Ticker and MBOFD)
  * In  Market Data Request (V), Both bid(MDEntryType(269)=0) and ask(MDEntryType(269)=1) must be subscribed together.
* Fix the TrdType(828) from 99 to 0 for individual leg trade on [#tradecapturereport-ae](../fix-trading/trading-messages-1.md#tradecapturereport-ae "mention")
* Changing the MarketModal value- from RA(Resume Auction) to UA (Unscheduled Auction)

## 2023-06-21

FIX API

* Additions to the security list (Y)-
  * New field - LegSymbol (600)  in addition to the existing LegSecurityID(602) that identify the legs of a strategy instrument.&#x20;
  * Tick size data is now part of the message: fields (20031, 20032, 20033)
* Changing the formula of tag OrderQty(38) in executionReports(8) to be - Original Order Qty- Removed Qty, till now it was always original quantity so in case of order modification system was not comply with `LeavesQty(151) = OrderQty(38) - CumQty(14)`
* &#x20;Market Data Snapshot Full Refresh _(MsgType = W)_ and Market Data Incremental Refresh _(MsgType =_ &#x58;_)_
  * <mark style="color:red;">Deprecation</mark> of OrderId(37)&#x20;
  * Addition of MDEntryID(278)  to replace OrderId(37)
* Removed tag 20028(Custom tag) that identifies if this tag represents real book state or a temporary non-real book state from Market Data Snapshot Full Refresh (MsgType = W) as all results will always be real book state.&#x20;
* Fixing the ExecType(150) in reject in ER(8) of response to OrderMassStatusRequest(AF) - from "l" (lowercase “L”) to “I” (uppercase "I")

WS API

* Adding to the [Reporting API -Trades ](../ws/reporting-api.md#trades-v2)
  * New filter - Trade date
* _Adding a new field to the orderBookDepth -_ realBookState this field identify if this event represents real book state or a temporary non-real book state.&#x20;

_Stop & StopLimit_

From this version new order types were added: Stop & Stop Limit, you can now place and cancel stop and stop limit orders. \
Those order types are available on both WS and FIX APIs.

* _Adding those New types to the:_
  * _Place Order_&#x20;
  * New order single(D) - (Stop=3, StopLimit=4)
  * ExecutionReport - In FIX and WS
* Adding the stopPrice to:
  * _Place Order_
  * New order single(D) - (StopPx=99)
  * ExecutionReport - In FIX and WS
  * Orders
* Adding a new message type to ExecutionReport  - Suspended

## 2023-06-15

* We made some cosmetic changes to the MD FIX documentation.&#x20;
  * To make MBP more clear we added that MBP requires at least BID and ASK&#x20;
  * We added the valid values to marketDepth(264) in Time & Sales and to the Auction Indicative Equilibrium Price (Imbalance)

## 2023-06-06

**Note!** From this version, there is a change in the Trades API (Both on Private Data API and Reporting API),&#x20;

This field MultiLegReportingType is **always** sent

FIX API

* Market Data:
  * Symbol is now mandatory in  <mark style="color:green;">Market by Order Full Depth</mark> and <mark style="color:green;">Auction Indicative Equilibrium Price</mark> in Market Data Request (V)
  * Adding new fields to the security list to support bonds details
  * Adding New tags to the  <mark style="color:green;">MBOFD</mark> response:&#x20;
    * 20028(Custom tag) - identify is this message represent real book state or temporary non real book state.
      * Market Data Incremental Refresh (MsgType = X)
      * <mark style="color:red;">To be deprecated</mark> Market Data Snapshot Full Refresh _(MsgType = W)_
    * 20029(Custom tag): Market Data Snapshot Full Refresh _(MsgType = W)_- identify last message of the snapshot
  * Adding new values to field 269 (MDEntryType) in Market Data Snapshot Full Refresh _(MsgType = W) and_ Market Data Request _(MsgType = V))_
    * r (Reference price)
    * v (Order book volume)
    * f (Block trades volume)
    * g (EFRP trades volume)
    * o (Other trades volume)
  * Considering Trade Entry data to the Market Data Snapshot Full Refresh _(MsgType = W) for those_ 269 (MDEntryType) _values:_
    * B (Trade volume)
    * 2 (Trade)
    * 7 (Trading session high price)&#x20;
    * 8 (Trading session Low price)
* Drop Copy:
  *   Changing the values for tag 442 (MultiLegReportingType) in the Trades Capture Report to be as follow:

      * 1 - Single security - not strategy trade&#x20;
      * 2 - Individual leg of a multi-leg security
      * 3 - Multi-leg security - the parent trade of multi-leg security

      On TradeCaptureReportRequest (AD) you should now specify 2 or 3. \


Trading API

New Time In Force was added for limit order - **DAY** - Order will be automatically canceled when trading day is closed. Can be used for both FIX and WS APIs.

Note: Next version we will allow configuration whether this new TIF is allowed or not.&#x20;

## 2023-03-22

* New fields in the Security list _(MsgType = y)_
* Adding a new field to Market Data Snapshot Full Refresh _(MsgType = W) -_ NumberOfOrders (346) for MBP message
* _Adding to_ MDUpdateAction(279) **Time & Sales -** 2 = Delete: When an trade is canceled (Market Data Incremental Refresh _(MsgType = X)_)
* _Only instruments that are active will be returned (_&#x4D;arket Data Request _(MsgType = V))_

## 2023-03-01

* New Market Data requests are now supported:
  * Market by Order Full Depth
  * Time & Sales&#x20;
  * Auction Indicative Equilibrium Price&#x20;
* **Trades Capture Report Request (AD)**:  Until this version MultiLegReportingType(442) was explicit so if subscribing using MultiLegReportingType(442) = 2 (Individual leg) you would get only strategy legs trades , if subscribing using MultiLegReportingType(442) = 1 (Single  security) you would get only strategy parent trades.\
  From this version:
  * If the MultiLegReportingType(442) = 1 (Single security) you will get:&#x20;
    * All regular trades (non strategy)&#x20;
    * Strategy trades - parent trades&#x20;
  * If the MultiLegReportingType(442) = 2 (Individual leg) you will get:&#x20;
    * All regular trades (non strategy)&#x20;
    * Strategy trades - legs trades&#x20;
  * In case noting was sent we return the same as 1 ( Single security).&#x20;
* New field added to the Security List(y) - text(58), used in case of rejection to include error message.&#x20;
* New fields to the Market Data Snapshot (W)  - SecurityID(48) and SecurityIDSource(22)
* OrigClOrdID(41) - From this version, we removed this tag from Trade Capture Report when 487 (TradeReportTransType) = 0 (New)&#x20;
* Adding "To be deprecated": label of the below fields from [placeOrder ](api-changes.md#placeorder)and derived messages&#x20;
  * userId
  * accountId
  * Note: those fields are still supported, Exberry operation team will coordinate the deprecation with Exberry clients.&#x20;
* Few valiodation messaged were updated in [Trading API ](../ws/trading-api.md)&#x20;



&#x20;

## 2023-02-08&#x20;

* New section added: Market Data for FIX GW - see [here ](https://app.gitbook.com/o/okJjrZYa0IvDclrhKkAo/s/-M-yqzGakbR2s9rOe4nu/~/changes/vh4Utk97DRg2Npf936du/fix-trading/market-data)
  * Market Data
  * Security List
  * Security Status
* Additional FIX GW Changes:
  * New Fields in [Trade Capture Reports (AE)](../fix-trading/trading-messages-1.md#tradecapturereport-ae) on trade cancellation (TradeReportTransType =  1):
    * orderId(37)
    * ClOrdID(11)&#x20;
  * Removed OrigClOrdID(41) from all Execution Reports except from:
    * ExecType(150) = 4(CANCELED)&#x20;
    * ExecType(150) = 5(REPLACE)
  * Adding ClOrdID(11) in:
    * OrderMassCancelReport (r)
    * OrderCancelReject (9)
  * Changing the ClOrdID(11) to be a number only in:
    * OrderMassCancelRequest (q)
    * OrderCancelRequest (F)
    * OrderCancelReplaceRequest (G)
  * Deprecation of the below fields from NewOrderSingle (MsgType = D) and ExecutionReport (MsgType = 8) :&#x20;
    * 200001 - userId
    * 200002 - accountId&#x20;

## 2023-01-18&#x20;

* New field in ExecutionReport - ClOrdID(11)
* The Rejected Execution Report can now be associated with the NewOrderSingle message. (By using ClOrdID (11) or OrigClOrdID(41))
* OrigClOrdID(41) is now optional in  ExecutionReport&#x20;

## 2023-01-03&#x20;

* New Filter in   [Trades Reports](../ws/reporting-api.md#trades-v2)&#x20;

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
* New filters in [orders](../ws/reporting-api.md#orders)

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

## 2022-12-06

* New FIX Drop Copy Client initiated message : [Mass Order Status Request ](../fix-trading/trading-messages-1.md#ordermassstatusrequest-af)
* Place Order validation that price must ne positive was removed
* New Filters in   [Trades Reports](../ws/reporting-api.md#trades-v2)  &#x20;

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

## 2022-11-21

Trading API

* From now on, negative prices are allowed, if you want to make sure that no negative process will be sent for a specific instrument you should configure that minimum price to be 0.0001 or so.&#x20;
*   FIX API:

    * On Order Cancel Reject (msgType=9) in case of failure: OrdStatus(39) was missing, now will have the value 8 = Rejected
    * On Execution Report (msgType=8) in case of rejection OrderID(37) was missing, now will have the value -1

    \


    ### Orders&#x20;

    On [#massorderstatus](../ws/private-data-api.md#massorderstatus "mention")and [#orders](../ws/reporting-api.md#orders "mention") new fields are added

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

## 2022-11-01

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

## 2022-10-03

Drop Copy for FIX GW - see [here](../fix-trading/trading-messages-1.md)&#x20;



New `trades` API that is a real time stream for getting all the trades data&#x20;

{% hint style="info" %}
`qualifier: v1/exchange.market/trades`
{% endhint %}

Available messages in that stream :&#x20;

* MatchedTrade: for order book trade&#x20;
* TradeReport: for trade entry trade&#x20;
* TradeCancel: for trade cancellation (separate records will be returned for cancellation)

<table><thead><tr><th width="149">Field</th><th width="337">Description</th><th width="100" align="center">Order Book</th><th width="80" align="center">Trade Entry</th><th width="92" align="center">Trade Cancel</th></tr></thead><tbody><tr><td>eventId</td><td> Event Id </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>timestamp</td><td>Event timestamp (in nanoseconds) in GMT</td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>actionType</td><td><p>MatchedTrade </p><p>TradeReport </p><p>TradeCancel</p></td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>orderId</td><td>Order id initiated the trade </td><td align="center">V</td><td align="center"> </td><td align="center"> </td></tr><tr><td>mpOrderId</td><td>From source order </td><td align="center">V</td><td align="center"> </td><td align="center"> </td></tr><tr><td>mpId</td><td> MP Id </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>mpName</td><td>MP Name </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>instrumentId</td><td> Instrument id </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>Instrument</td><td>instrument symbol</td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>side</td><td> Buy/ Sell </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>price</td><td> Trade price </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>quantity</td><td> Trade quantity </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>tradeId</td><td>matchId </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>tradingMode</td><td>IA - (Scheduled Intraday Auction) -When execution was as part of auction CT (Continuous Trading) - When execution was done on a regular trading ON - Trade Reporting (On Exchange)</td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>accountType</td><td>Optional, From source order </td><td align="center">V</td><td align="center">V</td><td align="center">V</td></tr><tr><td>parties</td><td>Optional, From source order </td><td align="center">V</td><td align="center">V</td><td align="center"><code>opt</code></td></tr><tr><td>tradeType</td><td>EFRP/Block/Other</td><td align="center"> </td><td align="center">V</td><td align="center"><code>opt</code></td></tr><tr><td>makerTaker</td><td><strong>Taker</strong> if order was never resting on the book for that trade <br><strong>Maker</strong> if order was resting on the book for that trade</td><td align="center">V</td><td align="center"> </td><td align="center"> </td></tr><tr><td>tradeDate</td><td>Date of the business day of that trade<br>Format: YYY-MM-DD</td><td align="center"> </td><td align="center"> </td><td align="center"> </td></tr><tr><td>trackingNumber</td><td> Tracking number </td><td align="center"> </td><td align="center"> </td><td align="center"> </td></tr></tbody></table>

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

## 2022-09-13

* On FIX API:
  * LOGON message can now be sent with a simple password authentication&#x20;
  * New tags on NewOrderSingle (MsgType = D) and ExecutionReport (MsgType = 8)
    * AccountType (581)&#x20;
    * Parties component
* On [#trades-v2](../ws/reporting-api.md#trades-v2 "mention")new field was added: `tradeDate`
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

## 2022-08-31

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



<table><thead><tr><th width="112">Name</th><th width="165.33333333333331"></th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>String</td><td>Party id<br>Max length = 20 </td></tr><tr><td>source</td><td>Char</td><td>Party source</td></tr><tr><td>role</td><td>Int</td><td>Party role </td></tr></tbody></table>

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

This is going to impact [#orderbookdepth](../ws/market-data.md#orderbookdepth "mention") and [#executionreports](../ws/private-data-api.md#executionreports "mention")

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

This is going to impact [#orderbookdepth](../ws/market-data.md#orderbookdepth "mention") and [#executionreports](../ws/private-data-api.md#executionreports "mention")



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
New version is going to be release to [#trades](../ws/reporting-api.md#trades "mention") API.\
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



## 2022-05-16

`replaceOrder` API is now available, it allows cancel and add new order in a single command.

See [#replaceorder](../ws/trading-api.md#replaceorder "mention") for more details.

## 2022-05-11

Introduce new version of `orderBookDepth` and `orderBookState`

{% hint style="info" %}
The new versions are available on:

* `v2/exchange.market/orderBookState`
* `v2/exchange.market/orderBookDepth`
{% endhint %}

This version includes market events, now you can get real time messages when trading is being halted / resumed, when market is open / closed and more.\
See [here ](../ws/market-data.md#instrument-status-message-1)for more details.

{% hint style="warning" %}
The old endpoints (v1) are still up and running, but will be deprecated in one of the next releases.\
New features/ fixes will be introduced only for the new version.

\
Please update your systems to work with the new endpoints.
{% endhint %}

## 2021-12-14

Reporting API is now available allow easy retrieval of orders and trades

## 2021-10-04

`orderBookDepth` and `executionReports` :

* Now have a dedicated `errorCode:2` for case the active stream is being disconnected.
* Bug fix regarding sorting

## 2021-06-29&#x20;

Additions:

* [FIX Gateway](../fix-trading/introduction.md) is now available, contact us if you would like to try it\\

Changes:

* `userId` is now optional field within `placeOrder` command
* Error message for errorCode =1 was changed for `orderBookState` and `massOrderStatus`
* Admin API [changes](https://documenter.getpostman.com/view/6229811/TzCV3jcq#api-changes):
  * Instrument symbol can now contains .(dot) and -(hyphen)
  * Calendar.tradingDays is now optional to allow auctions only trading schedule

## 2021-05-13&#x20;

Additions:

* [Private API](../ws/private-data-api.md) to get your own detailed orders data on a simple way.
* [Admin API ](../other/admin-api.md)for the entire administration activity can now be done via an API.

## 2021-03-10&#x20;

Additions:

* New trading API `modifyOrder` that allows to reduce order quantity without losing the order priority on book.
* New messageType `Modified` in`orderBookDepth` for modified orders
  * Note: this require a new `apiKey`
* New `placeOrder` validations (tick size, price collar, max order value and max book depth).
* Add `accountId` as optional parameter to `placeOrder`

## 2020-11-12&#x20;

Websocket endpoint changed to be `wss://exchange-gateway-uat.exchange.uat.exberry.io`

## 2020-11-11&#x20;

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

## 2020-08-26&#x20;

**Added**

* New Trading API method `massCancel`to cancel all resting orders for the relevant market participant.
* IOC & FOK are now supported in limit order (and not only market order).

## 2020-06-28&#x20;

**Added**

* New Market Data API method `orderBookState`to retrieve snapshot of order book state.

## 2020-06-24&#x20;

**Added**

* New `trackingNumber`as input and output parameter to `orderBookState`to allow stream consumption from specific event.

**Changed**

* On `orderBookState`for "Cancelled" events, `orderSide`changed to be `side`
* `q`field was changed in all messages, in addition it is now returned as part of stream closure message.
