# Trading Messages

## Supported Messages

### Client Initiated Messages

| Message                                                                              | MsgType | Usage                                                                     |
| ------------------------------------------------------------------------------------ | ------- | ------------------------------------------------------------------------- |
| [NewOrderSingle](trading-messages.md#newordersingle-msgtype-d)                       | D       | Allows the MP to submit a new order                                       |
| [OrderCancelRequest](trading-messages.md#ordercancelrequest-msgtype-f)               | F       | Allows the MP to cancel an active order                                   |
| [OrderCancelReplaceRequest](trading-messages.md#ordercancelreplacerequest-msgtype-g) | G       | Allows the MP to [modify ](../trading-api.md#modifyorder)an active order  |
| [OrderMassCancelRequest](trading-messages.md#ordermasscancelrequest-msgtype-q)       | q       | Allows the MP to mass cancel all active orders for a specific instrument  |

### Server Initiated Messages

| Message                                                                         | MsgType | Usage                                                                                     |
| ------------------------------------------------------------------------------- | ------- | ----------------------------------------------------------------------------------------- |
| [Execution Report](trading-messages.md#executionreport-msgtype-8)               | 8       | Indicates the result of order command.                                                    |
| [Order Mass Cancel Report](trading-messages.md#ordermasscancelreport-msgtype-r) | r       | Indicates the result of the mass cancel command.                                          |
| [Order Cancel Reject](trading-messages.md#ordercancelreject-msgtype-9)          | 9       | Indicates that an order cancel request or order cancel/replace request has been rejected. |
| [Business Message Reject](trading-messages.md#businessmessagereject-msgtype-j)  | j       | Indicates that an application message could not be processed                              |

### Request/ Response Messages

| Message                                                                                 | Success Message                                                                              | Failure Message                                                                                       |
| --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| [New Order Single](trading-messages.md#newordersingle-msgtype-d)                        | Execution Report                                                                             | <p>One of the below:</p><ul><li>ExecutionReport</li><li>BusinessReject</li><li>Reject</li></ul>       |
| [Order Cancel Request](trading-messages.md#ordercancelrequest-msgtype-f)                | Execution Report                                                                             | <p>One of the below:</p><ul><li>OrderCancelReject</li><li>BusinessReject</li><li>Reject</li></ul>     |
| [Order Mass Cancel Request](trading-messages.md#ordermasscancelrequest-msgtype-q)       | <p>Both:</p><ul><li>Execution Report- Order Level</li><li>Order Mass Cancel Report</li></ul> | <p>One of the below:</p><ul><li>OrderMassCancelReport</li><li>BusinessReject</li><li>Reject</li></ul> |
| [Order Cancel Replace Request](trading-messages.md#ordercancelreplacerequest-msgtype-g) | Execution Report                                                                             | <p>One of the below:</p><ul><li>OrderCancelReject</li><li>BusinessReject</li><li>Reject</li></ul>     |

### Order Lifecycle Scenarios

The below scenarios summarize the trading actions that are support with the corresponded Execution Report.

| Scenario                                                                                           | ExecType(150)                              | OrdStatus(39)                                                 |
| -------------------------------------------------------------------------------------------------- | ------------------------------------------ | ------------------------------------------------------------- |
| Order Rejected                                                                                     | 8 (Rejected)                               | 8 (Rejected)                                                  |
| Order accepted and order is fully resting on the book                                              | 0 (New)                                    | 0 (New)                                                       |
| Order accepted and order is fully executed immediately                                             | F (Trade)                                  | 2 (Filled)                                                    |
| Order accepted and order is partially executed immediately and the residual is resting on the book | <p>F (Trade) &#x26;</p><p>0 (New)</p>      | <p>1 (Partially filled) &#x26;</p><p>1 (Partially filled)</p> |
| Order accepted and order is partially executed immediately and the residual is cancelled (IOC)     | <p>F (Trade) &#x26;</p><p>4 (Canceled)</p> | <p>1 (Partially filled) &#x26;</p><p>4 (Canceled)</p>         |
| Order with immediate TIF (IOC, FOK) that failed to be executed                                     | 4 (Canceled)                               | 4 (Canceled)                                                  |
| GTD order that has expired                                                                         | 4 (Canceled)                               | 4 (Canceled)                                                  |
| Order cancelled                                                                                    | 4 (Canceled)                               | 4 (Canceled)                                                  |
| Order modified                                                                                     | 5 (Replaced)                               | <p>0 (New) OR</p><p>1 (Partially filled)</p>                  |

## Messages Specifications

### NewOrderSingle _(MsgType = D)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=162|35=D|49=MP1|56=EXBERRY|34=456|52=20221123-14:57:43.764000|11=1669215463763|38=1|40=2|44=1|54=2|55=INS1|59=1|453=2|448=123456|447=D|452=38|448=ABCD|447=D|452=12|10=010
```
{% endcode %}

| Tag                                              | Name        | Required | Description                                                                                                                                                 |
| ------------------------------------------------ | ----------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 11                                               | ClOrdID     | Y        | Client Order ID, always growing Number                                                                                                                      |
| 55                                               | Symbol      | Y        | Instrument symbol                                                                                                                                           |
| 54                                               | Side        | Y        | <p>1= Buy<br>2 = Sell</p>                                                                                                                                   |
| 38                                               | OrderQty    | Y        | Quantity                                                                                                                                                    |
| 40                                               | OrdType     | Y        | <p>Order type:<br>1 = Market<br>2 = Limit</p>                                                                                                               |
| 44                                               | Price       | N        | <p>Order price<br>Required for limit orders</p>                                                                                                             |
| 59                                               | TimeInForce | Y        | <p>Time in force:<br>1 = GTC<br>3 = IOC<br>4 = FOK<br>6 = GTD<br>B = GFA (in Exberry this is called GAA)</p>                                                |
| 126                                              | ExpireTime  | N        | <p>Expiry time in seconds in GMT</p><p>Format: YYYYMMDD-hh:mm:ss</p><p>Sample: 20201225-09:30:55</p><p>Required if TimeInForce = GTD</p>                    |
| 20001                                            | Custom tag  | N        | User Id (20 chars length, \[a-z,A-Z,0-9])                                                                                                                   |
| 20002                                            | Custom tag  | N        | <p>Account Id<br>Positive integer and less than or equal to max Integer (2147483647)</p>                                                                    |
| <mark style="color:blue;">NEW</mark>581          | AccountType | N        | <p>Account type </p><p>1 = Client </p><p>3 = House</p>                                                                                                      |
| <p><mark style="color:blue;">NEW</mark><br>+</p> | Parties     | N        | <p>This is party information related to the submitter of the request.<br>See <a href="trading-messages.md#parties-component">here </a>for more details </p> |

### <mark style="color:blue;">NEW</mark> Parties Component

Note: On server initiated massages (like ExecutionReport (8) or TradeCaptureReport (AE)),  MP name will always be added as additional party with the below parameters: \
\- 448 (PartyID) = MP Name \
\- 452 (PartyRole) = 7 (Entering Firm) \
\- 447 (PartyIDSource) = D (Custom)

| Tag | Name          | Required | Description                                                                         |
| --- | ------------- | -------- | ----------------------------------------------------------------------------------- |
| 453 | NoPartyIDs    | N        | Number of parties                                                                   |
| 448 | PartyID       | N        | <p>Required if NoPartyIDs(453) > 0. <br>Identification of the party.</p>            |
| 447 | PartyIDSource | N        | <p>Required if NoPartyIDs(453) > 0. <br>Used to identify classification source.</p> |
| 452 | PartyRole     | N        | <p>Required if NoPartyIDs(453) > 0. </p><p>Identifies the type of PartyID(448).</p> |



### OrderCancelRequest _(MsgType = F)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=89|35=F|49=MP1|56=EXBERRY|34=85|52=20221124-08:56:48.583000|11=1669280208583|37=4|55=INS1|10=144|
```
{% endcode %}

| Tag | Name        | Required | Description                                                                          |
| --- | ----------- | -------- | ------------------------------------------------------------------------------------ |
| 37  | OrderID     | N        | <p>Order Id to be cancelled</p><p>Mandatory if OrigClOrdID was not specified</p>     |
| 41  | OrigClOrdID | N        | <p>Original ClOrdID to be cancelled</p><p>Mandatory if OrderID was not specified</p> |
| 11  | ClOrdID     | Y        | Unique ID of cancel request                                                          |
| 55  | Symbol      | Y        | Instrument symbol                                                                    |

### OrderCancelReplaceRequest _(MsgType = G)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=97|35=G|49=MP1|56=EXBERRY|34=62|52=20221127-12:06:35.980000|11=1669550795980|37=8|38=0.12|55=INS1|10=251|
```
{% endcode %}

| Tag | Name     | Required | Description                                              |
| --- | -------- | -------- | -------------------------------------------------------- |
| 37  | OrderID  | Y        | Order Id to be modified                                  |
| 11  | ClOrdID  | Y        | Unique ID of cancel/replace request                      |
| 55  | Symbol   | Y        | Instrument symbol                                        |
| 38  | OrderQty | Y        | Quantity to apply on order (less than original quantity) |

### OrderMassCancelRequest _(MsgType = q)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=90|35=q|49=MP1|56=EXBERRY|34=12|52=20221124-09:32:09.451000|11=1669282329451|55=INS1|530=1|10=198|
```
{% endcode %}

| Tag | Name                  | Required | Description                      |
| --- | --------------------- | -------- | -------------------------------- |
| 11  | ClOrdID               | Y        | Unique ID of mass cancel request |
| 55  | Symbol                | Y        | Instrument symbol                |
| 530 | MassCancelRequestType | Y        | 1 = Cancel orders for a security |

### ExecutionReport _(MsgType = 8)_

{% tabs %}
{% tab title="New" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=255|35=8|49=EXBERRY|56=MP1|34=5|52=20221127-11:42:36.138119|17=37259|55=yael1|37=5|41=1669549352750|150=0|40=2|39=0|54=2|59=1|44=100.0|38=1.0|14=0.0|151=1.0|60=20221127-11:42:36.130166|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=MP1|447=D|452=7|10=221|
```
{% endcode %}
{% endtab %}

{% tab title="Rejected" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=121|35=8|49=EXBERRY|56=MP1|34=10|52=20221127-11:45:00.373231|17=37260|150=8|37=-1|39=8|103=1010|58=Instrument FIX not found|10=169|
```
{% endcode %}
{% endtab %}

{% tab title="Trade" %}
{% code overflow="wrap" %}
```
 8=FIXT.1.1|9=290|35=8|49=EXBERRY|56=MP1|34=47|52=20221127-12:00:27.256013|17=37266|55=yael1|37=7|41=1669550423866|150=F|40=2|39=2|54=1|59=1|44=100.0|38=1.0|14=1.0|151=0.0|31=100.0|32=1.0|880=4|60=20221127-12:00:27.243980|75=20221127|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=MP1|447=D|452=7|10=049|
```
{% endcode %}
{% endtab %}

{% tab title="Canceled" %}
{% code overflow="wrap" %}
```
 8=FIXT.1.1|9=254|35=8|49=EXBERRY|56=MP1|34=14|52=20221127-11:46:41.698965|17=37261|55=yael1|37=5|41=1669549352750|150=4|40=2|39=4|54=2|59=1|44=100.0|38=1.0|14=0.0|151=0|60=20221127-11:46:41.692550|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=MP1|447=D|452=7|10=204|
```
{% endcode %}
{% endtab %}

{% tab title="Replaced" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=258|35=8|49=EXBERRY|56=MP1|34=64|52=20221127-12:06:39.375990|17=37269|55=yael1|37=8|41=1669550567403|150=5|40=2|39=0|54=2|59=1|44=10.0|38=1.00|14=0.00|151=0.12|60=20221127-12:06:39.364379|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=MP1|447=D|452=7|10=156|
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Order Status" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=253|35=8|49=EXBERRY|56=Test1|34=18|52=20221206-13:08:55.829048|17=0|584=1670332136072|912=Y|150=I|55=Test1Feb23|37=261|41=1670365062864|40=2|39=0|54=2|59=1|44=5.74|38=1|14=0|151=1|31=0|32=0|60=20221130-12:53:35.853912|20001=User|453=1|448=Test1|447=D|452=7|10=149|
```
{% endcode %}
{% endtab %}
{% endtabs %}

| Tag                                                     | Name             | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------------------------------------- | ---------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 37                                                      | OrderID          | Y        | <p>Order Id<br>In case of rejection message will be -1 </p>                                                                                                                                                                                                                                                                                                                                                                                  |
| 41                                                      | OrigClOrdID      | N        | ClOrdID of the order                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 17                                                      | ExecID           | Y        | <p>Unique identifier of execution report<br><mark style="color:blue;">NEW</mark> 0 = Case of Order Mass Status Request (AF) response</p>                                                                                                                                                                                                                                                                                                     |
| 880                                                     | TrdMatchID       | N        | Match Id - only in case ExecType = Trade                                                                                                                                                                                                                                                                                                                                                                                                     |
| 150                                                     | ExecType         | Y        | <p>Describes the purpose of the execution report:</p><p>8 = [Rejected]- When request is rejected</p><p>0 = [New]- When order added to the book</p><p>F = [Trade]- When order was executed (Fully or partially)</p><p>4 = [Canceled] - When order canceled from book (also in case of GTD expired)</p><p>5 = [Replaced] - when order was modified<br><mark style="color:blue;">NEW</mark> I = [Order Status] - When mass status requested</p> |
| 39                                                      | OrdStatus        | Y        | <p>Current order status (after this message):</p><p>8 = [Rejected]- For failure to place order</p><p>0 = [New]- For success new/ modified order that is fully resting on the book</p><p>4 = [Canceled]- For success cancel order , also in case of GTD expired</p><p>1 = [PartiallyFilled]- For partially executed order:</p><p><code>(Order State = Active) AND (FilledQty>0)</code></p><p>2 = [Filled]- For fully executed order</p>       |
| 58                                                      | Text             | N        | In case of rejection, specifies the error message                                                                                                                                                                                                                                                                                                                                                                                            |
| 103                                                     | OrdRejReason     | N        | In case of rejection, specifies the exberry error code                                                                                                                                                                                                                                                                                                                                                                                       |
| 38                                                      | OrderQty         | N        | Order original quantity                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 14                                                      | CumQty           | N        | Order total Filled Quantity                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 151                                                     | LeavesQty        | N        | <p>Remaining open quantity</p><p><code>IF OrdStatus(39) =</code><br><code>[4(Canceled) or C(Expired) or 8(Rejected)]</code><br><code>Then LeavesQty(151) = 0</code></p><p><code>Else LeavesQty(151) = OrderQty(38) - CumQty(14)</code></p>                                                                                                                                                                                                   |
| 32                                                      | LastQty          | N        | <p>In case ExecType = Trade:<br>Executed amount on current fill only</p>                                                                                                                                                                                                                                                                                                                                                                     |
| 31                                                      | LastPx           | N        | <p>In case ExecType = Trade:</p><p>Executed price on current fill only</p>                                                                                                                                                                                                                                                                                                                                                                   |
| 55                                                      | Symbol           | N        | Instrument symbol                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 54                                                      | Side             | N        | <p>1= Buy</p><p>2 = Sell</p>                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 40                                                      | OrdType          | N        | <p>Order type:</p><p>1 = Market</p><p>2 = Limit</p>                                                                                                                                                                                                                                                                                                                                                                                          |
| 44                                                      | Price            | N        | Order price                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 59                                                      | TimeInForce      | N        | <p>Time in force:</p><p>1 = GTC</p><p>3 = IOC</p><p>4 = FOK</p><p>6 = GTD</p><p>B = GFA (in Exberry this is called GAA)</p>                                                                                                                                                                                                                                                                                                                  |
| 126                                                     | ExpireTime       | N        | <p>Expiry time in seconds in GMT</p><p>Format: YYYYMMDD-hh:mm:ss</p>                                                                                                                                                                                                                                                                                                                                                                         |
| 20001                                                   | Custom tag       | N        | User Id                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 20002                                                   | Custom tag       | N        | Account Id                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| <mark style="color:blue;">NEW</mark> 581                | AccountType      | N        | <p>Account type </p><p>1 = Client </p><p>3 = House</p>                                                                                                                                                                                                                                                                                                                                                                                       |
| <p><mark style="color:blue;">NEW</mark> <br>+</p>       | Parties          | N        | <p>This is party information related to the submitter of the request.<br>See <a href="trading-messages.md#parties-component">here </a>for more details </p>                                                                                                                                                                                                                                                                                  |
| <mark style="color:blue;">NEW</mark>  912               | LastRptRequested | N        | <p>Will be included only in case ExecType (150) = l (Order Status)<br>Identify if this is the last record<br>Y =  for the last record</p><p>N = for all the rest</p>                                                                                                                                                                                                                                                                         |
| <p><mark style="color:blue;">NEW</mark>  </p><p>584</p> | MassStatusReqID  | N        | Will be included only in case ExecType (150) = I (Order Status), from request                                                                                                                                                                                                                                                                                                                                                                |

### OrderMassCancelReport _(MsgType = r)_

{% tabs %}
{% tab title="Accepted" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=87|35=r|49=EXBERRY|56=MP1|34=20|52=20221127-11:48:39.808764|530=1|531=1|533=0|1369=37263|10=066|
```
{% endcode %}
{% endtab %}

{% tab title="Rejected" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=118|35=r|49=EXBERRY|56=MP1|34=24|52=20221124-09:37:01.311422|530=1|531=0|532=1010|58=Instrument FIX not found|1369=37256|10=126|
```
{% endcode %}
{% endtab %}
{% endtabs %}

| Tag  | Name                   | Required | Description                                                                |
| ---- | ---------------------- | -------- | -------------------------------------------------------------------------- |
| 530  | MassCancelRequestType  | Y        | 1 = Cancel orders for a security                                           |
| 531  | MassCancelResponse     | Y        | <p>0 = Cancel request rejected</p><p>1 = Cancel request succeeded</p>      |
| 532  | MassCancelRejectReason | N        | In case of reject, specify the Exberry error code                          |
| 533  | TotalAffectedOrders    | N        | In case of success, number of orders affected from the mass cancel request |
| 1369 | MassActionReportID     | Y        | Unique ID of mass cancel request                                           |
| 58   | Text                   | N        | In case of reject, specify the error message                               |

### OrderCancelReject _(MsgType = 9)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=125|35=9|49=EXBERRY|56=MP1|34=27|52=20221127-11:52:00.728920|37=NONE|434=1|102=1100|39=8|58=Order not found for that instrument|10=081
```
{% endcode %}

| Tag | Name             | Required | Description                                                                                        |
| --- | ---------------- | -------- | -------------------------------------------------------------------------------------------------- |
| 37  | OrderID          | Y        | orderId that was sent to be cancelled/ modified. Always “NONE”                                     |
| 39  | OrdStatus        | Y        |  8 = Rejected                                                                                      |
| 102 | CxlRejReason     | Y        | Specify the error code                                                                             |
| 434 | CxlRejResponseTo | Y        | <p>Original request is :</p><p>1 = Order cancel request</p><p>2 = Order cancel/replace request</p> |
| 58  | Text             | Y        | Specify the error message                                                                          |

### BusinessMessageReject _(MsgType = j)_

| Tag | Name                 | Required | Description                                                                                                       |
| --- | -------------------- | -------- | ----------------------------------------------------------------------------------------------------------------- |
| 380 | BusinessRejectReason | Y        | Specify the error code per [FIX specifications](https://fiximate.fixtrading.org/en/FIX.Latest\_EP266/tag380.html) |
| 58  | Text                 | Y        | Specify the error message                                                                                         |
| 45  | RefSeqNum            | Y        | Reference message sequence number                                                                                 |
| 372 | RefMsgType           | Y        | The MsgType (35) of the FIX message being referenced                                                              |
