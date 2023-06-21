# Trading API

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
* <mark style="color:blue;">NEW</mark> **Stop**: A Stop Order is a Market Order that will remain Suspended (will not be entered into order book) until the stop price is reached. Once triggered, it will be treated similar to a regular Market Order. ([Here](trading-api.md#stop-and-stop-limit-orders))
* <mark style="color:blue;">NEW</mark> **StopLimit**: A Stop Limit Order is a Limit Order that will remain Suspended (will not be entered into order book) until the stop price is reached. Once triggered, a Stop Limit Order will be treated as a regular Limit Order. ([Here](trading-api.md#stop-and-stop-limit-orders))

Time in force:

* **GTC** - Good Till Cancel (resting on book till cancellation)
* **GTD** - Good Till Date (at given time (expiryDate) order will be automatically cancelled).
* **FOK** - Fill Or Kill (all or nothing)
* **IOC** - Immediate Or Cancelled (allows partial fills)
* **GAA** - Good At Auction can be captured only during auctions and will expire after auction in case it was not filled .
* **DAY** - Order will be automatically cancelled when trading day is closed.&#x20;

<table><thead><tr><th width="137">Type</th><th width="85" align="center">GTC</th><th width="86" align="center">GTD</th><th width="79" align="center">FOK</th><th width="78" align="center">IOC</th><th width="76" align="center">GAA</th><th>DAY</th></tr></thead><tbody><tr><td>Limit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td>✔️</td></tr><tr><td>Market</td><td align="center">❌</td><td align="center">❌</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>❌</td></tr><tr><td><mark style="color:blue;">NEW</mark> <br>Stop</td><td align="center">❌</td><td align="center">❌</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>❌</td></tr><tr><td><mark style="color:blue;">NEW</mark> StopLimit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>✔️</td></tr></tbody></table>

{% hint style="info" %}
qualifier: `v1/exchange.market/placeOrder`
{% endhint %}

### **Request**

<table><thead><tr><th width="166.33333333333331">Parameter</th><th width="104">Type</th><th>Description</th></tr></thead><tbody><tr><td>mpOrderId</td><td>Long</td><td>Unique market participant order ID</td></tr><tr><td>orderType</td><td>Enum</td><td>Order type Limit or Market <mark style="color:blue;">NEW</mark> or Stop or StopLimit</td></tr><tr><td>side</td><td>Enum</td><td>Order side Buy or Sell</td></tr><tr><td>instrument</td><td>String</td><td>Instrument identifier</td></tr><tr><td>quantity</td><td>Decimal</td><td>Order quantity</td></tr><tr><td>price <code>optional</code></td><td>Decimal</td><td>The price of the Limit order/ <mark style="color:blue;">NEW</mark> StopLimit. <br>For Market and <mark style="color:blue;">NEW</mark> Stop orders, this will not be sent.</td></tr><tr><td><mark style="color:blue;">NEW</mark> stopPrice <code>optional</code></td><td>Decimal</td><td><p>Required for Stop &#x26; StopLimit orders. </p><p>The price at which the order will be injected to the market.</p></td></tr><tr><td>timeInForce</td><td>Enum</td><td><p>Limit Order: GTC, GTD, FOK, IOC, DAY, GAA<br><mark style="color:blue;">NEW</mark> StopLimit order: GTC, GTD, FOK, IOC, DAY </p><p>Market order <mark style="color:blue;">NEW</mark> and Stop order : FOK, IOC </p></td></tr><tr><td><p>expiryDate</p><p><code>optional</code></p></td><td>UTC Time</td><td>Required only for GTD orders - expiration date and time in seconds, in GMT.<br>In case that trading is halted or market is closed at expiry date - the order will be cancelled (even that cancelOrder requests are not allowed).</td></tr><tr><td><p><mark style="color:red;">To be deprecated</mark></p><p>userId</p><p><code>optional</code></p></td><td>String</td><td>Reference data only which is not being used in the exchange. 20 characters length, [a-z,A-Z,0-9]</td></tr><tr><td><p><mark style="color:red;">To be deprecated</mark></p><p>accountId <code>optional</code></p></td><td>Integer</td><td>Account ID of the order</td></tr><tr><td>accountType<br><code>optional</code></td><td>Enum</td><td>Account type Client or House</td></tr><tr><td>parties <code>optional</code></td><td>[] Object</td><td>Array of party objects, see details below.<br>Max number of parties =20 </td></tr></tbody></table>

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

<table><thead><tr><th width="128">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1000</td><td><code>Missing fields: [Fieldname]</code></td></tr><tr><td>1001</td><td><p><code>Order must contain a positive quantity</code> or<br><mark style="color:blue;">Changed</mark> <code>Market/</code>Stop <code>order must not specify price</code> or<br><mark style="color:red;">Removed</mark> <del><code>Limit order must contain a positive price</code> or</del><br><code>Wrong orderType</code> or<br><code>Wrong side</code> or<br><code>Wrong mpOrderId</code> or<br><code>Wrong userId</code> or<br><code>Wrong timeInForce</code> or<br><code>Wrong expiryDate</code>or<br><code>Latests expiryDate is one year</code>or</p><p><code>Wrong [FieldName]</code> or</p><p><code>[FieldName] must be ≤ [maxLong/10^ fieldPrecision]</code> or<br><code>[FieldName] must ≥ [minLong/10^ fieldPrecision]</code></p></td></tr><tr><td>1002</td><td><code>mpOrderId is already in use</code></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1005</td><td><code>Price precision is[PricePrecision]</code> or<br><code>Quantity precision is[QuantityPrecision]</code></td></tr><tr><td>1006</td><td><code>Minimum order quantity is [MinOrderQuantity]</code> or<br><code>Maximum order quantity is [MaxOrderQuantity]</code> or<br><code>Quantity increment is [quantityIncrement]</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><mark style="color:blue;">CHANGED</mark> <code>Insufficient permissions</code><br><del><code>This apiKey doesn’t have the right permission</code></del></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1010</td><td><code>Instrument [Instrument] not found</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1012</td><td><code>Price breaches [FieldName] of [FieldValue]</code></td></tr><tr><td>1013 </td><td><code>Maximum order value is [maxOrderValue]</code></td></tr><tr><td>1014 </td><td><code>Price tick size is [tickSize]</code></td></tr><tr><td>1015</td><td><code>You are not allowed place [orderSide] orders in current auction</code></td></tr><tr><td>1016</td><td><code>[oppositeOrderSide] order was not yet placed</code></td></tr><tr><td>1030</td><td><p><code>Only [Allowed TIF] limit orders are allowed</code> or</p><p><code>Trading is not allowed during crossing</code></p></td></tr><tr><td>1031</td><td><code>GAA is allowed only during auction</code></td></tr></tbody></table>

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

Stop & StopLimit are not allowed to placed during auction, and not allowed to be modified/replaced.

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

**Some Edge Cases**

* Case which the order will be canceled without being inject into the book:
  * For GTD Stop Limit’s with expiryDate in case order was not triggered until expiryDate, it will be expired without being injected into the book.
*   **Order will not injected although stopPrice was triggered:** &#x20;

    * In case the market price that triggered the stop/ stopLimit orders is received as a result of an auction.&#x20;
    * In case that the trade that supposed to trigger the orders, triggered also a CBR that halted the trading.&#x20;

    In those cases the status of the order we remain Suspended until it is triggered once again.
* **Order will be injected not immediately:**&#x20;
  * In case that the trade that supposed to trigger the orders, triggered also a CBR that open auction, the stop/ stop limit will be injected only after auction is completed.&#x20;

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

<table><thead><tr><th width="129">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1001 <mark style="color:blue;">NEW</mark></td><td><code>Wrong [FieldName]</code></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><mark style="color:blue;">CHANGED</mark> <code>Insufficient permissions</code><br><del><code>This apiKey doesn’t have the right permission</code></del></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1011 </td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><code>Trading is not allowed during crossing</code></td></tr><tr><td>1100</td><td><code>Order not found for that instrument</code></td></tr><tr><td>1103</td><td><code>Missing fields: [Fieldname]</code></td></tr><tr><td>1104</td><td><code>Please use only one from orderId or mpOrderId</code></td></tr></tbody></table>

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

The `massCancel` API is used to cancel all the active order <mark style="color:blue;">NEW</mark> and suspended orders for specific instrument for specific market participant.

If you send a valid request, you should receive a response that confirms the number of orders that were cancelled.

Non-valid cancel order will be responded with the error message.

{% hint style="info" %}
qualifier: `v1/exchange.market/massCancel`
{% endhint %}

### **Request**

| Parameter  | Type   | Description           |
| ---------- | ------ | --------------------- |
| instrument | String | Instrument identifier |

### **Response**

| Parameter      | Type | Description                          |
| -------------- | ---- | ------------------------------------ |
| numberOfOrders | Int  | Number of orders that were cancelled |

### **Error Codes**

<table><thead><tr><th width="129">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1001 </td><td><code>Wrong [FieldName]</code></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><p><mark style="color:blue;">CHANGED</mark> <code>Insufficient permissions</code></p><p><del><code>This apiKey doesn’t have the right permission</code></del></p></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1010</td><td><code>Instrument [Instrument] not found</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><code>Trading is not allowed during crossing</code></td></tr><tr><td>1103</td><td><code>Missing fields: [Fieldname]</code></td></tr></tbody></table>

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

<table><thead><tr><th width="140">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1001</td><td><p><code>Wrong [FieldName]</code> or</p><p><code>Order must contain a positive quantity</code></p></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1005</td><td><code>Quantity precision is[QuantityPrecision]</code></td></tr><tr><td>1006</td><td><code>Quantity increment is [quantityIncrement]</code> or<br><code>Minimum order quantity is [MinOrderQuantity]</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><mark style="color:blue;">CHANGED</mark> <code>Insufficient permissions</code><br><del><code>This apiKey doesn’t have the right permission</code></del></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><code>Trading is not allowed during crossing</code></td></tr><tr><td>1100</td><td><code>Order not found for that instrument</code></td></tr><tr><td>1103</td><td><code>Missing fields: [Fieldname]</code></td></tr><tr><td>1301</td><td><code>Quantity must be less than [remainaingActiveQuantity]]</code></td></tr><tr><td><mark style="color:blue;">NEW</mark> 1019</td><td><code>Not allowed on stop or stop limit orders</code></td></tr></tbody></table>

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

The `replaceOrder` API is used to change a few of the order parameters in a single command for order which is resting on the book, this action will lose the time priority as it going to cancel the previous order and place a new one.

If you send a valid order to be replaced, you should receive a response that confirms that order was replaced.

Non-valid replace order requests will be responded with the error message.

{% hint style="info" %}
qualifier: `v1/exchange.market/replaceOrder`
{% endhint %}

### **Request**

| Parameter              | Type     | Description                                                          |
| ---------------------- | -------- | -------------------------------------------------------------------- |
| orderId                | Long     | Exchange Order ID                                                    |
| instrument             | String   | Instrument identifier                                                |
| quantity               | Decimal  | New order quantity (can be more or less the original order quantity) |
| price                  | Decimal  | New order price                                                      |
| timeInForce `optional` | eNum     | New order time in force                                              |
| expiryDate `optional`  | UTC Time | Required if timeInForce = GTD                                        |

### **Response**

| Parameter       | Type | Description                             |
| --------------- | ---- | --------------------------------------- |
| originalOrderId | Long | Exchange Order ID of the original order |
| newOrderId      | Long | Exchange Order ID of the new order      |

### **Error Codes**

<table><thead><tr><th width="138">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>1000</td><td><code>Missing fields: [Fieldname]</code></td></tr><tr><td>1001</td><td><p><code>Wrong [FieldName]</code> or</p><p><code>Order must contain a positive quantity</code> or<br><code>Limit order must contain a positive price</code>or<br><code>Latest expiryDate is one year</code> or<br><code>Wrong timeInForce</code> or <br><code>Wrong expiryDate</code> or<br><code>Order [FieldName] must be less than [maxLong/10^ fieldPrecision]</code> or<br><code>Order [FieldName] must be above than [minLong/10^ fieldPrecision]</code></p></td></tr><tr><td>1003</td><td><code>Market is closed</code></td></tr><tr><td>1004</td><td><code>Instrument trading is not allowed</code></td></tr><tr><td>1005</td><td><code>Quantity precision is[QuantityPrecision]</code> or<br><code>Price precision is[PricePrecision]</code></td></tr><tr><td>1006</td><td><code>Minimum order quantity is [MinOrderQuantity]</code> or<br><code>Maximum order quantity is [MaxOrderQuantity]</code> or<br><code>Quantity increment is [quantityIncrement]</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><mark style="color:blue;">CHANGED</mark> <code>Insufficient permissions</code><br><del><code>This apiKey doesn’t have the right permission</code></del></td></tr><tr><td>1009</td><td><code>Instrument trading is halt</code></td></tr><tr><td>1010 </td><td><code>Instrument [Instrument] not found</code></td></tr><tr><td>1011</td><td><code>Permission denied for this instrument</code></td></tr><tr><td>1012</td><td><code>Price breaches [FieldName] of [FieldValue]</code></td></tr><tr><td>1013</td><td><code>Maximum order value is [maxOrderValue]</code></td></tr><tr><td>1014</td><td>​<code>Price tick size is [tickSize]</code></td></tr><tr><td>1017</td><td><code>This order exceeds maxDepth</code></td></tr><tr><td>1018 </td><td><code>Trading is not allowed</code></td></tr><tr><td>1030</td><td><code>Trading is not allowed during crossing</code> or<br><code>Only [Allowed TIF] limit orders are allowed</code></td></tr><tr><td>1031 </td><td><code>GAA is allowed only during auction</code></td></tr><tr><td>1100</td><td><code>Order not found for that instrument</code></td></tr><tr><td><mark style="color:blue;">NEW</mark> 1019</td><td><code>Not allowed on stop or stop limit orders</code></td></tr></tbody></table>

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
