# Trading Messages

## Supported Messages

### Client Initiated Messages

<table><thead><tr><th width="250">Message</th><th width="64.33333333333331">MsgType</th><th width="431.6666666666667">Usage</th></tr></thead><tbody><tr><td><a href="trading-messages.md#newordersingle-msgtype-d">NewOrderSingle</a></td><td>D</td><td>Allows the MP to submit a new order</td></tr><tr><td><a href="trading-messages.md#ordercancelrequest-msgtype-f">OrderCancelRequest</a></td><td>F</td><td>Allows the MP to cancel an active order</td></tr><tr><td><a href="trading-messages.md#ordercancelreplacerequest-msgtype-g">OrderCancelReplaceRequest</a></td><td>G</td><td>Allows the MP to <a href="../ws/trading-api.md#modifyorder">modify </a>an active order</td></tr><tr><td><a href="trading-messages.md#ordermasscancelrequest-msgtype-q">OrderMassCancelRequest</a></td><td>q</td><td>Allows the MP to mass cancel all active orders for a specific instrument </td></tr></tbody></table>

### Server Initiated Messages

<table><thead><tr><th width="248">Message</th><th width="104.33333333333331">MsgType</th><th width="394.6666666666667">Usage</th></tr></thead><tbody><tr><td><a href="trading-messages.md#executionreport-msgtype-8">Execution Report</a></td><td>8</td><td>Indicates the result of order command.</td></tr><tr><td><a href="trading-messages.md#ordermasscancelreport-msgtype-r">Order Mass Cancel Report</a></td><td>r</td><td>Indicates the result of the mass cancel command.</td></tr><tr><td><a href="trading-messages.md#ordercancelreject-msgtype-9">Order Cancel Reject</a></td><td>9</td><td>Indicates that an order cancel request or order cancel/replace request has been rejected.</td></tr><tr><td><a href="trading-messages.md#businessmessagereject-msgtype-j">Business Message Reject</a></td><td>j</td><td>Indicates that an application message could not be processed</td></tr></tbody></table>

### Request/ Response Messages

<table><thead><tr><th width="214.33333333333331">Message</th><th width="241">Success Message</th><th>Failure Message</th></tr></thead><tbody><tr><td><a href="trading-messages.md#newordersingle-msgtype-d">New Order Single</a></td><td>Execution Report</td><td><p>One of the below:</p><ul><li>ExecutionReport</li><li>BusinessReject</li><li>Reject</li></ul></td></tr><tr><td><a href="trading-messages.md#ordercancelrequest-msgtype-f">Order Cancel Request</a></td><td>Execution Report</td><td><p>One of the below:</p><ul><li>OrderCancelReject</li><li>BusinessReject</li><li>Reject</li></ul></td></tr><tr><td><a href="trading-messages.md#ordermasscancelrequest-msgtype-q">Order Mass Cancel Request</a></td><td><p>Both:</p><ul><li>Execution Report- Order Level</li><li>Order Mass Cancel Report</li></ul></td><td><p>One of the below:</p><ul><li>OrderMassCancelReport</li><li>BusinessReject</li><li>Reject</li></ul></td></tr><tr><td><a href="trading-messages.md#ordercancelreplacerequest-msgtype-g">Order Cancel Replace Request</a></td><td>Execution Report</td><td><p>One of the below:</p><ul><li>OrderCancelReject</li><li>BusinessReject</li><li>Reject</li></ul></td></tr></tbody></table>

### Order Lifecycle Scenarios

The below scenarios summarize the trading actions that are support with the corresponded Execution Report.

<table><thead><tr><th width="440.3333333333333">Scenario</th><th width="143">ExecType(150)</th><th>OrdStatus(39)</th></tr></thead><tbody><tr><td>Order Rejected</td><td>8 (Rejected)</td><td>8 (Rejected)</td></tr><tr><td>Order accepted and order is fully resting on the book</td><td>0 (New)</td><td>0 (New)</td></tr><tr><td>Order accepted (or triggered) and order is fully executed immediately</td><td>F (Trade)</td><td>2 (Filled)</td></tr><tr><td>Order accepted (or triggered) and order is partially executed immediately and the residual is resting on the book</td><td>F (Trade)</td><td>1 (Partially filled)</td></tr><tr><td>Order accepted (or triggered) and order is partially executed immediately and the residual is cancelled (IOC)</td><td><p>F (Trade) &#x26;</p><p>4 (Canceled)</p></td><td><p>1 (Partially filled) &#x26;</p><p>4 (Canceled)</p></td></tr><tr><td>Order with immediate TIF (IOC, FOK) that failed to be executed</td><td>4 (Canceled)</td><td>4 (Canceled)</td></tr><tr><td>GTD order that has expired</td><td>4 (Canceled)</td><td>4 (Canceled)</td></tr><tr><td>Order cancelled</td><td>4 (Canceled)</td><td>4 (Canceled)</td></tr><tr><td>Order modified without an immediate execution </td><td>5 (Replaced)</td><td><p>0 (New) OR</p><p>1 (Partially filled)</p></td></tr><tr><td>Order replaced without an immediate execution</td><td>4 (Canceled) &#x26;<br>5 (Replaced)</td><td>4 (Canceled) &#x26;<br>0 (New)</td></tr><tr><td>Order replaced with an immediate partial/full execution </td><td>4 (Canceled) &#x26;<br>F (Trade)</td><td>4 (Canceled) &#x26;<br>1 (Partially filled) OR 2 (Filled)</td></tr><tr><td>Reserve order replenishment</td><td>4 (Canceled) &#x26;<br>5 (Replaced)</td><td>4 (Canceled) &#x26;<br>0 (New)</td></tr><tr><td>Order adjustment due to corporate actions</td><td>4 (Canceled) &#x26;<br>5 (Replaced)</td><td>4 (Canceled) &#x26;<br>0 (New)</td></tr><tr><td>Order suspended <br>Note: 636 (WorkingIndicator) = N</td><td>0 (New)</td><td>0 (New)</td></tr><tr><td>Order injected <br>Note: 636 (WorkingIndicator) = Y</td><td>L (Triggered or Activated by System)</td><td>0 (New)</td></tr><tr><td>Inject order failed</td><td>4 (Canceled)</td><td>4 (Canceled)</td></tr></tbody></table>

