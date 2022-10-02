# Drop Copy (NEW)

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

| Message                                                                             | MsgType | Usage                                                   |
| ----------------------------------------------------------------------------------- | ------- | ------------------------------------------------------- |
| [Trades Capture Report Request](trading-messages-1.md#tradecapturereportrequest-ad) | AD      | Allows to request Trade Capture Reports from the server |

### Server Initiated Messages

| Message                                                                                    | MsgType | Usage                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------ | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Trades Capture Report Request Ack](trading-messages-1.md#tradecapturereportrequestack-aq) | AQ      | <p>Acknowledges the receipt of a Trade Capture Report Request message from the client.<br>Can indicates successful subscription or rejected one. </p>                                                                       |
| [Trades Capture Report](trading-messages-1.md#tradecapturereport-ae)                       | AE      | <p>Indicates one of the following: </p><ul><li>Trade </li><li>Trade Cancellation </li></ul>                                                                                                                                 |
| [Execution Report](trading-messages.md#executionreport-msgtype-8)                          | 8       | <p>Indicates one of the following: </p><ul><li>Order added to the book</li><li>Order was executed (Fully or partially)</li><li>Order canceled from book (also in case of GTD expired)</li><li>Order was modified </li></ul> |

## Messages Specifications

### **TradeCaptureReportRequest** _(AD)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=93|35=AD|49=MP1|56=EXBERRY|34=2|52=20221002-08:37:59.927000|263=0|568=1664699879927|569=0|10=049|
```
{% endcode %}

| Tag | Name                    | Required | Description                                                |
| --- | ----------------------- | -------- | ---------------------------------------------------------- |
| 568 | TradeRequestID          | Y        | Trade Capture Report Request ID                            |
| 569 | TradeRequestType        | Y        | <p>Type of Trade Capture Report. </p><p>0 = All Trades</p> |
| 263 | SubscriptionRequestType | Y        | <p>Subscription Request Type. </p><p>0 = Snapshot</p>      |

### **TradeCaptureReportRequestAck** _(AQ)_

{% code title="Accepted" overflow="wrap" %}
```
8=FIXT.1.1|9=94|35=AQ|49=EXBERRY|56=ABX1|34=2|52=20221002-08:42:21.610692|568=1664700144316|569=0|749=0|750=0|10=243|
```
{% endcode %}

{% code title="Rejected" overflow="wrap" %}
```
8=FIXT.1.1|9=95|35=AQ|49=EXBERRY|56=ABX1|34=5|52=20221002-09:02:20.774880|568=1664700144316|569=0|749=99|750=2|10=065|
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

{% tabs %}
{% tab title="Order Book Trade" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=235|35=AE|49=EXBERRY|56=MP1|34=5|52=20221002-08:42:21.616014|568=1664700144316|912=N|487=0|828=0|55=MP1|32=0.3000|31=100.480000|75=20220911|60=20220911-14:04:12.915450|552=1|54=1|453=1|448=MP1|447=D|452=7|797=Y|570=N|1003=2|37=16|41=44|10=145|
```
{% endcode %}
{% endtab %}

{% tab title="Trade Cancellation" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=290|35=AE|49=EXBERRY|56=MP1|34=4|52=20221002-08:42:21.615454|568=1664700144316|912=N|487=1|55=MP1|32=10.0000|31=1.234500|75=|60=20220829-16:48:05.306673|552=1|54=1|581=1|453=3|448=TST1|447=D|452=12|448=1234|447=D|452=38|448=MP1|447=D|452=7|797=Y|570=N|1003=1|10=069|
```
{% endcode %}
{% endtab %}

{% tab title="Trade Entry" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=296|35=AE|49=EXBERRY|56=MP1|34=3|52=20221002-08:42:21.615454|568=1664700144316|912=N|487=0|828=2|55=MP1|32=10.0000|31=1.234500|75=|60=20220829-16:46:58.509161|552=1|54=1|581=1|453=3|448=TST1|447=D|452=12|448=1234|447=D|452=38|448=MP1|447=D|452=7|797=Y|570=N|1003=1|10=094|
```
{% endcode %}
{% endtab %}
{% endtabs %}

| Tag  | Name                 | Required | Description                                                                                                                                                                                                                                                                                                       |
| ---- | -------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 568  | TradeRequestID       | Y        | From request                                                                                                                                                                                                                                                                                                      |
| 912  | LastRptRequested     | Y        | Indicates if this is the last record (applicable for snapshots only)                                                                                                                                                                                                                                              |
| 487  | TradeReportTransType | Y        | <p>Trade Report message transaction type <strong>0 (New)</strong>: execution or trade entry </p><p><strong>1 (Cancel)</strong> : Trade cancellation</p>                                                                                                                                                           |
| 828  | TrdType              | N        | <p>Only when TradeReportTransType = 0</p><p><strong>0 (Regular trade)</strong>: For order book trades <strong>1 (Block trade)</strong>: For trade entry where type = Block </p><p><strong>2 (EFP)</strong>: For trade entry where type = EFRP </p><p><strong>99</strong> : For trade entry where type = Other</p> |
| 797  | CopyMsgIndicator     | Y        | Indicates drop copy, always “Y”                                                                                                                                                                                                                                                                                   |
| 570  | PreviouslyReported   | Y        | Always “N”                                                                                                                                                                                                                                                                                                        |
| 1003 | TradeId              | Y        | Trade Id (Match Id)                                                                                                                                                                                                                                                                                               |
| 552  | NoSides              | Y        | Always 1 (One Side)                                                                                                                                                                                                                                                                                               |
| 37   | OrderID              | N        | <p>Order Id<br>Only when TradeReportTransType = 0 &#x26; TrdType = 0</p>                                                                                                                                                                                                                                          |
| 41   | OrigClOrdID          | N        | <p>ClOrdID of the order<br>Only when TradeReportTransType = 0 &#x26; TrdType = 0</p>                                                                                                                                                                                                                              |
| 32   | LastQty              | N        | Trade amount                                                                                                                                                                                                                                                                                                      |
| 31   | LastPx               | N        | Trade price                                                                                                                                                                                                                                                                                                       |
| 55   | Symbol               | N        | Instrument symbol                                                                                                                                                                                                                                                                                                 |
| 54   | Side                 | N        | <p>1= Buy</p><p>2 = Sell</p>                                                                                                                                                                                                                                                                                      |
| 44   | Price                | N        | Order price                                                                                                                                                                                                                                                                                                       |
| 60   | TransactTime         | Y        | Event timestamp in microseconds in UTC Format YYYYmmDD-hh:mm:ss.000000                                                                                                                                                                                                                                            |
| 75   | TradeDate            | Y        | Trade date. Format: YYYYMMDD                                                                                                                                                                                                                                                                                      |
| 581  | AccountType          | N        | <p>Account type </p><p>1 = Client </p><p>3 = House</p>                                                                                                                                                                                                                                                            |
| +    | Parties              | N        | <p>This is party information related to the submitter of the request.<br>See <a href="trading-messages.md#new-parties-component">here </a>for more details </p>                                                                                                                                                   |
