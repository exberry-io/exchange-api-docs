# Authentication API

## createSession

The `createSession` API lets you authenticate to exchange API.

Prior to any exchange API call you must create a valid session, this session remains active as long as WebSocket connection remains open. Any market participant connected to exchange will be provided with 1 (or more) set of `apiKey` and `secret`, each `apiKey` is assigned with the relevant permissions.

{% hint style="info" %}
`endpoint: exchange.market/createSession`
{% endhint %}

### **Request**

| Parameter | Type           | Description                                                                                                                                                                                                                                                                                                                                                       |
| --------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| apiKey    | String         | Unique market participant API key provided by exchange operator                                                                                                                                                                                                                                                                                                   |
| timestamp | Unix timestamp | Login timestamp in milliseconds, must be now in GMT                                                                                                                                                                                                                                                                                                               |
| signature | String         | <p><code>HMAC SHA256 signature</code> computed using provided <code>secret</code> key and message body.<br>Example:<br><code>Message</code> = <code>"apiKey":"1234567abcdz","timestamp":"1558941516123"</code><br><code>secret</code> = <code>MySecretKey</code><br><code>signature = 265cfbc40c22355d6c1ecc1f3a1e87e8c46954db9096a7bd6967241dd8bc65b6</code></p> |

How to compute the signature

```bash
echo -n '"apiKey":"1234567abcdz","timestamp":"1558941516123"' | openssl dgst -sha256 -hmac 'MySecretKey'
(stdin)= 265cfbc40c22355d6c1ecc1f3a1e87e8c46954db9096a7bd6967241dd8bc65b6
```

### **Error Codes**

| Code | Message                       |
| ---- | ----------------------------- |
| 6000 | `Authentication failed`       |
| 6001 | `Wrong timestamp`             |
| 6002 | `Missing fields: [Fieldname]` |

### **Samples**

{% tabs %}
{% tab title="Request" %}
```javascript
{
        "q":"exchange.market/createSession",
        "sid":15,
        "d":{
                    "apiKey":"6ggg",
                    "timestamp":"1563880778434",
                    "signature":"1842286ea411ebdc3dde3e6b3185cc85c19fa0140d0eecebb0c74137e9957981"
        }
}
```
{% endtab %}

{% tab title="Success Response" %}
```javascript
{
        "q":"exchange.market/createSession",
        "sid":15,
        "d":{}
}
```
{% endtab %}

{% tab title="Failure Response" %}
```javascript
{
  "sig": 2,
  "q": "exchange.market/createSession",
  "errorType": "401",
  "sid": 1,
  "d": {
    "errorCode": 6000,
    "errorMessage": "Authentication failed"
  }
}
```
{% endtab %}
{% endtabs %}
