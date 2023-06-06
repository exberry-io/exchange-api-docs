# Trading Messages

## Supported Messages

### Client Initiated Messages

<table><thead><tr><th width="272">Message</th><th width="104.33333333333331">MsgType</th><th width="394.6666666666667">Usage</th></tr></thead><tbody><tr><td><a href="trading-messages.md#newordersingle-msgtype-d">NewOrderSingle</a></td><td>D</td><td>Allows the MP to submit a new order</td></tr><tr><td><a href="trading-messages.md#ordercancelrequest-msgtype-f">OrderCancelRequest</a></td><td>F</td><td>Allows the MP to cancel an active order</td></tr><tr><td><a href="trading-messages.md#ordercancelreplacerequest-msgtype-g">OrderCancelReplaceRequest</a></td><td>G</td><td>Allows the MP to <a href="../trading-api.md#modifyorder">modify </a>an active order</td></tr><tr><td><a href="trading-messages.md#ordermasscancelrequest-msgtype-q">OrderMassCancelRequest</a></td><td>q</td><td>Allows the MP to mass cancel all active orders for a specific instrument </td></tr></tbody></table>

### Server Initiated Messages

<table><thead><tr><th width="248">Message</th><th width="104.33333333333331">MsgType</th><th width="394.6666666666667">Usage</th></tr></thead><tbody><tr><td><a href="trading-messages.md#executionreport-msgtype-8">Execution Report</a></td><td>8</td><td>Indicates the result of order command.</td></tr><tr><td><a href="trading-messages.md#ordermasscancelreport-msgtype-r">Order Mass Cancel Report</a></td><td>r</td><td>Indicates the result of the mass cancel command.</td></tr><tr><td><a href="trading-messages.md#ordercancelreject-msgtype-9">Order Cancel Reject</a></td><td>9</td><td>Indicates that an order cancel request or order cancel/replace request has been rejected.</td></tr><tr><td><a href="trading-messages.md#businessmessagereject-msgtype-j">Business Message Reject</a></td><td>j</td><td>Indicates that an application message could not be processed</td></tr></tbody></table>

### Request/ Response Messages

<table><thead><tr><th width="214.33333333333331">Message</th><th width="241">Success Message</th><th>Failure Message</th></tr></thead><tbody><tr><td><a href="trading-messages.md#newordersingle-msgtype-d">New Order Single</a></td><td>Execution Report</td><td><p>One of the below:</p><ul><li>ExecutionReport</li><li>BusinessReject</li><li>Reject</li></ul></td></tr><tr><td><a href="trading-messages.md#ordercancelrequest-msgtype-f">Order Cancel Request</a></td><td>Execution Report</td><td><p>One of the below:</p><ul><li>OrderCancelReject</li><li>BusinessReject</li><li>Reject</li></ul></td></tr><tr><td><a href="trading-messages.md#ordermasscancelrequest-msgtype-q">Order Mass Cancel Request</a></td><td><p>Both:</p><ul><li>Execution Report- Order Level</li><li>Order Mass Cancel Report</li></ul></td><td><p>One of the below:</p><ul><li>OrderMassCancelReport</li><li>BusinessReject</li><li>Reject</li></ul></td></tr><tr><td><a href="trading-messages.md#ordercancelreplacerequest-msgtype-g">Order Cancel Replace Request</a></td><td>Execution Report</td><td><p>One of the below:</p><ul><li>OrderCancelReject</li><li>BusinessReject</li><li>Reject</li></ul></td></tr></tbody></table>

### Order Lifecycle Scenarios

The below scenarios summarize the trading actions that are support with the corresponded Execution Report.

<table><thead><tr><th width="453.3333333333333">Scenario</th><th width="135">ExecType(150)</th><th>OrdStatus(39)</th></tr></thead><tbody><tr><td>Order Rejected</td><td>8 (Rejected)</td><td>8 (Rejected)</td></tr><tr><td>Order accepted and order is fully resting on the book</td><td>0 (New)</td><td>0 (New)</td></tr><tr><td>Order accepted and order is fully executed immediately</td><td>F (Trade)</td><td>2 (Filled)</td></tr><tr><td>Order accepted and order is partially executed immediately and the residual is resting on the book</td><td><p>F (Trade) &#x26;</p><p>0 (New)</p></td><td><p>1 (Partially filled) &#x26;</p><p>1 (Partially filled)</p></td></tr><tr><td>Order accepted and order is partially executed immediately and the residual is cancelled (IOC)</td><td><p>F (Trade) &#x26;</p><p>4 (Canceled)</p></td><td><p>1 (Partially filled) &#x26;</p><p>4 (Canceled)</p></td></tr><tr><td>Order with immediate TIF (IOC, FOK) that failed to be executed</td><td>4 (Canceled)</td><td>4 (Canceled)</td></tr><tr><td>GTD order that has expired</td><td>4 (Canceled)</td><td>4 (Canceled)</td></tr><tr><td>Order cancelled</td><td>4 (Canceled)</td><td>4 (Canceled)</td></tr><tr><td>Order modified</td><td>5 (Replaced)</td><td><p>0 (New) OR</p><p>1 (Partially filled)</p></td></tr></tbody></table>

