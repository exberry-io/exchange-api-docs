# Trading

## placeOrder

The `placeOrder` API lets you place a new order into exchange.

If you send a valid order, you should receive a response with "Pending" status, this means that order was validated and accepted. The response contains the exchange orderId which should be stored and used for later status changes, notified via the orderBookDepth stream.

Non-valid order will be responded with error message.

In case that market participant system didn't get a response within the timeout schedule the market participant should do the one of the below:

* **Retry** sending same order using same `mpOrderId` which will result:
  * Rejection in case that previous order was accepted
  * Success in case that previous order was not accepted
* **Cancel** the order using `mpOrderId` which will assure that order is no longer active (it might be already executed)

`mpOrderId` is unique identifier and should not be used for more than a single order. Note: system might accept already used `mpOrderId` after predefined period but it is not recommended to use same `mpOrderId` twice.

Order types:

* **Limit**: Order is being sent with a specific price. A buy order will be executed with the requested price or lower price a sell order will be executed with the requested price or higher price.
* **Market**: Order is attempted filled at the best price in the market. Partial filled is allowed. In case not all the amount can be filled, the residual amount will be cancelled.
* **Stop**: A Stop Order is a Market Order that will remain Suspended (will not be entered into order book) until the stop price is reached. Once triggered, it will be treated similar to a regular Market Order. ([Here](broken-reference))
* **StopLimit**: A Stop Limit Order is a Limit Order that will remain Suspended (will not be entered into order book) until the stop price is reached. Once triggered, a Stop Limit Order will be treated as a regular Limit Order. ([Here](broken-reference))

For more details about the **time in force** - see [here](../other/business-features/time-in-force.md)



{% hint style="info" %}
qualifier: `v1/exchange.market/placeOrder`
{% endhint %}

### **Request**

<table><thead><tr><th width="166.33333333333331">Parameter</th><th width="104">Type</th><th>Description</th></tr></thead><tbody><tr><td>mpOrderId</td><td>Long</td><td>Unique market participant order ID</td></tr><tr><td>orderType</td><td>Enum</td><td>Order type Limit or Market or Stop or StopLimit or <mark style="color:blue;">(NEW v1.38.0)</mark> MarketToLimit</td></tr><tr><td>side</td><td>Enum</td><td>Order side Buy or Sell</td></tr><tr><td>instrument</td><td>String</td><td>Instrument identifier</td></tr><tr><td>quantity</td><td>Decimal</td><td>Order quantity</td></tr><tr><td><mark style="color:blue;">(NEW v1.34.0)</mark><br>displayMethod</td><td>Enum</td><td><p>Optional</p><p>Required if <code>displayQuantity</code> exists.</p><p>Defines the behavior of display quantity</p><p><br>Available values:</p><ul><li>DisplayQuantity = Displays the quantity in displayQuantity</li><li>Hidden = Fully hidden order (displayQuantity is 0)</li><li><mark style="color:blue;">(NEW v1.35.0)</mark> Random = Displays the quantity in displayQuantity initially, then a quantity between displayLowQuantity and displayHighQuantity when replenished</li></ul></td></tr><tr><td><mark style="color:blue;">(NEW v1.34.0)</mark><br>displayQuantity</td><td>Decimal</td><td><p>Optional</p><p>Required if <code>displayMethod</code> exists.</p><p>The quantity to be displayed via public market data. </p><p>If it does not exist, the order is fully visible.</p></td></tr><tr><td><mark style="color:blue;">(NEW v1.35.0)</mark><br>displayLowQuantity</td><td>Decimal</td><td>Optional Required if displayMethod=Random The lower quantity limit to randomise the display quantity at replenishment</td></tr><tr><td><mark style="color:blue;">(NEW v1.35.0)</mark><br>displayHighQuantity</td><td>Decimal</td><td>Optional Required if displayMethod=Random The upper quantity limit to randomise the display quantity at replenishment</td></tr><tr><td>price <code>optional</code></td><td>Decimal</td><td>The price of the Limit order/ StopLimit. <br>For Market and Stop orders, this will not be sent.</td></tr><tr><td>stopPrice <code>optional</code></td><td>Decimal</td><td><p>Required for Stop &#x26; StopLimit orders. </p><p>The price at which the order will be injected to the market.</p></td></tr><tr><td>timeInForce</td><td>Enum</td><td><p>Allowed values: GTC, GTD, FOK, IOC, DAY, GAA, <mark style="color:blue;">(NEW v1.41.0)</mark> OPG, ATC<br></p><p>For more details about allowed <code>orderType</code> and <code>timeInForce</code> see <a href="../other/business-features/time-in-force.md">here</a></p></td></tr><tr><td><p>expiryDate</p><p><code>optional</code></p></td><td>UTC Time</td><td><p>Required if TimeInForce = GTD and expiryDay is not specified - expiration date and time in seconds, in GMT.</p><p><br>In case that trading is halted or market is closed at expiry date - the order will be cancelled (even that cancelOrder requests are not allowed).</p></td></tr><tr><td>expiryDay<br><code>optional</code></td><td>String</td><td><p>Required if TimeInForce = GTD and expiryDate is not specified.</p><p>format: YYYY-MM-DD<br><br>Order will expire in the same way the DAY order expired for that day.</p></td></tr><tr><td>accountType<br><code>optional</code></td><td>Enum</td><td>Account type Client or House<br><br>This field can be configured as a mandatory field</td></tr><tr><td>parties <code>optional</code></td><td>[] Object</td><td>Array of party objects, see details below.<br>Max number of parties =20 </td></tr><tr><td>minQuantity<br><code>optional</code></td><td>Decimal</td><td><p>Minimum quantity of an IOC order to be executed.</p><p><br></p></td></tr><tr><td>stpAction<br><code>optional</code></td><td>Enum</td><td><p>The action that should be taken in order to prevent a Self-trade. Possible actions:</p><ul><li>CancelIncomingOrder</li><li>CancelRestingOrder</li><li>CancelBothOrders</li></ul></td></tr></tbody></table>

