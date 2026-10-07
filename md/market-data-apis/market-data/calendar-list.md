# Calendar List

This API provides snapshot and real-time updates for calendar details.&#x20;

Upon successful subscription, a snapshot of all calendars is sent. The last message of the snapshot contains `lastMessage=Y`. Any changes to the calendars after the snapshot are sent as subsequent updates.

In cases where there are no calendars to return, the system will send an empty message.

{% hint style="info" %}
qualifier: v1/exchange.marketdata/calendarList
{% endhint %}

### **Request**

No request parameters



### **Response**

Similar to [this](https://documenter.getpostman.com/view/6229811/TzCV3jcq#94a026ea-4e5c-48ae-84f9-9980a8e58278) with below changes:

* All numbers are non-stringified

### **Error Codes**

<table><thead><tr><th width="150">Code</th><th width="554.4285714285713">Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td><mark style="color:blue;">(NEW v1.52.0)</mark><br>2</td><td>Stream disconnected</td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```javascript
{
  "q": "v1/exchange.marketdata/calendarList",
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
  "q": "v1/exchange.marketdata/calendarList",
  "sid": 14,
  "d": {
    "id": 2692,
    "name": "cal3",
    "timeZone": "+05:30",
    "marketOpen": "09:30",
    "marketClose": "12:37",
    "tradingDays": [
      "MONDAY",
      "TUESDAY",
      "WEDNESDAY",
      "THURSDAY",
      "FRIDAY",
      "SATURDAY",
      "SUNDAY"
    ],
    "holidays": [],
    "auctions": [
      {
        "days": [],
        "startTimes": [],
        "duration": 300000,
        "matchingAlgorithm": "EQUILIBRIUM_PRICE",
        "allowedTimeInForces": [
          "GTC",
          "GTD",
          "GAA",
          "DAY"
        ],
        "eventsModes": [
          "LIT",
          "INDICATIVE_PRICE"
        ],
        "trigger": "Resume",
        "overrideReferencePrice": false
      },
      {
        "days": [],
        "startTimes": [],
        "duration": 900000,
        "matchingAlgorithm": "EQUILIBRIUM_PRICE",
        "allowedTimeInForces": [
          "GTC",
          "GTD",
          "GAA",
          "DAY"
        ],
        "eventsModes": [
          "LIT",
          "INDICATIVE_PRICE"
        ],
        "trigger": "AutoResume",
        "overrideReferencePrice": false
      },
      {
        "days": [
          "MONDAY",
          "TUESDAY",
          "WEDNESDAY",
          "THURSDAY",
          "FRIDAY",
          "SATURDAY",
          "SUNDAY"
        ],
        "startTimes": [
          "17:14:00"
        ],
        "duration": 300000,
        "matchingAlgorithm": "EQUILIBRIUM_PRICE",
        "allowedTimeInForces": [
          "GTC",
          "GTD",
          "GAA"
        ],
        "eventsModes": [
          "INDICATIVE_PRICE"
        ],
        "trigger": "TimeBased",
        "overrideReferencePrice": false
      }
    ],
    "lastEodDate": "2025-03-13"
  }
}
```
{% endtab %}

{% tab title="Last message" %}
```javascript
{
  "q": "v1/exchange.marketdata/calendarList",
  "sid": 13,
  "d": {
    "id": 2692,
    "name": "cal3",
    "timeZone": "+05:30",
    "tradingDays": [
      "MONDAY",
      "TUESDAY",
      "WEDNESDAY",
      "THURSDAY",
      "FRIDAY",
      "SATURDAY",
      "SUNDAY"
    ],
    "holidays": [],
    "auctions": [],
    "lastMessage": "Y"
  }
}
```
{% endtab %}

{% tab title="Empty message" %}
```javascript
{
  "q": "v1/exchange.marketdata/calendarList",
  "sid": 17,
  "d": {
    "lastMessage": "Y"
  }
}
```
{% endtab %}
{% endtabs %}