## Messages Specifications

### NewOrderSingle _(MsgType = D)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=162|35=D|49=MP1|56=EXBERRY|34=456|52=20221123-14:57:43.764000|11=1669215463763|38=1|40=2|44=1|54=2|55=INS1|59=1|453=2|448=123456|447=D|452=38|448=ABCD|447=D|452=12|10=010
```
{% endcode %}

<table><thead><tr><th width="89">Tag</th><th width="135">Name</th><th width="110">Required</th><th>Description</th></tr></thead><tbody><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Client Order ID, always growing Number</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr><tr><td>54</td><td>Side</td><td>Y</td><td>1= Buy<br>2 = Sell</td></tr><tr><td>38</td><td>OrderQty</td><td>Y</td><td>Quantity</td></tr><tr><td>40</td><td>OrdType</td><td>Y</td><td>Order type:<br>1 = Market<br>2 = Limit</td></tr><tr><td>44</td><td>Price</td><td>N</td><td>Order price<br>Required for limit orders</td></tr><tr><td>59</td><td>TimeInForce</td><td>Y</td><td>Time in force:<br>1 = GTC<br>3 = IOC<br>4 = FOK<br>6 = GTD<br>B = GFA (in Exberry this is called GAA)<br><mark style="color:blue;">NEW</mark> 0 = DAY</td></tr><tr><td>126</td><td>ExpireTime</td><td>N</td><td><p>Expiry time in seconds in GMT</p><p>Format: YYYYMMDD-hh:mm:ss</p><p>Sample: 20201225-09:30:55</p><p>Required if TimeInForce = GTD</p></td></tr><tr><td><mark style="color:red;">Deprecated</mark><br><del>20001</del></td><td><del>Custom tag</del></td><td><del>N</del></td><td><del>User Id (20 chars length, [a-z,A-Z,0-9])</del></td></tr><tr><td><mark style="color:red;">Deprecated</mark><br><del>20002</del></td><td><del>Custom tag</del></td><td><del>N</del></td><td><del>Account Id</del><br><del>Positive integer and less than or equal to max Integer (2147483647)</del></td></tr><tr><td>581</td><td>AccountType</td><td>N</td><td><p>Account type </p><p>1 = Client </p><p>3 = House</p></td></tr><tr><td>+</td><td>Parties</td><td>N</td><td>This is party information related to the submitter of the request.<br>See <a href="trading-messages.md#parties-component">here </a>for more details </td></tr></tbody></table>

### Parties Component

Note: On server initiated massages (like ExecutionReport (8) or TradeCaptureReport (AE)),  MP name will always be added as additional party with the below parameters: \
\- 448 (PartyID) = MP Name \
\- 452 (PartyRole) = 7 (Entering Firm) \
\- 447 (PartyIDSource) = D (Custom)

<table><thead><tr><th width="86">Tag</th><th width="149">Name</th><th width="95">Required</th><th>Description</th></tr></thead><tbody><tr><td>453</td><td>NoPartyIDs</td><td>N</td><td>Number of parties </td></tr><tr><td>448</td><td>PartyID</td><td>N</td><td>Required if NoPartyIDs(453) > 0. <br>Identification of the party.</td></tr><tr><td>447</td><td>PartyIDSource</td><td>N</td><td>Required if NoPartyIDs(453) > 0. <br>Used to identify classification source.</td></tr><tr><td>452</td><td>PartyRole</td><td>N</td><td><p>Required if NoPartyIDs(453) > 0. </p><p>Identifies the type of PartyID(448).</p></td></tr></tbody></table>



### OrderCancelRequest _(MsgType = F)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=89|35=F|49=MP1|56=EXBERRY|34=85|52=20221124-08:56:48.583000|11=1669280208583|37=4|55=INS1|10=144|
```
{% endcode %}