Party specification:&#x20;

<table><thead><tr><th width="112">Name</th><th width="165.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>String</td><td>Party id<br>Max length = 20 <br><mark style="color:blue;">(CHANGED v1.38.0)</mark> Allowed characters <del>[a-z,A-Z,0-9,”.”,”-”,”^”,”/””=”, “ ”,"_",":"]</del> ASCII (32 -126)</td></tr><tr><td>source</td><td>Char</td><td>Party source<br>Allowed characters [0-9,a-z,A-Z]</td></tr><tr><td>role</td><td>Int</td><td>Party role </td></tr></tbody></table>

<mark style="color:blue;">(NEW v1.34.0)</mark> See [here](private-data-api.md#new-v1.34.0-account-assignment) for parties related to Account Assignment.

Mandatory Parties:

Specific parties(example, role=8, source=A) can be configured as mandatory. If configured as mandatory, they will be required in all requests with `parties` field.&#x20;

Sample:

```json
{
    "parties": [
      {
        "id": "ID123",
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



### **Response**

| Parameter   | Type   | Description           |
| ----------- | ------ | --------------------- |
| orderId     | Long   | Exchange Order ID     |
| orderStatus | String | Order status: Pending |

### **Error Codes**

<table><thead><tr><th width="128">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1000</td><td><p><code>Missing fields: [Fieldname]</code> or</p><p><code>Only one of expiryDay and expiryDate is allowed</code> or<br><code>Party of source=[ConfiguredParty.source] and role=[ConfiguredParty.role] is required</code></p></td></tr><tr><td>1001</td><td><p><code>Order must contain a positive quantity</code> or<br><code>Market/</code>Stop <code>order must not specify price</code> or<br><code>Wrong orderType</code> or<br><code>Wrong side</code> or<br><code>Wrong mpOrderId</code> or<br><code>Wrong userId</code> or<br><code>Wrong timeInForce</code> or<br><code>Wrong expiryDate</code>or<br><code>Latests expiryDate is one year</code>or</p><p><code>Wrong [FieldName]</code> or</p><p><code>[FieldName] must be &#x3C;= [maxLong/10^ fieldPrecision]</code> or<br><code>[FieldName] must >= [minLong/10^ fieldPrecision]</code><br><code>minQuantity is allowed only for IOC</code><br><code>Should be: [Min Order Quantity] &#x3C;= minQuantity &#x3C;= [Order quantity]</code><br><code>Order is not allowed</code><br><mark style="color:blue;">(NEW v1.34.0)</mark> <code>Wrong displayMethod</code><br><mark style="color:red;">(REMOVED v1.39.0)</mark> <mark style="color:blue;">(NEW v1.34.0)</mark> <del><code>[FieldName] not allowed with orderType and timeInForce</code></del><br><mark style="color:blue;">(NEW v1.34.0)</mark> <code>displayQuantity must be &#x3C; quantity</code><br><mark style="color:blue;">(NEW v1.34.0)</mark> <code>displayMethod Hidden is not allowed</code><br><mark style="color:blue;">(NEW v1.34.0)</mark> <code>Only displayMethod Hidden is allowed</code><br><mark style="color:blue;">(NEW v1.34.0)</mark> <code>displayMethod allowed only during continuous trading</code><br><mark style="color:red;">(REMOVED v1.35.0)</mark> <mark style="color:blue;">(NEW v1.34.0)</mark> <del><code>Reserve orders are not allowed when Implied Orders are configured</code></del><br><mark style="color:blue;">(NEW v1.39.0)</mark> <code>orderType and timeInForce is not allowed with displayMethod</code></p></td></tr><tr><td>1002</td><td><code>mpOrderId is already in use</code></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1005</td><td><code>Price precision is[PricePrecision]</code> or<br><code>Quantity precision is[QuantityPrecision]</code></td></tr><tr><td>1006</td><td><p><code>Minimum quantity is [MinOrderQuantity]</code>or</p><p><code>Maximum quantity is [MaxOrderQuantity]</code>or<br><code>Quantity increment is [quantityIncrement]</code></p></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1010</td><td><code>Instrument [Instrument] not found</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1012</td><td><code>Price breaches [FieldName] of [FieldValue]</code></td></tr><tr><td>1013 </td><td><code>Maximum order value is [maxOrderValue]</code></td></tr><tr><td>1014 </td><td><code>Price tick size is [tickSize]</code></td></tr><tr><td>1015</td><td><code>You are not allowed place [orderSide] orders in current auction</code></td></tr><tr><td>1016</td><td><code>[oppositeOrderSide] order was not yet placed</code></td></tr><tr><td>1030</td><td><p></p><p><mark style="color:blue;">CHANGED v1.33.0</mark> <code>Only [Allowed TIF] </code><del><code>limit orders</code></del><code> are allowed during auction</code> or</p><p><mark style="color:blue;">NEW v1.33.0</mark> <code>Only market or limit orders are allowed During Auction</code> or</p><p><code>Trading is not allowed during crossing</code></p></td></tr><tr><td>1031</td><td><code>GAA is allowed only during auction</code></td></tr><tr><td>1032</td><td><code>Party is not allowed</code><br><code>Account not found</code></td></tr><tr><td><mark style="color:blue;">NEW v1.33.0</mark><br>1033</td><td><code>minQuantity is allowed only during continuous trading</code></td></tr><tr><td>1001</td><td><code>[Limit|Market] order must not specify stopPrice</code></td></tr><tr><td>1001</td><td><code>For FOK or IOC only CancelIncomingOrder is allowed</code></td></tr><tr><td>1001</td><td><code>STP is not allowed</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
    "q":"v1/exchange.market/placeOrder",
    "sid": 1,
    "d":{
           "mpOrderId": 123,   
           "orderType": "Limit",
           "side": "Buy",
           "instrument": "BTC",
           "quantity": 123,
           "price": 100.555,
           "timeInForce": "GTC"
    }
}
```
{% endtab %}

{% tab title="Success Response" %}
```javascript
{
    "q":"v1/exchange.market/placeOrder",
    "sid":1,
    "d":{
           "orderId":2345,
           "orderStatus":"Pending"
       }

}
```
{% endtab %}

{% tab title="Failure Response" %}
```javascript
{
  "sig": 2,
  "q": "v1/exchange.market/placeOrder",
  "errorType": "500",
  "sid": 1,
  "d": {
    "errorCode": 1010,
    "errorMessage": "Instrument AMZ11 not found"
  }
}
```
{% endtab %}
{% endtabs %}

## cancelOrder

The `cancelOrder` API is used to request that an order be cancelled.

If you send a valid order to cancel, you should receive a response that confirms that order was cancelled. This means that remaining open quantity of the order was cancelled.

Non-valid cancel order will be responded with the error message.

{% hint style="info" %}
qualifier: `v1/exchange.market/cancelOrder`
{% endhint %}

### **Request**

<table><thead><tr><th width="254">Parameter</th><th width="115.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>orderId <code>optional</code></td><td>Long</td><td>Exchange Order ID. Not mandatory if <code>mpOrderId</code> was sent</td></tr><tr><td>mpOrderId <code>optional</code></td><td>Long</td><td>Market participant order ID. Not mandatory if <code>orderId</code> was sent</td></tr><tr><td>instrument</td><td>String</td><td>Instrument identifier</td></tr></tbody></table>

### **Response**

| Parameter | Type | Description       |
| --------- | ---- | ----------------- |
| orderId   | Long | Exchange Order ID |

### **Error Codes**

<table><thead><tr><th width="129">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1001 </td><td><code>Wrong [FieldName]</code></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1011 </td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><code>Trading is not allowed during crossing</code></td></tr><tr><td>1100</td><td><code>Order not found for that instrument</code></td></tr><tr><td>1103</td><td><code>Missing fields: [Fieldname]</code></td></tr><tr><td>1104</td><td><code>Please use only one from orderId or mpOrderId</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```javascript
{
        "q":"v1/exchange.market/cancelOrder",
        "sid": 1,
        "d":{
               "orderId":3456,
               "instrument": "BTC"
        }
}
```
{% endtab %}

{% tab title="Success Response" %}
```javascript
{
        "q":"v1/exchange.market/cancelOrder",
        "sid":1,
        "d":{
               "orderId":3456
        }
}
```
{% endtab %}

{% tab title="Failure Response" %}
```javascript
{
  "sig": 2,
  "q": "v1/exchange.market/cancelOrder",
  "errorType": "500",
  "sid": 1,
  "d": {
    "errorCode": 1100,
    "errorMessage": "Order not found for that instrument"
  }
}
```
{% endtab %}
{% endtabs %}

## massCancel

The `massCancel` API is used to cancel all the active order and suspended orders, <mark style="color:blue;">(NEW v1.30.0)</mark> active RFQs and Quotes for specific instrument for specific market participant.

MPs can specify the targetParties they wish to target the mass order cancel by specifying the targetParties.&#x20;

If no targetParties are defined: the system cancels all orders of the symbol of the MP.

If multiple targetParty objects are in a request, the system cancels orders with all targetParties.

_For example:_

* _Order1 has party1_
* _Order2 has party1 and party2_
* _Order3 has party1, party2 and party3_
* _Order4 has no parties_

_A mass cancel request with targetParties(party1 and party2) will cancel order 2 and 3._

If you send a valid request, you should receive a response that confirms the number of orders that were cancelled.

Non-valid cancel order will be responded with the error message.

{% hint style="info" %}
qualifier: `v1/exchange.market/massCancel`
{% endhint %}

### **Request**

<table><thead><tr><th width="182">Parameter</th><th width="185.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>instrument<br><code>optional</code></td><td>String</td><td>Instrument identifier<br><br><mark style="color:blue;">(CHANGED v1.31.0)</mark> Mandatory if targetScope=Instrument, else the value is ignored.</td></tr><tr><td>targetParties<br><code>optional</code></td><td>[] TargetParty Objects</td><td>Array of party objects, see details below.<br>Max number of targetParties =20 </td></tr><tr><td><mark style="color:blue;">(NEW v1.30.0)</mark><br>targetEntities<br><code>optional</code></td><td>[] Enum</td><td><p>The system cancels the type of active entities mentioned in the request.<br></p><p>Array of below values:</p><p><code>Order</code> - cancels all orders</p><p><code>RFQ</code> - cancels all RFQs and all active quotes belong to them</p><p><code>Quote</code> - cancels all Quotes<br><mark style="color:blue;">(NEW v1.31.0)</mark> <code>AllegedTrade</code> - cancels all Alleged Trades<br><br>If not exist, system considers as <code>targetEntities = [‘Order’]</code></p></td></tr><tr><td><mark style="color:blue;">(NEW v1.31.0)</mark><br>targetScope<br><code>optional</code></td><td>Enum</td><td><p>The field specifies the scope the mass cancel request applies to<br></p><p>Allowed values:</p><ul><li><code>Instrument</code> - cancel scope is a single instrument</li><li><code>All</code> - cancel scope is all instruments</li></ul><p>If not sent “Instrument” is considered.</p></td></tr></tbody></table>

targetParty specification:&#x20;

<table><thead><tr><th width="180">Name</th><th width="120.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>string</td><td>TargetParty id<br><mark style="color:blue;">(CHANGED v1.38.0)</mark> Allowed characters <del>[a-z,A-Z,0-9,”.”,”-”,”^”,”/””=”, “ ”,"_", ":"]</del> ASCII (32 -126)</td></tr><tr><td>source</td><td>char</td><td>TargetParty source<br>Allowed characters [0-9,a-z,A-Z]</td></tr><tr><td>role</td><td>int</td><td>TargetParty role</td></tr></tbody></table>



### **Response**

<table><thead><tr><th width="192">Parameter</th><th width="96.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>numberOfOrders<br><mark style="color:blue;">(CHANGED v1.30.0)</mark> <code>optional</code></td><td>Int</td><td>Number of orders that were cancelled, will be returned only in case it was targeted on request.</td></tr><tr><td><mark style="color:blue;">(NEW v1.30.0)</mark><br>numberOfRfqs<br><code>optional</code></td><td>Int</td><td>Number of RFQs that were cancelled, will be returned only in case it was targeted on request.</td></tr><tr><td><mark style="color:blue;">(NEW v1.30.0)</mark><br>numberOfQuotes<br><code>optional</code></td><td>Int</td><td>Number of Quotes that were cancelled, will be returned only in case it was targeted on request.</td></tr><tr><td><mark style="color:blue;">(NEW v1.31.0)</mark><br>numberOfAllegedTrades<br><code>optional</code></td><td>Int</td><td>Number of Alleged Trades that were cancelled, will be returned only in case it was targeted on request.</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="129">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1001 </td><td><p><code>Wrong [FieldName]</code> or</p><p><mark style="color:blue;">(NEW v1.31.0)</mark> <code>Missing or invalid parameter: [FieldName]</code></p></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1010</td><td><code>Instrument [Instrument] not found</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><code>Trading is not allowed during crossing</code></td></tr><tr><td>1032</td><td><code>Account not found</code><br><code>Party is not allowed</code></td></tr><tr><td>1103</td><td><code>Missing fields: [Fieldname]</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```javascript
{
  "q": "v1/exchange.market/massCancel",
  "sid": 1,
  "d": {
    "instrument": "ABC",
    "targetEntities": ["Order", "RFQ"]
  }
}
```
{% endtab %}

{% tab title="Success Response" %}
```javascript
{
  "q": "v1/exchange.market/massCancel",
  "sid": 1,
  "d": {
    "numberOfOrders": 5,
    "numberOfRfqs": 7
  }
}
```
{% endtab %}

{% tab title="Failure Response" %}
```javascript
{
  "sig": 2,
  "q": "v1/exchange.market/massCancel",
  "errorType": "500",
  "sid": 1,
  "d": {
    "errorCode": 1010,
    "errorMessage": "Instrument XYZ not found"
  }
}
```
{% endtab %}
{% endtabs %}

## modifyOrder

The `modifyOrder` API is used to reduce the order quantity without losing the time priority.

<mark style="color:blue;">(NEW v1.39.0)</mark> You can also reduce displayQuantity along with quantity as long as Delta.quantity ≥ Delta.displayQuantity (to prevent transfers from working display quantity to working hidden quantity).

If you send a valid order to modify, you should receive a response that confirms that order was modified. This means that remaining open quantity of the order was reduced.

Non-valid modify order requests will be responded with the error message.

{% hint style="info" %}
qualifier: `v1/exchange.market/modifyOrder`
{% endhint %}

### **Request**

| Parameter                                                                 | Type    | Description                                |
| ------------------------------------------------------------------------- | ------- | ------------------------------------------ |
| orderId                                                                   | Long    | Exchange Order ID                          |
| instrument                                                                | String  | Instrument identifier                      |
| quantity                                                                  | Decimal | New order quantity                         |
| <p><mark style="color:blue;">(NEW v1.39.0)</mark> <br>displayQuantity</p> | Decimal | <p>Optional</p><p>New display quantity</p> |

### **Response**

| Parameter | Type | Description       |
| --------- | ---- | ----------------- |
| orderId   | Long | Exchange Order ID |

### **Error Codes**

<table><thead><tr><th width="140">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1001</td><td><p><code>Wrong [FieldName]</code> or</p><p><code>Order must contain a positive quantity</code><br><mark style="color:blue;">(NEW v1.39.0)</mark> <code>displayQuantity must be &#x3C; quantity</code><br><mark style="color:blue;">(NEW v1.39.0)</mark> <code>displayQuantity must be >= 0</code></p></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1005</td><td><code>Quantity precision is[QuantityPrecision]</code></td></tr><tr><td>1006</td><td><code>Quantity increment is [quantityIncrement]</code> or<br><code>Minimum quantity is [MinOrderQuantity]</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><code>Trading is not allowed during crossing</code></td></tr><tr><td>1100</td><td><code>Order not found for that instrument</code></td></tr><tr><td>1103</td><td><code>Missing fields: [Fieldname]</code></td></tr><tr><td>1301</td><td><p><code>Quantity must be less than [remainaingActiveQuantity]]</code><br><mark style="color:blue;">(CHANGED v1.41.0 NEW v1.39.0)</mark> <del><code>Display quantity must be less than [Order.displayQuantity]</code></del><code> Display quantity must be &#x3C;= [Order.displayQuantity]</code></p><p><mark style="color:blue;">(CHANGED v1.41.0 NEW v1.39.0)</mark> <del><code>RemovedQuantity must be ≥ removedDisplayQuantity</code></del><code> RemovedQuantity must be >= removedDisplayQuantity</code></p></td></tr><tr><td><del>1019</del></td><td><del><code>Not allowed on stop or stop limit orders</code></del></td></tr><tr><td><mark style="color:red;">(REMOVED v1.39.0)</mark> <br><mark style="color:blue;">(NEW v1.34.0)</mark> <br><del>1034</del></td><td><del><code>Changes to the reserve orders are not allowed</code></del></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```javascript
{
        "q":"v1/exchange.market/modifyOrder",
        "sid": 1,
        "d":{
               "orderId":3456,
               "instrument": "BTC",
               "quantity": 50
        }
}
```
{% endtab %}

{% tab title="Success Response" %}
```javascript
{
        "q":"v1/exchange.market/modifyOrder",
        "sid":1,
        "d":{
               "orderId":3456
        }
}
```
{% endtab %}

{% tab title="Failure Response" %}
```javascript
{
  "sig": 2,
  "q": "v1/exchange.market/modifyOrder",
  "errorType": "500",
  "sid": 1,
  "d": {
    "errorCode": 1100,
    "errorMessage": "Order not found for that instrument"
  }
}
```
{% endtab %}
{% endtabs %}

## replaceOrder

The `replaceOrder` API is used to change a few of the order parameters in a single command for order which is resting on the book.&#x20;

In case the request is eligible for [modification](trading-api.md#modifyorder)  (other fields may present with the same values as the original order or be omitted from request), the system modifies the existing order, the behavior is similar to [modifyOrder](trading-api.md#modifyorder).

Any other request would result in the system canceling the existing order and creating a new one. This will result in losing the time priority as it cancels the previous order and places a new one.

If you send a valid order to be replaced, you should receive a response that confirms the action.

Non-valid replace order requests will be responded with the error message.

{% hint style="info" %}
qualifier: `v1/exchange.market/replaceOrder`
{% endhint %}

### **Request**

<table><thead><tr><th>Parameter</th><th width="169.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>orderId</td><td>Long</td><td>Exchange Order ID</td></tr><tr><td>instrument</td><td>String</td><td>Instrument identifier</td></tr><tr><td>quantity</td><td>Decimal</td><td>New order quantity (can be more or less the original order quantity)</td></tr><tr><td>price</td><td>Decimal</td><td>New order price<br>Required If quantity >= remainingOpenQuantity of original order</td></tr><tr><td>timeInForce <code>optional</code></td><td>eNum</td><td>New order time in force</td></tr><tr><td>expiryDate <code>optional</code></td><td>UTC Time</td><td>Required if TimeInForce = GTD and expiryDay is not specified.</td></tr><tr><td>expiryDay<br><code>optional</code></td><td>String</td><td><p>Required if TimeInForce = GTD and expiryDate is not specified.</p><p>format: YYYY-MM-DD</p></td></tr><tr><td>stopPrice  <code>optional</code></td><td>Decimal</td><td><p>Required for Stop &#x26; StopLimit orders. </p><p>The price at which the order will be injected to the market.</p></td></tr><tr><td>minQuantity <code>optional</code></td><td>Decimal</td><td>Minimum quantity of an IOC order to be executed.</td></tr><tr><td>stpAction<br><code>optional</code></td><td>Enum</td><td><p>The action that should be taken in order to prevent a Self-trade. Possible actions:</p><ul><li>CancelIncomingOrder</li><li>CancelRestingOrder</li><li>CancelBothOrders</li></ul></td></tr><tr><td><mark style="color:blue;">(NEW v1.39.0)</mark> <br>displayQuantity</td><td>Decimal</td><td>New display quantity </td></tr><tr><td><mark style="color:blue;">(NEW v1.39.0)</mark> <br>displayLowQuantity</td><td>Decimal</td><td>New display low quantity </td></tr><tr><td><mark style="color:blue;">(NEW v1.39.0)</mark> <br>displayHighQuantity</td><td>Decimal</td><td>New display high quantity </td></tr></tbody></table>

### **Response**

| Parameter       | Type | Description                             |
| --------------- | ---- | --------------------------------------- |
| originalOrderId | Long | Exchange Order ID of the original order |
| newOrderId      | Long | Exchange Order ID of the new order      |

### **Error Codes**

<table><thead><tr><th width="138">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1000</td><td><p><code>Missing fields: [Fieldname]</code> or</p><p>Only one of expiryDay and expiryDate is allowed</p></td></tr><tr><td>1001</td><td><p><code>Wrong [FieldName]</code> or</p><p><code>Order must contain a positive quantity</code> or<br><code>Limit order must contain a positive price</code>or<br><code>Latest expiryDate is one year</code> or<br><code>Wrong timeInForce</code> or <br><code>Wrong expiryDate</code> or<br><code>Order [FieldName] must be less than [maxLong/10^ fieldPrecision]</code> or<br><code>Order [FieldName] must be above than [minLong/10^ fieldPrecision]</code><br><code>minQuantity is allowed only for IOC TIF</code><br><code>Should be: [Min Order Quantity] &#x3C;= minQuantity &#x3C;= [Order quantity]</code><br><mark style="color:blue;">(NEW v1.39.0)</mark> <code>orderType and timeInForce is not allowed with displayMethod</code><br><mark style="color:blue;">(NEW v1.39.0)</mark> <code>displayQuantity must be &#x3C; quantity</code><br><mark style="color:blue;">(NEW v1.39.0)</mark> <code>Quantity fields must be displayLowQuantity &#x3C; displayHighQuantity &#x3C; quantity</code></p></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1005</td><td><code>Quantity precision is[QuantityPrecision]</code> or<br><code>Price precision is[PricePrecision]</code></td></tr><tr><td>1006</td><td><code>Minimum quantity is [MinOrderQuantity]</code>or<br><code>Maximum quantity is [MaxOrderQuantity]</code>or<br><mark style="color:blue;">(CHANGED v1.39.0)</mark> <del><code>Quantity</code></del><code>[FieldName] increment is [quantityIncrement]</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1010 </td><td><code>Instrument [Instrument] not found</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1012</td><td><code>Price breaches [FieldName] of [FieldValue]</code></td></tr><tr><td>1013</td><td><code>Maximum order value is [maxOrderValue]</code></td></tr><tr><td>1014</td><td>​<code>Price tick size is [tickSize]</code></td></tr><tr><td>1017</td><td><code>This order exceeds maxDepth</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><p><mark style="color:blue;">Changed v1.33.0</mark><code>Only [Allowed TIF] </code><del><code>limit orders</code></del><code> are allowed during auction</code>or</p><p><mark style="color:blue;">NEW v1.33.0</mark><code>Only market or limit orders are allowed During Auction</code>or</p><p><code>Trading is not allowed during crossing</code> </p></td></tr><tr><td>1031 </td><td><code>GAA is allowed only during auction</code></td></tr><tr><td>1100</td><td><code>Order not found for that instrument</code></td></tr><tr><td>1019</td><td><code>Stop or Stop limit can be replaced only while suspended</code></td></tr><tr><td>1001</td><td><code>[Limit|Market] order must not specify stopPrice</code></td></tr><tr><td>1001</td><td><code>STP is not allowed</code></td></tr><tr><td>1001</td><td><code>For FOK or IOC only CancelIncomingOrder is allowed</code></td></tr><tr><td><mark style="color:blue;">NEW V1.33.0</mark><br>1033</td><td><code>minQuantity is allowed only during continuous trading</code></td></tr><tr><td><mark style="color:red;">(REMOVED v1.39.0)</mark><br><mark style="color:blue;">(NEW v1.34.0)</mark> <br><del>1034</del></td><td><del><code>Changes to the reserve orders are not allowed</code></del></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```javascript
{
"q": "v1/exchange.market/replaceOrder",
"sid": 1,
  "d": {
    "orderId": 709,
    "instrument": "INS1",
    "quantity": 10.5,
    "price": 55
  }
}
```
{% endtab %}

{% tab title="Success Response" %}
```javascript
{
  "q": "v1/exchange.market/replaceOrder",
  "sid": 1,
  "d": {
    "originalOrderId": 709,
    "newOrderId": 710
  }
}
```
{% endtab %}

{% tab title="Failure Response" %}
```javascript
{
  "sig": 2,
  "q": "v1/exchange.market/replaceOrder",
  "errorType": "500",
  "sid": 1,
  "d": {
    "errorCode": 1100,
    "errorMessage": "Order not found for that instrument"
  }
}
```
{% endtab %}
{% endtabs %}
