# Trading API

placeOrder

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
* **Stop**: A Stop Order is a Market Order that will remain Suspended (will not be entered into order book) until the stop price is reached. Once triggered, it will be treated similar to a regular Market Order. ([Here](trading-api.md#stop-and-stop-limit-orders))
* **StopLimit**: A Stop Limit Order is a Limit Order that will remain Suspended (will not be entered into order book) until the stop price is reached. Once triggered, a Stop Limit Order will be treated as a regular Limit Order. ([Here](trading-api.md#stop-and-stop-limit-orders))

Time in force:

* **GTC** - Good Till Cancel (resting on book till cancellation)
* **GTD** - Good Till Date (at given time (expiryDate) order will be automatically cancelled).
* **FOK** - Fill Or Kill (all or nothing)
* **IOC** - Immediate Or Cancelled (allows partial fills)
* **GAA** - Good At Auction can be captured only during auctions and will expire after auction in case it was not filled .
* **DAY** - Order will be automatically cancelled when trading day is closed.&#x20;

<table><thead><tr><th width="137">Type</th><th width="85" align="center">GTC</th><th width="86" align="center">GTD</th><th width="79" align="center">FOK</th><th width="78" align="center">IOC</th><th width="76" align="center">GAA</th><th>DAY</th></tr></thead><tbody><tr><td>Limit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td>✔️</td></tr><tr><td>Market</td><td align="center">❌</td><td align="center">❌</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>❌</td></tr><tr><td>Stop</td><td align="center">❌</td><td align="center">❌</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>❌</td></tr><tr><td>StopLimit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>✔️</td></tr></tbody></table>

{% hint style="info" %}
qualifier: `v1/exchange.market/placeOrder`
{% endhint %}

### **Request**

<table><thead><tr><th width="166.33333333333331">Parameter</th><th width="104">Type</th><th>Description</th></tr></thead><tbody><tr><td>mpOrderId</td><td>Long</td><td>Unique market participant order ID</td></tr><tr><td>orderType</td><td>Enum</td><td>Order type Limit or Market or Stop or StopLimit</td></tr><tr><td>side</td><td>Enum</td><td>Order side Buy or Sell</td></tr><tr><td>instrument</td><td>String</td><td>Instrument identifier</td></tr><tr><td>quantity</td><td>Decimal</td><td>Order quantity</td></tr><tr><td>price <code>optional</code></td><td>Decimal</td><td>The price of the Limit order/ StopLimit. <br>For Market and Stop orders, this will not be sent.</td></tr><tr><td>stopPrice <code>optional</code></td><td>Decimal</td><td><p>Required for Stop &#x26; StopLimit orders. </p><p>The price at which the order will be injected to the market.</p></td></tr><tr><td>timeInForce</td><td>Enum</td><td><p>Limit Order: GTC, GTD, FOK, IOC, DAY, GAA<br>StopLimit order: GTC, GTD, FOK, IOC, DAY </p><p>Market order and Stop order : FOK, IOC </p></td></tr><tr><td><p>expiryDate</p><p><code>optional</code></p></td><td>UTC Time</td><td>Required only for GTD orders - expiration date and time in seconds, in GMT.<br>In case that trading is halted or market is closed at expiry date - the order will be cancelled (even that cancelOrder requests are not allowed).</td></tr><tr><td><p><mark style="color:red;">To be deprecated</mark></p><p>userId</p><p><code>optional</code></p></td><td>String</td><td>Reference data only which is not being used in the exchange. 20 characters length, [a-z,A-Z,0-9]</td></tr><tr><td><p><mark style="color:red;">To be deprecated</mark></p><p>accountId <code>optional</code></p></td><td>Integer</td><td>Account ID of the order</td></tr><tr><td>accountType<br><code>optional</code></td><td>Enum</td><td>Account type Client or House</td></tr><tr><td>parties <code>optional</code></td><td>[] Object</td><td>Array of party objects, see details below.<br>Max number of parties =20 </td></tr><tr><td><mark style="color:blue;">NEW</mark><br>minQuantity<br><code>optional</code></td><td>Decimal</td><td><p>Minimum quantity of an IOC order to be executed.</p><p><br></p></td></tr></tbody></table>

Party specification:&#x20;

<table><thead><tr><th width="112">Name</th><th width="165.33333333333331"></th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>String</td><td>Party id<br>Max length = 20 </td></tr><tr><td>source</td><td>Char</td><td>Party source</td></tr><tr><td>role</td><td>Int</td><td>Party role </td></tr></tbody></table>

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

<table><thead><tr><th width="128">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1000</td><td><code>Missing fields: [Fieldname]</code></td></tr><tr><td>1001</td><td><p><code>Order must contain a positive quantity</code> or<br><code>Market/</code>Stop <code>order must not specify price</code> or<br><mark style="color:red;">Removed</mark> <del><code>Limit order must contain a positive price</code> or</del><br><code>Wrong orderType</code> or<br><code>Wrong side</code> or<br><code>Wrong mpOrderId</code> or<br><code>Wrong userId</code> or<br><code>Wrong timeInForce</code> or<br><code>Wrong expiryDate</code>or<br><code>Latests expiryDate is one year</code>or</p><p><code>Wrong [FieldName]</code> or</p><p><code>[FieldName] must be ≤ [maxLong/10^ fieldPrecision]</code> or<br><code>[FieldName] must ≥ [minLong/10^ fieldPrecision]</code><br><mark style="color:blue;">NEW</mark> <code>minQuantity is allowed only for IOC</code><br><mark style="color:blue;">NEW</mark> <code>Should be: [Min Order Quantity] ≤ minQuantity ≤ [Order quantity]</code></p></td></tr><tr><td>1002</td><td><code>mpOrderId is already in use</code></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1005</td><td><code>Price precision is[PricePrecision]</code> or<br><code>Quantity precision is[QuantityPrecision]</code></td></tr><tr><td>1006</td><td><code>Minimum order quantity is [MinOrderQuantity]</code> or<br><code>Maximum order quantity is [MaxOrderQuantity]</code> or<br><code>Quantity increment is [quantityIncrement]</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1010</td><td><code>Instrument [Instrument] not found</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1012</td><td><code>Price breaches [FieldName] of [FieldValue]</code></td></tr><tr><td>1013 </td><td><code>Maximum order value is [maxOrderValue]</code></td></tr><tr><td>1014 </td><td><code>Price tick size is [tickSize]</code></td></tr><tr><td>1015</td><td><code>You are not allowed place [orderSide] orders in current auction</code></td></tr><tr><td>1016</td><td><code>[oppositeOrderSide] order was not yet placed</code></td></tr><tr><td>1030</td><td><p><code>Only [Allowed TIF] limit orders are allowed</code> or</p><p><code>Trading is not allowed during crossing</code></p></td></tr><tr><td>1031</td><td><code>GAA is allowed only during auction</code></td></tr><tr><td>1001</td><td><code>[Limit|Market] order must not specify stopPrice</code></td></tr></tbody></table>

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

## Stop and Stop Limit Orders

#### General

Stop & StopLimit are not allowed to be placed during an auction.&#x20;

Replacement of Stop & StopLimit is allowed only before injection.&#x20;

Stop and StopLimit are not being published in the MD APIs and only shown in Private Data APIs:

* Mass Order Status (WS) / Order Mass Status Request (FIX)
* Execution Reports (Both WS and FIX)
* Order History (WS & Admin UI)

Once Stop / StopLimit orders are triggered and injected to the order book it will published in the MD like any regular order.



#### Stop & StopLimit Trigger Rules:

The Stop and StopLimit orders will be injected into the order book by those rules:

* For buy orders → In case the stopPrice is lower or equal to the market price
* For sell orders → In case the stopPrice is higher or equal to the market price
* An incoming Stop / StopLimit Order will be injected immediately  if the stop price is already met.
  * Order will be immediately injected only in case there was an order book execution since last time market opened. (so on 24/7 it will always take last execution and non 24 hours calendars it will only use executed trade since today market open.)
* If taker order is executed against multiple resting orders, The Stop / StopLimit orders must be injected into order book only after place (taker) order operation is completed. If during any execution, the stop price is triggered, the order must be injected into order book. Even in case that after place (taker) order operation is completed the price has changed and it no longer meets the rules of the trigger, the order will still be injected into the book.
* System will inject orders that were triggered only when the marketStatus = "Opened" & tradingStatus = "Trade".
  * When it might happen:
    * on auction ends and it changes the market status from `AuctionCrossing` to `Opened`
    * on auto calendar event (start time) and the market status changes to `Opened`
    * on manual calendar changes and the market status changes to `Opened`
    * on manual resume and the trading status changes to `TRADE`
    * on auto resume and the trading status changes to `TRADE`
* After auction system will triggered Stop and Stop Limit orders according to the auction price (for Equilibrium Price Auctions) or according to the highest and lowest execution prices (for Price & Time Auction)



* ####



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

The `massCancel` API is used to cancel all the active order and suspended orders for specific instrument for specific market participant.

<mark style="color:blue;">NEW</mark>  MPs can specify the targetParties they wish to target the mass order cancel by specifying the targetParties.&#x20;

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

<table><thead><tr><th width="182">Parameter</th><th width="185.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>instrument</td><td>String</td><td>Instrument identifier</td></tr><tr><td><mark style="color:blue;">NEW</mark> targetParties</td><td>[] TargetParty Objects</td><td>Array of party objects, see details below.<br>Max number of targetParties =20 </td></tr></tbody></table>

<mark style="color:blue;">NEW</mark> TargetParty specification:&#x20;

<table><thead><tr><th width="180">Name</th><th width="120.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>string</td><td>TargetParty id</td></tr><tr><td>source</td><td>char</td><td>TargetParty source</td></tr><tr><td>role</td><td>int</td><td>TargetParty role</td></tr></tbody></table>



### **Response**

<table><thead><tr><th width="192">Parameter</th><th width="96.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>numberOfOrders</td><td>Int</td><td>Number of orders that were cancelled</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="129">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1001 </td><td><code>Wrong [FieldName]</code></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1010</td><td><code>Instrument [Instrument] not found</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><code>Trading is not allowed during crossing</code></td></tr><tr><td><mark style="color:blue;">NEW</mark> 1032</td><td><code>Account not found</code><br><code>Party is not allowed</code></td></tr><tr><td>1103</td><td><code>Missing fields: [Fieldname]</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```javascript
{
  "q": "v1/exchange.market/massCancel",
  "sid": 1,
  "d": {
    "instrument": "ABC"
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
    "numberOfOrders": 5
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

If you send a valid order to modify, you should receive a response that confirms that order was modified. This means that remaining open quantity of the order was reduced.

Non-valid modify order requests will be responded with the error message.

{% hint style="info" %}
qualifier: `v1/exchange.market/modifyOrder`
{% endhint %}

### **Request**

| Parameter  | Type    | Description           |
| ---------- | ------- | --------------------- |
| orderId    | Long    | Exchange Order ID     |
| instrument | String  | Instrument identifier |
| quantity   | Decimal | New order quantity    |

### **Response**

| Parameter | Type | Description       |
| --------- | ---- | ----------------- |
| orderId   | Long | Exchange Order ID |

### **Error Codes**

<table><thead><tr><th width="140">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1001</td><td><p><code>Wrong [FieldName]</code> or</p><p><code>Order must contain a positive quantity</code></p></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1005</td><td><code>Quantity precision is[QuantityPrecision]</code></td></tr><tr><td>1006</td><td><code>Quantity increment is [quantityIncrement]</code> or<br><code>Minimum order quantity is [MinOrderQuantity]</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><code>Trading is not allowed during crossing</code></td></tr><tr><td>1100</td><td><code>Order not found for that instrument</code></td></tr><tr><td>1103</td><td><code>Missing fields: [Fieldname]</code></td></tr><tr><td>1301</td><td><code>Quantity must be less than [remainaingActiveQuantity]]</code></td></tr><tr><td><del>1019</del></td><td><del><code>Not allowed on stop or stop limit orders</code></del></td></tr></tbody></table>

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

The `replaceOrder` API is used to change a few of the order parameters in a single command for order which is resting on the book. <mark style="color:red;">Removed v1.19.0</mark> ~~this action will lose the time priority as it going to cancel the previous order and place a new one.~~

<mark style="color:blue;">NEW v1.19.0</mark> \
In case the request is only to reduce the remaining open quantity of an order (other fields may present with the same values as the original order or be omitted from request), the system modifies the existing order, the behavior is similar to [modifyOrder](trading-api.md#modifyorder).

Any other request would result in the system canceling the existing order and creating a new one. This will result in losing the time priority as it cancels the previous order and places a new one.

If you send a valid order to be replaced, you should receive a response that confirms the action.

Non-valid replace order requests will be responded with the error message.

{% hint style="info" %}
qualifier: `v1/exchange.market/replaceOrder`
{% endhint %}

### **Request**

<table><thead><tr><th>Parameter</th><th width="169.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>orderId</td><td>Long</td><td>Exchange Order ID</td></tr><tr><td>instrument</td><td>String</td><td>Instrument identifier</td></tr><tr><td>quantity</td><td>Decimal</td><td>New order quantity (can be more or less the original order quantity)</td></tr><tr><td>price</td><td>Decimal</td><td>New order price<br><mark style="color:blue;">CHANGED</mark> Required If quantity >= remainingOpenQuantity of original order</td></tr><tr><td>timeInForce <code>optional</code></td><td>eNum</td><td>New order time in force</td></tr><tr><td>expiryDate <code>optional</code></td><td>UTC Time</td><td>Required if timeInForce = GTD</td></tr><tr><td>stopPrice  <code>optional</code></td><td>Decimal</td><td><p>Required for Stop &#x26; StopLimit orders. </p><p>The price at which the order will be injected to the market.</p></td></tr><tr><td><mark style="color:blue;">NEW</mark> minQuantity <code>optional</code></td><td>Decimal</td><td>Minimum quantity of an IOC order to be executed.</td></tr></tbody></table>

### **Response**

| Parameter       | Type | Description                             |
| --------------- | ---- | --------------------------------------- |
| originalOrderId | Long | Exchange Order ID of the original order |
| newOrderId      | Long | Exchange Order ID of the new order      |

### **Error Codes**

<table><thead><tr><th width="138">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1000</td><td><code>Missing fields: [Fieldname]</code></td></tr><tr><td>1001</td><td><p><code>Wrong [FieldName]</code> or</p><p><code>Order must contain a positive quantity</code> or<br><code>Limit order must contain a positive price</code>or<br><code>Latest expiryDate is one year</code> or<br><code>Wrong timeInForce</code> or <br><code>Wrong expiryDate</code> or<br><code>Order [FieldName] must be less than [maxLong/10^ fieldPrecision]</code> or<br><code>Order [FieldName] must be above than [minLong/10^ fieldPrecision]</code><br><mark style="color:blue;">NEW</mark> <code>minQuantity is allowed only for IOC TIF</code><br><mark style="color:blue;">NEW</mark> <code>Should be: [Min Order Quantity] ≤ minQuantity ≤ [Order quantity]</code></p></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1005</td><td><code>Quantity precision is[QuantityPrecision]</code> or<br><code>Price precision is[PricePrecision]</code></td></tr><tr><td>1006</td><td><code>Minimum order quantity is [MinOrderQuantity]</code> or<br><code>Maximum order quantity is [MaxOrderQuantity]</code> or<br><code>Quantity increment is [quantityIncrement]</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1010 </td><td><code>Instrument [Instrument] not found</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1012</td><td><code>Price breaches [FieldName] of [FieldValue]</code></td></tr><tr><td>1013</td><td><code>Maximum order value is [maxOrderValue]</code></td></tr><tr><td>1014</td><td>​<code>Price tick size is [tickSize]</code></td></tr><tr><td>1017</td><td><code>This order exceeds maxDepth</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><code>Trading is not allowed during crossing</code> or<br><code>Only [Allowed TIF] limit orders are allowed</code></td></tr><tr><td>1031 </td><td><code>GAA is allowed only during auction</code></td></tr><tr><td>1100</td><td><code>Order not found for that instrument</code></td></tr><tr><td>1019</td><td><code>Stop or Stop limit can be replaced only while suspended</code></td></tr><tr><td>1001</td><td><code>[Limit|Market] order must not specify stopPrice</code></td></tr></tbody></table>

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