<table><thead><tr><th width="86">Tag</th><th width="133">Name</th><th width="95">Required</th><th>Description</th></tr></thead><tbody><tr><td>37</td><td>OrderID</td><td>N</td><td><p>Order Id to be cancelled</p><p>Mandatory if OrigClOrdID was not specified</p></td></tr><tr><td>41</td><td>OrigClOrdID</td><td>N</td><td><p>Original ClOrdID to be cancelled</p><p>Mandatory if OrderID was not specified</p></td></tr><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Unique ID of cancel request, numbers only</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr></tbody></table>

### OrderCancelReplaceRequest _(MsgType = G)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=97|35=G|49=MP1|56=EXBERRY|34=62|52=20221127-12:06:35.980000|11=1669550795980|37=8|38=0.12|55=INS1|10=251|
```
{% endcode %}

<table><thead><tr><th width="77">Tag</th><th width="112">Name</th><th width="95">Required</th><th>Description</th></tr></thead><tbody><tr><td>37</td><td>OrderID</td><td>Y</td><td>Order Id to be modified</td></tr><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Unique ID of cancel/replace request, numbers only</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr><tr><td>38</td><td>OrderQty</td><td>Y</td><td>Quantity to apply on order (less than original quantity)</td></tr></tbody></table>

### OrderMassCancelRequest _(MsgType = q)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=90|35=q|49=MP1|56=EXBERRY|34=12|52=20221124-09:32:09.451000|11=1669282329451|55=INS1|530=1|10=198|
```
{% endcode %}

<table><thead><tr><th width="80">Tag</th><th width="237">Name</th><th width="97">Required</th><th>Description</th></tr></thead><tbody><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Unique ID of mass cancel request, numbers only</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr><tr><td>530</td><td>MassCancelRequestType</td><td>Y</td><td>1 = Cancel orders for a security</td></tr></tbody></table>

### ExecutionReport _(MsgType = 8)_

