# Introduction

The exberry exchange API offers real time market data as well as the ability to trade with the exchange.&#x20;

The underlying technology used is [Scalecube](https://scalecube.github.io/) a Novel Open-source application-platform that addresses inherent challenges involved in the development of distributed computing.

Exchange API protocol is WebSocket.\
Any request body should be a valid `JSON`, non valid `JSON` objects will be ignored.

Within the valid `JSON` please be aware that:

* System ignores any additional parameter that was sent on request body but was not specified in this document.
* In case of multiple parameters sent with different values, system will always use the last value provided.

### **Request Parameters**

<table><thead><tr><th width="134.33333333333331">Parameter</th><th width="81">Type</th><th>Description</th></tr></thead><tbody><tr><td>q</td><td>String</td><td>qualifier, contains the method for the specific API call.</td></tr><tr><td>sid</td><td>Int</td><td>stream identifier, for each WebSocket connection this is a unique identifier for the API call. Please note that as long as the sid was not ended (other by exchange or by consumer) this can’t be used again on the same WebSocket connection</td></tr><tr><td>d</td><td>Json</td><td>data object, contain the request body</td></tr></tbody></table>

```javascript
{ 
 "q":"exchange.market/placeOrder", 
 "sid":1, 
 "d": 
   { 
     Object
   } 
}
```

### **Success Response**

The response will always include the `q` and `sid` parameters from request and a `d` parameter with the response body.

```javascript
{ 
   "q":"exchange.market/placeOrder", 
   "sid":1, 
   "d": 
     { 
       Object
     } 
}
```

In case of short living stream (i.e. trading action), additional response will be sent upon stream closure with success, this message will include the `q` and `sid` parameters from request and `sig:1`.

```javascript
{
  "sig": 1,
  "q": "exchange.market/createSession",
  "sid": 1
}
```

### **Failure Response**

The response will always include the below parameters:

<table><thead><tr><th width="146">Parameter</th><th>Description</th></tr></thead><tbody><tr><td>sig</td><td>signal will be equal to "2"</td></tr><tr><td>q</td><td>from request</td></tr><tr><td>errorType</td><td>internal error type that should be ignored</td></tr><tr><td>sid</td><td>from request</td></tr><tr><td>d</td><td>data that contain errorCode and errorMessage.<br>Those are the error code and message to consider.</td></tr></tbody></table>

```javascript
{
  "sig": 2,
  "q": "exchange.market/createSession",
  "errorType": "401",
  "sid": 1,
  "d": {
    "errorCode": 6002,
    "errorMessage": "Missing fields: 'apiKey'"
  }
}
```

### **Stream Closure**

In order to close active stream need to send a message with `sig:3` in addition to `q` and `sid` from request.

```javascript
{
  "sig": 3,
  "q": "exchange.market/orderBookDepth",
  "sid": 100
}
```

sig parameter summary table:

<table><thead><tr><th width="89">sig</th><th>Description</th></tr></thead><tbody><tr><td>1</td><td>Stream closed with success</td></tr><tr><td>2</td><td>Stream closed with failure</td></tr><tr><td>3</td><td>Stream was closed due to consumer request</td></tr></tbody></table>
