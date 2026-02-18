# Reporting

## Holdings Summary

&#x20;`Holdings Summary` API is used to retrieve a list of holdings.

{% hint style="info" %}
qualifier: v1/exchange.reporting/mp/holdingsSummary
{% endhint %}

### Request

<table><thead><tr><th width="164.33333333333331">Parameter</th><th width="105.2222900390625">Type</th><th>Description</th></tr></thead><tbody><tr><td>groupBy <code>optional</code></td><td>eNum</td><td><p>One of the below options:</p><p>- MP: aggregation will be by MP</p><p>- Account: aggregation will be by account<br></p><p>Group by will always be by asset &#x26; eod date, and in addition the above selection</p></td></tr><tr><td>eodDateFrom  <code>optional</code></td><td>Date</td><td>Search for the holdings that EOD date ≥ eodDateFrom <br>Format: YYYY-MM-DD</td></tr><tr><td>eodDateTo  <code>optional</code></td><td>Date</td><td>Search for the holdings that EOD date&#x3C; eodDateTo<br>Format: YYYY-MM-DD</td></tr><tr><td>mpId  <code>optional</code></td><td>int</td><td>Search for the holdings by MP Id</td></tr><tr><td>ledgerAaccountId  <code>optional</code></td><td>int</td><td>Search for the holdings by ledger account id</td></tr><tr><td>assetId  <code>optional</code></td><td>int</td><td>Search for the holdings by asset Id</td></tr><tr><td>limit  <code>optional</code></td><td>int </td><td><p>How many records to include in each page </p><p></p><p>If nothing was sent default is 25</p><p>Max value = 100</p></td></tr><tr><td>offset  <code>optional</code></td><td>int</td><td><p>Which record to start send from (if you want page 3 and there are 50 records per page offset should be 150) </p><p>If nothing was sent default is 0 (=first record)</p></td></tr></tbody></table>

### **Response**

&#x20;`Holdings Summary` response provides list of all holdings for the requested dates.

