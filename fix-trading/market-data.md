# Market Data

Supported Messages

### Client-Initiated Messages

<table><thead><tr><th width="231">Message</th><th width="104.33333333333331">MsgType</th><th width="414.6666666666667">Usage</th></tr></thead><tbody><tr><td><a href="https://docs.exberry.io/fix-trading/market-data#market-data-request-msgtype-v">Market Data Request</a></td><td>V</td><td>Allows the client to request or manage subscriptions for market data for multiple instruments</td></tr><tr><td><a href="https://docs.exberry.io/fix-trading/market-data#security-list-request-msgtype-x">Security List Request</a></td><td>x</td><td>Allows to request a list of instruments details that match the criteria provided</td></tr><tr><td><a href="https://docs.exberry.io/fix-trading/market-data#security-status-request-msgtype-e">Security Status Request</a></td><td>e</td><td>Allows to request the trading status of an instrument </td></tr></tbody></table>

### Server Initiated Messages

<table><thead><tr><th width="262">Message</th><th width="107.33333333333331">MsgType</th><th>Usage</th></tr></thead><tbody><tr><td><a href="https://docs.exberry.io/fix-trading/market-data#market-data-snapshot-full-refresh-msgtype-w">Market Data Snapshot Full Refresh</a></td><td>W</td><td>Used to return a snapshot of market data</td></tr><tr><td><a href="https://docs.exberry.io/fix-trading/market-data#market-data-incremental-refresh-msgtype-x">Market Data Incremental Refresh</a></td><td>X</td><td>Used to return incremental market data updates</td></tr><tr><td><a href="https://docs.exberry.io/fix-trading/market-data#market-data-request-reject-msgtype-y">Market Data Request Reject</a></td><td>Y</td><td>Indicates that a Market Data Request has been rejected</td></tr><tr><td><a href="https://docs.exberry.io/fix-trading/market-data#security-list-msgtype-y">Security List</a></td><td>y</td><td>Used to return a list of instruments and their details </td></tr><tr><td><a href="https://docs.exberry.io/fix-trading/market-data#security-status-msgtype-f">Security Status</a></td><td>f</td><td>Used to return trading status of an instrument </td></tr></tbody></table>

### Request/ Response Messages

<table><thead><tr><th width="337">Request</th><th width="208">Success Message</th><th>Failure Message</th></tr></thead><tbody><tr><td><p>Market Data Request (V)</p><p></p><p>For <mark style="color:green;">Ticker</mark> data use the below parameters:</p><ul><li>SubscriptionRequestType(263) = 0/1/2</li><li>MDUpdateType(265) = 0 (Full refresh)</li><li>MarketDepth(264) = 1(Top of book)</li><li>MDEntryType(269) = 0 (bid), 1(ask) Both bid and ask should be subscribed together.</li><li>Symbol (55)</li><li>Scope(546) (Optional) = A, C</li></ul><p>Note: Request for ticker data supports all MDEntryType values mentioned <a href="market-data.md#market-data-request-msgtype-v">here</a> except A: Imbalance.</p></td><td><ul><li>Market Data Snapshot Full Refresh (W)</li></ul></td><td><p>One of the below:</p><ul><li>Market Data Request Reject (Y)</li><li>BusinessMessageReject (j)</li></ul><p></p></td></tr><tr><td><p>Market Data Request (V)</p><p></p><p>For <mark style="color:green;">Market by Price (MBP)</mark> data use the below parameters:</p><ul><li>SubscriptionRequestType(263) = 0/1/2  </li><li>MDUpdateType(265) = 0 (Full refresh)</li><li>MarketDepth(264) = 5/10/20/100</li><li><p>MDEntryType(269) = 0 (bid), 1(ask) Both bid and ask should be subscribed together.</p><ul><li>Note: any other value can be sent and will be handled as a ticker</li></ul></li><li>Symbol (55)</li></ul></td><td><ul><li>Market Data Snapshot Full Refresh (W)</li></ul></td><td><p>One of the below:</p><ul><li>Market Data Request Reject (Y)</li><li>BusinessMessageReject (j)</li></ul><p></p></td></tr><tr><td><p>Market Data Request (V)</p><p></p><p>For <mark style="color:green;">Market by Order Full Depth (MBOFD)</mark> data use the below parameters:</p><p></p><ul><li>SubscriptionRequestType(263) = 0 /1 / 2 </li><li>MDUpdateType(265) = 1 (Incremental)</li><li>MarketDepth(264) = 0 (full book depth)</li><li>NoMDEntryTypes(267) = 2</li><li>MDEntryType(269) = 0 (bid), 1(ask) Both bid and ask should be subscribed together.</li><li>Symbol (55)</li></ul><p><mark style="color:blue;">NEW v1.33.0</mark> Note: This will also return MDEntryType(269) = b (Market bid) and c (Market offer), even if they are not subscribed. This is applicable for auctions only. </p></td><td><ul><li>Market Data Snapshot (W)</li><li>Market Data Incremental Refresh (X)</li></ul></td><td><p>One of the below:</p><ul><li>Market Data Request Reject (Y)</li><li>BusinessMessageReject (j)</li></ul><p></p></td></tr><tr><td><p>Market Data Request (V)</p><p></p><p>For <mark style="color:green;">Time &#x26; Sales</mark> data use the below parameters:</p><p></p><ul><li>SubscriptionRequestType(263) = 1</li><li>MDUpdateType(265) = 1 (Incremental)</li><li>MarketDepth(264) = 0/1/5/10/20/100 (this field is ignored)</li><li>NoMDEntryTypes(267) = 1</li><li>MDEntryType(269) = 2 (Trades)</li><li>Symbol(55)</li></ul></td><td><ul><li>Market Data Incremental Refresh (X)</li></ul></td><td><p>One of the below:</p><ul><li>Market Data Request Reject (Y)</li><li>BusinessMessageReject (j)</li></ul><p></p></td></tr><tr><td><p>Market Data Request (V)</p><p></p><p>For <mark style="color:green;">Auction Indicative Equilibrium Price (Imbalance)</mark> data use the below parameters:<br></p><ul><li>SubscriptionRequestType(263) = 1</li><li>MDUpdateType(265) = 1 (Incremental)</li><li>MarketDepth(264) = 0/1/5/10/20/100 (this field is ignored)</li><li>NoMDEntryTypes(267) = 1</li><li>MDEntryType(269) = A (Imbalance))</li><li>Symbol (55) </li></ul></td><td><ul><li>Market Data Incremental Refresh (X)</li></ul></td><td><p>One of the below:</p><ul><li>Market Data Request Reject (Y)</li><li>BusinessMessageReject (j)</li></ul></td></tr><tr><td>Security List Request (x)</td><td><ul><li>Security List (y)</li></ul></td><td><p>One of the below:</p><ul><li>Security List (y) </li><li>BusinessMessageReject (j)</li><li>Reject (3)</li></ul></td></tr><tr><td>Security Status Request (e)</td><td><ul><li>Security Status (f)</li></ul></td><td><p>One of the below:</p><ul><li>BusinessMessageReject (j)</li><li>Reject (3)</li></ul></td></tr></tbody></table>

