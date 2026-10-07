# Tick Size List

This API provides snapshot and real-time updates for tick size details.&#x20;

Upon successful subscription, a snapshot of all tick size tables is sent. The last message of the snapshot contains `lastMessage=Y`. Any changes to the tick size tables after the snapshot are sent as subsequent updates.

In cases where there are no tick size tables to return, the system will send an empty message.

{% hint style="info" %}
qualifier: v1/exchange.marketdata/tickSizeList
{% endhint %}

### **Request**

No request parameters



### **Response**

Similar to [this](https://documenter.getpostman.com/view/6229811/TzCV3jcq#701e3523-7014-42ad-b20d-244b695b1039) with below changes:

* All numbers are non-stringified

### **Error Codes**

<table><thead><tr><th width="150">Code</th><th width="554.4285714285713">Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td><mark style="color:blue;">(NEW v1.52.0)</mark><br>2</td><td>Stream disconnected</td></tr></tbody></table>



### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```javascript
{
  "q": "v1/exchange.marketdata/tickSizeList",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 10,
  "d": {
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "q": "v1/exchange.marketdata/tickSizeList",
  "sid": 15,
  "d": {
    "id": 1045,
    "name": "Group 2",
    "items": [
      {
        "price": 2,
        "tickSize": 0.001
      },
      {
        "tickSize": 0.00001
      }
    ]
  }
}
```
{% endtab %}

{% tab title="Last message" %}
```javascript
{
  "q": "v1/exchange.marketdata/tickSizeList",
  "sid": 14,
  "d": {
    "id": "1045",
    "name": "Group 2",
    "items": [
      {
        "price": "2",
        "tickSize": 0.001
      },
      {
        "tickSize": 0.00001
      }
    ]
  }
}
```
{% endtab %}

{% tab title="Empty message" %}
```javascript
{
  "q": "v1/exchange.marketdata/tickSizeList",
  "sid": 10,
  "d": {
    "lastMessage": "Y"
  }
}
```
{% endtab %}
{% endtabs %}