<table><thead><tr><th width="199.7777099609375">Field</th><th>Description</th></tr></thead><tbody><tr><td>eodDate</td><td><p>Date of EOD when holding was stored</p><p>Format: YYYY-MM-DD</p></td></tr><tr><td>mpName <code>optional</code></td><td>MP Name, will not be included if groupBy is empty</td></tr><tr><td>instrumentId</td><td>Instrument identifier</td></tr><tr><td>ledgerAccountName <code>optional</code></td><td>Ledger account name, will not be included if groupBy= MP or empty </td></tr><tr><td>assetName</td><td>Asset name</td></tr><tr><td>eodHoldings</td><td><p>If groupBy = MP: </p><ul><li>Sum of all accounts totalBalance per EOD date and Asset &#x26; MP for the selected period.</li></ul><p>Else  If groupBy = Account</p><ul><li>Total balance per account per asset per EOD date</li></ul><p>Else (groupBy is empty)</p><ul><li>Sum of all accounts totalBalance for all MPs  per EOD date and Asset for the selected period.</li></ul></td></tr><tr><td>transactionUnits</td><td>Asset transaction units as at EOD Date</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="152">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Temporary failure to retrieve this data</code></td></tr><tr><td>1001</td><td><code>Wrong [FieldName] format</code> <br><code>eodDateTo must be >= eodDateFrom</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v1/exchange.reporting/mp/holdingsSummary",
  "sid": 100,
  "d": {
    "eodDateFrom": "2025-12-01",
    "eodDateTo": "2025-12-31",
    "limit": 10
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "q": "v1/exchange.reporting/mp/holdingsSummary",
  "sid": 100,
  "d": {
    "holdings": [
      {
        "eodDate": "2025-12-01",
        "assetName": "GOLD",
        "eodHoldings": 39.019,
        "transactionUnits": "oz"
      },
      {
        "eodDate": "2025-12-01",
        "assetName": "USD",
        "eodHoldings": 11328.49,
        "transactionUnits": "USD"
      },
      {
        "eodDate": "2025-12-01",
        "assetName": "USD-2",
        "eodHoldings": 50,
        "transactionUnits": "USD-2"
      },
      {
        "eodDate": "2025-12-01",
        "assetName": "USD-3",
        "eodHoldings": 100,
        "transactionUnits": "USD"
      },
      {
        "eodDate": "2025-12-02",
        "assetName": "GOLD",
        "eodHoldings": 39.019,
        "transactionUnits": "oz"
      },
      {
        "eodDate": "2025-12-02",
        "assetName": "USD",
        "eodHoldings": 11328.49,
        "transactionUnits": "USD"
      },
      {
        "eodDate": "2025-12-02",
        "assetName": "USD-2",
        "eodHoldings": 50,
        "transactionUnits": "USD-2"
      },
      {
        "eodDate": "2025-12-02",
        "assetName": "USD-3",
        "eodHoldings": 100,
        "transactionUnits": "USD"
      },
      {
        "eodDate": "2025-12-03",
        "assetName": "GOLD",
        "eodHoldings": 39.019,
        "transactionUnits": "oz"
      },
      {
        "eodDate": "2025-12-03",
        "assetName": "USD",
        "eodHoldings": 11328.49,
        "transactionUnits": "USD"
      }
    ],
    "count": 104
  }
}
```
{% endtab %}
{% endtabs %}

## Transactions

&#x20;`Transactions` API is used to retrieve a list of Transactions.

{% hint style="info" %}
qualifier:v1/exchange.reporting/mp/transactions
{% endhint %}

### Request

<table><thead><tr><th width="164.33333333333331">Parameter</th><th width="123">Type</th><th>Description</th></tr></thead><tbody><tr><td>dateFrom <code>optional</code></td><td>DateTime</td><td>Search for the transactions that Timestamp ≥ dateFrom<br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</td></tr><tr><td>dateTo <code>optional</code></td><td>DateTime</td><td>Search for the transactions that Timestamp &#x3C; dateTo<br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</td></tr><tr><td>assetId <code>optional</code></td><td>int</td><td>Search by assetId</td></tr><tr><td>mpId <code>optional</code></td><td>int</td><td>Search by mpId</td></tr><tr><td>ledgerAaccountId <code>optional</code></td><td>int</td><td>Search by Ledger account id</td></tr><tr><td>transactionId <code>optional</code></td><td>int</td><td>Search by transactionId</td></tr><tr><td>transactionTypes <code>optional</code></td><td>eNum[]</td><td><p>Search by list of transaction types.</p><p></p><p>Any combination of the below values:</p><p>- Deposit</p><p>- Withdrawal</p><p>- Adjustment</p><p>- Trade</p><p>- DvP</p><p>- RvP</p><p>- FoP</p><p>- CashTransfer</p><p>- DvD</p><p></p><p>Empty = All except:</p><p>- ReserveBalance </p><p>- ReleaseBalance</p></td></tr><tr><td>limit <code>optional</code></td><td>int </td><td><p>How many records to include in each page </p><p></p><p>If nothing was sent default is 25</p><p>Max value = 100</p></td></tr><tr><td>offset <code>optional</code></td><td>int</td><td><p>Which record to start send from (if you want page 3 and there are 50 records per page offset should be 150) </p><p>If nothing was sent default is 0 (=first record)</p></td></tr></tbody></table>

### **Response**

&#x20;`Transactions` response provides close to real time list of all Transactions for the requested period.

<table><thead><tr><th width="180.888916015625">Field</th><th>Description</th></tr></thead><tbody><tr><td>timestamp</td><td><p>The timestamp of the transaction</p><p>Format: YYYY-MM-DDThh:mm:ss.SSS</p></td></tr><tr><td>transactionId</td><td>Transaction Id</td></tr><tr><td>transactionType</td><td>Transaction type</td></tr><tr><td>amount</td><td><p>Transaction amount (in transaction units)</p><p></p><p>The amount will be negative when:</p><p>- Total balance is to be reduced</p><p>- Reserve balance is released </p></td></tr><tr><td>transactionUnits</td><td>Asset transaction units at the time transaction happend</td></tr><tr><td>amountInOutUnits <code>optional</code></td><td><p>Amount In inOutUnits at the time of the transaction <br></p><p>Returned only in case of below transactionType</p><p>- Deposit</p><p>- Withdrawal</p><p>- Redemption</p><p>- Adjustment</p></td></tr><tr><td>inOutUnits <code>optional</code></td><td><p>inOutUnits of the assetId at the time of the event<br></p><p>Returned only in case of <code>amountInOutUnits</code> returned </p></td></tr><tr><td>mpId</td><td>MP Id</td></tr><tr><td>mpName</td><td>MP Name</td></tr><tr><td>ledgerAccountId</td><td>Id of the Ledger Account that the transaction occurred</td></tr><tr><td>ledgerAccountName</td><td>Ledger account name</td></tr><tr><td>assetId</td><td>Id of the asset</td></tr><tr><td>assetName</td><td>Asset name</td></tr><tr><td>description</td><td><p>Transaction description.</p><p>Per transactionType, see the table below</p></td></tr><tr><td>linkId <code>optional</code></td><td>Used for grouping few transactions generated in below cases:<br>- Trade (4 transactions)<br>- Transfer DvP (4 transactions)<br>- Transfer DvP/RvP (total 4 transactions, 2 of each type)<br>- Transfer FOP (2 transactions)<br>- Transfer DvD (up to 6 transactions)</td></tr><tr><td>counterpartyMpId <code>optional</code></td><td><p>Counterparty MP</p><p></p><p>Returned only in case of Transfers (DvP/ RvP /FoP/ DvD )</p></td></tr><tr><td>counterpartyMpName <code>optional</code></td><td>Counterparty MP Name</td></tr></tbody></table>

**Description Field**

<table><thead><tr><th width="252">Transaction Type</th><th>Description</th></tr></thead><tbody><tr><td>Trade</td><td>[symbol] |trade |[matchId] </td></tr><tr><td>Reserve Balance &#x26; Release Balance</td><td><p>For orders: </p><p>[symbol] |order |orderId] </p><p><br>For quotes: </p><p>[symbol] |quote |quoteId] </p><p><br>For alleged trades: </p><p>[symbol] |allegedTrade |allegedTradeId] </p><p><br>For Withdrawal:</p><p>[Asset.name] |withdrawal request |[transaction request Id]<br><br>For alleged transfer:</p><p>[Asset.name] |allegedTransfer |[alleged transfer id]</p></td></tr><tr><td>Other</td><td>Free text captured by initiator </td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="152">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Temporary failure to retrieve this data</code></td></tr><tr><td>1001</td><td><code>Wrong [FieldName] format</code> <br><code>Invalid parameter: [FieldName]</code> <br><code>dateTo must be greater than dateFrom</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v1/exchange.reporting/mp/transactions",
  "sid": 100,
  "d": {
    "dateFrom": "2025-12-01T08:42:16.092",
    "dateTo": "2025-12-31T08:42:16.092",
    "limit": 5
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "q": "v1/exchange.reporting/mp/transactions",
  "sid": 100,
  "d": {
    "transactions": [
      {
        "timestamp": "2025-12-16T07:03:39.230333",
        "transactionId": 7100,
        "transactionType": "Trade",
        "amount": 32.148,
        "transactionUnits": "oz",
        "mpId": 9,
        "mpName": "JPM1",
        "ledgerAccountId": 14,
        "ledgerAccountName": "Acc1",
        "assetId": 10,
        "assetName": "GOLD",
        "description": "GOLD/USD|trade|13",
        "linkId": 1385
      },
      {
        "timestamp": "2025-12-16T07:03:39.230333",
        "transactionId": 7097,
        "transactionType": "Trade",
        "amount": -0.33,
        "transactionUnits": "USD",
        "mpId": 9,
        "mpName": "JPM1",
        "ledgerAccountId": 14,
        "ledgerAccountName": "Acc1",
        "assetId": 44,
        "assetName": "USD",
        "description": "GOLD/USD|trade|13",
        "linkId": 1385
      },
      {
        "timestamp": "2025-12-16T07:02:32.387301",
        "transactionId": 7096,
        "transactionType": "Trade",
        "amount": 0.322,
        "transactionUnits": "oz",
        "mpId": 9,
        "mpName": "JPM1",
        "ledgerAccountId": 14,
        "ledgerAccountName": "Acc1",
        "assetId": 10,
        "assetName": "GOLD",
        "description": "GOLD/USD|trade|12",
        "linkId": 1384
      },
      {
        "timestamp": "2025-12-16T07:02:32.387301",
        "transactionId": 7093,
        "transactionType": "Trade",
        "amount": -0.33,
        "transactionUnits": "USD",
        "mpId": 9,
        "mpName": "JPM1",
        "ledgerAccountId": 14,
        "ledgerAccountName": "Acc1",
        "assetId": 44,
        "assetName": "USD",
        "description": "GOLD/USD|trade|12",
        "linkId": 1384
      },
      {
        "timestamp": "2025-12-15T14:58:29.552781",
        "transactionId": 7091,
        "transactionType": "Trade",
        "amount": 0.322,
        "transactionUnits": "oz",
        "mpId": 9,
        "mpName": "JPM1",
        "ledgerAccountId": 14,
        "ledgerAccountName": "Acc1",
        "assetId": 10,
        "assetName": "GOLD",
        "description": "GOLD/USD|trade|11",
        "linkId": 1365
      }
    ],
    "count": 11
  }
}
```
{% endtab %}
{% endtabs %}



