# API Changes

### v1.61.0(2026-10-05)

* Added `instructions` field to Operations/Place Order
* Added `eodTime` to Calendar entity&#x20;
* Added `tradeDate` to Calendar entity  and to the response of End of Day API

{% hint style="info" %}
Open API specs: [https://registry.scalar.com/@exberry/apis/exberry-admin-open-api@1.61.00](https://registry.scalar.com/@exberry/apis/exberry-admin-open-api@1.61.00)
{% endhint %}

### v1.60.0(2026-08-25)

* A new `actionType` value was added to the CircuitBreakerRule
* Added a common ASCII validation for multiple fields. Validations:
  * Allowed ASCII chars: ASCII range (32 -126) including both
  * Blank value (multiple spaces only) is not allowed

{% hint style="info" %}
Open API specs: [https://registry.scalar.com/@exberry/apis/exberry-admin-open-api@1.60.0](https://registry.scalar.com/@exberry/apis/exberry-admin-open-api@1.60.0)
{% endhint %}

### v1.59.0(2026-07-30)

* Typos & cleanup
* Added pagination and filters to the Account List
* Changed `status` values of CBR entity
* Added `status` to Calendar, Tick Size, Instrument Group, MP Group
* Removed the `limit` and `offset` fields from the responses of the APIs listed below. These fields were previously returned when included in the request, even though they were not documented as part of the API response. Including them in the request had no impact since the APIs didn't support pagination.
  * Calendars List, Instrument List, Instrument Group List, Tick Size Tables List, CBR List, MP List, apiKey List (both for MP and MP group), MP Group List, Account List, Asset List, Get Candle Adjustments, Get Fee Configurations
* Changed `count` field of the response and `limit` field of the request of `getCandleAdjustments`
* Bugfix to remove `tradingStatus` and `marketStatus` fierlds from the response of `Instruments` API, where added on version 1.58
* Documentation only (changed on v1.58.0):
  * Added `ownerId` and `ownerType` to apiKey object
  * expireIn Get token response is stringified int instead of Int

{% hint style="info" %}
Open API specs: [https://registry.scalar.com/@exberry/apis/exberry-admin-open-api@1.59.21](https://registry.scalar.com/@exberry/apis/exberry-admin-open-api@1.59.21)
{% endhint %}

### v1.58.0(2026-06-24)

* This release includes significant internal refactoring of platform components. These changes are designed to be transparent to users and integrations. Except below items, no functional or API changes are expected:
  * Optional `data` fiels in error response was removed
  * The `code` field in error responses is now consistently returned as a numeric value. Previously, this field could be returned either as a number or as a stringified number
  * Documentation change only:
    * Status code 400 is not returned only for When exceeding Maximum request size of 60KB , but also for Invalid JSON
    * Route not found has 404 http status code
* New error code in CBRs operations: "You can send up to 750 instruments per request"

### v1.57.0(2026-06-04)

* Added Scheduled Trading Halt
* Change "Account already exists" error code in include accountId

### v1.56.0(2026-05-12)

* Added supporting Binary Event Contract instruments
* Added additional details to the price band validation error message on orders API
* Added `metadata` as optional property to replace order API (it was added on version 1.51)
* Fix typo on Set News sample

### v1.55.0(2026-04-08)

* Added the list of `applicablePriceBands`

### v1.54.0(2026-03-17)

* Added `priceMultiplier` and `priceUnit` to the Instrument
* Fix typos on Accounts API

### v1.52.0(2026-02-02)

* Added `dailyMinPriceAbsolute`, `dailyMaxPriceAbsolute`, `tickMinPriceAbsolute` and `tickMaxPriceAbsolute` to the Instrument
