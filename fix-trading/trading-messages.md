# Trading Messages

## Supported Messages

### Client Initiated Messages

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

| Tag | Name          | Required | Description                                                                         |
| --- | ------------- | -------- | ----------------------------------------------------------------------------------- |
| 453 | NoPartyIDs    | N        | Number of parties                                                                   |
| 448 | PartyID       | N        | <p>Required if NoPartyIDs(453) > 0. <br>Identification of the party.</p>            |
| 447 | PartyIDSource | N        | <p>Required if NoPartyIDs(453) > 0. <br>Used to identify classification source.</p> |
| 452 | PartyRole     | N        | <p>Required if NoPartyIDs(453) > 0. </p><p>Identifies the type of PartyID(448).</p> |



### OrderCancelRequest _(MsgType = F)_

| Tag | Name        | Required | Description                                                                          |
| --- | ----------- | -------- | ------------------------------------------------------------------------------------ |
| 37  | OrderID     | N        | <p>Order Id to be cancelled</p><p>Mandatory if OrigClOrdID was not specified</p>     |
| 41  | OrigClOrdID | N        | <p>Original ClOrdID to be cancelled</p><p>Mandatory if OrderID was not specified</p> |
| 11  | ClOrdID     | Y        | Unique ID of cancel request                                                          |
| 55  | Symbol      | Y        | Instrument symbol                                                                    |

### OrderCancelReplaceRequest _(MsgType = G)_

| Tag | Name     | Required | Description                                              |
| --- | -------- | -------- | -------------------------------------------------------- |
| 37  | OrderID  | Y        | Order Id to be modified                                  |
| 11  | ClOrdID  | Y        | Unique ID of cancel/replace request                      |
| 55  | Symbol   | Y        | Instrument symbol                                        |
| 38  | OrderQty | Y        | Quantity to apply on order (less than original quantity) |

### OrderMassCancelRequest _(MsgType = q)_

| Tag | Name                  | Required | Description                      |
| --- | --------------------- | -------- | -------------------------------- |
| 11  | ClOrdID               | Y        | Unique ID of mass cancel request |
| 55  | Symbol                | Y        | Instrument symbol                |
| 530 | MassCancelRequestType | Y        | 1 = Cancel orders for a security |

### ExecutionReport _(MsgType = 8)_

