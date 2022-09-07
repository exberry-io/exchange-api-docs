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

Time in force:

* **GTC** - Good Till Cancel (resting on book till cancellation)
* **GTD** - Good Till Date (at given time (expiryDate) order will be automatically cancelled).
* **FOK** - Fill Or Kill (all or nothing)
* **IOC** - Immediate Or Cancelled (allows partial fills)
* **GAA** - Good At Auction can be captured only during auctions and will expire after auction in case it was not filled .

| Type   | GTC | GTD | FOK | IOC | GAA |
| ------ | :-: | :-: | :-: | :-: | :-: |
| Limit  |  ✔️ |  ✔️ |  ✔️ |  ✔️ |  ✔️ |
| Market |  ❌  |  ❌  |  ✔️ |  ✔️ |  ❌  |

{% hint style="info" %}
qualifier: `v1/exchange.market/placeOrder`
{% endhint %}

### **Request**

| Parameter                                                    | Type       | Description                                                                                                                                                                                                                             |
| ------------------------------------------------------------ | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mpOrderId                                                    | Long       | Unique market participant order ID                                                                                                                                                                                                      |
| orderType                                                    | Enum       | Order type Limit or Market                                                                                                                                                                                                              |
| side                                                         | Enum       | Order side Buy or Sell                                                                                                                                                                                                                  |
| instrument                                                   | String     | Instrument identifier                                                                                                                                                                                                                   |
| quantity                                                     | Decimal    | Order quantity                                                                                                                                                                                                                          |
| price `optional`                                             | Decimal    | The price of the Limit order. For Market order this will not be sent.                                                                                                                                                                   |
| timeInForce                                                  | Enum       | <p>GTC, GTD, FOK, IOC, GAA for Limit order</p><p>FOK, IOC - for Market order</p>                                                                                                                                                        |
| <p>expiryDate</p><p><code>optional</code></p>                | UTC Time   | <p>Required only for GTD orders - expiration date and time in seconds, in GMT.<br>In case that trading is halted or market is closed at expiry date - the order will be cancelled (even that cancelOrder requests are not allowed).</p> |
| <p>userId</p><p><code>optional</code></p>                    | String     | Reference data only which is not being used in the exchange. 20 characters length, \[a-z,A-Z,0-9]                                                                                                                                       |
| accountId `optional`                                         | Integer    | Account ID of the order                                                                                                                                                                                                                 |
| <p>accountType<br><strong></strong><code>optional</code></p> | Enum       | Account type Client or House                                                                                                                                                                                                            |
| parties `optional`                                           | \[] Object | Array of party objects, see details below                                                                                                                                                                                               |

Party specification:&#x20;

| Name   |        | Description                         |
| ------ | ------ | ----------------------------------- |
| id     | String | <p>Party id<br>Max length = 20 </p> |
| source | Char   | Party source                        |
| role   | Int    | Party role                          |

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

| Code | Message                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | `Exchange is unavailable`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 1000 | `Missing fields: [Fieldname]`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 1001 | <p><code>Order must contain a positive quantity</code> or<br><code>Market order must not specify price</code> or<br><code>Limit order must contain a positive price</code> or<br><code>Wrong orderType</code> or<br><code>Wrong side</code> or<br><code>Wrong mpOrderId</code> or<br><code>Wrong userId</code> or<br><code>Wrong timeInForce</code> or<br><code>Wrong expiryDate</code>or<br><code>Latests expiryDate is one year</code>or</p><p><code>Order quantity must be less than 922337203.6854775807</code></p> |
| 1002 | `mpOrderId is already in use`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 1003 | `Market is closed`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 1004 | `Instrument trading is not allowed`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 1005 | <p><code>Price precision is[PricePrecision]</code> or<br><code>Quantity precision is[QuantityPrecision]</code></p>                                                                                                                                                                                                                                                                                                                                                                                                      |
| 1006 | <p><code>Minimum order quantity is [MinOrderQuantity]</code> or<br><code>Maximum order quantity is [MaxOrderQuantity]</code> or<br><code>Quantity increment is [quantityIncrement]</code></p>                                                                                                                                                                                                                                                                                                                           |
| 1007 | `Invalid session`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 1008 | `This apiKey doesn’t have the right permission`                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 1009 | `Instrument trading is halt`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 1010 | `Instrument [Instrument] not found`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 1011 | `Permission denied for this instrument`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 1015 | `You are not allowed place [orderSide] orders in current auction`                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 1016 | `[oppositeOrderSide] order was not yet placed`                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 1030 | <p><code>Only [Allowed TIF] limit orders are allowed</code> or</p><p><code>Trading is not allowed during crossing</code></p>                                                                                                                                                                                                                                                                                                                                                                                            |
| 1031 | `GAA is allowed only during auction`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

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

