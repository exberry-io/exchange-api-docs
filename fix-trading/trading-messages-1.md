# Drop Copy

There are 2 drop copy services provided by Exberry:

* **Execution Reports**: To receive a real-time copy of each eligible Execution Report immediately after it is published.
* **Trade Capture Reports**: To receive information on the order book trades as well as trade entries along with notifications of any trade cancel. Trades are availble for the last day only , details of trades on previous trading days are not available here.

**Data Eligibility**&#x20;

Drop copy can be published for single market participant (MP) or for group of MPs that are pre-configured by exchange operations team.

MP name will always be added as additional party with the below parameters:

* 448 (PartyID) = MP Name&#x20;
* 452 (PartyRole) = 7 (Entering Firm)&#x20;
* 447 (PartyIDSource) = D (Custom)

**Subscription**&#x20;

* **Execution Reports**: No need to subscribe, assuming the credentials provided by operations team include the right permission, the Execution Reports will start to be published  once sessions is properly initiated.&#x20;
* **Trade Capture Reports**: Explicit subscription is required, see below for more details, successful subscription will occur only if the appropriate permission was granted to connection credentials. &#x20;

## **Strategies/ Multi Legs Handling**&#x20;

There are 2 options for strategies trade reporting:

* Send one strategy summary message with MultiLegReportingType(442) = 3 (Multi-leg security)
* Send individual legs messages with MultiLegReportingType(442) = 2 (Individual leg of a multi-leg security)

On ExecutionReport (8) system sends only the fill of the strategy summary, while on TradeCaptureReport(AE) system allows subscriber to determine how to get the trades.&#x20;

For example - for Spread instrument ABC-Feb23-Mar23 which is spread on future contracts ABC-Feb23 and ABC-Mar23.&#x20;

ExecutionReport will include only ABC-Feb23-Mar23 (without any value tag 442)

On TradeCaptureReport subscriber has 2 options how to get the data:

**Option 1**

Using MultiLegReportingType(442) = 3 (Multi-leg security) on the TradeCaptureReportRequest (AD) Single trade report will be sent:

* Trade Report 1:&#x20;
  * Symbol (55) = ABC-Feb23-Mar23
  * MultiLegReportingType (442) = 3 (Multi-leg security)

**Option 2**

Using MultiLegReportingType(442) = 2 (Individual leg of a multi-leg security) on the TradeCaptureReportRequest (AD) two trade reports will be sent:

* Trade Report 1:&#x20;
  * Symbol (55) = ABC-Feb23
  * MultiLegReportingType (442) = 2 (Individual leg of a multi-leg security)&#x20;
  * tradeLegRefId (824) = 1&#x20;
  * multiLegDifferentialPrice (1522) = Trade price of the strategy&#x20;
*   Trade Report 2:&#x20;

    * Symbol (55) = ABC-Mar23&#x20;
    * MultiLegReportingType (442) = 2 (Individual leg of a multi-leg security)&#x20;
    * tradeLegRefId (824) = 2&#x20;
    * multiLegDifferentialPrice (1522) = Trade price of the strategy



<mark style="color:blue;">**NEW**</mark>** How To Associate Individual Leg Trade Report To The Parent Trade and Order -**

* Case placed 2 sell orders with quantity 1, OrderID (37) = 11 & 12
* Then placed 1 buy order with quantity 2 on the same price as sell orders, OrderID (37) = 13
* Let's consider that 2 sell orders were filled by this buy order.

In this case we shall receive:

* For sell order #11:
  * Fill ExecutionReport for strategy TrdMatchID (880) =1 and OrderID (37) = 11
  * 2 TradeCaptureReport legs messages where TradeLinkID (820) = 1  and RelatedOrderID(2887)  = 11
* For sell order #12:
  * Fill ExecutionReport for strategy TrdMatchID (880) =2 and OrderID (37) = 12
  * 2 TradeCaptureReport legs messages where TradeLinkID (820) = 2 and RelatedOrderID(2887)  = 12
* For buy order's:
  * 1 fill ExecutionReport for strategy TrdMatchID (880) =1 and OrderID (37) = 13
  * 1 fill ExecutionReport for strategy TrdMatchID (880) =2 and OrderID (37) = 13
  * 2 TradeCaptureReport legs messages where TradeLinkID (820) = 1 and RelatedOrderID(2887)  = 13
  * 2 TradeCaptureReport legs messages where TradeLinkID (820) = 2 and RelatedOrderID(2887)  = 1

