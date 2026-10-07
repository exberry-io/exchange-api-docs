# Technical Guidelines

For general technical guidelines on the Exberry WebSocket API, please refer to [https://docs.exberry.io](https://docs.exberry.io).\
This section provides additional guidelines specific to the **Market Data API**.

## **Public Access Mode**

In this mode, each request must include a **token** in addition to the standard parameters: `q`, `sid`, and `d`.&#x20;

<table><thead><tr><th>Parameter</th><th width="89">Type</th><th>Description</th></tr></thead><tbody><tr><td>token</td><td>String</td><td>Token is configurable value per environment, it allows accurate and efficient instrument discovery. <br>Token generation described on the <a href="./">Introduction </a>section.</td></tr></tbody></table>

Sample

```json
{
  "q": "v1/exchange.marketdata/lightTickers",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 102432,
  "d": {
    "symbols": [
      "XYZ"
    ],
    "interval": 1000
  }
}
```

Generic Error Codes &#x20;

<table><thead><tr><th width="169.57142857142856">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td>2</td><td>Missing fields: token</td></tr><tr><td>3</td><td>Invalid token</td></tr></tbody></table>

For long-lived streams, the following error message will be received if a disconnection occurs:

<table><thead><tr><th width="169.57142857142856">Code</th><th>Message</th></tr></thead><tbody><tr><td>2</td><td>Stream disconnected </td></tr></tbody></table>

## **Authenticated Access Mode**

In this mode, a session is established in advance via the WebSocket connection. Therefore, no token is required in individual requests.

<table><thead><tr><th width="169.57142857142856">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td>1008</td><td>Insufficient permissions (in case session was not established in advance) </td></tr></tbody></table>



## Heartbeats (Pings) <a href="#websocket-heartbeats" id="websocket-heartbeats"></a>

The WebSocket connection supports ping messages for the client to identify connection drops. Ping requests can be made on-demand, and the server responds to each request.

It is recommended that this API should only be used for heartbeats from the browser. Use WebSocket [native ping/pong](https://datatracker.ietf.org/doc/html/rfc6455#section-5.5.2) for other use cases.

{% hint style="info" %}
qualifier: v1/heartbeat/ping
{% endhint %}

### **Request Parameters**

<table><thead><tr><th width="121.4">Parameter</th><th width="150">Type</th><th>Description</th></tr></thead><tbody><tr><td>d</td><td>Int</td><td>This value will be returned in the response.</td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v1/heartbeat/ping",
  "sid": 1,
  "d": 42
}
```
{% endtab %}

{% tab title="Success Response" %}
```javascript
{
  "q": "v1/heartbeat/ping",
  "sid": 1,
  "d": 42
}
```
{% endtab %}

{% tab title="Failure Response" %}
```json
{
  "sig": 2,
  "q": "v1/heartbeat/ping",
  "errorType": "500",
  "sid": 1,
  "d": {
    "errorCode": 500,
    "errorMessage": "Failed to decode service message data"
  }
}
```
{% endtab %}
{% endtabs %}



