# Instrument List

## General

This API provides snapshot and real-time updates for instruments details.&#x20;

The response is sorted by symbol (case-insensitive)

**Use Cases:**

1. **Retrieving All Instruments:** When all instruments are required, the API should be used without pagination and without filters.
2. **Retrieving a Specific Segment of Instruments:** For scenarios where a specific segment of instruments is needed, the API should be used without pagination but with filters.
3. **UI Applications:** When the consumer needs to paginate through the instruments, the API should be used with pagination enabled—utilizing the `fromSymbol` request parameter.

## instrumentList

{% hint style="info" %}
qualifier: v1/exchange.marketdata/instrumentList
{% endhint %}

This API does not include pagination and support filters on the request.&#x20;

Upon successful subscription, a snapshot of all active instruments is sent. The last message of the snapshot contains `lastMessage=Y`. Any changes to the instruments after the snapshot are sent as subsequent updates.

In cases where there are no instruments to return, the system will send an empty message.

**Snapshot**-  will receive all instruments that meet the filters from the request (If no filter exists, only “Active” instruments will be sent)

**Live updates-**&#x20;

You will receive live updates for the following:

1. **All Instruments from the Snapshot:** Continuous updates on all instruments included in the initial snapshot.
2. **New Instruments Meeting Filter Criteria:** Notifications for any new instruments that meet your specified filter criteria.
3. **Updated Instruments Meeting Filter Criteria:** Updates on instruments that were modified and now meet your requested filters.
4. **Instruments No Longer Meeting Filter Criteria:** If an instrument initially met the filters but no longer does due to an update, you will receive a notification for the first change. After this, no further updates will be sent for that instrument.
5. **Status Change Notifications:** When an instrument changes status from Active to Disabled/Archived, you will receive a notification of this change—unless the instrument now meets the specified filters.

### **Request**