The below scenarios summarize the Execution Reports related to the response of OrderMassStatusRequest.

<table><thead><tr><th width="349.3333333333333">Scenario</th><th width="195">ExecType(150)</th><th>OrdStatus(39)</th></tr></thead><tbody><tr><td>Order status of non-Stop/StopLimit order</td><td>I (Order Status)</td><td>0 (New) OR 1 (Partially filled)</td></tr><tr><td><p>Order status of suspended Stop/SL order</p><p>Note: 636 (WorkingIndicator) = N</p></td><td>I (Order Status)</td><td>0 (New)</td></tr><tr><td><p>Order status of injected Stop/SL order which is currently fully resting on the book</p><p>Note: 636 (WorkingIndicator) = Y</p></td><td>I (Order Status)</td><td>0 (New)</td></tr></tbody></table>

## Messages Specifications

### NewOrderSingle _(MsgType = D)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=162|35=D|49=MP1|56=EXBERRY|34=456|52=20221123-14:57:43.764000|11=1669215463763|38=1|40=2|44=1|54=2|55=INS1|59=1|453=2|448=123456|447=D|452=38|448=ABCD|447=D|452=12|10=010
```
{% endcode %}

<table><thead><tr><th width="81">Tag</th><th width="113">Name</th><th width="92">Required</th><th>Description</th></tr></thead><tbody><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Client Order ID, always growing Number <mark style="color:blue;">(NEW v1.46.0)</mark> uniqueness validated at SenderCompID(49) level.</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr><tr><td>54</td><td>Side</td><td>Y</td><td>1= Buy<br>2 = Sell</td></tr><tr><td>38</td><td>OrderQty</td><td>Y</td><td>Quantity</td></tr><tr><td>40</td><td>OrdType</td><td>Y</td><td>Order type:<br>1 = Market<br>2 = Limit<br>3 = Stop<br>4 = StopLimit<br>K = (Market With Left Over as Limit)</td></tr><tr><td>44</td><td>Price</td><td>N</td><td>Order price<br>Required for limit orders stop limit orders </td></tr><tr><td><p></p><p>99</p></td><td>StopPx</td><td>N</td><td>Stop price<br>Required for Stop &#x26; Stop Limit orders </td></tr><tr><td>59</td><td>TimeInForce</td><td>Y</td><td>Time in force:<br>1 = GTC<br>3 = IOC<br>4 = FOK<br>6 = GTD<br>B = GFA (in Exberry this is called GAA)<br>0 = DAY<br><mark style="color:blue;">(NEW v1.41.0)</mark> 2 = OPG (At the Opening (OPG))<br><mark style="color:blue;">(NEW v1.41.0)</mark> 7 = ATC (At the Close)</td></tr><tr><td>432</td><td>ExpireDate</td><td>N</td><td><p>Expiry Date</p><p>Format: YYYYMMDD</p><p>Required if TimeInForce = GTD and ExpireTime(126) is not specified.</p></td></tr><tr><td>126</td><td>ExpireTime</td><td>N</td><td><p>Expiry time in seconds in GMT</p><p>Format: YYYYMMDD-hh:mm:ss</p><p>Sample: 20201225-09:30:55</p><p>Required if TimeInForce = GTD and ExpireDate(432) is not specified.</p></td></tr><tr><td>581</td><td>AccountType</td><td>N</td><td><p>Account type </p><p>1 = Client </p><p>3 = House</p></td></tr><tr><td>20033</td><td>Custom tag</td><td>N</td><td><p>STP Action</p><p></p><p>1 = CancelIncomingOrder</p><p>2 = CancelRestingOrder</p><p>3 = CancelBothOrders</p></td></tr><tr><td>+</td><td>Parties</td><td>N</td><td>This is party information related to the submitter of the request.<br>See <a href="trading-messages.md#parties-component">here </a>for more details </td></tr><tr><td>110</td><td>MinQty</td><td>N</td><td>Minimum quantity of an IOC order to be executed.</td></tr><tr><td>1138</td><td>DisplayQty</td><td>N</td><td><p>Optional</p><p>Required if displayMethod exists.</p><p>The quantity to be displayed via public market data. <br>If not exist, the order is fully visible.</p></td></tr><tr><td>1084</td><td>DisplayMethod</td><td>N</td><td><p>Optional</p><p>Required if displayQuantity exists</p><p>Defines the behavior of display quantity</p><p><br>Available values:</p><ul><li>1 = Initial (use original DisplayQty)</li><li>3 = Random (Displays the quantity in DisplayQty initially, then a quantity between DisplayLowQty and DisplayHighQty when replenished)</li><li>4 = Undisclosed (hidden/invisible order)</li></ul></td></tr><tr><td>1085</td><td>DisplayLowQty</td><td>N</td><td><p>Optional</p><p>The lower quantity limit to randomize the display quantity at replenishment</p></td></tr><tr><td>1086</td><td>DisplayHighQty</td><td>N</td><td><p>Optional</p><p>The upper quantity limit to randomize the display quantity at replenishment</p></td></tr></tbody></table>

### Parties Component

Note: On server initiated massages (like ExecutionReport (8) or TradeCaptureReport (AE)),  MP name will always be added as additional party with the below parameters: \
\- 448 (PartyID) = MP Name \
\- 452 (PartyRole) = 7 (Entering Firm) \
\- 447 (PartyIDSource) = D (Custom)

<table><thead><tr><th width="86">Tag</th><th width="149">Name</th><th width="95">Required</th><th>Description</th></tr></thead><tbody><tr><td>453</td><td>NoPartyIDs</td><td>N</td><td>Number of parties </td></tr><tr><td>448</td><td>PartyID</td><td>N</td><td>Required if NoPartyIDs(453) > 0. <br>Identification of the party.<br>Max length = 20 <br>Allowed characters ASCII (32 -126)</td></tr><tr><td>447</td><td>PartyIDSource</td><td>N</td><td>Required if NoPartyIDs(453) > 0. <br>Used to identify classification source.<br>Max length = 1<br>Allowed characters [0-9,a-z,A-Z]</td></tr><tr><td>452</td><td>PartyRole</td><td>N</td><td><p>Required if NoPartyIDs(453) > 0. </p><p>Identifies the type of PartyID(448).<br>Integer</p></td></tr></tbody></table>

See [here](trading-messages-1.md#new-v1.33.0-account-assignment) for parties related to Account Assignment.

Mandatory Parties:

Specific parties(example, role=8, source=A) can be configured as a mandatory. If configured as mandatory, they will be required in all requests with `parties` field.&#x20;



### OrderCancelRequest _(MsgType = F)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=89|35=F|49=MP1|56=EXBERRY|34=85|52=20221124-08:56:48.583000|11=1669280208583|37=4|55=INS1|10=144|
```
{% endcode %}

