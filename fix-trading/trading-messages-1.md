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

## Supported Messages

### Client Initiated Messages

| Message                                                                             | MsgType | Usage                                                                                   |
| ----------------------------------------------------------------------------------- | ------- | --------------------------------------------------------------------------------------- |
| [Trades Capture Report Request](trading-messages-1.md#tradecapturereportrequest-ad) | AD      | Allows to request Trade Capture Reports from the server                                 |
| [Order Mass Status Request](trading-messages-1.md#ordermassstatusrequest-af)        | AF      | Request the list of active orders, will response with Execution Reports (MsgType = 8 )  |

### Server Initiated Messages

| Message                                                                                    | MsgType | Usage                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------ | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Trades Capture Report Request Ack](trading-messages-1.md#tradecapturereportrequestack-aq) | AQ      | <p>Acknowledges the receipt of a Trade Capture Report Request message from the client.<br>Can indicates successful subscription or rejected one. </p>                                                                       |
| [Trades Capture Report](trading-messages-1.md#tradecapturereport-ae)                       | AE      | <p>Indicates one of the following: </p><ul><li>Trade </li><li>Trade Cancellation </li></ul>                                                                                                                                 |
| [Execution Report](trading-messages.md#executionreport-msgtype-8)                          | 8       | <p>Indicates one of the following: </p><ul><li>Order added to the book</li><li>Order was executed (Fully or partially)</li><li>Order canceled from book (also in case of GTD expired)</li><li>Order was modified </li></ul> |

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

| Tag | Name              | Required | Description                                                                                       |
| --- | ----------------- | -------- | ------------------------------------------------------------------------------------------------- |
| 584 | MassStatusReqID   | Y        | Order Mass Status Request ID                                                                      |
| 585 | MassStatusReqType | Y        | <p>Type of Mass Status Report. <br>7 = Status for all orders (will return active orders only)</p> |

### **TradeCaptureReportRequest** _(AD)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=93|35=AD|49=MP1|56=EXBERRY|34=2|52=20221002-08:37:59.927000|263=0|568=1664699879927|569=0|10=049|
```
{% endcode %}

| Tag | Name                    | Required | Description                                                                                                                                                                                                                                                                                          |
| --- | ----------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 568 | TradeRequestID          | Y        | Trade Capture Report Request ID                                                                                                                                                                                                                                                                      |
| 569 | TradeRequestType        | Y        | <p>Type of Trade Capture Report. </p><p>0 = All Trades</p>                                                                                                                                                                                                                                           |
| 263 | SubscriptionRequestType | Y        | <p>Subscription Request Type. </p><p>0 = Snapshot<br>1 = Snapshot + Updates (Subscribe)<br>2 =  Disable previous Snapshot + Update Request (Unsubscribe)</p>                                                                                                                                         |
| 442 | MultiLegReportingType   | N        | <p>This determines how to publish strategy trades: <br>1 = Single security (default if not specified): only the parent trade is being sent. <br>2 = Individual leg of a multi-leg security: only the underlying legs trades are being sent.<br><br>Note: Non strategy trades will always be sent</p> |

### **TradeCaptureReportRequestAck** _(AQ)_

{% code title="Accepted" overflow="wrap" %}
```
8=FIXT.1.1|9=94|35=AQ|49=EXBERRY|56=ABC1|34=2|52=20221002-08:42:21.610692|568=1664700144316|569=0|749=0|750=0|10=243|
```
{% endcode %}

{% code title="Rejected" overflow="wrap" %}
```
8=FIXT.1.1|9=95|35=AQ|49=EXBERRY|56=ABC1|34=5|52=20221002-09:02:20.774880|568=1664700144316|569=0|749=99|750=2|10=065|
```
{% endcode %}

| Tag | Name               | Required | Description                                                                                                                                                                                                                                                                                 |
| --- | ------------------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 568 | TradeRequestID     | Y        | From request                                                                                                                                                                                                                                                                                |
| 569 | TradeRequestType   | Y        | From request                                                                                                                                                                                                                                                                                |
| 749 | TradeRequestResult | Y        | <p>Subscription Request Type. </p><p><strong>0</strong> = Successful </p><p><strong>8</strong> = TradeRequestType not supported </p><p><strong>9</strong> = Not authorized (in case credentials doesn’t have the right permission) </p><p><strong>99</strong> = Other (any other case )</p> |
| 750 | TradeRequestStatus | Y        | <p><strong>0</strong> = Accepted (in case validations passed)</p><p><strong>2</strong> = Rejected (in case of issue on the request or no results were found or problem was found during results publication )</p>                                                                           |
| 58  | Text               | N        | Will be populated in case TradeRequestStatus = Rejected                                                                                                                                                                                                                                     |



### **TradeCaptureReport** _(AE)_

**TradeCaptureReport** _(AE)- Single Leg_

{% tabs %}
{% tab title="Order Book Trade" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=303|35=AE|49=EXBERRY|56=MP1|34=204|52=20230209-08:13:17.022751|568=1675930324481|912=N|487=0|828=0|55=INS1|32=0.88|31=1.0|75=20230209|60=20230209-08:13:16.996985|552=1|54=1|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=MP1|447=D|452=7|797=Y|570=N|1003=7|37=13|11=1675930396980|41=1675930396980|10=223|
```
{% endcode %}
{% endtab %}

{% tab title="Trade Cancellation" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=297|35=AE|49=EXBERRY|56=MP1|34=216|52=20230209-08:18:51.753707|568=1675930324481|912=N|487=1|55=INS1|32=0.88|31=1.0|75=20230209|60=20230209-08:18:51.733963|552=1|54=1|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=MP|447=D|452=7|797=Y|570=N|1003=7|37=13|11=1675930396980|41=1675930396980|10=227|
```
{% endcode %}
{% endtab %}

{% tab title="Trade Entry" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=245|35=AE|49=EXBERRY|56=MP1|34=222|52=20230209-08:20:59.193384|568=1675930324481|912=N|487=0|828=2|55=INS1|32=10.00|31=100.0|75=20230209|60=20230209-08:20:59.169972|552=1|54=2|581=3|453=2|448=2|447=D|452=38|448=MP|447=D|452=7|797=Y|570=N|1003=8|10=132|
```
{% endcode %}
{% endtab %}
{% endtabs %}

**TradeCaptureReport** _(AE)- Multi Leg_

{% tabs %}
{% tab title="SINGLE SECURITY" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=303|35=AE|49=EXBERRY|56=MP1|34=204|52=20230209-08:13:17.022751|568=1675930324481|912=N|487=0|828=0|55=INS1|32=0.88|31=1.0|75=20230209|60=20230209-08:13:16.996985|552=1|54=1|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=MP1|447=D|452=7|797=Y|570=N|1003=7|37=13|11=1675930396980|10=223|
```
{% endcode %}
{% endtab %}

{% tab title="INDIVIDUAL LEG " %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=290|35=AE|49=EXBERRY|56=mastergroup|34=6|52=20221115-15:13:05.331013|568=1668525185446|912=Y|487=0|828=2|55=Test1FMar23|32=10.0000|31=-1.500000|75=20221115|60=20221115-15:07:15.153740|552=1|54=2|581=1|453=2|448=33|447=D|452=38|448=Test1|447=D|452=7|797=Y|570=N|1003=1|442=2|824=2|1522=3.000000|10=223|
```
{% endcode %}
{% endtab %}
{% endtabs %}



| Tag                                                                   | Name                      | Required | Description                                                                                                                                                                                                                                                                                                              |
| --------------------------------------------------------------------- | ------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 568                                                                   | TradeRequestID            | Y        | From request                                                                                                                                                                                                                                                                                                             |
| 912                                                                   | LastRptRequested          | Y        | Indicates if this is the last record (applicable for snapshots only)                                                                                                                                                                                                                                                     |
| 487                                                                   | TradeReportTransType      | Y        | <p>Trade Report message transaction type </p><p><strong>0 (New)</strong>: execution or trade entry </p><p><strong>1 (Cancel)</strong> : Trade cancellation</p>                                                                                                                                                           |
| 828                                                                   | TrdType                   | N        | <p>Only when TradeReportTransType = 0</p><p><strong>0 (Regular trade)</strong>: For order book trades </p><p><strong>1 (Block trade)</strong>: For trade entry where type = Block </p><p><strong>2 (EFP)</strong>: For trade entry where type = EFRP </p><p><strong>99</strong> : For trade entry where type = Other</p> |
| 797                                                                   | CopyMsgIndicator          | Y        | Indicates drop copy, always “Y”                                                                                                                                                                                                                                                                                          |
| 570                                                                   | PreviouslyReported        | Y        | Always “N”                                                                                                                                                                                                                                                                                                               |
| 1003                                                                  | TradeId                   | Y        | Trade Id (Match Id)                                                                                                                                                                                                                                                                                                      |
| 552                                                                   | NoSides                   | Y        | Always 1 (One Side)                                                                                                                                                                                                                                                                                                      |
| 37                                                                    | OrderID                   | N        | <p>Order Id<br><code>Changed</code> Only when TrdType = 0 (Regular trade) <br>For both TradeReportTransType 0 (New) and 1(Cancel)</p>                                                                                                                                                                                    |
| 11                                                                    | ClOrdID                   | N        | <p>ClOrdID of the order<br><code>Changed</code> Only when TrdType = 0 (Regular trade)<br>For both TradeReportTransType 0 (New) and 1(Cancel) </p>                                                                                                                                                                        |
| <p><mark style="color:red;">Deprecated</mark></p><p><del>41</del></p> | ~~OrigClOrdID~~           | ~~N~~    | ~~Ignore~~                                                                                                                                                                                                                                                                                                               |
| 32                                                                    | LastQty                   | N        | Trade amount                                                                                                                                                                                                                                                                                                             |
| 31                                                                    | LastPx                    | N        | Trade price                                                                                                                                                                                                                                                                                                              |
| 55                                                                    | Symbol                    | N        | Instrument symbol                                                                                                                                                                                                                                                                                                        |
| 54                                                                    | Side                      | N        | <p>1= Buy</p><p>2 = Sell</p>                                                                                                                                                                                                                                                                                             |
| 44                                                                    | Price                     | N        | Order price                                                                                                                                                                                                                                                                                                              |
| 60                                                                    | TransactTime              | Y        | Event timestamp in microseconds in UTC Format YYYYmmDD-hh:mm:ss.000000                                                                                                                                                                                                                                                   |
| 75                                                                    | TradeDate                 | Y        | Trade date. Format: YYYYMMDD                                                                                                                                                                                                                                                                                             |
| 581                                                                   | AccountType               | N        | <p>Account type </p><p>1 = Client </p><p>3 = House</p>                                                                                                                                                                                                                                                                   |
| +                                                                     | Parties                   | N        | <p>This is party information related to the submitter of the request.<br>See <a href="trading-messages.md#new-parties-component">here </a>for more details </p>                                                                                                                                                          |
| <p><mark style="color:blue;">NEW</mark><br>442</p>                    | MultiLegReportingType     | N        | Will be shown for strategy trades only - per request parameter                                                                                                                                                                                                                                                           |
| <p><mark style="color:blue;">NEW</mark><br>824</p>                    | tradeLegRefId             | N        | Only when MultiLegReportingType(442) = 2, will show leg index (leg1→1, leg2→ 2 etc)                                                                                                                                                                                                                                      |
| <p><mark style="color:blue;">NEW</mark><br>1522</p>                   | multiLegDifferentialPrice | N        | Only when MultiLegReportingType(442) = 2, will show parent trade price                                                                                                                                                                                                                                                   |
