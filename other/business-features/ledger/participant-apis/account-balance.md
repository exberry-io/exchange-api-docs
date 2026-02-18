# Account Balance

## accountBalance

The API allows to retrieve a snapshot of current balance + updates when there are changes.

{% hint style="info" %}
qualifier: v1/exchange.ledger/accountBalance
{% endhint %}

### Request

<table><thead><tr><th width="125">Parameter</th><th width="86.44441731770831">Type</th><th>Description</th></tr></thead><tbody><tr><td>accountId</td><td>Int</td><td>Trading account id- system will respond with the ledger account balances associated with this trading account</td></tr></tbody></table>

### **Response**

List of balances

<table><thead><tr><th width="174.88887532552081">Parameter</th><th width="139.3333740234375">Type</th><th width="303.7777506510417">Description</th></tr></thead><tbody><tr><td>assetId</td><td>Int</td><td>Asset Id</td></tr><tr><td>assetName</td><td>String</td><td>Asset name</td></tr><tr><td>availableBalance</td><td>Decimal</td><td>Available balance</td></tr><tr><td>reservedBalance</td><td>Decimal</td><td>Reserved balance used for active orders or pending transactions</td></tr><tr><td>totalBalance</td><td>Decimal</td><td>Total balance<br>availableBalance + reservedBalance</td></tr><tr><td>transactionUnits</td><td>Decimal</td><td>Asset transaction units</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="128">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>2</td><td><code>Stream disconnected</code></td></tr><tr><td>1001</td><td><code>Wrong [FieldName] format</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1032</td><td><code>Account not found</code></td></tr><tr><td>2000</td><td><code>Ledger account not configured for selected trading account</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v1/exchange.ledger/accountBalance",
  "sid": 53,
  "d": {
    "accountId": "37"
  }
}
```
{% endtab %}

{% tab title="Success Response" %}
```json
{
  "q": "v1/exchange.ledger/accountBalance",
  "sid": 53,
  "d": {
    "balances": [
      {
        "assetId": 85,
        "assetName": "EUR",
        "availableBalance": 5000,
        "reservedBalance": 0,
        "totalBalance": 5000,
        "transactionUnits": "EUR"
      },
      {
        "assetId": 10,
        "assetName": "GOLD",
        "availableBalance": 3265.167,
        "reservedBalance": 0,
        "totalBalance": 3265.167,
        "transactionUnits": "oz"
      },
      {
        "assetId": 97,
        "assetName": "Silver",
        "availableBalance": 989984,
        "reservedBalance": 0,
        "totalBalance": 989984,
        "transactionUnits": "oz"
      },
      {
        "assetId": 44,
        "assetName": "USD",
        "availableBalance": 4680.24,
        "reservedBalance": 0,
        "totalBalance": 4680.24,
        "transactionUnits": "USD"
      }
    ]
  }
}
```
{% endtab %}

{% tab title="Failure Response" %}
```json
{
  "sig": 2,
  "q": "v1/exchange.ledger/accountBalance",
  "errorType": "500",
  "sid": 1,
  "d": {
    "errorCode": 1008,
    "errorMessage": "Insufficient permissions"
  }
}
```
{% endtab %}
{% endtabs %}

