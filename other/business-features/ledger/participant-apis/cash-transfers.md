# Cash Transfers

* Cash Transfers can be created by both MP and Trader
  * In case of MP, both Sender Account and Receiver Account should belong to the MP
  * In case of Trader, trader should be authorized for both Sender Account and Receiver Account.
* Cash Transfers settle immediately without going through a workflow

## createCashTransfer

This API allows creating cash transfers

{% hint style="info" %}
qualifier: v1/exchange.ledger/createCashTransfer
{% endhint %}

### Request

<table><thead><tr><th width="167.22222900390625">Parameter</th><th width="107.55550130208331">Type</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>eNum</td><td><p>Transfer Type</p><p>Allowed values:</p><p>- Cash</p></td></tr><tr><td>senderAccountId</td><td>Int</td><td>Ledger Account of the sender</td></tr><tr><td>sendingAssets <code>optional</code></td><td>[ ] Asset Objects</td><td><p>An array of Asset Objects (see the separate table below for more details)</p><p>The sending asset to the counterparty. Can have only 1 object.</p></td></tr><tr><td>receiverAccountId</td><td>Int</td><td>Ledger Account of the receiver</td></tr><tr><td>description <code>optional</code></td><td>String</td><td>Description</td></tr></tbody></table>

#### Asset Object

<table><thead><tr><th width="175">Parameter</th><th width="115.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>assetId</td><td>Int</td><td>Asset Id</td></tr><tr><td>amount</td><td>Decimal</td><td>Amount of the asset in transaction units</td></tr></tbody></table>

### **Response**

<table><thead><tr><th width="175.51654052734375">Parameter</th><th width="107.58418782552081">Type</th><th>Description</th></tr></thead><tbody><tr><td>senderAccountId</td><td>Int</td><td>Same as the request</td></tr><tr><td>receiverAccountId</td><td>Int</td><td>Same as the request</td></tr><tr><td>transactionIds</td><td>[ ]Int</td><td>Transaction ids created by the transfer</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="128">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Exchange is unavailable</code></td></tr><tr><td>100</td><td><code>Missing or invalid parameter: [FieldName]</code></td></tr><tr><td>101</td><td><code>Minimum [FieldName] is [Asset.minTransfer]</code><br><code>[Field] should be an increment of [transferIncrement]</code></td></tr><tr><td>102</td><td><p><code>Trading is not allowed</code></p><p><code>Not allowed</code></p></td></tr><tr><td>200</td><td><code>Insufficient balance: [Amt] [TransactionUnits] of [AssetName] are missing</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr><tr><td>1020</td><td><code>Unsupported counterparty</code><br><code>Both sides can’t have the same ledgerAccountId</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v1/exchange.ledger/createCashTransfer",
  "sid": 58,
  "d": {
    "type": "Cash",
    "senderAccountId": "36",
    "senderAssets": [
      {
        "assetId": "44",
        "amount": "105"
      }
    ],
    "receiverAccountId": "826",
    "description": "Transfer 242"
  }
}
```
{% endtab %}

{% tab title="Success Response" %}
```json
{
  "q": "v1/exchange.ledger/createCashTransfer",
  "sid": 58,
  "d": {
    "senderAccountId": "36",
    "receiverAccountId": "826",
    "transactionIds": [
      "7731",
      "7732"
    ]
  }
}
```
{% endtab %}

{% tab title="Failure Response" %}
```json
{
  "sig": 2,
  "q": "v1/exchange.ledger/createCashTransfer",
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