| Parameter            | Type   | Description                                                      |
| -------------------- | ------ | ---------------------------------------------------------------- |
| orderId `optional`   | Long   | Exchange Order ID. Not mandatory if `mpOrderId` was sent         |
| mpOrderId `optional` | Long   | Market participant order ID. Not mandatory if `orderId` was sent |
| instrument           | String | Instrument identifier                                            |

### **Response**

| Parameter | Type | Description       |
| --------- | ---- | ----------------- |
| orderId   | Long | Exchange Order ID |

### **Error Codes**

| Code | Message                                         |
| ---- | ----------------------------------------------- |
| 1    | `Exchange is unavailable`                       |
| 1003 | `Market is closed`                              |
| 1004 | `Instrument trading is not allowed`             |
| 1007 | `Invalid session`                               |
| 1008 | `This apiKey doesn’t have the right permission` |
| 1009 | `Instrument trading is halt`                    |
| 1011 | `Permission denied for this instrument`         |
| 1030 | `Trading is not allowed during crossing`        |
| 1100 | `Order not found for that instrument`           |
| 1103 | `Missing fields: [Fieldname]`                   |
| 1104 | `Please use only one from orderId or mpOrderId` |

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

The `massCancel` API is used to cancel all the active order for specific instrument for specific market participant.

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

| Code | Message                                         |
| ---- | ----------------------------------------------- |
| 1    | `Exchange is unavailable`                       |
| 1003 | `Market is closed`                              |
| 1004 | `Instrument trading is not allowed`             |
| 1007 | `Invalid session`                               |
| 1008 | `This apiKey doesn’t have the right permission` |
| 1009 | `Instrument trading is halt`                    |
| 1010 | `Instrument [Instrument] not found`             |
| 1011 | `Permission denied for this instrument`         |
| 1030 | `Trading is not allowed during crossing`        |
| 1103 | `Missing fields: [Fieldname]`                   |

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

| Code | Message                                                                                            |
| ---- | -------------------------------------------------------------------------------------------------- |
| 1    | `Exchange is unavailable`                                                                          |
| 1001 | <p><code>Wrong [FieldName]</code> or</p><p><code>Order must contain a positive quantity</code></p> |
| 1003 | `Market is closed`                                                                                 |
| 1004 | `Instrument trading is not allowed`                                                                |
| 1005 | `Quantity precision is[QuantityPrecision]`                                                         |
| 1006 | `Quantity increment is [quantityIncrement]`                                                        |
| 1007 | `Invalid session`                                                                                  |
| 1008 | `This apiKey doesn’t have the right permission`                                                    |
| 1009 | `Instrument trading is halt`                                                                       |
| 1011 | `Permission denied for this instrument`                                                            |
| 1030 | `Trading is not allowed during crossing`                                                           |
| 1100 | `Order not found for that instrument`                                                              |
| 1103 | `Missing fields: [Fieldname]`                                                                      |
| 1301 | `Quantity must be less than [remainaingActiveQuantity]]`                                           |

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

| Code | Message                                                                                                                                                                                                         |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | `Exchange is unavailable`                                                                                                                                                                                       |
| 1000 | `Missing fields: [Fieldname]`                                                                                                                                                                                   |
| 1001 | <p><code>Wrong [FieldName]</code> or</p><p><code>Order must contain a positive quantity</code> or<br><code>Limit order must contain a positive price</code>or<br><code>Latest expiryDate is one year</code></p> |
| 1003 | `Market is closed`                                                                                                                                                                                              |
| 1004 | `Instrument trading is not allowed`                                                                                                                                                                             |
| 1005 | `Quantity precision is[QuantityPrecision]`                                                                                                                                                                      |
| 1006 | <p><code>Minimum order quantity is [MinOrderQuantity]</code> or<br><code>Maximum order quantity is [MaxOrderQuantity]</code> or<br><code>Quantity increment is [quantityIncrement]</code></p>                   |
| 1007 | `Invalid session`                                                                                                                                                                                               |
| 1008 | `This apiKey doesn’t have the right permission`                                                                                                                                                                 |
| 1009 | `Instrument trading is halt`                                                                                                                                                                                    |
| 1011 | `Permission denied for this instrument`                                                                                                                                                                         |
| 1012 | `Price breaches [FieldName] of [FieldValue]`                                                                                                                                                                    |
| 1013 | `Maximum order value is [maxOrderValue]`                                                                                                                                                                        |
| 1014 | ​`Price tick size is [tickSize]`                                                                                                                                                                                |
| 1017 | `This order exceeds maxDepth`                                                                                                                                                                                   |
| 1030 | `Trading is not allowed during crossing`                                                                                                                                                                        |
| 1100 | `Order not found for that instrument`                                                                                                                                                                           |

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