**Trade Cancellation**&#x20;

Trade cancellation trade reports works exactly the same as trade report.

## Supported Messages

### Client Initiated Messages

<table><thead><tr><th width="228">Message</th><th width="104.33333333333331">MsgType</th><th width="402.6666666666667">Usage</th></tr></thead><tbody><tr><td><a href="trading-messages-1.md#tradecapturereportrequest-ad">Trades Capture Report Request</a></td><td>AD</td><td>Allows to request Trade Capture Reports from the server</td></tr><tr><td><a href="trading-messages-1.md#ordermassstatusrequest-af">Order Mass Status Request</a></td><td>AF</td><td>Request the list of active orders, will response with Execution Reports (MsgType = 8 ) </td></tr></tbody></table>

### Server Initiated Messages

<table><thead><tr><th width="227">Message</th><th width="104.33333333333331">MsgType</th><th width="402.6666666666667">Usage</th></tr></thead><tbody><tr><td><a href="trading-messages-1.md#tradecapturereportrequestack-aq">Trades Capture Report Request Ack</a></td><td>AQ</td><td>Acknowledges the receipt of a Trade Capture Report Request message from the client.<br>Can indicates successful subscription or rejected one. </td></tr><tr><td><a href="trading-messages-1.md#tradecapturereport-ae">Trades Capture Report</a></td><td>AE</td><td><p>Indicates one of the following: </p><ul><li>Trade </li><li>Trade Cancellation </li></ul></td></tr><tr><td><a href="trading-messages.md#executionreport-msgtype-8">Execution Report</a></td><td>8</td><td><p>Indicates one of the following: </p><ul><li>Order added to the book</li><li>Order was executed (Fully or partially)</li><li>Order canceled from book (also in case of GTD expired)</li><li>Order was modified </li></ul></td></tr></tbody></table>

### Request/ Response Messages