## Transaction Requests

&#x20;`Transaction Requests` API is used to retrieve the current state of a list of Transaction Requests.

{% hint style="info" %}
qualifier: v1/exchange.reporting/mp/transactionRequests
{% endhint %}

### Request

<table><thead><tr><th width="151.00002034505206">Parameter</th><th width="119.6666259765625">Type</th><th>Description</th></tr></thead><tbody><tr><td>dateFrom <code>optional</code></td><td>DateTime</td><td>Search for the transaction requests that Created At ≥ dateFrom<br>Format:YYYY-MM-DDThh:mm:ss[.SSS]</td></tr><tr><td>dateTo <code>optional</code></td><td>DateTime</td><td>Search for the transaction requests that Created At &#x3C; dateTo<br>Format: YYYY-MM-DDThh:mm:ss[.SSS]</td></tr><tr><td>type <code>optional</code></td><td>eNum</td><td><p>Available values:<br>- Deposit<br>- Withdrawal</p><p>If empty, all types</p></td></tr><tr><td>statuses</td><td>[ ]eNum</td><td>List of below values:<br>- Instructed<br>- InProcess<br>- Settled<br>- Cancelled</td></tr><tr><td>assetId <code>optional</code></td><td>int</td><td>Search by assetId</td></tr><tr><td>mpId <code>optional</code></td><td>int</td><td>Search by mpId</td></tr><tr><td>ledgerAccountId <code>optional</code></td><td>int</td><td>Search by Ledger account id</td></tr><tr><td>id <code>optional</code></td><td>int</td><td>Search by transaction request id</td></tr><tr><td>orderBy <code>optional</code></td><td>object</td><td><p>object with 2 parameters:<br>- field (String)  (only createdAt, lastEventTimestamp  is allowed) <br>- direction (Asc, Desc) direction</p><p></p><p>If nothing or invalid field was sent the default is [createdAt, Desc]</p></td></tr><tr><td>limit <code>optional</code></td><td>int</td><td><p>How many records to include in each page <br></p><p>If nothing was sent default is 25</p><p>Max value = 100</p></td></tr><tr><td>offset <code>optional</code></td><td>int</td><td><p>Which record to start send from (if you want page 3 and there are 50 records per page offset should be 150) </p><p>If nothing was sent default is 0 (=first record)</p></td></tr></tbody></table>