{% tabs %}
{% tab title="New" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=257|35=8|49=EXBERRY|56=MP1|34=11|52=20230208-14:52:35.748854|17=41567|55=INS1|37=8|11=1675867953922|150=0|40=2|39=0|54=2|59=1|44=1.0|38=1.00|14=0.00|151=1.00|60=20230208-14:52:35.740531|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=user|447=D|452=7|10=088|
```
{% endcode %}
{% endtab %}

{% tab title="Rejected" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=166|35=8|49=EXBERRY|56=MP1|34=4|52=20230208-14:49:16.166387|17=41566|150=8|37=-1|11=1675867754336|39=8|103=1011|54=1|14=0|151=0|58=Permission denied for this instrument|10=037|
```
{% endcode %}
{% endtab %}

{% tab title="Trade" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=290|35=8|49=EXBERRY|56=MP1|34=16|52=20230208-14:54:12.909888|17=41569|55=INS1|37=9|11=1675868051083|150=F|40=2|39=2|54=1|59=1|44=1.0|38=1.00|14=1.00|151=0.00|31=1.0|32=1.00|880=5|60=20230208-14:54:12.900801|75=20230208|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=user|447=D|452=7|10=071|
```
{% endcode %}
{% endtab %}

{% tab title="Canceled" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=272|35=8|49=EXBERRY|56=MP1|34=24|52=20230208-14:57:46.764247|17=41571|55=INS1|37=10|41=1675868153947|11=1675868264931|150=4|40=2|39=4|54=2|59=1|44=1.0|38=1.00|14=0.00|151=0|60=20230208-14:57:46.751133|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=user|447=D|452=7|10=083|
```
{% endcode %}
{% endtab %}

{% tab title="Replaced" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=275|35=8|49=EXBERRY|56=MP1|34=38|52=20230208-15:03:11.859441|17=41573|55=INS1|37=11|41=1675868335775|11=1675868590036|150=5|40=2|39=0|54=2|59=1|44=1.0|38=1.00|14=0.00|151=0.12|60=20230208-15:03:11.850057|453=3|448=123456|447=D|452=38|448=ABCD|447=D|452=12|448=user|447=D|452=7|10=209|
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Order Status" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=253|35=8|49=EXBERRY|56=Test1|34=18|52=20221206-13:08:55.829048|17=0|584=1670332136072|912=Y|150=I|55=INS1|37=261|41=1670365062864|40=2|39=0|54=2|59=1|44=5.74|38=1|14=0|151=1|31=0|32=0|60=20221130-12:53:35.853912|20001=User|453=1|448=Test1|447=D|452=7|10=149|
```
{% endcode %}
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="99">Tag</th><th width="144">Name</th><th width="96">Required</th><th>Description</th></tr></thead><tbody><tr><td>37</td><td>OrderID</td><td>Y</td><td>Order Id<br>In case of rejection message will be -1 </td></tr><tr><td>41</td><td>OrigClOrdID</td><td>N</td><td>ClOrdID of the cancelled/ modified order<br>Returned in case 150 (ExecType) is: <br> 4 = [Canceled]<br> 5 = [Replaced]</td></tr><tr><td>11</td><td>ClOrdID</td><td>N</td><td><p>ClOrdID of the request message in case 150 (ExecType) is: <br>8 = [Rejected] </p><p>4 = [Canceled] </p><p>5 = [Replaced]<br>(In case request is done not from FIX GW it will not be returned)<br><br>ClOrdID of the order in case 150 (ExecType) is: </p><p>0 = [New]</p><p>F = [Trade]<br>I = [Order Status]</p></td></tr><tr><td>17</td><td>ExecID</td><td>Y</td><td>Unique identifier of execution report<br>0 = Case of Order Mass Status Request (AF) response</td></tr><tr><td>880</td><td>TrdMatchID</td><td>N</td><td>Match Id - only in case ExecType = Trade</td></tr><tr><td>150</td><td>ExecType</td><td>Y</td><td><p>Describes the purpose of the execution report:</p><p>8 = [Rejected]- When request is rejected</p><p>0 = [New]- When order added to the book</p><p>F = [Trade]- When order was executed (Fully or partially)</p><p>4 = [Canceled] - When order canceled from book (also in case of GTD expired)</p><p>5 = [Replaced] - when order was modified<br>I = [Order Status] - When mass status requested</p></td></tr><tr><td>39</td><td>OrdStatus</td><td>Y</td><td><p>Current order status (after this message):</p><p>8 = [Rejected]- For failure to place order</p><p>0 = [New]- For success new/ modified order that is fully resting on the book</p><p>4 = [Canceled]- For success cancel order , also in case of GTD expired</p><p>1 = [PartiallyFilled]- For partially executed order:</p><p><code>(Order State = Active) AND (FilledQty>0)</code></p><p>2 = [Filled]- For fully executed order</p></td></tr><tr><td>58</td><td>Text</td><td>N</td><td>In case of rejection, specifies the error message</td></tr><tr><td>103</td><td>OrdRejReason</td><td>N</td><td>In case of rejection, specifies the exberry error code</td></tr><tr><td>38</td><td>OrderQty</td><td>N</td><td>Order original quantity</td></tr><tr><td>14</td><td>CumQty</td><td>N</td><td>Order total Filled Quantity</td></tr><tr><td>151</td><td>LeavesQty</td><td>N</td><td><p>Remaining open quantity</p><p><code>IF OrdStatus(39) =</code><br><code>[4(Canceled) or C(Expired) or 8(Rejected)]</code><br><code>Then LeavesQty(151) = 0</code></p><p><code>Else LeavesQty(151) = OrderQty(38) - CumQty(14)</code></p></td></tr><tr><td>32</td><td>LastQty</td><td>N</td><td>In case ExecType = Trade:<br>Executed amount on current fill only</td></tr><tr><td>31</td><td>LastPx</td><td>N</td><td><p>In case ExecType = Trade:</p><p>Executed price on current fill only</p></td></tr><tr><td>55</td><td>Symbol</td><td>N</td><td>Instrument symbol</td></tr><tr><td>54</td><td>Side</td><td>N</td><td><p>1= Buy</p><p>2 = Sell<br><br>On "Rejected" message will always be 1 (Buy)</p></td></tr><tr><td>40</td><td>OrdType</td><td>N</td><td><p>Order type:</p><p>1 = Market</p><p>2 = Limit</p></td></tr><tr><td>44</td><td>Price</td><td>N</td><td>Order price</td></tr><tr><td>59</td><td>TimeInForce</td><td>N</td><td><p>Time in force:</p><p>1 = GTC</p><p>3 = IOC</p><p>4 = FOK</p><p>6 = GTD</p><p>B = GFA (in Exberry this is called GAA)</p></td></tr><tr><td>126</td><td>ExpireTime</td><td>N</td><td><p>Expiry time in seconds in GMT</p><p>Format: YYYYMMDD-hh:mm:ss</p></td></tr><tr><td><mark style="color:red;">Deprecated</mark><br><del>20001</del></td><td><del>Custom tag</del></td><td><del>N</del></td><td><del>User Id</del></td></tr><tr><td><mark style="color:red;">Deprecated</mark><br><del>20002</del></td><td><del>Custom tag</del></td><td><del>N</del></td><td><del>Account Id</del></td></tr><tr><td>581</td><td>AccountType</td><td>N</td><td><p>Account type </p><p>1 = Client </p><p>3 = House</p></td></tr><tr><td><br>+</td><td>Parties</td><td>N</td><td>This is party information related to the submitter of the request.<br>See <a href="trading-messages.md#parties-component">here </a>for more details </td></tr><tr><td>912</td><td>LastRptRequested</td><td>N</td><td><p>Will be included only in case ExecType (150) = l (Order Status)<br>Identify if this is the last record<br>Y =  for the last record</p><p>N = for all the rest</p></td></tr><tr><td><p></p><p>584</p></td><td>MassStatusReqID</td><td>N</td><td>Will be included only in case ExecType (150) = I (Order Status), from request</td></tr></tbody></table>

### OrderMassCancelReport _(MsgType = r)_

{% tabs %}
{% tab title="Accepted" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=105|35=r|49=EXBERRY|56=Test1|34=26|52=20230208-12:59:24.189454|11=1675861165216|530=1|531=1|533=1|1369=41489|10=221|
```
{% endcode %}
{% endtab %}

{% tab title="Rejected" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=136|35=r|49=EXBERRY|56=Test1|34=19|52=20230208-12:56:07.210122|11=1675860968239|530=1|531=0|532=1010|58=Instrument FIX not found|1369=41488|10=255|
```
{% endcode %}
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="107">Tag</th><th width="237">Name</th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Unique ID of mass cancel request</td></tr><tr><td>530</td><td>MassCancelRequestType</td><td>Y</td><td>1 = Cancel orders for a security</td></tr><tr><td>531</td><td>MassCancelResponse</td><td>Y</td><td><p>0 = Cancel request rejected</p><p>1 = Cancel request succeeded</p></td></tr><tr><td>532</td><td>MassCancelRejectReason</td><td>N</td><td>In case of reject, specify the Exberry error code</td></tr><tr><td>533</td><td>TotalAffectedOrders</td><td>N</td><td>In case of success, number of orders affected from the mass cancel request</td></tr><tr><td>1369</td><td>MassActionReportID</td><td>Y</td><td>Unique ID of mass cancel request</td></tr><tr><td>58</td><td>Text</td><td>N</td><td>In case of reject, specify the error message</td></tr></tbody></table>

### OrderCancelReject _(MsgType = 9)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=142|35=9|49=EXBERRY|56=Test1|34=2|52=20230208-12:48:17.846582|11=1675860498874|37=NONE|434=1|102=1100|39=8|58=Order not found for that instrument|10=171|
```
{% endcode %}

<table><thead><tr><th width="92">Tag</th><th width="174">Name</th><th width="85">Required</th><th>Description</th></tr></thead><tbody><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Unique ID of cancel request</td></tr><tr><td>37</td><td>OrderID</td><td>Y</td><td>orderId that was sent to be cancelled/ modified. Always “NONE” </td></tr><tr><td>39</td><td>OrdStatus</td><td>Y</td><td> 8 = Rejected</td></tr><tr><td>102</td><td>CxlRejReason</td><td>Y</td><td>Specify the error code</td></tr><tr><td>434</td><td>CxlRejResponseTo</td><td>Y</td><td><p>Original request is :</p><p>1 = Order cancel request</p><p>2 = Order cancel/replace request</p></td></tr><tr><td>58</td><td>Text</td><td>Y</td><td>Specify the error message</td></tr></tbody></table>

### BusinessMessageReject _(MsgType = j)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=123|35=j|49=EXBERRY|56=Target|34=2|52=20230202-13:46:43.112040|45=2|372=x|380=6|58=This apiKey doesn't have the right permission|10=127|
```
{% endcode %}

<table><thead><tr><th width="98">Tag</th><th width="208">Name</th><th width="103">Required</th><th>Description</th></tr></thead><tbody><tr><td>380</td><td>BusinessRejectReason</td><td>Y</td><td>Specify the error code per <a href="https://fiximate.fixtrading.org/en/FIX.Latest/tag380.html">FIX specifications</a></td></tr><tr><td>58</td><td>Text</td><td>Y</td><td>Specify the error message</td></tr><tr><td>45</td><td>RefSeqNum</td><td>Y</td><td>Reference message sequence number</td></tr><tr><td>372</td><td>RefMsgType</td><td>Y</td><td>The MsgType (35) of the FIX message being referenced</td></tr></tbody></table>