| Message                                                                                                              | Success Message                                                                                       | Failure Message                                                                      | No Result                                                                                                      |
| -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| [TradeCaptureReportRequest](https://docs.exberry.io/fix-trading/trading-messages-1#tradecapturereportrequest-ad)(AD) | <ul><li>TradeCaptureReportRequestAck (Accepted)(AQ)</li></ul><ul><li>TradeCaptureReport(AE)</li></ul> | <ul><li>TradeCaptureReportRequestAck (Rejected)(AQ)</li></ul>                        | <ul><li>TradeCaptureReportRequestAck(Success)(AQ)</li><li>TradeCaptureReportRequestAck(Rejected)(AQ)</li></ul> |
| [OrderMassStatusRequest](https://docs.exberry.io/fix-trading/trading-messages-1#ordermassstatusrequest-af)(AF)       | <ul><li>Execution Reports(8)</li></ul>                                                                | <p>One of the below:</p><ul><li>BusinessMessageReject(j)</li><li>Reject(3)</li></ul> | <ul><li>Execution Reports (Rejected status)(8)</li></ul>                                                       |

## Messages Specifications

### OrderMassStatusRequest (AF)

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=82|35=AF|49=MP1|56=EXBERRY|34=3|52=20221201-10:20:20.940000|584=1669890020940|585=7|10=097|
```
{% endcode %}

<table><thead><tr><th width="109">Tag</th><th width="211">Name</th><th width="103">Required</th><th>Description</th></tr></thead><tbody><tr><td>584</td><td>MassStatusReqID</td><td>Y</td><td>Order Mass Status Request ID</td></tr><tr><td>585</td><td>MassStatusReqType</td><td>Y</td><td>Type of Mass Status Report. <br>7 = Status for all orders (will return active orders only)</td></tr></tbody></table>

### **TradeCaptureReportRequest** _(AD)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=94|35=AD|49=user|56=EXBERRY|34=2|52=20230606-13:38:44.185000|263=1|442=2|568=1686058724185|569=0|10=149|
```
{% endcode %}

<table><thead><tr><th width="89.16216216216213">Tag</th><th width="229">Name</th><th width="105">Required</th><th>Description</th></tr></thead><tbody><tr><td>568</td><td>TradeRequestID</td><td>Y</td><td>Trade Capture Report Request ID</td></tr><tr><td>569</td><td>TradeRequestType</td><td>Y</td><td><p>Type of Trade Capture Report. </p><p>0 = All Trades</p></td></tr><tr><td>263</td><td>SubscriptionRequestType</td><td>Y</td><td><p>Subscription Request Type. </p><p>0 = Snapshot<br>1 = Snapshot + Updates (Subscribe)<br>2 =  Disable previous Snapshot + Update Request (Unsubscribe)</p></td></tr><tr><td>442</td><td>MultiLegReportingType</td><td>N</td><td><p>This determines how to publish strategy trades: <br><del>1 = Single security (default if not specified): only the parent trade is being sent.</del> <br>2 = Individual leg of a multi-leg security (default if not specified):  Request outright trades and Individual legs of strategy trades</p><p><mark style="color:blue;">NEW</mark> 3 =  Multi-leg security: Request outright trades and strategy trades</p></td></tr></tbody></table>

### **TradeCaptureReportRequestAck** _(AQ)_

{% code title="Accepted" overflow="wrap" %}
```
 8=FIXT.1.1|9=94|35=AQ|49=EXBERRY|56=user|34=2|52=20230606-13:38:46.799302|568=1686058724185|569=0|749=0|750=0|10=188|
```
{% endcode %}

{% code title="Rejected" overflow="wrap" %}
```
8=FIXT.1.1|9=95|35=AQ|49=EXBERRY|56=ABC1|34=5|52=20221002-09:02:20.774880|568=1664700144316|569=0|749=99|750=2|10=065|
```
{% endcode %}

<table><thead><tr><th width="89.16216216216213">Tag</th><th width="188">Name</th><th width="105">Required</th><th>Description</th></tr></thead><tbody><tr><td>568</td><td>TradeRequestID</td><td>Y</td><td>From request </td></tr><tr><td>569</td><td>TradeRequestType</td><td>Y</td><td>From request </td></tr><tr><td>749</td><td>TradeRequestResult</td><td>Y</td><td><p>Subscription Request Type. </p><p><strong>0</strong> = Successful </p><p><strong>8</strong> = TradeRequestType not supported </p><p><strong>9</strong> = Not authorized (in case credentials doesn’t have the right permission) </p><p><strong>99</strong> = Other (any other case )</p></td></tr><tr><td>750</td><td>TradeRequestStatus</td><td>Y</td><td><p><strong>0</strong> = Accepted (in case validations passed)</p><p><strong>2</strong> = Rejected (in case of issue on the request or no results were found or problem was found during results publication )</p></td></tr><tr><td>58</td><td>Text</td><td>N</td><td>Will be populated in case TradeRequestStatus = Rejected </td></tr></tbody></table>



### **TradeCaptureReport** _(AE)_

**TradeCaptureReport** _(AE)- Single Leg_

{% tabs %}
{% tab title="Order Book Trade" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=257|35=AE|49=EXBERRY|56=user|34=11|52=20230606-13:38:51.373312|568=1686058728753|912=Y|487=0|828=0|55=INS1|32=10.0000|31=180.0000000|75=20230606|60=20230606-13:36:46.317402|552=1|54=2|453=1|448=user|447=D|452=7|797=Y|570=N|1003=23|37=51|11=1564403702139|442=1|10=212|
```
{% endcode %}
{% endtab %}

{% tab title="Trade Cancellation" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=252|35=AE|49=EXBERRY|56=user|34=28|52=20230606-13:43:59.229711|568=1686058728753|912=N|487=1|55=INS1|32=10.0000|31=15.0000000|75=20230606|60=20230606-13:43:59.213709|552=1|54=2|581=3|453=2|448=2|447=D|452=38|448=user|447=D|452=7|797=Y|570=N|1003=24|442=1|10=211|
```
{% endcode %}
{% endtab %}

{% tab title="Trade Entry" %}
{% code overflow="wrap" %}
```
 8=FIXT.1.1|9=259|35=AE|49=EXBERRY|56=user|34=20|52=20230606-13:41:40.469923|568=1686058728753|912=N|487=0|828=99|55=INS1|32=10.0000|31=15.0000000|75=20230606|60=20230606-13:41:40.460882|552=1|54=2|581=3|453=2|448=2|447=D|452=38|448=user|447=D|452=7|797=Y|570=N|1003=24|442=1|10=028|
```
{% endcode %}
{% endtab %}
{% endtabs %}

**TradeCaptureReport** _(AE)- Multi Leg_

{% tabs %}
{% tab title="SINGLE SECURITY" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=257|35=AE|49=EXBERRY|56=user|34=4|52=20230606-11:20:31.329393|568=1686050428437|912=Y|487=0|828=1|55=INS1|32=10.0000|31=10.0000000|75=20230605|60=20230606-10:33:57.567265|552=1|54=2|581=3|453=2|448=2|447=D|452=38|448=user|447=D|452=7|797=Y|570=N|1003=22|442=1|10=162|
```
{% endcode %}
{% endtab %}

{% tab title="INDIVIDUAL LEG " %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=385|35=AE|49=EXBERRY|56=MP|34=16|52=20230723-06:51:29.294782|568=2|912=N|487=0|828=0|55=INS1|32=15.0|31=3602879701896396.8|75=20230723|60=20230723-06:51:29.279568|552=1|54=1|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=USER|447=D|452=7|1003=5|73=2|2887=20|2888=1|2887=1690095087437|2888=2|797=Y|570=N|442=2|824=1|1522=10.0|820=10|1647=1|1648=99|1649=Spread|1650=14837|1651=99|10=019|
```
{% endcode %}
{% endtab %}

{% tab title="	MULTI LEG SECURITY" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=246|35=AE|49=EXBERRY|56=user|34=30|52=20230606-13:46:46.495498|568=1686059203864|912=Y|487=0|828=0|55=Spread|32=4.0|31=10.0|75=20230606|60=20230606-13:46:34.083207|552=1|54=2|453=1|448=user|447=D|452=7|797=Y|570=N|1003=6|37=13|11=1564403702140|442=3|10=023|
```
{% endcode %}
{% endtab %}
{% endtabs %}



<table><thead><tr><th width="99">Tag</th><th width="223">Name</th><th width="103">Required</th><th>Description</th></tr></thead><tbody><tr><td>568</td><td>TradeRequestID</td><td>Y</td><td>From request </td></tr><tr><td>912</td><td>LastRptRequested</td><td>Y</td><td>Indicates if this is the last record (applicable for snapshots only)</td></tr><tr><td>487</td><td>TradeReportTransType</td><td>Y</td><td><p>Trade Report message transaction type </p><p><strong>0 (New)</strong>: execution or trade entry </p><p><strong>1 (Cancel)</strong> : Trade cancellation</p></td></tr><tr><td>828</td><td>TrdType</td><td>N</td><td><p>Only when TradeReportTransType = 0</p><p><strong>0 (Regular trade)</strong>: For order book trades </p><p><strong>1 (Block trade)</strong>: For trade entry where type = Block </p><p><strong>2 (EFP)</strong>: For trade entry where type = EFRP </p><p><strong>99</strong> : For trade entry where type = Other</p></td></tr><tr><td>797</td><td>CopyMsgIndicator</td><td>Y</td><td>Indicates drop copy, always “Y”</td></tr><tr><td>570</td><td>PreviouslyReported</td><td>Y</td><td>Always “N”</td></tr><tr><td>1003</td><td>TradeId</td><td>Y</td><td>Trade Id (Match Id) </td></tr><tr><td>552</td><td>NoSides</td><td>Y</td><td>Always 1 (One Side)</td></tr><tr><td>37</td><td>OrderID</td><td>N</td><td>Order Id<br><code>Changed</code> Only when TrdType = 0 (Regular trade) <br>For both TradeReportTransType 0 (New) and 1(Cancel)</td></tr><tr><td>11</td><td>ClOrdID</td><td>N</td><td>ClOrdID of the order<br><code>Changed</code> Only when TrdType = 0 (Regular trade)<br>For both TradeReportTransType 0 (New) and 1(Cancel) </td></tr><tr><td><p><mark style="color:red;">Deprecated</mark></p><p><del>41</del></p></td><td><del>OrigClOrdID</del></td><td><del>N</del></td><td><del>Ignore</del></td></tr><tr><td>32</td><td>LastQty</td><td>N</td><td>Trade amount </td></tr><tr><td>31</td><td>LastPx</td><td>N</td><td>Trade price </td></tr><tr><td>55</td><td>Symbol</td><td>N</td><td>Instrument symbol</td></tr><tr><td>54</td><td>Side</td><td>N</td><td><p>1= Buy</p><p>2 = Sell</p></td></tr><tr><td>44</td><td>Price</td><td>N</td><td>Order price</td></tr><tr><td>60</td><td>TransactTime</td><td>Y</td><td>Event timestamp in microseconds in UTC Format YYYYmmDD-hh:mm:ss.000000</td></tr><tr><td>75</td><td>TradeDate</td><td>Y</td><td>Trade date. Format: YYYYMMDD</td></tr><tr><td>581</td><td>AccountType</td><td>N</td><td><p>Account type </p><p>1 = Client </p><p>3 = House</p></td></tr><tr><td><mark style="color:blue;">NEW</mark> 73</td><td>NoOrders</td><td>N</td><td><p>Only when MultiLegReportingType(442) = 2, </p><p>Indicates number of orders to be combined.</p><p>Will always be 2 </p></td></tr><tr><td><mark style="color:blue;">NEW</mark> <br>>2887</td><td>RelatedOrderID</td><td>N</td><td><p>Only when NoOrders(73) >0<br></p><p>Identifier of a related order.</p><ul><li>If RelatedOrderIDSource(2888) = 1 (Order identifier) → Parent orderId</li><li>If RelatedOrderIDSource(2888) = 2 (Client order identifier) →  Parent ClOrderId (=mpOrderId)</li></ul></td></tr><tr><td><mark style="color:blue;">NEW</mark> > 2888</td><td>RelatedOrderIDSource</td><td>N</td><td><p>Only when NoOrders(73) >0<br>Describes the source of the identifier that RelatedOrderID(2887) represents.</p><p>1 = Order identifier</p><p>2 = Client order identifier</p></td></tr><tr><td><mark style="color:blue;">NEW</mark> 1647</td><td>NoRelatedInstruments</td><td>N</td><td><p>Only when MultiLegReportingType(442) = 2, shows number of related instruments</p><p>Will always be 1</p></td></tr><tr><td><mark style="color:blue;">NEW</mark> <br>>1648</td><td>RelatedInstrumentType</td><td>N</td><td><p>Only when NoRelatedInstruments(1647) >0<br>The type of instrument relationship</p><p>99 (Custom: Multi-leg security)</p></td></tr><tr><td><mark style="color:blue;">NEW</mark> <br>>1649</td><td>RelatedSymbol</td><td>N</td><td>Only when NoRelatedInstruments(1647) >0<br>Parent Symbol</td></tr><tr><td><mark style="color:blue;">NEW</mark> <br>>1650</td><td>RelatedSecurityID</td><td>N</td><td>Only when NoRelatedInstruments(1647) >0<br>Parent instrumentId</td></tr><tr><td><mark style="color:blue;">NEW</mark> <br>>1651</td><td>RelatedSecurityIDSource</td><td>N</td><td><p>Only when NoRelatedInstruments(1647) >0<br>Identifies class or source of the RelatedSecurityID (1650) value.</p><p>99 (Custom)</p></td></tr><tr><td>+</td><td>Parties</td><td>N</td><td>This is party information related to the submitter of the request.<br>See <a href="trading-messages.md#new-parties-component">here </a>for more details </td></tr><tr><td>442</td><td>MultiLegReportingType</td><td>N</td><td><p>Will be shown for strategy trades only - per request parameter   </p><p>Will be shown for strategy trades only (per request):</p><p>1 - Single security - outright trade </p><p>2 - Individual leg of a multi-leg security </p><p>3 - Multi-leg security - the strategy trade of multi-leg security</p></td></tr><tr><td>824</td><td>tradeLegRefId</td><td>N</td><td>Only when MultiLegReportingType(442) = 2, will show leg index (leg1→1, leg2→ 2 etc)</td></tr><tr><td>1522</td><td>multiLegDifferentialPrice</td><td>N</td><td>Only when MultiLegReportingType(442) = 2, will show parent trade price</td></tr></tbody></table>