<table><thead><tr><th width="86">Tag</th><th width="133">Name</th><th width="95">Required</th><th>Description</th></tr></thead><tbody><tr><td>37</td><td>OrderID</td><td>N</td><td><p>Order Id to be cancelled</p><p>Mandatory if OrigClOrdID was not specified</p></td></tr><tr><td>41</td><td>OrigClOrdID</td><td>N</td><td><p>Original ClOrdID to be cancelled</p><p>Mandatory if OrderID was not specified</p></td></tr><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Unique ID of cancel request, numbers only</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr></tbody></table>

### OrderCancelReplaceRequest _(MsgType = G)_

The order OrderCancelReplaceRequest supports modification of an existing order or replacing existing order by canceling the existing order and entering a new order based on parameters of the request.

If the request is only to reduce the LeavesQty of an order(OrderQty of the request < LeavesQty of the order) or also displayQuantity as long as Delta.quantity ≥ Delta.displayQuantity); other fields may present with the same values as the original order or be omitted from request, the system modifies the existing order, the behavior is similar to [modifyOrder](../ws/trading-api.md#modifyorder) of the WS.

Any other request would result in the system canceling the existing order and creating a new one, the behavior is similar to [replaceOrder](../ws/trading-api.md#replaceorder) of the WS.

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=166|35=G|34=16|49= MP1|52=20230808-05:54:19.861956|56= EXBERRY|37=42|11=344137727371000|55=ABC|54=1|60=20230808-05:54:19.860|38=2|40=2|44=3.26|10=041|
```
{% endcode %}

<table><thead><tr><th width="88">Tag</th><th width="141">Name</th><th width="73">Required</th><th>Description</th></tr></thead><tbody><tr><td>37</td><td>OrderID</td><td>Y</td><td>Order Id to be modified</td></tr><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Unique ID of cancel/replace request, numbers only</td></tr><tr><td>110</td><td>MinQty</td><td>N</td><td><p>minQuantity</p><p>Allowed only when 59(TIF)= 3(IOC)</p></td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr><tr><td>54</td><td>Side</td><td>Y</td><td>Side must be equal to the side of original order</td></tr><tr><td>60</td><td>TransactTime</td><td>Y</td><td>Time this request was initiated/released by the trader or trading system</td></tr><tr><td>38</td><td>OrderQty</td><td>Y</td><td>Quantity to apply on order</td></tr><tr><td>40</td><td>OrdType</td><td>Y</td><td>OrdType must be equal to the OrdType of original order</td></tr><tr><td>44</td><td>Price</td><td>N</td><td>New order Price<br><br>Required If OrderQty >= LeavesQty of original order</td></tr><tr><td>99</td><td>StopPx</td><td>N</td><td><p>New order Stop price</p><p>Required for stop orders and stop limit orders</p></td></tr><tr><td>1138</td><td>DisplayQty</td><td>N</td><td>New display quantity.</td></tr><tr><td>1085</td><td>DisplayLowQty</td><td>N</td><td>New lower quantity limit to randomize the display quantity at replenishment</td></tr><tr><td>1086</td><td>DisplayHighQty</td><td>N</td><td>New upper quantity limit to randomize the display quantity at replenishment</td></tr><tr><td>59</td><td>TimeInForce</td><td>N</td><td>New order Time In Force<br>Values same as New Order Single</td></tr><tr><td>432</td><td>ExpireDate</td><td>N</td><td><p>Expiry Date</p><p>Format: YYYYMMDD</p><p>Required if TimeInForce = GTD and ExpireTime(126) is not specified.</p></td></tr><tr><td>126</td><td>ExpireTime</td><td>N</td><td><p>New order Expiry time in seconds(UTC Time).</p><p>Format: YYYYMMDD-hh:mm:ss</p><p>Required if TimeInForce = GTD and ExpireDate(432) is not specified.</p></td></tr><tr><td>20033</td><td>Custom tag</td><td>N</td><td>New Order stpAction</td></tr></tbody></table>

### OrderMassCancelRequest _(MsgType = q)_

{% tabs %}
{% tab title="without TargetParties" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=90|35=q|49=MP1|56=EXBERRY|34=12|52=20221124-09:32:09.451000|11=1669282329451|55=INS1|530=1|10=198|
```
{% endcode %}
{% endtab %}

{% tab title="with TargetParties" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=90|35=q|49=MP1|56=EXBERRY|34=12|52=20221124-09:32:09.451000|11=1669282329451|55=INS1|530=1|1461=1|1462=acc123|1463=D|1464=1001|10=198|
```
{% endcode %}
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="89">Tag</th><th width="180">Name</th><th width="78">Required</th><th>Description</th></tr></thead><tbody><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Unique ID of mass cancel request, numbers only</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol<br><br>Required if MassCancelRequestType(530)=1, else not required</td></tr><tr><td>530</td><td>MassCancelRequestType</td><td>Y</td><td>1 = Cancel orders for a security<br>7 = Cancel all orders</td></tr><tr><td>1461</td><td>NoTargetPartyIDs</td><td>N</td><td>Identifies the number of target parties identified in a mass action.</td></tr><tr><td>>1462</td><td>TargetPartyID</td><td>N</td><td>Required if NoTargetPartyIDs(1461) > 0. Used to identify the party targeted for the action specified in the message.</td></tr><tr><td>>1463</td><td>TargetPartyIDSource</td><td>N</td><td>Required if NoTargetPartyIDs(1461) > 0. Used to identify source of target party identifier.</td></tr><tr><td>>1464</td><td>TargetPartyRole</td><td>N</td><td>Required if NoTargetPartyIDs(1461) > 0. Used to identify the role of source party identifier.</td></tr></tbody></table>



### ExecutionReport _(MsgType = 8)_

{% tabs %}
{% tab title="New" %}
{% code overflow="wrap" %}
```
fixin 8=FIXT.1.1|9=256|35=8|49=EXBERRY|56=JPMA|34=10|52=20250305-12:17:53.016640|17=183181837|55=111111|37=20|11=1741177072699|150=0|40=2|39=0|54=1|59=0|44=100|38=10|14=0|151=10|6=0|60=20250305-12:17:53.005375|453=1|448=JPM-1|447=D|452=7|768=1|769=20250305-12:17:53.005375|770=9|10=002|
```
{% endcode %}
{% endtab %}

{% tab title="Rejected" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=162|35=8|49=EXBERRY|56=Test1|34=3|52=20250306-12:18:33.196750|17=183183641|150=8|37=-1|11=1741263512584|39=8|103=1009|54=1|14=0|151=0|58=Instrument trading is halted|10=100|
```
{% endcode %}
{% endtab %}

{% tab title="Trade" %}
{% code overflow="wrap" %}
```
fixin 8=FIXT.1.1|9=292|35=8|49=EXBERRY|56=JPMA|34=20|52=20250305-05:21:07.673953|17=183180935|55=111111|37=7|11=1741151964134|150=F|40=2|39=1|54=1|59=0|44=100|38=10|14=2|151=8|6=100|31=100|32=2|880=3|75=20250305|60=20250305-05:21:07.650530|453=1|448=JPM-1|447=D|452=7|442=1|768=1|769=20250305-05:19:24.523680|770=9|10=131|
```
{% endcode %}
{% endtab %}

{% tab title="Canceled" %}
{% code overflow="wrap" %}
```
fixin 8=FIXT.1.1|9=277|35=8|49=EXBERRY|56=JPMA|34=49|52=20250305-05:52:14.470134|17=183181051|55=111111|37=17|41=1741153920809|150=4|40=2|39=4|54=1|59=1|44=100|38=5|14=0|151=0|6=0|60=20250305-05:52:14.458022|58=CancelOrderOnBehalf|453=1|448=JPM-1|447=D|452=7|768=1|769=20250305-05:52:01.210005|770=9|10=136|
```
{% endcode %}
{% endtab %}

{% tab title="Replaced" %}
<pre data-overflow="wrap"><code>//Limit
fixin 8=FIXT.1.1|9=255|35=8|49=EXBERRY|56=JPMA|34=108|52=20250305-05:35:11.252279|17=183180974|55=111111|37=10|11=1741151964134|150=5|40=2|39=0|54=1|59=0|44=105|38=8|14=0|151=8|6=0|60=20250305-05:35:11.235625|453=1|448=JPM-1|447=D|452=7|768=1|769=20250305-05:35:11.235625|770=9|10=231|

<strong>//Stop Limit 
</strong>fixin 8=FIXT.1.1|9=270|35=8|49=EXBERRY|56=JPMA|34=138|52=20250305-05:39:47.082871|17=183180991|55=111111|37=12|11=1741153161424|150=5|40=4|39=0|54=1|59=0|44=111|99=120|38=15|14=0|151=15|6=0|60=20250305-05:39:47.070358|636=N|453=1|448=JPM-1|447=D|452=7|768=1|769=20250305-05:39:47.070358|770=9|10=208|

</code></pre>
{% endtab %}

{% tab title="Suspended" %}
{% code overflow="wrap" %}
```
fixin 8=FIXT.1.1|9=268|35=8|49=EXBERRY|56=JPMA|34=8|52=20250305-05:46:27.220199|17=183181014|55=111111|37=13|11=1741153586808|150=0|40=4|39=0|54=1|59=0|44=100|99=150|38=20|14=0|151=20|6=0|60=20250305-05:46:27.211314|636=N|453=1|448=JPM-1|447=D|452=7|768=1|769=20250305-05:46:27.211314|770=9|10=071|
```
{% endcode %}
{% endtab %}

{% tab title="Triggered" %}
{% code overflow="wrap" %}
```
fixin 8=FIXT.1.1|9=269|35=8|49=EXBERRY|56=JPMA|34=27|52=20250305-05:49:07.213169|17=183181030|55=111111|37=13|11=1741153586808|150=L|40=4|39=0|54=1|59=0|44=100|99=150|38=20|14=0|151=20|6=0|60=20250305-05:49:07.192012|636=Y|453=1|448=JPM-1|447=D|452=7|768=1|769=20250305-05:46:27.211314|770=9|10=162|
```
{% endcode %}
{% endtab %}
{% endtabs %}



{% tabs %}
{% tab title="Order Status" %}
{% code overflow="wrap" %}
```
fixin 8=FIXT.1.1|9=299|35=8|49=EXBERRY|56=JPMA|34=8|52=20250306-10:32:39.906883|17=0|584=1741257159424|912=Y|150=I|55=111111|37=23|11=1741244405214|40=2|39=1|54=2|59=1|44=105.0000|38=20.000000|14=1.000000|151=19.000000|6=105|60=20250306-07:47:00.006887|453=1|448=JPM-1|447=D|452=7|768=1|769=20250306-07:00:05.396090|770=9|10=096|
```
{% endcode %}
{% endtab %}

{% tab title="Trade With Legs" %}
{% code overflow="wrap" %}
```
//Optional Functionality
8=FIXT.1.1|9=289|35=8|49=EXBERRY|56=JPMA|34=81|52=20231114-13:47:29.293668|17=6724|55=S1|37=18|11=1564413812395|150=F|40=2|39=2|54=1|59=0|44=295.33|38=1|14=1|151=0|31=295.33|32=1|880=8|75=20231114|60=20231114-13:47:29.278413|453=3|448=ACC3|447=D|452=12|448=user123|447=D|452=13|448=JPM-1|447=D|452=7|442=3|10=051|
8=FIXT.1.1|9=324|35=8|49=EXBERRY|56=JPMA|34=82|52=20231114-13:47:29.293668|17=6725|55=Leg1|37=-1|150=F|39=2|54=1|38=1.000000|14=1.000000|151=0|31=1.0000|32=1.000000|880=24|75=20231114|60=20231114-13:47:29.278413|453=3|448=ACC3|447=D|452=12|448=user123|447=D|452=13|448=JPM-1|447=D|452=7|442=2|820=8|1647=1|1648=99|1649=S1|1650=13299|1651=99|10=231|
8=FIXT.1.1|9=327|35=8|49=EXBERRY|56=JPMA|34=85|52=20231114-13:47:29.297588|17=6728|55=Leg2|37=-1|150=F|39=2|54=2|38=1.000000|14=1.000000|151=0|31=-294.3300|32=1.000000|880=12|75=20231114|60=20231114-13:47:29.278413|453=3|448=ACC3|447=D|452=12|448=user123|447=D|452=13|448=JPM-1|447=D|452=7|442=2|820=8|1647=1|1648=99|1649=S1|1650=13299|1651=99|10=176|
```
{% endcode %}
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="89">Tag</th><th width="132">Name</th><th width="75">Required</th><th>Description</th></tr></thead><tbody><tr><td>37</td><td>OrderID</td><td>Y</td><td>Order Id<br>In case of rejection message will be -1 </td></tr><tr><td>41</td><td>OrigClOrdID</td><td>N</td><td>ClOrdID of the cancelled/ modified order<br>Returned in case 150 (ExecType) is: <br> 4 = [Canceled]<br> 5 = [Replaced]</td></tr><tr><td>11</td><td>ClOrdID</td><td>N</td><td><p>ClOrdID of the request message in case 150 (ExecType) is: <br>8 = [Rejected] </p><p>4 = [Canceled] </p><p>5 = [Replaced]<br>(In case request is done not from FIX GW it will not be returned)<br><br>ClOrdID of the order in case 150 (ExecType) is: </p><p>0 = [New]</p><p>F = [Trade]<br>I = [Order Status]</p></td></tr><tr><td>17</td><td>ExecID</td><td>Y</td><td>Unique identifier of execution report<br>0 = Case of Order Mass Status Request (AF) response</td></tr><tr><td>880</td><td>TrdMatchID</td><td>N</td><td>Match Id - only in case ExecType = Trade</td></tr><tr><td>150</td><td>ExecType</td><td>Y</td><td><p>Describes the purpose of the execution report:</p><p>8 = [Rejected]- When request is rejected</p><p>0 = [New]- When order added to the book or suspended (stop/stop limit orders)</p><p>F = [Trade]- When order was executed (Fully or partially)</p><p>4 = [Canceled] - When order canceled from book (also in case of GTD expired)</p><p>5 = [Replaced] - when order was modified or replaced (for newly created order message)<br>I = [Order Status] - When mass status requested<br>C = [Expired] - When order is expired<br>L = [Triggered or Activated by System] -  When a Stop/StopLimit order is triggered(injected) only in case order is fully being resting on the book</p></td></tr><tr><td>39</td><td>OrdStatus</td><td>Y</td><td><p>Current order status (after this message):</p><p>8 = [Rejected]- For failure to place order</p><p>0 = [New]- For success new/ modified order that is fully resting on the book or suspended.</p><p>4 = [Canceled]- For success cancel order , also in case of GTD expired</p><p>1 = [PartiallyFilled]- For partially executed order:</p><p><code>(Order State = Active) AND (FilledQty>0)</code></p><p>2 = [Filled]- For fully executed order</p></td></tr><tr><td>636</td><td>WorkingIndicator</td><td>N</td><td><p>Applicable only when OrdStatus=0(New) and 1 of the below:</p><ul><li>Order is suspended or </li><li>Order is/was triggered (i.e. Stop/Stop Limit order) and and fully resting on the book</li></ul><p>Y = Order is triggered<br>N = Order is suspended</p></td></tr><tr><td>58</td><td>Text</td><td>N</td><td>In case of rejection, specifies the error message<br>In case of cancellation, specifies the cancellation reason. Refer <a href="../ws/private-data-api.md#cancel-reason">here</a> for values.</td></tr><tr><td>103</td><td>OrdRejReason</td><td>N</td><td>In case of rejection, specifies the exberry error code</td></tr><tr><td>38</td><td>OrderQty</td><td>N</td><td>Original Order Qty- Removed (modification) Qty</td></tr><tr><td>14</td><td>CumQty</td><td>N</td><td>Order total Filled Quantity</td></tr><tr><td>151</td><td>LeavesQty</td><td>N</td><td><p>Remaining open quantity</p><pre class="language-mathematica"><code class="lang-mathematica">If OrdStatus(39) = [4(Canceled) or C(Expired) or 8(Rejected)]
Then LeavesQty(151) = 0
Else LeavesQty(151) = OrderQty(38) - CumQty(14)
</code></pre></td></tr><tr><td>32</td><td>LastQty</td><td>N</td><td>In case ExecType = Trade:<br>Executed amount on current fill only</td></tr><tr><td>31</td><td>LastPx</td><td>N</td><td><p>In case ExecType = Trade:</p><p>Executed price on current fill only</p></td></tr><tr><td><mark style="color:blue;">(NEW v1.41.0)</mark><br>6</td><td>AvgPx</td><td>N</td><td><p>Average price of all fills of the order.<br>Weighted average filled price for all fills on that order <br><em>Sum(LastQty∗ LastPx)/ Sum(LastQty)</em></p><p><br>For “Rejected” message will not be populated </p></td></tr><tr><td>55</td><td>Symbol</td><td>N</td><td>Instrument symbol</td></tr><tr><td>54</td><td>Side</td><td>N</td><td><p>1= Buy</p><p>2 = Sell<br><br>On "Rejected" message will always be 1 (Buy)</p></td></tr><tr><td>40</td><td>OrdType</td><td>N</td><td><p>Order type:</p><p>1 = Market</p><p>2 = Limit<br>3 = Stop<br>4 = StopLimit<br>K = (Market With Left Over as Limit)</p></td></tr><tr><td>44</td><td>Price</td><td>N</td><td>Order price</td></tr><tr><td>99</td><td>StopPx</td><td>N</td><td>Stop price</td></tr><tr><td>59</td><td>TimeInForce</td><td>N</td><td><p>Time in force:</p><p>1 = GTC</p><p>3 = IOC</p><p>4 = FOK</p><p>6 = GTD</p><p>B = GFA (in Exberry this is called GAA)<br>0 = DAY<br><mark style="color:blue;">(NEW v1.41.0)</mark> 2 = OPG (At the Opening (OPG))<br><mark style="color:blue;">(NEW v1.41.0)</mark> 7 = ATC (At the Close)</p></td></tr><tr><td>60</td><td>TransactTime</td><td>N</td><td><p>Transaction time in microseconds in UTC</p><p>Format: YYYYMMDD-HH:MM:SS.ssssss</p></td></tr><tr><td>432</td><td>ExpireDate</td><td>N</td><td><p>Expiry Date</p><p>Format: YYYYMMDD</p></td></tr><tr><td>126</td><td>ExpireTime</td><td>N</td><td><p>Expiry time in seconds in GMT</p><p>Format: YYYYMMDD-hh:mm:ss</p></td></tr><tr><td>581</td><td>AccountType</td><td>N</td><td><p>Account type </p><p>1 = Client </p><p>3 = House</p></td></tr><tr><td><br>+</td><td>Parties</td><td>N</td><td>This is party information related to the submitter of the request.<br>See <a href="trading-messages.md#parties-component">here </a>for more details </td></tr><tr><td>912</td><td>LastRptRequested</td><td>N</td><td><p>Will be included only in case ExecType (150) = l (Order Status)<br>Identify if this is the last record<br>Y =  for the last record</p><p>N = for all the rest</p></td></tr><tr><td><p></p><p>584</p></td><td>MassStatusReqID</td><td>N</td><td>Will be included only in case ExecType (150) = I (Order Status), from request</td></tr><tr><td>110</td><td>MinQty</td><td>N</td><td>Minimum quantity of an order to be executed.</td></tr><tr><td>1138</td><td>DisplayQty</td><td>N</td><td>Working display quantity if exist.</td></tr><tr><td>1608</td><td>InitialDisplayQty</td><td>N</td><td>DisplayQty(1138) of the order entry message if exist.</td></tr><tr><td>1084</td><td>DisplayMethod</td><td>N</td><td>DisplayMethod(1084) of the order entry message if exist.</td></tr><tr><td>1085</td><td>DisplayLowQty</td><td>N</td><td>DisplayLowQty(1085) of the order entry message if exist.</td></tr><tr><td>1086</td><td>DisplayHighQty</td><td>N</td><td>DisplayLowQty(1085) of the order entry message if exist.</td></tr><tr><td>442</td><td>MultiLegReportingType</td><td>N</td><td><p>A value is always returned if in LOGON(A) → 20037 = Y else tag is not returned.</p><p>1 - Single security - not strategy trade </p><p>2 - Individual leg of a multi-leg security</p><p>3 - Multi-leg security - the parent trade of multi-leg security</p></td></tr><tr><td>820</td><td>TradeLinkID</td><td>N</td><td><p>Only if in LOGON(A) → 20037 = Y</p><p>Only when MultiLegReportingType(442) = 2, will show </p><p>Parent trade ID (multiLegStrategyTradeId in WS API)</p><p>(this can be associated with strategy ExecutionReport TrdMatchID (880) of the parent)</p></td></tr><tr><td>1647</td><td>NoRelatedInstruments</td><td>N</td><td><p>Only if in LOGON(A) → 20037 = Y</p><p>Only when MultiLegReportingType(442) = 2, shows number of related instruments</p><p>Will always be 1</p></td></tr><tr><td>>1648</td><td>RelatedInstrumentType</td><td>N</td><td><p>Required if NoRelatedInstruments > 0<br>The type of instrument relationship</p><p>99 (Custom: Multi-leg security)</p></td></tr><tr><td>1649</td><td>RelatedSymbol</td><td>N</td><td>Required if NoRelatedInstruments > 0<br>Parent Symbol (This is required in order to know explicitly which strategy trade is the parent trade).</td></tr><tr><td>1650</td><td>RelatedSecurityID</td><td>N</td><td>Required if NoRelatedInstruments > 0<br>Parent instrumentId</td></tr><tr><td>1651</td><td>RelatedSecurityIDSource</td><td>N</td><td><p>Required if NoRelatedInstruments > 0<br>Identifies class or source of the RelatedSecurityID (1650) value.</p><p>99 (Custom)</p></td></tr><tr><td>20033</td><td>Custom tag</td><td>N</td><td><p>STP Action</p><p>1 = CancelIncomingOrder</p><p>2 = CancelRestingOrder</p><p>3 = CancelBothOrders</p></td></tr><tr><td><mark style="color:blue;">(NEW v1.41.0)</mark><br>768</td><td>NoTrdRegTimestamps</td><td>N</td><td><p>Populated when OrdStatus(39) != 8 (Rejected)</p><p>Number of TrdRegTimestamp entries.</p><p>Value = 1</p></td></tr><tr><td><mark style="color:blue;">(NEW v1.41.0)</mark><br>>769</td><td>TrdRegTimestamp</td><td>N</td><td>Order created Timestamp in microseconds, in UTC Format: YYYYMMDD-HH:MM:SS.ssssss</td></tr><tr><td><mark style="color:blue;">(NEW v1.41.0)</mark><br>>770</td><td>TrdRegTimestampType</td><td>N</td><td><p>Trading / Regulatory timestamp type.</p><p>Value = 9 (Orderbook entry time)</p></td></tr></tbody></table>

### OrderMassCancelReport _(MsgType = r)_

{% tabs %}
{% tab title="Accepted (without TargetParties)" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=105|35=r|49=EXBERRY|56=Test1|34=26|52=20230208-12:59:24.189454|11=1675861165216|530=1|531=1|533=1|1369=41489|10=221|
```
{% endcode %}
{% endtab %}

{% tab title="Accepted (with TargetParties)" %}
{% code overflow="wrap" %}
```
fixin 8=FIXT.1.1|9=147|35=r|49=EXBERRY|56=Mp5Comp1|34=4|52=20230720-06:51:28.706700|1369=704884|11=1689835888612|530=1|531=1|533=2|1461=1|1462=verification|1463=D|1464=4|10=165|
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

<table><thead><tr><th width="85">Tag</th><th width="218">Name</th><th width="81">Required</th><th>Description</th></tr></thead><tbody><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Unique ID of mass cancel request</td></tr><tr><td>530</td><td>MassCancelRequestType</td><td>Y</td><td>1 = Cancel orders for a security</td></tr><tr><td>531</td><td>MassCancelResponse</td><td>Y</td><td><p>0 = Cancel request rejected</p><p>1 = Cancel request succeeded</p></td></tr><tr><td>532</td><td>MassCancelRejectReason</td><td>N</td><td>In case of reject, specify the Exberry error code</td></tr><tr><td>533</td><td>TotalAffectedOrders</td><td>N</td><td>In case of success, number of orders affected from the mass cancel request</td></tr><tr><td>1369</td><td>MassActionReportID</td><td>Y</td><td>Unique ID of mass cancel request</td></tr><tr><td>1461</td><td>NoTargetPartyIDs</td><td>N</td><td><p>Should be populated with the values provided on the associated OrderMassCancelRequest(MsgType=Q).<br><br>Repeating group below should contain unique combinations of </p><ul><li>TargetPartyID</li><li>TargetPartyIDSource</li><li>TargetPartyRole.</li></ul></td></tr><tr><td>>1462</td><td>TargetPartyID</td><td>N</td><td>Should be populated with the values provided on the associated OrderMassCancelRequest(MsgType=Q).</td></tr><tr><td>>1463</td><td>TargetPartyIDSource</td><td>N</td><td>Should be populated with the values provided on the associated OrderMassCancelRequest(MsgType=Q).</td></tr><tr><td>>1464</td><td>TargetPartyRole</td><td>N</td><td>Should be populated with the values provided on the associated OrderMassCancelRequest(MsgType=Q).</td></tr><tr><td>58</td><td>Text</td><td>N</td><td>In case of reject, specify the error message</td></tr></tbody></table>

### OrderCancelReject _(MsgType = 9)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=142|35=9|49=EXBERRY|56=Test1|34=2|52=20230208-12:48:17.846582|11=1675860498874|37=NONE|434=1|102=1100|39=8|58=Order not found for that instrument|10=171|
```
{% endcode %}

<table><thead><tr><th width="77">Tag</th><th width="159">Name</th><th width="76">Required</th><th>Description</th></tr></thead><tbody><tr><td>11</td><td>ClOrdID</td><td>Y</td><td>Unique ID of cancel request</td></tr><tr><td>37</td><td>OrderID</td><td>Y</td><td>orderId that was sent to be cancelled/ modified. Always “NONE” </td></tr><tr><td>39</td><td>OrdStatus</td><td>Y</td><td> 8 = Rejected</td></tr><tr><td>102</td><td>CxlRejReason</td><td>Y</td><td>Specify the error code</td></tr><tr><td>434</td><td>CxlRejResponseTo</td><td>Y</td><td><p>Original request is :</p><p>1 = Order cancel request</p><p>2 = Order cancel/replace request</p></td></tr><tr><td>58</td><td>Text</td><td>Y</td><td>Specify the error message</td></tr></tbody></table>

### BusinessMessageReject _(MsgType = j)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=123|35=j|49=EXBERRY|56=Target|34=2|52=20230202-13:46:43.112040|45=2|372=x|380=6|58=Insufficient permissions|10=127|
```
{% endcode %}

<table><thead><tr><th width="98">Tag</th><th width="208">Name</th><th width="103">Required</th><th>Description</th></tr></thead><tbody><tr><td>380</td><td>BusinessRejectReason</td><td>Y</td><td>Specify the error code per <a href="https://fiximate.fixtrading.org/en/FIX.Latest/tag380.html">FIX specifications</a></td></tr><tr><td>58</td><td>Text</td><td>Y</td><td>Specify the error message</td></tr><tr><td>45</td><td>RefSeqNum</td><td>Y</td><td>Reference message sequence number</td></tr><tr><td>372</td><td>RefMsgType</td><td>Y</td><td>The MsgType (35) of the FIX message being referenced</td></tr></tbody></table>