### Messages related to auctions

Market Data updates related to the auction are disseminated using [MarketDataIncrementalRefresh(X)](market-data.md#market-data-incremental-refresh-msgtype-x) messages. A single update can have multiple entries with different MDEntryTypes as below.\
Check [AuctionIndicativeEP](../ws/market-data.md#auction-indicative-equilibrium-price-message) to see more details explanation about auction indicative price.

<table><thead><tr><th width="367">Condition</th><th>MDEntryType entry</th></tr></thead><tbody><tr><td>when the book is empty</td><td>MDEntryType(269)=J (Empty book)</td></tr><tr><td>Best bid exist without IndicativePrice</td><td>MDEntryType(269)=0 (Bid)/ <mark style="color:blue;">NEW v1.33.0</mark> b (Market bid)</td></tr><tr><td>Best offer exist without IndicativePrice</td><td>MDEntryType(269)=1 (Offer)/ <mark style="color:blue;">NEW v1.33.0</mark> c (Market offer) </td></tr><tr><td>Best bid &#x26; offer exist without IndicativePrice</td><td>MDEntryType(269)=0 (Bid)/ <mark style="color:blue;">NEW v1.33.0</mark> b (Market bid)<br>MDEntryType(269)=1 (Offer)/ <mark style="color:blue;">NEW v1.33.0</mark> c (Market offer) </td></tr><tr><td>When IndicativePrice exist</td><td>MDEntryType(269)=Q (Auction clearing price)</td></tr><tr><td>When imbalanceQuantity exist</td><td>MDEntryType(269)=A (Imbalance)</td></tr><tr><td>When IndicativePrice &#x26; imbalanceQuantity exist</td><td>MDEntryType(269)=Q (Auction clearing price)<br>MDEntryType(269)=A (Imbalance)</td></tr></tbody></table>



## Messages Specifications

### Market Data Request _(MsgType = V)_

{% tabs %}
{% tab title="Ticker " %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=201|35=V|49=user|56=EXBERRY|34=258|52=20230606-13:29:11.857000|146=1|55=INS1|262=1686058151857|263=1|264=1|265=0|267=14|269=0|269=1|269=2|269=B|269=4|269=5|269=6|269=7|269=8|269=r|269=v|269=o|269=f|269=g|10=246|
```
{% endcode %}
{% endtab %}

{% tab title="MBOFD" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=135|35=V|49=MP|56=EXBERRY|34=6|52=20230228-12:37:26.380000|146=2|55=INS1|55=INS2|262=1677587846380|263=1|264=0|265=1|267=2|269=0|269=1|10=207|

```
{% endcode %}
{% endtab %}

{% tab title="MBP" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=137|35=V|49=MP1|56=EXBERRY|34=250|52=20230209-08:39:25.474000|146=2|55=INS1|55=INS2|262=1675931965474|263=1|264=5|265=0|267=2|269=0|269=1|10=060|
```
{% endcode %}
{% endtab %}

{% tab title="Time & Sales" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=130|35=V|49=MP|56=EXBERRY|34=83|52=20230228-09:55:37.812000|146=1|55=INS1|262=1677578137812|263=1|264=1|265=1|267=1|269=2|10=242|
```
{% endcode %}
{% endtab %}

{% tab title="Imbalance" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=121|35=V|49=MP|56=EXBERRY|34=50|52=20230228-09:41:53.355000|146=1|55=INS1|262=1677577313355|263=1|264=1|265=1|267=1|269=A|10=109|
```
{% endcode %}
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="111">Tag</th><th width="170">Name</th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td>262</td><td>MDReqID</td><td>Y</td><td>Unique ID for this request.</td></tr><tr><td>263</td><td>SubscriptionRequestType</td><td>Y</td><td>Subscription Request Type. <br>0 = Snapshot<br>1 = Snapshot + Updates (Subscribe)<br>2 =  Disable previous Snapshot + Update Request (Unsubscribe)</td></tr><tr><td>264</td><td>MarketDepth</td><td>N</td><td><p>Mandatory in case SubscriptionRequestType (263)  = 0 or 1</p><p></p><p>Market depth to return in a snapshot<br>values- 1(Top Of Book),5,10,20,100</p></td></tr><tr><td>265</td><td>MDUpdateType</td><td>N</td><td><p></p><p></p><p>Type of messages FIX sends when updating market data.<br>0 = Full refresh</p></td></tr><tr><td>267</td><td>NoMDEntryTypes</td><td>Y</td><td><p>Mandatory in case SubscriptionRequestType (263)  = 0 or 1</p><p></p><p>Number of 269 (MDEntryType) fields in the request.</p></td></tr><tr><td>269</td><td>MDEntryType</td><td>Y</td><td><p>Mandatory in case SubscriptionRequestType (263)  = 0 or 1</p><p></p><p>Type of market data to request.</p><p>Available values: <br>0 = Bid</p><p>1 = Ask</p><p>2 = Trade</p><p>B = Trade volume (all trades volume)</p><p>4 = Opening price</p><p>5 = Closing price</p><p>6 = Settlement price</p><p>7 = Trading session high price</p><p>8 = Trading session Low price<br>A = Imbalance<br>r = Reference price<br>v = Order book trades volume</p><p>g = EFRP trades volume</p><p>f =  Block trades volume<br>o =  Other trades volume (Trade type = Other)<br>q = RFQ volume</p></td></tr><tr><td>546</td><td>Scope</td><td>N</td><td><p>Optional<br></p><p>Applicable only to <a href="market-data.md#request-response-messages">Ticker</a> (Ignored from other requests)</p><p>If not sent - ‘C’ is the default<br></p><p>Specifies the type of data used to calculate: </p><p>MDEntryType(269) = </p><ul><li>2 (Trade)</li><li>7 (Trading session high price)</li><li>8 (Trading session low price)</li><li>B (Trade volume)</li></ul><p>Possible values include:</p><p>A = data is derived from all trades (CLOB, trade entry and RFQ)</p><p>C = data derived from CLOB executions only</p></td></tr><tr><td>146</td><td>NoRelatedSym</td><td>Y</td><td>Specifies the number of repeating instruments specified</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td><p>Mandatory in case SubscriptionRequestType (263)  = 0 or 1</p><p></p><p>Instrument symbol <br>Note: Data will be returned only for an instrument that is active</p></td></tr></tbody></table>

### Market Data Request Reject _(MsgType = Y)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=103|35=Y|49=EXBERRY|56=Target|34=243|52=20230130-08:52:23.767763|262=1675068743722|281=0|58=Wrong Symbol(55)|10=226|
```
{% endcode %}

<table><thead><tr><th width="103">Tag</th><th width="170">Name</th><th width="100">Required</th><th>Description</th></tr></thead><tbody><tr><td>262</td><td>MDReqID</td><td>Y</td><td>Unique ID from the incoming request ID</td></tr><tr><td>58</td><td>text</td><td>N</td><td>Reason for the rejection</td></tr><tr><td>281</td><td>MDReqRejReason</td><td>N</td><td><p>Reason for the rejection of a Market Data request.</p><p>0 = (Unknown symbol)</p><p>1 = (Duplicate MDReqID)</p><p>4 = (Unsupported SubscriptionRequestType)</p><p>5  = (Unsupported MarketDepth)</p><p>6 = (Unsupported MDUpdateType)</p><p>8 = (Unsupported MDEntryType)</p></td></tr></tbody></table>

### Market Data Snapshot Full Refresh _(MsgType = W)_

{% tabs %}
{% tab title="Ticker" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=334|35=W|49=EXBERRY|56=user|34=262|52=20230606-13:29:14.714613|262=1686058151857|55=INS1|48=12208|22=99|779=20230606-10:35:43.020000|268=14|269=0|270=180|271=20|269=1|271=0|269=2|270=10|271=10|269=B|271=10|269=4|270=10|269=5|270=5000|269=6|270=200|269=7|270=10|269=8|270=10|269=r|270=200|269=v|271=0|269=o|271=0|269=f|271=10|269=g|271=0|20029=N|10=236|
```
{% endcode %}
{% endtab %}

{% tab title="MBOFD" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=191|35=W|49=EXBERRY|56=MP|34=18|52=20230524-11:18:34.035468|262=1684927114016|55=INS1|48=12208|22=99|779=20230521-09:50:43.910817|268=1|269=1|270=5000.0000000|271=2.0000|278=37|20029=Y|10=235|

8=FIXT.1.1|9=131|35=W|49=EXBERRY|56=JPME|34=11|52=20230709-09:00:00.831180|262=777|55=INST2|48=16624|22=99|779=20230709-09:00:00.830568|268=1|269=J|10=201|

```
{% endcode %}
{% endtab %}

{% tab title="MBP" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=167|35=W|49=EXBERRY|56=user|34=14|52=20230323-14:16:40.220696|262=1679580998362|55=INS1|48=1013|22=99|779=20230323-07:00:00.001000|268=1|269=0|270=215|271=9|1023=1|346=1|10=026|

8=FIXT.1.1|9=130|35=W|49=EXBERRY|56=JPME|34=5|52=20230709-09:01:48.980261|262=777|55=INST2|48=16624|22=99|779=20230709-08:51:44.240000|268=1|269=J|10=162|

```
{% endcode %}
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="101">Tag</th><th width="183">Name</th><th width="96">Required</th><th>Description</th></tr></thead><tbody><tr><td>262</td><td>MDReqID</td><td>Y</td><td>Unique ID from the incoming request ID.</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr><tr><td>779</td><td>LastUpdateTime</td><td>Y</td><td><p>Timestamp of last update to data item</p><p><br>Format YYYYmmDD-hh:mm:ss.000000  </p></td></tr><tr><td>268</td><td>NoMDEntries</td><td>Y</td><td><p>Repeating group.</p><p>Number of market data entries in the message</p></td></tr><tr><td>>269</td><td>MDEntryType</td><td>Y</td><td><p>Type of market data to request.</p><p>0 = Bid</p><p>1 = Ask</p><p>2 = Trade (include trade entry data)</p><p>B = Trade volume (all trades volume, include trade entry data)</p><p>4 = Opening price</p><p>5 = Closing price</p><p>6 = Settlement price</p><p>7 = Trading session high price (include trade entry data)</p><p>8 = Trading session Low price (include trade entry data)</p><p><mark style="color:blue;">NEW v1.33.0</mark> b = Market bid (during auction)</p><p><mark style="color:blue;">NEW v1.33.0</mark> c = Market offer (during auction)</p><p>J = Empty book<br>r = Reference price<br>v = Order book trades volume</p><p>g = EFRP trades volume</p><p>f =  Block trades volume</p><p>o =  Other trades volume (Trade type = Other)<br>q = RFQ volume</p></td></tr><tr><td>>270</td><td>MDEntryPx</td><td>N</td><td><p>Price of the instrument associated with this entry.<br>Applicable for the below MDEntryType(269):<br>0 = Bid<br>1 = Ask<br>2 = Trade (Per scope)<br>4 = Opening price</p><p>5 = Closing price</p><p>6 = Settlement price</p><p>7 = Trading session high price (Per scope)</p><p>8 = Trading session Low price (Per scope)<br><mark style="color:blue;">NEW v1.33.0</mark> b = Market bid (will be empty)</p><p><mark style="color:blue;">NEW v1.33.0</mark> c = Market offer (will be empty)</p><p>r = Reference price</p></td></tr><tr><td>>271</td><td>MDEntrySize</td><td>N</td><td><p>Quantity associated with the related entry.<br>Applicable for the below MDEntryType(269):<br>0 = Bid<br>1 = Ask<br>2 = Trade (Per scope)<br>B = Trade volume (Per scope)<br><mark style="color:blue;">NEW v1.33.0</mark> b = Market bid </p><p><mark style="color:blue;">NEW v1.33.0</mark> c = Market offer </p><p>v =  Order book trades volume</p><p>g = EFRP trades volume</p><p>f =  Block trades volume</p><p>o =  Other trades volume (Trade type = Other)<br>q = RFQ volume</p></td></tr><tr><td>>1023</td><td>MDPriceLevel</td><td>N</td><td><p>Relevant only for bid and ask - MBP</p><p>Integer to convey the level of a bid or offer at a given price level.<br><mark style="color:blue;">NEW v1.33.0</mark> b: Market bid &#x26; c: Market offer → this will be empty.</p></td></tr><tr><td>>48</td><td>SecurityID</td><td>Y</td><td>In case the symbol is changed after subscription, the permanent instrument identifier can be identified using this field.</td></tr><tr><td>>22</td><td>SecurityIDSource</td><td>Y</td><td><p>Identifies the class or source of the SecurityID(48) value</p><p><br>99 = Custom</p></td></tr><tr><td>>278</td><td>MDEntryID</td><td>N</td><td><p>Relevant only for bid and ask - MBOFD</p><p></p><p>Market data entry id</p></td></tr><tr><td>>346</td><td>NumberOfOrders</td><td>N</td><td><p>Relevant only for bid and ask - MBP:</p><p><br>In an Aggregated Book, used to show how many individual orders make up an MDEntry</p></td></tr><tr><td><p></p><p>20029</p></td><td>Custom tag</td><td>N</td><td><p><strong>If the MDEntryType(269) = 0 (bid) &#x26; 1 (ask) for MBOFD &#x26; for Ticker :</strong></p><p>Indicates whether this message is the last in a sequence of messages on the current snapshot </p><p></p><p>N = Not Last Message</p><p>Y = Last Message (of the snapshot)</p></td></tr></tbody></table>

### Market Data Incremental Refresh _(MsgType = X)_

{% tabs %}
{% tab title="MBOFD" %}
<pre data-overflow="wrap"><code><strong>8=FIXT.1.1|9=193|35=X|49=EXBERRY|56=MP|34=30|52=20230518-07:05:10.398793|262=1684393391021|268=1|279=0|55=INS1|48=12208|22=99|278=36|269=1|270=5000.0000000|271=2.0000|272=20230518|273=07:05:10.384906|10=028|
</strong></code></pre>
{% endtab %}

{% tab title="Time & Sales" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=169|35=X|49=EXBERRY|56=MP|34=7|52=20230228-12:42:38.595568|262=1677588144937|268=1|279=0|269=2|55=INS1|48=1013|22=99|270=22|271=1|272=20230228|273=12:42:38.579000|2446=1|442=1|10=155|
```
{% endcode %}
{% endtab %}

{% tab title="Imbalance" %}
<pre data-overflow="wrap"><code>//Case Empty order book 
<strong>8=FIXT.1.1|9=218|35=X|49=EXBERRY|56=JPME|34=87|52=20230712-11:13:32.631144|262=777|268=1|279=0|269=J|55=INST1|48=16620|22=99|272=20230712|273=13:12:40.412615|10=242|
</strong>
//Case no Equilibrium Price Available 
8=FIXT.1.1|9=216|35=X|49=EXBERRY|56=JPME|34=76|52=20230712-11:12:02.624686|262=777|268=1|279=0|269=1|55=INST1|48=16620|22=99|270=300.33|271=5|272=20230712|273=13:12:40.322613|276=Z|1023=1|10=244|

//Case Equilibrium Price &#x26; Imbalance Available 
8=FIXT.1.1|9=218|35=X|49=EXBERRY|56=JPME|34=87|52=20230712-11:13:32.631144|262=777|268=2|279=0|269=Q|55=INST1|48=16620|22=99|270=300.33|271=1|272=20230712|273=13:12:40.412615|1070=0|279=0|269=A|55=INST1|48=16620|22=99|271=4|272=20230712|273=13:12:40.412615|277=Q|1070=0|10=242|

//Case Equilibrium Price Available But no Imbalance 
8=FIXT.1.1|9=218|35=X|49=EXBERRY|56=JPME|34=87|52=20230712-11:13:32.631144|262=777|268=2|279=0|269=Q|55=INST1|48=16620|22=99|270=300.33|271=1|272=20230712|273=13:12:40.412615|1070=0|10=242|
</code></pre>
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="111"></th><th width="135"></th><th width="114"></th><th></th></tr></thead><tbody><tr><td>262</td><td>MDReqID</td><td>Y</td><td>Unique ID from the incoming request ID.</td></tr><tr><td>268</td><td>NoMDEntries</td><td>Y</td><td><p>Repeating group.</p><p>Number of market data entries in the message</p></td></tr><tr><td>>279</td><td>MDUpdateAction</td><td>Y</td><td><p>Specifies the type of market data update.</p><p><br><strong>MBOFD- MDEntryType(269) = 0 (bid) &#x26; 1 (ask):</strong></p><p>Possible values include:</p><p>0 = New: When order is added </p><p>1 = Change: When order is modified or resting order partially executed</p><p>2 = Delete: When order is canceled or resting order fully executed </p><p></p><p>Sample 1: </p><ul><li>OrderId 1 Buy 100 @1.3 Placed → 0 (New) event sent for order Id 1</li><li>OrderId 2 Sell 70 @1.3 Placed → 1 (Change) event sent for order id 1 (amount = 30)</li></ul><p><mark style="color:blue;">NEW v1.33.0</mark> Same values available for  b (Market bid) &#x26; c(Market offer)</p><p><br><strong>Time &#x26; Sales- MDEntryType(269) = 2 (trade):</strong></p><p>Possible values include:</p><p>0 = New: When trade happens</p><p>2 = Delete: When an order is canceled</p><p></p><p><strong>Auction Imbalance - MDEntryType(269) = A (Imbalance):</strong></p><p>Possible values include:</p><p>0 = New: When an auction imbalance is sent<br><br><strong>Auction Indicative Equilibrium Price-</strong><br><strong>MDEntryType(269) = Q (AuctionClearingPrice):</strong><br>Possible values include:</p><p>0 = New: When an equilibrium price is sent<br><br><strong>Best Bid/Ask during an auction when there is no Equilibrium Price-</strong><br><strong>MDEntryType(269) = 0 (bid)/1 (ask) AND QuoteCondition(276)=Z(Order Imbalance)</strong></p><p>Possible values include:</p><p>0 = New: When a best bid/ask is available</p><p><br><strong>Empty Order Book-</strong><br><strong>MDEntryType(269) = J (EmptyBook):</strong><br>Possible values include:</p><p>0 = New: When the order book is empty</p></td></tr><tr><td>>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr><tr><td>>48</td><td>SecurityID</td><td>Y</td><td>Permanent instrument identifier, in case symbol is changed after subscription, you can identify the instrument using this field.</td></tr><tr><td>>22</td><td>SecurityIDSource</td><td>Y</td><td><p>Identifies class or source of the SecurityID(48) value</p><p><br>99 = Custom</p></td></tr><tr><td>>278</td><td>MDEntryID</td><td>N</td><td><strong>If the MDEntryType(269) =  0 (bid) or 1 (ask)</strong></td></tr><tr><td>>269</td><td>MDEntryType</td><td>Y</td><td><p>Mandatory in case SubscriptionRequestType (263)  = 0 or 1</p><p></p><p>Type of market data to request.</p><p>Available values: <br>0 = Bid</p><p>1 = Ask</p><p>2 = Trade<br>A = Imbalance<br>Q = Auction Equilibrium Price (=Clearing Price) <br>J = Empty Book<br><mark style="color:blue;">NEW v1.33.0</mark> b = Market bid (during auction)</p><p><mark style="color:blue;">NEW v1.33.0</mark> c = Market offer (during auction)</p></td></tr><tr><td>>270</td><td>MDEntryPx</td><td>Y</td><td><p><strong>If the MDEntryType(269) = 0 (bid) &#x26; 1 (ask):</strong></p><p>Price of the order associated with this entry.</p><p>Interpret the value based on the entry type.</p><p><mark style="color:blue;">NEW v1.33.0</mark> b: Market bid &#x26; c: Market offer → Price will be empty.</p><p><br><strong>If the MDEntryType(269) = 2 (Trade):</strong></p><p>Price of the trade associated with this entry.<br></p><p><strong>If the MDEntryType(269) = Q (AuctionClearingPrice):</strong></p><p><a href="https://docs.exberry.io/market-data#auction-indicative-equilibrium-price-message">AuctionIndicativeEP</a>.IndicativePrice</p></td></tr><tr><td>>271</td><td>MDEntrySize</td><td>Y</td><td><p><strong>If the MDEntryType(269) = 0 (bid) &#x26; 1 (ask)</strong></p><p><strong>AND MDUpdateAction(279) != 2 (Delete):</strong></p><p>Quantity of the order associated with this entry.<br><br><strong>If the MDEntryType(269) = 2 (Trade):</strong></p><p>Quantity of the trade associated with this entry.<br></p><p><strong>If the MDEntryType(269) = A (Imbalance):</strong></p><p><a href="https://docs.exberry.io/market-data#auction-indicative-equilibrium-price-message">AuctionIndicativeEP</a>.imbalanceQuantity<br><br><strong>If the MDEntryType(269) = Q (AuctionClearingPrice):</strong></p><p><a href="https://docs.exberry.io/market-data#auction-indicative-equilibrium-price-message">AuctionIndicativeEP</a>.pairedQuantity</p></td></tr><tr><td>>272</td><td>MDEntryDate</td><td>Y</td><td><p>Date of Market Data Entry.</p><p>Timestamp  in UTC Format is YYYYMMDD</p></td></tr><tr><td>>273</td><td>MDEntryTime</td><td>Y</td><td><p>Time of Market Data Entry.</p><p>Timestamp  in UTC in microseconds</p><p>Format is HH:MM:SS.ssssss</p></td></tr><tr><td>>276</td><td>QuoteCondition</td><td>N</td><td><p><strong>If the message is sent as a reply for an imbalance request AND [MDEntryType(269)= 0(Bid) OR 1(Ask)]:</strong></p><p><br>Available values: <br>Z  = Order Imbalance</p></td></tr><tr><td>>277</td><td>TradeCondition</td><td>N</td><td><strong>If the MDEntryType(269) = A (Imbalance):</strong><br><br>Available values: <br>P = Imbalance More Buyers [<a href="https://docs.exberry.io/market-data#auction-indicative-equilibrium-price-message">AuctionIndicativeEP</a>.imbalanceSide=Buy]<br><br>Q = Imbalance More Sellers<br>[<a href="https://docs.exberry.io/market-data#auction-indicative-equilibrium-price-message">AuctionIndicativeEP</a>.imbalanceSide=Sell]<br><br>This field is not populated when <a href="https://docs.exberry.io/market-data#auction-indicative-equilibrium-price-message">AuctionIndicativeEP</a>.imbalanceSide=None or NA</td></tr><tr><td>>1023</td><td>MDPriceLevel</td><td>N</td><td><p><strong>If the message is sent as a reply for an imbalance request AND [MDEntryType(269)= 0(Bid) OR 1(Ask)]:</strong></p><p>Value = 1<br><mark style="color:blue;">NEW v1.33.0</mark> b: Market bid &#x26; c: Market offer → this will be empty.</p></td></tr><tr><td>>1070</td><td>MDQuoteType</td><td>N</td><td><p><strong>If MDEntryType(269) = A (Imbalance) OR Q (AuctionClearingPrice):</strong></p><p></p><p>Available values: </p><p>0 = Indicative</p></td></tr><tr><td>>2446</td><td>AggressorSide</td><td>N</td><td><p><strong>If the MDEntryType(269) = 2 (Trade):</strong></p><p>Mandatory only for trades that are not TradeEntry or cancellation</p><p></p><p>Indicates which side is the aggressor (or if there is no aggressor) </p><ul><li>1 (Buy)</li><li>2 (Sell)</li></ul><p></p><p>If dealer side of RFQ is Buy→ 2 (Sell) else 1 (Buy)<br></p><p>Note: </p><ul><li>Aggressive side = Taker = RFQ Initiator </li><li>Passive side = Maker = RFQ Dealer</li></ul></td></tr><tr><td>442</td><td>MultiLegReportingType</td><td>N</td><td><p><strong>If the MDEntryType(269) = 2 (Trade):</strong></p><p>1 - Single security - not strategy trade </p><p>2 - Individual leg of a multi-leg security</p><p>3 - Multi-leg security - the parent trade of multi-leg security</p></td></tr></tbody></table>

### Security List Request _(MsgType = x)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=89|35=x|49=Target|56=EXBERRY|34=367|52=20230130-09:54:45.064000|263=1|320=1675072485064|559=4|10=214|
```
{% endcode %}

<table><thead><tr><th width="101">Tag</th><th width="186">Name</th><th width="105">Required</th><th>Description</th></tr></thead><tbody><tr><td>320</td><td>SecurityReqID</td><td>Y</td><td>Unique ID for this request</td></tr><tr><td>559</td><td>SecurityListRequestType</td><td>Y</td><td><p>Type of Security List Request being made.</p><p>Available values: <br>4: All Securities (will return all instrument where status = Active</p></td></tr><tr><td>263</td><td>SubscriptionRequestType</td><td>N</td><td><p>Subscription Request Type. </p><p>Available values: <br>0 = Snapshot<br>1 = Snapshot + Updates (Subscribe)<br>2 =  Disable previous Snapshot + Update Request (Unsubscribe)<br><br>Default value if not sent is  0 </p></td></tr></tbody></table>

### Security List _(MsgType = y)_

{% tabs %}
{% tab title="Spot Instrument" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=205|35=y|49=EXBERRY|56=user|34=136|52=20230830-09:49:15.521518|320=777|560=0|893=N|146=1|55=spot|48=240|22=99|167=SPOT|762=Foreign exchange (F)|965=1|107=spot|2576=10|1786=0|562=0.1|1140=99999|15=USD|20011=10|10=231|
```
{% endcode %}
{% endtab %}

{% tab title="Future Instrument" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=329|35=y|49=EXBERRY|56=user|34=137|52=20230830-09:49:15.521518|320=777|560=0|893=N|146=1|55=futures|48=284|22=99|461=abcdef|167=FUT|762=Financial futures (F)|541=20270828|1079=08:10:00Z|965=1|225=20230829|231=10|1193=C|207=abcd|107=Futures instrument|2576=10|1786=0|562=1|1140=99999|15=USD|20010=08:10:00Z|20011=10|20026=F|20027=240|10=037|
```
{% endcode %}
{% endtab %}

{% tab title="Option Instrument" %}
{% code overflow="wrap" %}
```
fixin 8=FIXT.1.1|9=357|35=y|49=EXBERRY|56=user|34=138|52=20230830-09:49:15.521518|320=777|560=0|893=N|146=1|55=option|48=308|22=99|461=ABCDEF|167=OPT|762=Call options (C)|541=20240830|1079=09:44:00Z|965=1|225=20230831|202=10|231=10|1193=C|1194=1|201=1|207=ABCD|107=option instrument|2576=0|1786=0|562=1|1140=99999|15=USD|20010=09:44:00Z|20011=0|20026=O|20027=240|20017=2025-01-01|10=070|
```
{% endcode %}
{% endtab %}

{% tab title="Spread Instrument" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=347|35=y|49=EXBERRY|56=user|34=139|52=20230830-09:49:15.521518|320=777|560=0|893=N|146=1|55=spread|48=309|22=99|461=ABCDEF|167=MLEG|541=20240830|1079=09:45:00Z|965=1|225=20230831|231=10|1193=C|207=ABCD|107=spread instrument|2576=10|1786=0|562=0.1|1140=999999|15=USD|555=2|600=spot|602=240|566=10|600=futures|602=284|20010=09:45:00Z|20011=10|20027=240|10=155|    
```
{% endcode %}
{% endtab %}

{% tab title="Bonds Instrument" %}
{% code overflow="wrap" %}
```
fixin 8=FIXT.1.1|9=349|35=y|49=EXBERRY|56=user|34=140|52=20230830-09:49:15.521518|320=777|560=0|893=Y|146=1|55=bonds|48=310|22=99|461=ABCDEF|167=Other|762=Bonds (B)|541=20240830|1079=09:47:00Z|965=1|225=20230831|207=ABCD|107=bonds instrument|2576=0|40019=1|40000=1|40012=0.05|40013=20240101|40016=12|40017=Mo|1786=0|562=1|1140=99999|15=USD|20010=09:47:00Z|20011=0|20026=Z|10=226| 
```
{% endcode %}
{% endtab %}

{% tab title="Invalid Request Rejection" %}
{% code overflow="wrap" %}
```
8=FIXT.1.1|9=112|35=y|49=EXBERRY|56=user|34=133|52=20230830-09:48:38.452219|320=777|560=1|1607=3|58=SecurityListRequestType(559)|10=081|
```
{% endcode %}
{% endtab %}
{% endtabs %}

<table><thead><tr><th width="115">Tag</th><th width="207">Name</th><th width="109">Required</th><th>Description</th></tr></thead><tbody><tr><td>320</td><td>SecurityReqID</td><td>Y</td><td>Unique Id for the Security Definition Request.</td></tr><tr><td>560</td><td>SecurityRequestResult</td><td>Y</td><td><p>The results returned to a Security Request message</p><p><br></p><p>0 = Valid request</p><p>1 = Invalid or unsupported request</p><p></p></td></tr><tr><td>1607</td><td>SecurityRejectReason</td><td>N</td><td>Identifies the reason a security definition request is being rejected.<br>(will be sent only when SecurityRequestResult(560) = 1)</td></tr><tr><td>893</td><td>LastFragment</td><td>N</td><td><p>Indicates whether this message is the last in a sequence of messages on the current snapshot<br></p><p>N = Not Last Message</p><p>Y = Last Message (of the snapshot)</p><p></p><p>Note: this field will return only from the snapshot (not for updates)</p></td></tr><tr><td>58</td><td>text</td><td>N</td><td>In case of rejection, specifies the error message</td></tr><tr><td>146</td><td>NoRelatedSym</td><td>Y</td><td><p>Specifies the number of repeating symbols specified</p><p><br>Note: In case there is no data and the request was valid, this field won't be returned</p></td></tr><tr><td>>55</td><td>Symbol</td><td>Y</td><td>Instrument Symbol</td></tr><tr><td>>48</td><td>SecurityID</td><td>Y</td><td>Permanent instrument identifier, in case symbol is changed after subscription, you can identify the instrument using this field.</td></tr><tr><td>>22</td><td>SecurityIDSource</td><td>Y</td><td><p>Identifies class or source of the SecurityID(48) value</p><p><br>99 = Custom</p></td></tr><tr><td>>461</td><td>CFICode</td><td>N</td><td>Classification of Financial Instruments codes (CFI) </td></tr><tr><td>>167</td><td>SecurityType</td><td>N</td><td><p>Instrument category, available values: </p><p>FUT (Future)</p><p>OPT (Option)</p><p>SPOT</p><p>MLEG (Multileg Instrument)</p><p>CS (Common Stock)</p><p>Other</p></td></tr><tr><td>>762</td><td>SecuritySubType</td><td>N</td><td><p>The Sub-Catagory of the instrument</p><p>Available values: </p><p></p><p>Foreign exchange (F)</p><p>Commodities (T)</p><p>Financial futures (F)</p><p>Commodities futures (C)</p><p>Call options (C)</p><p>Put options (P)</p><p>Other (miscellaneous) (M)</p></td></tr><tr><td>>541</td><td>MaturityDate</td><td>N</td><td><p>Trading is allowed up to this time.</p><p>format: YYYYMMDD</p></td></tr><tr><td>>1079</td><td>MaturityTime</td><td>N</td><td><p>Trading is allowed up to this time.</p><p>Format is HH:MM:SSZ </p></td></tr><tr><td>>965</td><td>SecurityStatus</td><td>Y</td><td><p>Indicates the current state of the instrument. </p><p><br>2 = Inactive (applicable only in case instrument was disabled after subscription)</p><p> 1 = Active</p></td></tr><tr><td>>225</td><td>IssueDate</td><td>N</td><td><p>Trading is allowed from this time</p><p> format: YYYYMMDD</p></td></tr><tr><td>>202</td><td>StrikePrice</td><td>N</td><td>Strike</td></tr><tr><td>>231</td><td>ContractMultiplier</td><td>N</td><td>Contract size</td></tr><tr><td>>1193</td><td>SettlMethod</td><td>N</td><td><p>Settlement method for a contract or instrument.</p><p><br>C = Cash settlement required<br>P = Physical settlement required<br>E=Election at exercise</p></td></tr><tr><td>>1194</td><td>ExerciseStyle</td><td>N</td><td><p>Type of exercise of a derivatives security.</p><p><br>0 = European<br>1 = American<br>2 = Bermuda<br>99 = Other</p></td></tr><tr><td>>201</td><td>PutOrCall</td><td>N</td><td><p>In case SecurityType(167) = OPT </p><p>Case subCategory :</p><p>Call options (C)   = 1</p><p>Put options (P) = 0</p><p>Other (miscellaneous) (M) = 2</p></td></tr><tr><td>>207</td><td>SecurityExchange</td><td>N</td><td>Market identifier codes (MIC)</td></tr><tr><td>>107</td><td>SecurityDesc</td><td>Y</td><td>Description</td></tr><tr><td>>2576</td><td>InstrumentPricePrecision</td><td>Y</td><td>Price precision, number of decimals allowed in price field</td></tr><tr><td>864</td><td>NoEvents</td><td>N</td><td><p>Repeating group</p><p>Number of events in the repeating group.</p></td></tr><tr><td>>865</td><td>EventType</td><td>N</td><td><p>Code to represent the type of event</p><p>13 = First delivery date (deliveryStartDate)</p><p>14 = Last delivery date (deliveryEndDate)</p></td></tr><tr><td>>866</td><td>EventDate</td><td>N</td><td><p>Per the value in EventType (865):</p><ul><li>When 13: deliveryStartDate</li><li>When 14: deliveryEndDate</li></ul><p>Format :  YYYYMMDD</p></td></tr><tr><td>>40019</td><td>NoAdditionalTerms</td><td>N</td><td>Number of additional terms in the repeating group.</td></tr><tr><td>>>40000</td><td>NoAdditionalTermBondRefs</td><td>N</td><td>Optional Number of bonds in the repeating group.</td></tr><tr><td>>>>40012</td><td>AdditionalTermBondCouponRate</td><td>N</td><td>Coupon (NOTE: 0.05 represents 5% and 0.9525 represents 95.25%)</td></tr><tr><td>>>>40013</td><td>AdditionalTermBondMaturityDate</td><td>N</td><td><p>MaturityDate </p><p>format: YYYYMMDD</p></td></tr><tr><td>>>>40016</td><td>AdditionalTermBondCouponFrequencyPeriod</td><td>N</td><td>Period of coupon</td></tr><tr><td>>>>40017</td><td>AdditionalTermBondCouponFrequencyUnit</td><td>N</td><td><p>Time unit associated with the frequency of the bond's coupon payment. </p><p>Mo = Month</p></td></tr><tr><td>>1148</td><td>LowLimitPrice</td><td>N</td><td>Min price</td></tr><tr><td>>1149</td><td>HighLimitPrice</td><td>N</td><td>Max price</td></tr><tr><td>>1786</td><td>TradeVolType</td><td>Y</td><td><p>Define the type of trade volume applicable for the MinTradeVol(562) and MaxTradeVol(1140)</p><p><br></p><p>0 = Number of units</p></td></tr><tr><td>>562</td><td>MinTradeVol</td><td>Y</td><td>The minimum order quantity</td></tr><tr><td>>1140</td><td>MaxTradeVol</td><td>Y</td><td>The maximum order quantity</td></tr><tr><td>>15</td><td>Currency</td><td>Y</td><td>Quote currency</td></tr><tr><td>>555</td><td>NoLegs</td><td>N</td><td>Number of legs that make up the Security</td></tr><tr><td>>>600</td><td>LegSymbol</td><td>N</td><td>The symbol of the leg</td></tr><tr><td>>>602</td><td>LegSecurityID</td><td>N</td><td>Legs Instrument Id</td></tr><tr><td>>>566</td><td>LegPrice</td><td>N</td><td>Leg 1 Reference Price</td></tr><tr><td>>20010</td><td>Custom tag</td><td>N</td><td>Trading is allowed from this timeFormat is HH:MM:SSZ </td></tr><tr><td>>20011</td><td>Custom tag</td><td>N</td><td>Quantity Precision</td></tr><tr><td>>20012</td><td>Custom tag</td><td>N</td><td>Quantity Increment</td></tr><tr><td>>20013</td><td>Custom tag</td><td>N</td><td>Tick Min Price Percentage</td></tr><tr><td>>20014</td><td>Custom tag</td><td>N</td><td>Tick Max Price Percentage</td></tr><tr><td>>20015</td><td>Custom tag</td><td>N</td><td>Daily Min Price Percentage</td></tr><tr><td>>20016</td><td>Custom tag</td><td>N</td><td>Daily Max Price Percentage</td></tr><tr><td>>20026</td><td>Custom tag</td><td>N</td><td><p>Underlying Assets<br>The value will be the same as the value in <a href="https://documenter.getpostman.com/view/6229811/TzCV3jcq#f11697b8-fc27-4c58-90d1-b15e73f47de3">UnderlyingAssets List</a></p><p>Example- </p><p>Category (Spot) &#x26; Sub Category (Commodities) →</p><p>Agriculture (A)</p><p><br></p></td></tr><tr><td>>20027</td><td>Custom tag</td><td>N</td><td>Underlying Instrument Id</td></tr><tr><td>>20017</td><td>Custom tag</td><td>N</td><td>Expiry Date Format is YYYY-MM-DD</td></tr><tr><td>>20018</td><td>Custom tag</td><td>N</td><td>Max Order Value</td></tr><tr><td>>20030</td><td>Custom tag</td><td>N</td><td>Number of tick size items in one entry in the tick table repeating group</td></tr><tr><td>>>20031</td><td>Custom tag</td><td>N</td><td>Price range</td></tr><tr><td>>>20032</td><td>Custom tag</td><td>N</td><td>Tick size</td></tr></tbody></table>

### Security Status Request _(MsgType = e)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=92|35=e|49=Target|56=EXBERRY|34=407|52=20230130-10:14:40.763000|55=INS1|263=1|324=1675073680763|10=036|
```
{% endcode %}

<table><thead><tr><th width="101">Tag</th><th width="248">Name</th><th width="116">Required</th><th>Description</th></tr></thead><tbody><tr><td>324</td><td>SecurityStatusReqID</td><td>Y</td><td>Unique ID for this security request.</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr><tr><td>263</td><td>SubscriptionRequestType</td><td>Y</td><td>Subscription Request Type. <br>0 = Snapshot<br>1 = Snapshot + Updates (Subscribe)<br>2 =  Disable previous Snapshot + Update Request (Unsubscribe)<br><br>Default value if not sent is  0 </td></tr></tbody></table>

### Security Status _(MsgType = f)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=94|35=f|49=EXBERRY|56=Target|34=412|52=20230130-10:14:40.781401|324=1675073680763|55=INS1|326=100|10=137|
```
{% endcode %}

<table><thead><tr><th width="101">Tag</th><th width="249">Name</th><th width="96">Required</th><th>Description</th></tr></thead><tbody><tr><td>324</td><td>SecurityStatusReqID</td><td>Y</td><td>Unique ID for this security request.</td></tr><tr><td>55</td><td>Symbol</td><td>Y</td><td>Instrument symbol</td></tr><tr><td>326</td><td>SecurityTradingStatus</td><td>Y</td><td><p>Represents a specific market status for security.</p><p><br>100 = Trading<br>101 = Closed<br>102 = Auction call<br>103 = Auction crossing<br>104 = Halted</p></td></tr></tbody></table>