### **Response**

&#x20;`Transaction Request` response provides close to real time list of all Transaction Request for the requested period.

<table><thead><tr><th width="182">Field</th><th>Description</th></tr></thead><tbody><tr><td>createdAt</td><td>Timestamp of the transaction request creation<br>Format YYYY-MM-DDThh:mm:ss.SSS</td></tr><tr><td>id</td><td>transaction request id</td></tr><tr><td>type</td><td><p>Type</p><p>Available values:<br>- Deposit<br>- Withdrawal</p></td></tr><tr><td>mpId</td><td>MP Id</td></tr><tr><td>mpName</td><td>MP Name</td></tr><tr><td>status</td><td><p>Status </p><p>Available values:<br>- Instructed <br>- InProcess<br>- Settled<br>- Cancelled</p></td></tr><tr><td>ledgerAccountId</td><td>Id of the Ledger Account of the transaction request</td></tr><tr><td>ledgerAccountName</td><td>Ledger account name</td></tr><tr><td>assetId</td><td>Id of the asset</td></tr><tr><td>assetName</td><td>Asset name</td></tr><tr><td>amount</td><td>Amount </td></tr><tr><td>units</td><td>Asset inOutUnits of the assetId at the time of creating</td></tr><tr><td>description <code>optional</code></td><td>Description </td></tr><tr><td>instructionDetailsId<code>optional</code></td><td>uuid to be used to get url for to latest instructionDetails</td></tr><tr><td>assetDetailsId<code>optional</code></td><td>uuid to be used to get url to latest assetDetails</td></tr><tr><td>lastEventTimestamp</td><td><p>eventTimestamp of the last event of the transaction request </p><p>Fformat YYYY-MM-DDThh:mm:ss.SSS</p></td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="152">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Temporary failure to retrieve this data</code></td></tr><tr><td>1001</td><td><code>Wrong [FieldName] format</code> <br><code>dateTo must be greater than dateFrom</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v1/exchange.reporting/mp/transactionRequests",
  "sid": 100,
  "d": {
    "dateFrom": "2025-01-01T08:42:16.092",
    "dateTo": "2025-12-31T08:42:16.092",
    "limit": 5
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "q": "v1/exchange.reporting/mp/transactionRequests",
  "sid": 100,
  "d": {
    "transactionRequests": [
      {
        "createdAt": "2025-09-22T11:52:45.564068",
        "id": 213,
        "type": "Deposit",
        "mpId": 9,
        "mpName": "JPM1",
        "status": "Settled",
        "ledgerAccountId": 14,
        "ledgerAccountName": "Acc1",
        "assetId": 44,
        "assetName": "USD",
        "amount": 950,
        "units": "USD",
        "description": "23534445",
        "instructionDetailsId": "5a56d2c8-0ae9-48d7-bd7f-eccdb761834d",
        "assetDetailsId": "414459d9-e7a2-4c68-9194-fee475cf3575",
        "lastEventTimestamp": "2025-09-22T12:03:35.307328"
      },
      {
        "createdAt": "2025-09-11T13:16:13.82398",
        "id": 201,
        "type": "Withdrawal",
        "mpId": 9,
        "mpName": "JPM1",
        "status": "Instructed",
        "ledgerAccountId": 14,
        "ledgerAccountName": "Acc1",
        "assetId": 44,
        "assetName": "USD",
        "amount": 1,
        "units": "USD",
        "description": "dwefef",
        "lastEventTimestamp": "2025-09-11T13:16:13.82398"
      },
      {
        "createdAt": "2025-08-04T08:57:29.393645",
        "id": 56,
        "type": "Withdrawal",
        "mpId": 9,
        "mpName": "JPM1",
        "status": "Instructed",
        "ledgerAccountId": 14,
        "ledgerAccountName": "Acc1",
        "assetId": 44,
        "assetName": "USD",
        "amount": 1,
        "units": "USD",
        "description": "wd1",
        "lastEventTimestamp": "2025-08-04T08:57:29.393645"
      }
    ],
    "count": 3
  }
}
```
{% endtab %}
{% endtabs %}



## Alleged Transfers

&#x20;`Alleged Transfers` API is used to retrieve the current state of a list of Alleged Transfers.

{% hint style="info" %}
qualifier: v1/exchange.reporting/mp/allegedTransfers
{% endhint %}

### Request

<table><thead><tr><th width="157.66664632161456">Parameter</th><th width="116.3333740234375">Type</th><th>Description</th></tr></thead><tbody><tr><td>dateFrom <code>optional</code></td><td>DateTime</td><td><p>Optional</p><p>Search for the transaction requests that Created At ≥ dateFrom. Format:YYYY-MM-DDThh:mm:ss[.SSS]</p></td></tr><tr><td>dateTo <code>optional</code></td><td>DateTime</td><td><p>Optional</p><p>Search for the transaction requests that Created At &#x3C; dateTo . Format: YYYY-MM-DDThh:mm:ss[.SSS]</p></td></tr><tr><td>type <code>optional</code></td><td>eNum</td><td><p>Available values:<br>- DvP<br>- RvP<br>- FoP<br>- FoP_Receive<br>- DvD<br></p><p>If empty, all types</p></td></tr><tr><td>statuses <code>optional</code></td><td>[ ]eNum</td><td>List of below values:<br>- Instructed<br>- Settled<br>- Cancelled</td></tr><tr><td>assetId <code>optional</code></td><td>int</td><td>Search by assetId in sendingAssetId and receivingAssetId fields</td></tr><tr><td>mpId <code>optional</code></td><td>int</td><td>Search by mpId in both mpId and counterpartyMpId</td></tr><tr><td>ledgerAccountId <code>optional</code></td><td>int</td><td>Search by Ledger account id in ledgerAccountId </td></tr><tr><td>allegedTransferId <code>optional</code></td><td>int</td><td>Search by Alleged Transfer id</td></tr><tr><td>externalId <code>optional</code></td><td>int</td><td>Search by External id</td></tr><tr><td>orderBy <code>optional</code></td><td>object</td><td><p>object with 2 parameters:<br>- field (String) <br>- Allowed values: createdAt, lastEventTimestamp<br>- direction (Asc, Desc) direction</p><p></p><p>If nothing or invalid field was sent the default is [createdAt, Desc]</p></td></tr><tr><td>limit <code>optional</code></td><td>int</td><td><p>How many records to include in each page </p><p></p><p>If nothing was sent default is 25</p><p>Max value = 100</p></td></tr><tr><td>offset <code>optional</code></td><td>int</td><td><p>Which record to start send from (if you want page 3 and there are 50 records per page offset should be 150) </p><p>If nothing was sent default is 0 (=first record)</p></td></tr></tbody></table>

### **Response**

`Alleged Transfers` response provides close to real time list of all Alleged Transfers for the requested period.

<table><thead><tr><th width="222">Field</th><th>Description</th></tr></thead><tbody><tr><td>createdAt</td><td>Timestamp of the alleged transfer creation<br>Format YYYY-MM-DDThh:mm:ss.SSS</td></tr><tr><td>allegedTransferId</td><td>Unique is generated by the system for the alleged transfer</td></tr><tr><td>externalId</td><td>External Id agreed between the counterparties of the transfer and is used in the matching criteria to uniquely identify transfers.</td></tr><tr><td>mpId</td><td>MP Id</td></tr><tr><td>mpName</td><td>MP Name </td></tr><tr><td>ledgerAccountId</td><td>Ledger Account Id </td></tr><tr><td>ledgerAccountName</td><td>Ledger Account Name of the ledgerAccountId</td></tr><tr><td>type</td><td>Transfer type </td></tr><tr><td>sendingAssets</td><td><p>Sending Assets</p><p>An array of Asset Objects (see the separate table below for more details)</p></td></tr><tr><td>receivingAssets</td><td>Receiving Assets<br>An array of Asset Objects (see the separate table below for more details)</td></tr><tr><td>description</td><td>Description</td></tr><tr><td>counterpartyMpId</td><td>CP MP Id </td></tr><tr><td>counterpartyMpName</td><td>Current MP Name of the counterpartyMpId</td></tr><tr><td>status</td><td>Available values:<br>- Instructed: alleged transfer can be matched with another alleged transfer<br>- Settled: Alleged transfer is matched by the system<br>- Cancelled: Alleged transfer is cancelled</td></tr><tr><td>expireTime</td><td>Expiration time of the Alleged Transfer<br>Format: YYYY-MM-DDThh:mm:ss.SSSSSS</td></tr><tr><td>cancelReason</td><td>One of the below options:<br>- Expiration - when cancelled due to EOD expiration<br>- CancelRequest - when cancelled by the reporting MP<br>- CancelOrderOnBehalf - when cancelled by admin user</td></tr><tr><td>lastEventTimestamp</td><td><p>eventTimestamp of the last event of the alleged transfer </p><p>Format YYYY-MM-DDThh:mm:ss.SSS</p></td></tr></tbody></table>

#### Asset Object

<table data-header-hidden><thead><tr><th width="224"></th><th width="412.888916015625"></th></tr></thead><tbody><tr><td>Field</td><td>Description </td></tr><tr><td>assetId</td><td>Asset Id</td></tr><tr><td>assetName</td><td>Current Asset Name of the asset</td></tr><tr><td>amount</td><td>Amount of the asset in transaction units</td></tr><tr><td>units</td><td>Current Asset transactionsUnits of the asset</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="152">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Temporary failure to retrieve this data</code></td></tr><tr><td>1001</td><td><code>Wrong [FieldName] format</code> <br><code>dateTo must be greater than dateFrom</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v1/exchange.reporting/mp/allegedTransfers",
  "sid": 100,
  "d": {
    "dateFrom": "2025-01-01T08:42:16.092",
    "dateTo": "2026-12-31T08:42:16.092",
    "limit": 5
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "q": "v1/exchange.reporting/mp/allegedTransfers",
  "sid": 100,
  "d": {
    "allegedTransfers": [
      {
        "allegedTransferId": 311,
        "externalId": "12341353",
        "type": "DvD",
        "status": "Cancelled",
        "mpId": 15,
        "mpName": "JPM2",
        "ledgerAccountId": 36,
        "ledgerAccountName": "Acc3",
        "sendingAssets": [
          {
            "assetId": 10,
            "assetName": "GOLD",
            "amount": 1,
            "units": "oz"
          }
        ],
        "receivingAssets": [
          {
            "assetId": 97,
            "assetName": "Silver",
            "amount": 1000000,
            "units": "oz"
          },
          {
            "assetId": 44,
            "assetName": "USD",
            "amount": 100000,
            "units": "USD"
          }
        ],
        "counterpartyMpId": 9,
        "counterpartyMpName": "JPM1",
        "cancelReason": "CancelRequest",
        "createdAt": "2026-01-14T05:41:48.382233",
        "lastEventTimestamp": "2026-01-14T06:06:04.184731"
      },
      {
        "allegedTransferId": 310,
        "externalId": "123334",
        "type": "DvD",
        "status": "Cancelled",
        "mpId": 15,
        "mpName": "JPM2",
        "ledgerAccountId": 36,
        "ledgerAccountName": "Acc3",
        "sendingAssets": [
          {
            "assetId": 10,
            "assetName": "GOLD",
            "amount": 1,
            "units": "oz"
          }
        ],
        "receivingAssets": [
          {
            "assetId": 97,
            "assetName": "Silver",
            "amount": 100000,
            "units": "oz"
          },
          {
            "assetId": 44,
            "assetName": "USD",
            "amount": 100,
            "units": "USD"
          }
        ],
        "description": "TFR1",
        "counterpartyMpId": 9,
        "counterpartyMpName": "JPM1",
        "cancelReason": "Expiration",
        "createdAt": "2026-01-14T05:39:57.912466",
        "lastEventTimestamp": "2026-01-14T18:30:00.010941"
      },
      {
        "allegedTransferId": 245,
        "externalId": "555",
        "type": "RvP",
        "status": "Cancelled",
        "mpId": 15,
        "mpName": "JPM2",
        "ledgerAccountId": 826,
        "ledgerAccountName": "Acc6",
        "sendingAssets": [
          {
            "assetId": 44,
            "assetName": "USD",
            "amount": 1,
            "units": "USD"
          }
        ],
        "receivingAssets": [
          {
            "assetId": 10,
            "assetName": "GOLD",
            "amount": 0.01,
            "units": "oz"
          }
        ],
        "counterpartyMpId": 9,
        "counterpartyMpName": "JPM1",
        "cancelReason": "CancelOrderOnBehalf",
        "createdAt": "2025-12-16T12:06:31.174647",
        "lastEventTimestamp": "2025-12-16T12:07:27.892344"
      },
      {
        "allegedTransferId": 244,
        "externalId": "555",
        "type": "RvP",
        "status": "Cancelled",
        "mpId": 15,
        "mpName": "JPM2",
        "ledgerAccountId": 826,
        "ledgerAccountName": "Acc6",
        "sendingAssets": [
          {
            "assetId": 44,
            "assetName": "USD",
            "amount": 1,
            "units": "USD"
          }
        ],
        "receivingAssets": [
          {
            "assetId": 68,
            "assetName": "AssetX",
            "amount": 0.01,
            "units": "oz"
          }
        ],
        "counterpartyMpId": 15,
        "counterpartyMpName": "JPM2",
        "cancelReason": "CancelOrderOnBehalf",
        "createdAt": "2025-12-16T12:06:02.857466",
        "lastEventTimestamp": "2025-12-16T12:06:14.234592"
      },
      {
        "allegedTransferId": 243,
        "externalId": "555",
        "type": "DvP",
        "status": "Settled",
        "mpId": 15,
        "mpName": "JPM2",
        "ledgerAccountId": 36,
        "ledgerAccountName": "Acc3",
        "sendingAssets": [
          {
            "assetId": 10,
            "assetName": "GOLD",
            "amount": 0.01,
            "units": "oz"
          }
        ],
        "receivingAssets": [
          {
            "assetId": 44,
            "assetName": "USD",
            "amount": 1,
            "units": "USD"
          }
        ],
        "counterpartyMpId": 15,
        "counterpartyMpName": "JPM2",
        "createdAt": "2025-12-16T12:04:31.365095",
        "lastEventTimestamp": "2025-12-16T12:07:46.875187"
      }
    ],
    "count": 12
  }
}
    "count": 3
  }
}
```
{% endtab %}
{% endtabs %}

