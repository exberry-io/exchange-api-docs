# Reporting

Refer [here](../../../ws/reporting-api.md) for more details on Reporting APIs.

## Positions

&#x20;`Positions` API is used to retrieve a list of positions.

{% hint style="info" %}
qualifier: v1/exchange.reporting/mp/positions
{% endhint %}

### Request

<table><thead><tr><th width="164.33333333333331">Parameter</th><th width="105.2222900390625">Type</th><th>Description</th></tr></thead><tbody><tr><td>eodDateFrom</td><td>Date</td><td>Optional<br>search for the Positions that EOD date ≥ eodDateFrom<br>Format: YYYY-MM-DD</td></tr><tr><td>eodDateTo</td><td>Date</td><td>Optional<br>search for the Positions that EOD date&#x3C; eodDateTo<br>Format: YYYY-MM-DD</td></tr><tr><td>reportType</td><td>eNum</td><td><p>Allowed values for field:</p><ul><li>MP - returns positions of MPs</li><li>Account - returns positions of positions accounts</li></ul></td></tr><tr><td>mpId</td><td>int</td><td>Optional<br>Search for the Positions by MP (by ID)</td></tr><tr><td>positionAccountId</td><td>int</td><td>Optional<br>Search for the Positions by position Account Id</td></tr><tr><td>instrument</td><td>String</td><td><p>Optional</p><p>Search for the Positions by instrument symbol</p></td></tr><tr><td>orderBy</td><td>object</td><td><p>Optional</p><p>object with 2 parameters:</p><ul><li>field (String) </li><li>Allowed values: lastEventTimestamp</li><li>direction (Asc, Desc) direction</li></ul><p></p><p>If nothing or invalid field was sent the default is [lastEventTimestamp, Desc]</p></td></tr><tr><td>limit</td><td>int</td><td><p>Optional</p><p>How many records to include in each page <br></p><p>If nothing was sent default is 25</p><p>Max value = 100</p></td></tr><tr><td>offset</td><td>int</td><td><p>Optional</p><p>Which record to start send from (if you want page 3 and there are 50 records per page offset should be 150) </p><p>If nothing was sent default is 0 (=first record)</p></td></tr></tbody></table>

### **Response**

&#x20;`Positions` response provides list of all positions for the requested parameters.

<table><thead><tr><th width="199.7777099609375">Field</th><th>Description</th></tr></thead><tbody><tr><td>eodDate</td><td><p>Date that position was persisted as an EOD state.</p><p>Format: YYYY-MM-DD</p><p>Not populated for the current position. </p><p></p><p>Current position is returned when eodDateTo doesn't exist in the request.</p></td></tr><tr><td>mpId</td><td>MP Id</td></tr><tr><td>mpName</td><td>MP Name</td></tr><tr><td>positionAccountId</td><td><p>Id of the Position Account that the positions are held</p><p>Not populated when the position is related to MP</p></td></tr><tr><td>positionAccountName</td><td><p>Position account name</p><p>Not populated when the position is related to MP</p></td></tr><tr><td>instrumentId</td><td>Instrument ID</td></tr><tr><td>Instrument</td><td>Instrument</td></tr><tr><td>grossLongPosition</td><td><p>Gross Long Position</p><p>Returned only when positionCalculation=Gross</p></td></tr><tr><td>grossLongAvgPrice</td><td><p>Average Price of the gross long position (weighted average)</p><p>Returned only when positionCalculation=Gross</p><p><br>grossLongAvgPrice=abs[(BuyPrice*BuyQuantity)BuyQuantity]</p></td></tr><tr><td>grossShortPosition</td><td><p>Gross Short Position</p><p>Returned only when positionCalculation=Gross</p></td></tr><tr><td>grossShortAvgPrice</td><td><p>Average Price of the gross short position (weighted average)</p><p>Returned only when positionCalculation=Gross</p><p><br>grossShortAvgPrice=abs[(SellPrice*SellQuantity)SellQuantity]</p></td></tr><tr><td>netPosition</td><td><p>Net Position</p><p>Returned only when positionCalculation=Net</p></td></tr><tr><td>netAvgPrice</td><td><p>Average Price of the net position (weighted average)</p><p>Returned only when positionCalculation=Net</p><p><br>netAvgPrice=[max(grossLong + grossShort, 0)*grossLongAvgPrice - max(grossShort*(-1) -grossLong, 0)*grossShortAvgPrice)abs(netPosition)]</p></td></tr><tr><td>lastEventTimestamp</td><td><p>eventTimestamp of the last event of the position</p><p>format YYYY-MM-DDThh:mm:ss.SSSSSS</p></td></tr><tr><td>lastEventId</td><td>eventId of the last event of the position</td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="152">Code</th><th>Message</th></tr></thead><tbody><tr><td>1</td><td><code>Temporary failure to retrieve this data</code></td></tr><tr><td>1001</td><td><code>Wrong [FieldName] format</code> <br><code>eodDateTo must be greater than eodDateFrom</code></td></tr><tr><td>1007</td><td><code>Invalid session</code></td></tr><tr><td>1008</td><td><code>Insufficient permissions</code></td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Request" %}
```json
{
  "q": "v1/exchange.reporting/mp/positions",
  "sid": 55,
  "d": {
    "positionAccountId": "12940",
    "reportType": "Account",
    "limit": 25,
    "offset": 0
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "q": "v1/exchange.reporting/mp/positions",
  "sid": 55,
  "d": {
    "positions": [
      {
        "mpId": 15,
        "mpName": "JPM2",
        "positionAccountId": 12940,
        "positionAccountName": "MP2-PosAcc1",
        "instrumentId": 46,
        "instrument": "INST1",
        "netPosition": -100,
        "netAvgPrice": 1.23,
        "lastEventTimestamp": "2026-05-03T12:36:52.810155",
        "lastEventId": 102
      }
    ],
    "count": 1
  }
}
```
{% endtab %}
{% endtabs %}