Request parameters are the same as define [here ](https://documenter.getpostman.com/view/6229811/TzCV3jcq#c1b23770-4cc1-41b0-a035-15f3e280bbe4), below parameters are not included:

* `offset`
* `limit`
* `orderBy`

### **Response**

Similar to [this](https://documenter.getpostman.com/view/6229811/TzCV3jcq#c1b23770-4cc1-41b0-a035-15f3e280bbe4) with below changes:

* `groupIds` field is not returned
* All numbers are non-stringified

### **Error Codes**

<table><thead><tr><th width="165.27803690934905">Code</th><th width="554.4285714285713">Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td><mark style="color:blue;">(NEW v1.52.0)</mark><br>2</td><td>Stream disconnected</td></tr><tr><td>100</td><td>Missing or invalid parameter: [FieldName]</td></tr><tr><td>102</td><td>Single category should be sent</td></tr><tr><td>102</td><td>Wrong Sub Category</td></tr><tr><td>102</td><td>Strategies category should be sent</td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Subscription " %}
```json
{
  "q": "v1/exchange.marketdata/instrumentList",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 10,
  "d": {
  }
}
```
{% endtab %}

{% tab title="Snapshot " %}
```json
{
  "q": "v1/exchange.marketdata/instrumentList",
  "sid": 13,
  "d": {
    "id": 268,
    "symbol": "BA",
    "calendarId": 7,
    "activityStatus": "ACTIVE",
    "minQuantity": 0.000001,
    "maxQuantity": 1000000,
    "pricePrecision": 4,
    "quantityPrecision": 6,
    "description": "Boeing Co.",
    "quoteCurrency": "USD",
    "category": "E",
    "tradingModels": [
      "CLOB"
    ]
  }
}
```
{% endtab %}

{% tab title="Last message" %}
```json
{
  "q": "v1/exchange.marketdata/instrumentList",
  "sid": 13,
  "d": {
    "id": 4136,
    "symbol": "INST2",
    "calendarId": 7,
    "activityStatus": "ACTIVE",
    "minQuantity": 1,
    "maxQuantity": 1000000,
    "pricePrecision": 0,
    "quantityPrecision": 0,
    "description": "INST 2",
    "quoteCurrency": "USD",
    "category": "E",
    "lastMessage": "Y"
  }
}
```
{% endtab %}

{% tab title="Empty message" %}
```json
{
  "q": "v1/exchange.marketdata/instrumentList",
  "sid": 17,
  "d": {
    "lastMessage": "Y"
  }
}
```
{% endtab %}

{% tab title="Live updates" %}
```json
{
  "q": "v1/exchange.marketdata/instrumentList",
  "sid": 12,
  "d": {
    "id": 310,
    "symbol": "bonds",
    "calendarId": 10,
    "activityStatus": "ACTIVE",
    "minQuantity": 0.1,
    "maxQuantity": 77777,
    "pricePrecision": 4,
    "quantityPrecision": 5,
    "description": "bonds instrument - try",
    "quoteCurrency": "USD",
    "startDate": 1693475220,
    "stopDate": 1725097620,
    "category": "D",
    "subCategory": "B",
    "underlyingAssets": "V",
    "coupon": 5,
    "periodOfCoupon": "ANNUAL",
    "accrualConvention": "ACT_ACT",
    "maturityDate": "2024-01-01",
    "tradingModels": [
      "CLOB"
    ],
    "minQuantityTradeEntries": [
      {
        "type": "Block",
        "amount": 444
      },
      {
        "type": "EFRP",
        "amount": 555
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

## instrumentListWithPagination

{% hint style="info" %}
`qualifier: v1/exchange.marketdata/instrumentListWithPagination`
{% endhint %}

This API includes pagination and filters.&#x20;

**Snapshot**-  will receive all instruments that meet the filters from the request (If no filter exists, only “Active” instruments will be sent)

**Live updates-**&#x20;

You will receive live updates for the following:

* **Snapshot Instruments Only:** Updates will be sent exclusively for the instruments included in the initial snapshot.
* **No New Instruments:** You will not receive updates for any new instruments created after you subscribed.
* **Status Change Alerts:** If an instrument’s status changes from Active to Disabled, you will receive a notification. After this, no further updates will be sent for that instrument, even if it becomes active again.

### **Request**

Similar to [this](https://documenter.getpostman.com/view/6229811/TzCV3jcq#c1b23770-4cc1-41b0-a035-15f3e280bbe4) with the below changes:

Request parameters are the same as define [here ](https://documenter.getpostman.com/view/6229811/TzCV3jcq#c1b23770-4cc1-41b0-a035-15f3e280bbe4)with the below changes:

* `offset` is not included
* Additional parameter: &#x20;

<table><thead><tr><th width="139">Name</th><th width="88">Type</th><th>Description</th></tr></thead><tbody><tr><td>fromSymbol</td><td>String</td><td><p>Optional</p><p>From which symbol, start to send, exclusive.<br></p><p>For example:</p><p>We have instruments - “AA”, “BA”, “CB”, “CA”</p><p>Request: limit: 1, fromSymbol: “BA”</p><p>Response: “CB”</p></td></tr></tbody></table>

### **Response**

Similar to [this](https://documenter.getpostman.com/view/6229811/TzCV3jcq#c1b23770-4cc1-41b0-a035-15f3e280bbe4) with the below change:

* `totalCount` in Admin API called here `count`
* `fromSymbol` is returned&#x20;

### **Error Codes**

Same as [here ](instrument-list.md#error-codes)

### **Samples**

{% tabs %}
{% tab title="Subscription " %}
```json
{
  "q": "v1/exchange.marketdata/instrumentListWithPagination",
  "token": "ABC",
  "sid": 18,
  "d": {
    "fromSymbol": "bonds",
    "calendarId": 10
  }
}
```
{% endtab %}

{% tab title="Snapshot " %}
```json
{
  "q": "v1/exchange.marketdata/instrumentListWithPagination",
  "sid": 18,
  "d": {
    "messageType": "snapshot",
    "instruments": [
      {
        "id": 123,
        "symbol": "futures",
        "calendarId": 10,
        "activityStatus": "ACTIVE",
        "minQuantity": 0.1,
        "maxQuantity": 99999,
        "pricePrecision": 10,
        "quantityPrecision": 10,
        "minPrice": 1.25,
        "maxPrice": 500,
        "description": "Futures instrument",
        "quoteCurrency": "USD",
        "startDate": 1693296600,
        "stopDate": 1819440600,
        "category": "F",
        "subCategory": "F",
        "underlyingAssets": "F",
        "underlyingInstrumentId": 240,
        "deliveryType": "P",
        "contractSize": 10,
        "tradingModels": [
          "CLOB"
        ]
      },
      {
        "id": 124,
        "symbol": "option",
        "calendarId": 10,
        "activityStatus": "ACTIVE",
        "minQuantity": 1,
        "maxQuantity": 99999,
        "pricePrecision": 0,
        "quantityPrecision": 0,
        "description": "option instrument",
        "quoteCurrency": "USD",
        "startDate": 1693475040,
        "stopDate": 1725011040,
        "category": "O",
        "subCategory": "C",
        "underlyingAssets": "O",
        "underlyingInstrumentId": 284,
        "deliveryType": "P",
        "contractSize": 10,
        "expiryDate": "2025-01-01",
        "strike": 10,
        "exerciseType": "A",
        "tradingModels": [
          "CLOB"
        ]
      },
      {
        "id": 125,
        "symbol": "spot",
        "calendarId": 10,
        "activityStatus": "ACTIVE",
        "minQuantity": 1,
        "maxQuantity": 99999,
        "pricePrecision": 10,
        "quantityPrecision": 10,
        "minPrice": 1,
        "maxPrice": 1000,
        "description": "spot",
        "quoteCurrency": "USD",
        "auctions": [],
        "category": "I",
        "subCategory": "F",
        "tradingModels": [
          "CLOB"
        ]
      },
      {
        "id": 126,
        "symbol": "spread",
        "calendarId": 10,
        "activityStatus": "ACTIVE",
        "minQuantity": 1,
        "maxQuantity": 999999,
        "pricePrecision": 10,
        "quantityPrecision": 10,
        "description": "spread instrument",
        "quoteCurrency": "USD",
        "startDate": 1693475100,
        "stopDate": 1725011100,
        "category": "K",
        "underlyingInstrumentId": 240,
        "deliveryType": "P",
        "contractSize": 10,
        "dailyMinPricePercentage": 5,
        "dailyMaxPricePercentage": 15,
        "tickMinPricePercentage": 3,
        "tickMaxPricePercentage": 20,
        "strategyType": "SPREAD",
        "leg1ReferencePrice": 10,
        "legsInstrumentIds": [
          240,
          284
        ],
        "tradingModels": [
          "CLOB"
        ]
      },
      {
        "id": 127,
        "symbol": "spread1",
        "calendarId": 10,
        "activityStatus": ACTIVE,
        "minQuantity": 1,
        "maxQuantity": 999999,
        "pricePrecision": 10,
        "quantityPrecision": 10,
        "description": "spread instrument",
        "quoteCurrency": "USD",
        "startDate": 1693475100,
        "stopDate": 1725011100,
        "category": "K",
        "underlyingInstrumentId": 240,
        "deliveryType": "P",
        "contractSize": 10,
        "strategyType": "SPREAD",
        "leg1ReferencePrice": 10,
        "legsInstrumentIds": [
          239,
          284
        ],
        "tradingModels": [
          "CLOB"
        ]
      },
      {
        "id": 128,
        "symbol": "test",
        "calendarId": 10,
        "activityStatus": "ACTIVE",
        "minQuantity": 0.1,
        "maxQuantity": 9999,
        "pricePrecision": 10,
        "quantityPrecision": 10,
        "description": "Test",
        "quoteCurrency": "USD",
        "tradingModels": [
          "CLOB"
        ]
      }
    ],
    "count": 6,
    "fromSymbol": "test"
  }
}
```
{% endtab %}

{% tab title="Live Update" %}
```json
{
  "q": "v1/exchange.marketdata/instrumentListWithPagination",
  "sid": 17,
  "d": {
    "messageType": "update",
    "instrument": {
      "id": 310,
      "symbol": "bonds",
      "calendarId": 10,
      "activityStatus": "ACTIVE",
      "minQuantity": 0.1,
      "maxQuantity": 77777,
      "pricePrecision": 4,
      "quantityPrecision": 5,
      "description": "bonds instrument - try",
      "quoteCurrency": "USD",
      "startDate": 1693475220,
      "stopDate": 1725097620,
      "category": "D",
      "subCategory": "B",
      "underlyingAssets": "V",
      "coupon": 5,
      "periodOfCoupon": "ANNUAL",
      "accrualConvention": "ACT_ACT",
      "maturityDate": "2024-01-01",
      "tradingModels": [
        "CLOB"
      ],
      "minQuantityTradeEntries": [
        {
          "type": "Block",
          "amount": 444
        },
        {
          "type": "EFRP",
          "amount": 555
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