| Tag                                               | Name         | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------------------------- | ------------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 37                                                | OrderID      | Y        | Order Id                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 41                                                | OrigClOrdID  | N        | ClOrdID of the order                                                                                                                                                                                                                                                                                                                                                                                                                   |
| 17                                                | ExecID       | Y        | Unique identifier of execution report                                                                                                                                                                                                                                                                                                                                                                                                  |
| 880                                               | TrdMatchID   | N        | Match Id - only in case ExecType = Trade                                                                                                                                                                                                                                                                                                                                                                                               |
| 150                                               | ExecType     | Y        | <p>Describes the purpose of the execution report:</p><p>8 = [Rejected]- When request is rejected</p><p>0 = [New]- When order added to the book</p><p>F = [Trade]- When order was executed (Fully or partially)</p><p>4 = [Canceled] - When order canceled from book (also in case of GTD expired)</p><p>5 = [Replaced] - when order was modified</p>                                                                                   |
| 39                                                | OrdStatus    | Y        | <p>Current order status (after this message):</p><p>8 = [Rejected]- For failure to place order</p><p>0 = [New]- For success new/ modified order that is fully resting on the book</p><p>4 = [Canceled]- For success cancel order , also in case of GTD expired</p><p>1 = [PartiallyFilled]- For partially executed order:</p><p><code>(Order State = Active) AND (FilledQty>0)</code></p><p>2 = [Filled]- For fully executed order</p> |
| 58                                                | Text         | N        | In case of rejection, specifies the error message                                                                                                                                                                                                                                                                                                                                                                                      |
| 103                                               | OrdRejReason | N        | In case of rejection, specifies the exberry error code                                                                                                                                                                                                                                                                                                                                                                                 |
| 38                                                | OrderQty     | N        | Order original quantity                                                                                                                                                                                                                                                                                                                                                                                                                |
| 14                                                | CumQty       | N        | Order total Filled Quantity                                                                                                                                                                                                                                                                                                                                                                                                            |
| 151                                               | LeavesQty    | N        | <p>Remaining open quantity</p><p><code>IF OrdStatus(39) =</code><br><code>[4(Canceled) or C(Expired) or 8(Rejected)]</code><br><code>Then LeavesQty(151) = 0</code></p><p><code>Else LeavesQty(151) = OrderQty(38) - CumQty(14)</code></p>                                                                                                                                                                                             |
| 32                                                | LastQty      | N        | <p>In case ExecType = Trade:<br>Executed amount on current fill only</p>                                                                                                                                                                                                                                                                                                                                                               |
| 31                                                | LastPx       | N        | <p>In case ExecType = Trade:</p><p>Executed price on current fill only</p>                                                                                                                                                                                                                                                                                                                                                             |
| 55                                                | Symbol       | N        | Instrument symbol                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 54                                                | Side         | N        | <p>1= Buy</p><p>2 = Sell</p>                                                                                                                                                                                                                                                                                                                                                                                                           |
| 40                                                | OrdType      | N        | <p>Order type:</p><p>1 = Market</p><p>2 = Limit</p>                                                                                                                                                                                                                                                                                                                                                                                    |
| 44                                                | Price        | N        | Order price                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 59                                                | TimeInForce  | N        | <p>Time in force:</p><p>1 = GTC</p><p>3 = IOC</p><p>4 = FOK</p><p>6 = GTD</p><p>B = GFA (in Exberry this is called GAA)</p>                                                                                                                                                                                                                                                                                                            |
| 126                                               | ExpireTime   | N        | <p>Expiry time in seconds in GMT</p><p>Format: YYYYMMDD-hh:mm:ss</p>                                                                                                                                                                                                                                                                                                                                                                   |
| 20001                                             | Custom tag   | N        | User Id                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 20002                                             | Custom tag   | N        | Account Id                                                                                                                                                                                                                                                                                                                                                                                                                             |
| <mark style="color:blue;">NEW</mark> 581          | AccountType  | N        | <p>Account type </p><p>1 = Client </p><p>3 = House</p>                                                                                                                                                                                                                                                                                                                                                                                 |
| <p><mark style="color:blue;">NEW</mark> <br>+</p> | Parties      | N        | <p>This is party information related to the submitter of the request.<br>See <a href="trading-messages.md#parties-component">here </a>for more details </p>                                                                                                                                                                                                                                                                            |

### OrderMassCancelReport _(MsgType = r)_

| Tag  | Name                   | Required | Description                                                                |
| ---- | ---------------------- | -------- | -------------------------------------------------------------------------- |
| 530  | MassCancelRequestType  | Y        | 1 = Cancel orders for a security                                           |
| 531  | MassCancelResponse     | Y        | <p>0 = Cancel request rejected</p><p>1 = Cancel request succeeded</p>      |
| 532  | MassCancelRejectReason | N        | In case of reject, specify the Exberry error code                          |
| 533  | TotalAffectedOrders    | N        | In case of success, number of orders affected from the mass cancel request |
| 1369 | MassActionReportID     | Y        | Unique ID of mass cancel request                                           |
| 58   | Text                   | N        | In case of reject, specify the error message                               |

### OrderCancelReject _(MsgType = 9)_

| Tag | Name             | Required | Description                                                                                        |
| --- | ---------------- | -------- | -------------------------------------------------------------------------------------------------- |
| 37  | OrderID          | Y        | orderId that was sent to be cancelled/ modified. Will be “NONE” if the order is unknown.           |
| 434 | CxlRejResponseTo | Y        | <p>Original request is :</p><p>1 = Order cancel request</p><p>2 = Order cancel/replace request</p> |
| 58  | Text             | Y        | Specify the error message                                                                          |
| 102 | CxlRejReason     | Y        | Specify the error code                                                                             |

### BusinessMessageReject _(MsgType = j)_

| Tag | Name                 | Required | Description                                                                                                       |
| --- | -------------------- | -------- | ----------------------------------------------------------------------------------------------------------------- |
| 380 | BusinessRejectReason | Y        | Specify the error code per [FIX specifications](https://fiximate.fixtrading.org/en/FIX.Latest\_EP266/tag380.html) |
| 58  | Text                 | Y        | Specify the error message                                                                                         |
| 45  | RefSeqNum            | Y        | Reference message sequence number                                                                                 |
| 372 | RefMsgType           | Y        | The MsgType (35) of the FIX message being referenced                                                              |
