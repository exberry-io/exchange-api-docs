# Authentication

## createSession

The `createSession` API lets you authenticate to exchange API.

Prior to any exchange API call you must create a valid session, this session remains active as long as WebSocket connection remains open. Any market participant connected to exchange will be provided with 1 (or more) set of `apiKey` and `secret`, each `apiKey` is assigned with the relevant permissions.\
A single WebSocket connection can have single active session at a time.

{% hint style="info" %}
qualifier: exchange.market/createSession
{% endhint %}

### **Request**

<table><thead><tr><th width="121.33333333333331">Parameter</th><th width="144">Type</th><th>Description</th></tr></thead><tbody><tr><td>apiKey</td><td>String</td><td>Unique market participant API key provided by exchange operator</td></tr><tr><td>timestamp</td><td>Unix timestamp</td><td>Login timestamp in milliseconds, must be now in GMT</td></tr><tr><td>signature</td><td>String</td><td><code>HMAC SHA256 signature</code> computed using provided <code>secret</code> key and message body.<br>Example:<br><code>Message</code> = <code>"apiKey":"1234567abcdz","timestamp":"1558941516123"</code><br><code>secret</code> = <code>MySecretKey</code><br><code>signature = 265cfbc40c22355d6c1ecc1f3a1e87e8c46954db9096a7bd6967241dd8bc65b6</code></td></tr></tbody></table>

How to compute the signature

```bash
echo -n '"apiKey":"1234567abcdz","timestamp":"1558941516123"' | openssl dgst -sha256 -hmac 'MySecretKey'
(stdin)= 265cfbc40c22355d6c1ecc1f3a1e87e8c46954db9096a7bd6967241dd8bc65b6
```

### **Error Codes**

<table><thead><tr><th width="141">Code</th><th>Message</th></tr></thead><tbody><tr><td>6000</td><td><code>Authentication failed</code></td></tr><tr><td>6001</td><td><code>Wrong timestamp</code></td></tr><tr><td>6002</td><td><code>Missing fields: [Fieldname]</code></td></tr><tr><td>6003</td><td><code>Create session failed</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
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
```json
{
        "q":"exchange.market/createSession",
        "sid":15,
        "d":{}
}
```
{% endtab %}

{% tab title="Failure Response" %}
```json
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
