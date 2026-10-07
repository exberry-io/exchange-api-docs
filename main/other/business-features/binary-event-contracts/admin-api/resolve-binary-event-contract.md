# Resolve Binary Event Contract

<mark style="color:blue;">NEW v1.57.0</mark>

This API allows operators to resolve a Binary Event Contract instruments.

{% hint style="info" %}
\[POST] {Admin REST}/api/operations/resolve-binary-event-contract
{% endhint %}

### Request

<table><thead><tr><th width="186.0869140625">Parameter</th><th width="113.95247395833331">Type</th><th>Description</th></tr></thead><tbody><tr><td>instrumentId</td><td>Int String</td><td>Instrument ID to resolve</td></tr><tr><td>resolution</td><td>eNum</td><td>Resolution, allowed values:<br>- YES<br>- NO</td></tr></tbody></table>

### **Response**

Empty response.

### **Error Codes**

<table><thead><tr><th width="128">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>System is unavailable</code></td></tr><tr><td>100</td><td><code>Missing or invalid parameter: [FieldName]</code></td></tr><tr><td>101</td><td><code>instrumentId not found</code><br><code>Not allowed before stopDate</code></td></tr><tr><td>102</td><td><code>Not allowed when LEDGER is not enabled</code><br><code>Action is allowed once per instrument</code></td></tr><tr><td>10001</td><td><code>Permission denied</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
curl --location 'https://admin-api-master.rnd.exberry-rnd.io/api/operations/resolve-binary-event-contract' \
--header 'Content-Type: application/json' \
--header 'Authorization: ••••••' \
--data '{
    "instrumentId": "97774",
    "resolution": "YES"
}'
```
{% endtab %}

{% tab title="Success Response" %}
```json
{}
```
{% endtab %}

{% tab title="Failure Response" %}
```json
{
    "code": "102",
    "message": "Action is allowed once per instrument"
}
```
{% endtab %}
{% endtabs %}
