# Market Data (NEW)

Supported Messages

### Client-Initiated Messages

| Message                                                                                                                                                                                    | MsgType | Usage                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | --------------------------------------------------------------------------------------------- |
| [Market Data Request](https://app.gitbook.com/o/okJjrZYa0IvDclrhKkAo/s/-M-yqzGakbR2s9rOe4nu/\~/changes/vh4Utk97DRg2Npf936du/fix-trading/market-data#market-data-request-msgtype-v)         | V       | Allows the client to request or manage subscriptions for market data for multiple instruments |
| [Security List Request](https://app.gitbook.com/o/okJjrZYa0IvDclrhKkAo/s/-M-yqzGakbR2s9rOe4nu/\~/changes/vh4Utk97DRg2Npf936du/fix-trading/market-data#security-list-request-msgtype-x)     | x       | Allows to request a list of instruments details that match the criteria provided              |
| [Security Status Request](https://app.gitbook.com/o/okJjrZYa0IvDclrhKkAo/s/-M-yqzGakbR2s9rOe4nu/\~/changes/vh4Utk97DRg2Npf936du/fix-trading/market-data#security-status-request-msgtype-e) | e       | Allows to request the trading status of an instrument                                         |

### Server Initiated Messages

| Message                                                                                                                                                                                          | MsgType | Usage                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | ------------------------------------------------------- |
| [Market Data Snapshot Full Refresh](market-data.md#market-data-snapshot-msgtype-w)                                                                                                               | W       | Used to return a snapshot of market data                |
| [Market Data Request Reject](https://app.gitbook.com/o/okJjrZYa0IvDclrhKkAo/s/-M-yqzGakbR2s9rOe4nu/\~/changes/vh4Utk97DRg2Npf936du/fix-trading/market-data#market-data-request-reject-msgtype-y) | Y       | Indicates that a Market Data Request has been rejected. |
| [Security List](https://app.gitbook.com/o/okJjrZYa0IvDclrhKkAo/s/-M-yqzGakbR2s9rOe4nu/\~/changes/vh4Utk97DRg2Npf936du/fix-trading/market-data#security-list-msgtype-y)                           | y       | Used to return a list of instruments and their details  |
| [Security Status](https://app.gitbook.com/o/okJjrZYa0IvDclrhKkAo/s/-M-yqzGakbR2s9rOe4nu/\~/changes/vh4Utk97DRg2Npf936du/fix-trading/market-data#security-status-msgtype-f)                       | f       | Used to return trading status of an instrument          |

### Request/ Response Messages

| Request                                                                                                                                                                                                                                    | Success Message                                         | Failure Message                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| <p>Market Data Request (V)</p><p></p><p>For ticker data use the below parameters:</p><ul><li>SubscriptionRequestType(263) = 0/1/2</li><li>MDUpdateType(265) = 0 (Full refresh)</li><li>MarketDepth(264) = 1(Top of book)</li></ul>         | <ul><li>Market Data Snapshot Full Refresh (W)</li></ul> | <p>One of the below:</p><ul><li>Market Data Request Reject (Y)</li><li>BusinessMessageReject (j)</li></ul><p></p> |
| <p>Market Data Request (V)</p><p></p><p>For Market by Price data use the below parameters:</p><ul><li>SubscriptionRequestType(263) = 0/1/2  </li><li>MDUpdateType(265) = 0 (Full refresh)</li><li>MarketDepth(264) = 5/10/20/100</li></ul> | <ul><li>Market Data Snapshot Full Refresh (W)</li></ul> | <p>One of the below:</p><ul><li>Market Data Request Reject (Y)</li><li>BusinessMessageReject (j)</li></ul>        |
| Security List Request (x)                                                                                                                                                                                                                  | <ul><li>Security List (y)</li></ul>                     | <p>One of the below:</p><ul><li>Security List (y) </li><li>BusinessMessageReject (j)</li><li>Reject (3)</li></ul> |
| Security Status Request (e)                                                                                                                                                                                                                | <ul><li>Security Status (f)</li></ul>                   | <p>One of the below:</p><ul><li>BusinessMessageReject (j)</li><li>Reject (3)</li></ul>                            |

## Messages Specifications

### Market Data Request _(MsgType = V)_

{% tabs %}
{% tab title="Ticker Data" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=132|35=V|49=Target|56=EXBERRY|34=13|52=20230125-12:48:59.637000|146=2|55=ABC|55=ECD|262=1674650939637|263=0|264=1|265=0|267=2|269=0|269=1|10=213|
```
{% endcode %}
{% endtab %}

{% tab title="Market By Price Data" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=137|35=V|49=MP1|56=EXBERRY|34=250|52=20230209-08:39:25.474000|146=2|55=INS1|55=INS2|262=1675931965474|263=1|264=5|265=0|267=2|269=0|269=1|10=060|
```
{% endcode %}
{% endtab %}
{% endtabs %}

| Tag | Name                    | Required | Description                                                                                                                                                                                                                                                                                                                                                |
| --- | ----------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 262 | MDReqID                 | Y        | Unique ID for this request.                                                                                                                                                                                                                                                                                                                                |
| 263 | SubscriptionRequestType | Y        | <p>Subscription Request Type. <br>0 = Snapshot<br>1 = Snapshot + Updates (Subscribe)<br>2 =  Disable previous Snapshot + Update Request (Unsubscribe)</p>                                                                                                                                                                                                  |
| 264 | MarketDepth             | N        | <p>Mandatory in case SubscriptionRequestType (263)  = 0 or 1</p><p></p><p>Market depth to return in a snapshot<br>values- 1(Top Of Book),5,10,20,100</p>                                                                                                                                                                                                   |
| 265 | MDUpdateType            | N        | <p>Mandatory in case  SubscriptionRequestType (263) = 1</p><p></p><p>Type of messages FIX sends when updating market data.<br>0 = Full refresh</p>                                                                                                                                                                                                         |
| 267 | NoMDEntryTypes          | Y        | <p>Mandatory in case SubscriptionRequestType (263)  = 0 or 1</p><p></p><p>Number of 269 (MDEntryType) fields in the request.</p>                                                                                                                                                                                                                           |
| 269 | MDEntryType             | Y        | <p>Mandatory in case SubscriptionRequestType (263)  = 0 or 1</p><p></p><p>Type of market data to request.</p><p>Available values: <br>0 = Bid</p><p>1 = Ask</p><p>2 = Trade</p><p>B = Trade volume</p><p>4 = Opening price</p><p>5 = Closing price</p><p>6 = Settlement price</p><p>7 = Trading session high price</p><p>8 = Trading session Low price</p> |
| 146 | NoRelatedSym            | Y        | Specifies the number of repeating instruments specified                                                                                                                                                                                                                                                                                                    |
| 55  | Symbol                  | Y        | <p>Mandatory in case SubscriptionRequestType (263)  = 0 or 1</p><p></p><p>Instrument symbol </p>                                                                                                                                                                                                                                                           |

### Market Data Request Reject _(MsgType = Y)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=103|35=Y|49=EXBERRY|56=Target|34=243|52=20230130-08:52:23.767763|262=1675068743722|281=0|58=Wrong Symbol(55)|10=226|
```
{% endcode %}

| Tag | Name           | Required | Description                                                                                                                                                                                                                                                                      |
| --- | -------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 262 | MDReqID        | Y        | Unique ID from the incoming request ID                                                                                                                                                                                                                                           |
| 58  | text           | N        | Reason for the rejection                                                                                                                                                                                                                                                         |
| 281 | MDReqRejReason | N        | <p>Reason for the rejection of a Market Data request.</p><p>0 = (Unknown symbol)</p><p>1 = (Duplicate MDReqID)</p><p>4 = (Unsupported SubscriptionRequestType)</p><p>5  = (Unsupported MarketDepth)</p><p>6 = (Unsupported MDUpdateType)</p><p>8 = (Unsupported MDEntryType)</p> |

### Market Data Snapshot_(MsgType = W)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=155|35=W|49=EXBERRY|56=Target|34=247|52=20230130-08:54:05.167612|262=1675068845109|55=INS1|779=20230130-07:00:00.000000|268=2|269=0|270=10|271=0.88|269=1|271=0|10=239|
```
{% endcode %}

| Tag   | Name           | Required | Description                                                                                                                                                                                                                                                   |
| ----- | -------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 262   | MDReqID        | Y        | Unique ID from the incoming request ID.                                                                                                                                                                                                                       |
| 55    | Symbol         | Y        | Instrument symbol                                                                                                                                                                                                                                             |
| 779   | LastUpdateTime | Y        | <p>Timestamp of last update to data item</p><p><br>Format YYYYmmDD-hh:mm:ss.000000  </p>                                                                                                                                                                      |
| 268   | NoMDEntries    | Y        | <p>Repeating group.</p><p>Number of market data entries in the message</p>                                                                                                                                                                                    |
| >269  | MDEntryType    | Y        | <p>Type of market data to request.</p><p>0 = Bid</p><p>1 = Ask</p><p>2 = Trade</p><p>B = Trade volume</p><p>4 = Opening price</p><p>5 = Closing price</p><p>6 = Settlement price</p><p>7 = Trading session high price</p><p>8 = Trading session Low price</p> |
| >270  | MDEntryPx      | N        | Price of the instrument associated with this entry.                                                                                                                                                                                                           |
| >271  | MDEntrySize    | N        | <p>Quantity associated with the related entry.<br>0 = Bid<br>1 = Ask<br>2 = Trade<br>B = Trade volume</p>                                                                                                                                                     |
| >1023 | MDPriceLevel   | N        | <p>Mandatory in case MarketDepth (264) != 1</p><p>Integer to convey the level of a bid or offer at a given price level.</p><p><br></p>                                                                                                                        |

### Security List Request _(MsgType = x)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=89|35=x|49=Target|56=EXBERRY|34=367|52=20230130-09:54:45.064000|263=0|320=1675072485064|559=4|10=214|
```
{% endcode %}

| Tag | Name                    | Required | Description                                                                                                                                                                                                                |
| --- | ----------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 320 | SecurityReqID           | Y        | Unique ID for this request                                                                                                                                                                                                 |
| 559 | SecurityListRequestType | Y        | <p>Type of Security List Request being made.</p><p>Available values: <br>4: All Securities (will return all instrument where status = Active</p>                                                                           |
| 263 | SubscriptionRequestType | N        | <p>Subscription Request Type. </p><p>Available values: <br>0 = Snapshot<br>1 = Snapshot + Updates (Subscribe)<br>2 =  Disable previous Snapshot + Update Request (Unsubscribe)<br><br>Default value if not sent is  0 </p> |

### Security List _(MsgType = y)_

{% tabs %}
{% tab title="Spot Instrument" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=187|35=y|49=EXBERRY|56=Target|34=370|52=20230130-09:54:45.085434|320=1675072485064|560=0|893=N|146=1|55=INS1|48=1013|22=99|965=1|2576=1|1786=0|562=1|1140=99999|107=spread test|167=SPOT|15=USD|10=233|
```
{% endcode %}
{% endtab %}

{% tab title="Future Instrument" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=269|35=y|49=EXBERRY|56=Target|34=2|52=20230202-13:04:19.939497|320=1675343059898|560=0|893=N|146=1|55=INS|48=20|22=99|965=1|2576=1|1786=0|562=1|1140=9999|107=test1|167=FUT|762=Financial futures (F)|15=USE|225=20230204|20010=13:03:27|541=20230206|1079=13:03:43|231=2|1193=C|10=035|
```
{% endcode %}
{% endtab %}

{% tab title="Option Instrument" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=284|35=y|49=EXBERRY|56=Target|34=13|52=20230202-13:08:03.320633|320=1675343283343|560=0|893=N|146=1|55=INS|48=20|22=99|965=1|2576=1|1786=0|562=1|1140=9999|107=test1|167=OPT|762=Call options (C)|201=1|15=USE|225=20230204|20010=13:03:27|541=20230206|1079=13:03:43|231=2|1193=C|1194=1|202=6|10=101|
```
{% endcode %}
{% endtab %}

{% tab title="Invalid Request Rejection" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=90|35=y|49=EXBERRY|56=Terget|34=364|52=20230130-09:52:24.863054|320=1675072344844|560=1|1607=3|10=002|
```
{% endcode %}
{% endtab %}
{% endtabs %}

| Tag    | Name                     | Required | Description                                                                                                                                                                                                                                                       |
| ------ | ------------------------ | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 320    | SecurityReqID            | Y        | Unique Id for the Security Definition Request.                                                                                                                                                                                                                    |
| 560    | SecurityRequestResult    | Y        | <p>The results returned to a Security Request message</p><p><br></p><p>0 = Valid request</p><p>1 = Invalid or unsupported request</p><p></p>                                                                                                                      |
| 1607   | SecurityRejectReason     | N        | <p>Identifies the reason a security definition request is being rejected.<br>(will be sent only when SecurityRequestResult(560) = 1)</p>                                                                                                                          |
| 893    | LastFragment             | N        | <p>Indicates whether this message is the last in a sequence of messages on the current snapshot<br></p><p>N = Not Last Message</p><p>Y = Last Message (of the snapshot)</p><p></p><p>Note: this field will return only from the snapshot (not for updates)</p>    |
| 146    | NoRelatedSym             | Y        | <p>Specifies the number of repeating symbols specified</p><p><br>Note: In case there is no data and the request was valid, this field won't be returned</p>                                                                                                       |
| 55     | Symbol                   | Y        | Instrument Symbol                                                                                                                                                                                                                                                 |
| 48     | SecurityID               | Y        | Permanent instrument identifier, in case symbol is changed after subscription, you can identify the instrument using this field.                                                                                                                                  |
| 22     | SecurityIDSource         | Y        | <p>Identifies class or source of the SecurityID(48) value</p><p><br>99 = Custom</p>                                                                                                                                                                               |
| 107    | SecurityDesc             | Y        | Description                                                                                                                                                                                                                                                       |
| 167    | SecurityType             | N        | <p>Instrument category, available values: </p><p>FUT (Future)</p><p>OPT (Option)</p><p>SPOT</p><p>MLEG (Multileg Instrument)</p><p>CS (Common Stock)</p><p>Other</p>                                                                                              |
| 762    | SecuritySubType          | N        | <p>The Sub-Catagory of the instrument</p><p>Available values: </p><p></p><p>Foreign exchange (F)</p><p>Commodities (T)</p><p>Financial futures (F)</p><p>Commodities futures (C)</p><p>Call options (C)</p><p>Put options (P)</p><p>Other (miscellaneous) (M)</p> |
| 15     | Currency                 | Y        | Quote currency                                                                                                                                                                                                                                                    |
| 2576   | InstrumentPricePrecision | Y        | Price precision, number of decimals allowed in price field                                                                                                                                                                                                        |
| 965    | SecurityStatus           | Y        | <p>Indicates the current state of the instrument. </p><p><br>2 = Inactive (applicable only in case instrument was disabled after subscription)</p><p> 1 = Active</p>                                                                                              |
| 207    | SecurityExchange         | N        | Market identifier codes (MIC)                                                                                                                                                                                                                                     |
| 461    | CFICode                  | N        | Classification of Financial Instruments codes (CFI)                                                                                                                                                                                                               |
| 541    | MaturityDate             | N        | <p>Trading is allowed up to this time.</p><p>format: YYYYMMDD</p>                                                                                                                                                                                                 |
| 1079   | MaturityTime             | N        | <p>Trading is allowed up to this time.</p><p>Format is HH:MM:SSZ </p>                                                                                                                                                                                             |
| 225    | IssueDate                | N        | <p>Trading is allowed from this time</p><p> format: YYYYMMDD</p>                                                                                                                                                                                                  |
| 220010 | Custom tag               | N        | Trading is allowed from this timeFormat is HH:MM:SSZ                                                                                                                                                                                                              |
| 201    | PutOrCall                | N        | <p>In case SecurityType(167) = OPT </p><p>Case subCategory :</p><p>Call options (C)   = 1</p><p>Put options (P) = 0</p><p>Other (miscellaneous) (M) = 2</p>                                                                                                       |
| 231    | ContractMultiplier       | N        | Contract size                                                                                                                                                                                                                                                     |
| 1193   | SettlMethod              | N        | <p>Settlement method for a contract or instrument.</p><p><br>C = Cash settlement required<br>P = Physical settlement required<br>E=Election at exercise</p>                                                                                                       |
| 1194   | ExerciseStyle            | N        | <p>Type of exercise of a derivatives security.</p><p><br>0 = European<br>1 = American<br>2 = Bermuda<br>99 = Other</p>                                                                                                                                            |
| 202    | StrikePrice              | N        | Strike                                                                                                                                                                                                                                                            |
| 1148   | LowLimitPrice            | N        | Min price                                                                                                                                                                                                                                                         |
| 1149   | HighLimitPrice           | N        | Max price                                                                                                                                                                                                                                                         |
| 562    | MinTradeVol              | Y        | The minimum order quantity                                                                                                                                                                                                                                        |
| 1140   | MaxTradeVol              | Y        | The maximum order quantity                                                                                                                                                                                                                                        |
| 1786   | TradeVolType             | Y        | <p>Define the type of trade volume applicable for the MinTradeVol(562) and MaxTradeVol(1140)</p><p><br></p><p>0 = Number of units</p>                                                                                                                             |

### Security Status Request _(MsgType = e)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=92|35=e|49=Target|56=EXBERRY|34=407|52=20230130-10:14:40.763000|55=INS1|263=0|324=1675073680763|10=036|
```
{% endcode %}

| Tag | Name                    | Required | Description                                                                                                                                                                                       |
| --- | ----------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 324 | SecurityStatusReqID     | Y        | Unique ID for this security request.                                                                                                                                                              |
| 55  | Symbol                  | Y        | Instrument symbol                                                                                                                                                                                 |
| 263 | SubscriptionRequestType | Y        | <p>Subscription Request Type. <br>0 = Snapshot<br>1 = Snapshot + Updates (Subscribe)<br>2 =  Disable previous Snapshot + Update Request (Unsubscribe)<br><br>Default value if not sent is  0 </p> |

### Security Status _(MsgType = f)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=94|35=f|49=EXBERRY|56=Target|34=412|52=20230130-10:14:40.781401|324=1675073680763|55=INS1|326=100|10=137|
```
{% endcode %}

| Tag | Name                    | Required | Description                                                                                                                                                      |
| --- | ----------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 324 | SecurityStatusReqID     | Y        | Unique ID for this security request.                                                                                                                             |
| 55  | Symbol                  | Y        | Instrument symbol                                                                                                                                                |
| 326 | SubscriptionRequestType | Y        | <p>Represents a specific market status for security.</p><p><br>100 = Trading<br>101 = Closed<br>102 = Auction call<br>103 = Auction crossing<br>104 = Halted</p> |
